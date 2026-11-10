# Análise inicial da integração com SupriTI

Autor: Luis Felipe Vieira (identidade Git configurada no ambiente)
Data: 2026-11-09

## Alcance

Leitura estática do repositório `C:/Users/luis.felipe/Desktop/codex/solicitaTEC`, incluindo instruções, trechos do escopo, controladores, serviços de estoque e solicitações, autenticação e modelo de dados. Não foram alterados arquivos do SupriTI, executadas movimentações ou consultados dados de produção. Os resultados descrevem esta cópia do código; não comprovam a versão instalada em produção. Esta análise não constitui revisão Goldfish nem aprovação de arquitetura.

## Evidências encontradas

| Tema | Comportamento encontrado | Fonte no SupriTI |
| --- | --- | --- |
| Solicitações comuns | Criação restrita a REQUESTER; decisão e entrega permitidas a TECHNICIAN e ADMIN; confirmação de recebimento restrita a REQUESTER. | `apps/api/src/requests/requests.controller.ts` |
| Duas confirmações | O técnico registra a entrega e o solicitante confirma o recebimento. A baixa ocorre no registro da entrega, antes da confirmação do solicitante. | `apps/api/src/requests/requests.service.ts`, métodos `deliver` e `confirm` |
| Reserva comum | A aprovação reserva saldo; há ajuste de quantidade, prorrogação e rotina de expiração que libera reservas. O escopo estabelece sete dias corridos, salvo prorrogação. | `requests.service.ts`, métodos de decisão, ajuste, prorrogação e expiração; `docs/ESCOPO_FUNCIONAL_TECNICO.md`, seção 7 |
| Itens internos | `requesterVisible` controla a disponibilidade ao solicitante. O serviço de solicitações rejeita produto inativo ou não visível ao solicitante na criação. | `apps/api/prisma/schema.prisma`; `requests.service.ts`, criação; `apps/api/src/catalog/catalog.service.ts` |
| Uso interno | `INTERNAL_USE` é uma movimentação manual permitida a técnico e administrador. Não passa pelo pedido comum nem por uma segunda aprovação nesse caminho. | `apps/api/src/stock/stock.controller.ts`; `stock.service.ts`, `validateManualMovement`; escopo, seções de perfis |
| Efeito do uso interno | Reduz imediatamente o saldo físico e grava `reservedDelta: 0`. Impede saldo físico resultante inferior ao reservado. Não converte uma reserva específica em consumo. | `stock.service.ts`, `createMovement` e `physicalDelta` |
| Dados da saída | Produto, quantidade, motivo, observação opcional e chave UUID de idempotência; produtos fora de insumos de impressão exigem estado Novo ou Recondicionado. | `apps/api/src/stock/dto/create-stock-movement.dto.ts`; `stock.service.ts` |
| Repetição da saída | Consulta chave persistida, compara campos da operação e retorna movimentação existente; há índice único e tratamento de conflito de unicidade. | `stock.service.ts`, `createMovement`, `assertSameOperation`, `resolveIdempotencyConflict`; `schema.prisma` |
| Autoria e correções | Movimentação registra usuário executor; auditoria registra autor, papel e saldos. Estorno manual é restrito a administrador e registra outra movimentação. | `stock.controller.ts`; `stock.service.ts`, criação e estorno |
| Autenticação examinada | Rotas de estoque usam sessão por cookie ligada a usuário ativo; os perfis autorizados nessas rotas são TECHNICIAN e ADMIN. | `apps/api/src/auth/session.guard.ts`; `stock.controller.ts` |

## Consequências para o planejamento

A segregação entre solicitante e equipe técnica existe no fluxo de solicitações comuns. Ela não descreve todo o consumo interno: o próprio produto já autoriza técnico ou administrador a registrar uso interno diretamente. Isso esclarece o alcance da restrição registrada anteriormente, sem autorizar uma futura conta de integração a acumular permissões.

O requisito de reservar ao selecionar uma peça e consumir somente ao resolver o chamado ainda não encontra um fluxo completo nas rotas examinadas. Reutilizar a saída direta apenas na resolução deixaria a peça sem reserva durante o atendimento. Lançar a saída na seleção anteciparia a baixa, contrariando D08. Usar uma solicitação comum para peças internas também encontra bloqueios de perfil e visibilidade.

Uma reserva criada para o chamado precisaria ser consumida por uma operação que também reduza sua reserva. A saída manual atual não faz isso e pode ser recusada por proteger o saldo já reservado. Não basta acrescentar uma chamada de reserva antes de usar a saída existente.

A idempotência existente é evidência favorável para o consumo manual, mas não comprova um contrato completo para integração. Ainda faltam verificar por testes a perda de resposta e a concorrência, além de definir consulta de resultado, identidade e vínculo ao chamado. A comparação de repetição examinada não inclui motivo nem observação; logo, repetir a mesma chave com outra observação não atualiza automaticamente a referência do chamado.

Uma referência ao chamado cabe na observação atual, porém não equivale a um vínculo estruturado. Nas rotas examinadas também não foi encontrada reserva própria de chamado nem autenticação específica de serviço. Isso não é uma afirmação sobre todos os arquivos do repositório.

## Decisão confirmada após a análise

O usuário confirmou que o técnico deve continuar podendo consumir peças por conta própria, como no uso interno atual. O consumo interno associado a chamado não exigirá aprovação de outra pessoa. Isso não altera a segregação das solicitações comuns nem aprova uma identidade técnica com permissões amplas.

Permanecem vigentes a reserva durante o atendimento e a baixa somente na resolução, com remoção manual dos materiais não usados e revisão final. A autonomia do técnico não elimina essas etapas. A identidade da integração, a identificação do técnico humano e a implementação da reserva continuam pendentes.

## Próxima decisão a esclarecer

Definir o tratamento das reservas em cancelamento ou abandono de chamado. As mudanças necessárias no SupriTI passam a ser consolidadas em `mudancas_necessarias_supriti.md`.

## Autenticações separadas e confiança entre aplicações

O usuário informou que não existe serviço institucional central de contas; há domínio para hospedagem de sites e o AD está pendente. Decidiu adiar login único para futura integração com AD e continuar o planejamento com autenticações humanas separadas.

O usuário propôs que Chamados envie reserva ou baixa com autorização própria da aplicação, e que o SupriTI registre o usuário informado como responsável, sem autenticar essa pessoa no SupriTI. Também sugeriu uma categoria própria para operações originadas de Chamados. Essa sugestão ainda não define se categoria significa origem ou novo tipo de movimentação.

O fluxo é viável como proposta se o SupriTI autenticar a aplicação remetente. Nas rotas examinadas, existe autenticação humana por sessão; não foi identificado um mecanismo próprio para essa integração. Não é necessário compartilhar logins humanos ou aguardar AD para implementá-lo.

Hipótese técnica: credencial exclusiva entre backends, enviada por canal protegido e verificada pelo SupriTI. As permissões são concedidas no próprio SupriTI à identidade autenticada da aplicação; não são aceitas apenas porque o corpo da requisição declara um nível de autorização. A credencial não deve chegar ao navegador. Escolha entre segredo de serviço, tokens ou outro mecanismo ainda não foi feita.

Chamados valida a sessão humana e a permissão para a ação antes de registrar a intenção na fila. O SupriTI valida a identidade da aplicação, o conjunto de operações permitido e as regras de estoque. Ele confia na autoria externa atestada por Chamados, sem autenticar novamente o técnico. Não existe prova independente de login dessa pessoa no SupriTI nesse desenho.

Registrar separadamente executor técnico (identidade da aplicação), autor humano externo (identificador estável e nome registrado no momento), origem, chamado, intervenção e operação. Nome ou login sozinho pode mudar, repetir ou ser reutilizado; não é chave confiável de identidade. A origem e o identificador externo permitem distinguir pessoas sem exigir uma conta SupriTI por técnico. A escolha de adaptar o modelo de auditoria ou usar identidade técnica local continua pendente.

Uma origem estruturada pode distinguir consumo por Chamados sem alterar a natureza de uso interno. Criar novo tipo de movimentação é uma alternativa a avaliar pelos efeitos em relatórios, estorno, validações e compatibilidade. Não usar uma categoria de produto para representar a origem da movimentação.

### Fluxo próprio de materiais de Chamados

O usuário esclareceu que pretende regras diferentes, especificamente para suprir a ausência de reserva no uso interno atual. Fica confirmada a necessidade funcional de um fluxo próprio de integração para reserva e posterior consumo de materiais de Chamados. O nome e a representação em tipos de movimentação ainda não estão definidos. Uma nova categoria ou valor enumerado, isoladamente, não implementa esse fluxo.

Regras necessárias para detalhamento: reserva confirmada reduz disponibilidade sem reduzir saldo físico; consumo converte a reserva vinculada em saída, reduzindo saldo físico e encerrando a quantidade reservada na mesma transação; remoção manual solicita liberação da reserva sem consumo; alterações de quantidade precisam atualizar a reserva correspondente. Vincular reserva à origem, chamado e intervenção, distinguindo saldo reservado global de uma reserva pertencente ao chamado.

Se a resolução ocorrer antes de a reserva ter sido confirmada, a intenção vigente passa a consumo, conforme decisão anterior. O SupriTI deverá distinguir reserva comprovadamente inexistente de reserva cujo resultado é desconhecido. Na ausência comprovada de reserva, a operação de consumo precisa validar disponibilidade e efetuar a saída atomicamente; não deve depender de uma reserva obsoleta enviada depois. Se houver reserva, consumir a reserva correspondente sem descontar duas vezes. Essas operações específicas ainda não existem no caminho manual examinado.

A reserva de Chamados não precisa herdar automaticamente a aprovação, confirmação de recebimento ou expiração de sete dias das solicitações comuns. A autonomia de consumo do técnico permanece vigente. A validade das novas reservas e o tratamento de chamados abandonados ainda precisam ser definidos. Estorno continua distinto de liberação de reserva, e reabertura não desfaz consumo anterior.

Decisão posterior: o usuário definiu que reservas de Chamados não têm prazo de validade e dependem do ciclo do chamado. Solicitou processo independente das solicitações comuns e um arquivo específico com todas as mudanças necessárias no SupriTI. Essas decisões e a lista de alterações estão consolidadas em `mudancas_necessarias_supriti.md`. A remoção manual de material continua liberando a reserva correspondente; cancelamento e abandono ainda exigem definição.

Ao discutir cancelamento, o usuário acrescentou que o encerramento deve perguntar ao técnico se as peças usadas foram devolvidas ao estoque, executando a operação correspondente. O alcance da pergunta e as regras por material e estado de sincronização estão pendentes de esclarecimento no arquivo de mudanças.

Esclarecimento posterior: na resolução normal deve aparecer mensagem de confirmação com o resumo dos itens usados, perguntando se o técnico confirma o uso. Assim, a revisão da resolução confirma consumo, enquanto a pergunta sobre devolução permanece no contexto de cancelamento. Detalhes por material e quantidade no cancelamento continuam pendentes.

O usuário definiu que, no cancelamento, o técnico pode manter todos ou alguns dos itens, desde que descreva por que os manteve ou utilizou. Registrar a baixa vinculada ao chamado cancelado e à justificativa, sem repetir baixas já confirmadas. Quantidades parciais de um mesmo material e cancelamento com integração pendente ainda precisam de esclarecimento. A decisão está detalhada em `mudancas_necessarias_supriti.md`.

Esclarecimento posterior: o cancelamento também pode ser concluído com integração pendente e deve seguir o mesmo tratamento de falhas da resolução. Preservar intenções de estoque, apresentar pendência ou divergência e retomar com os limites e a prevenção de duplicidade acordados. A divisão parcial de quantidades do mesmo material ainda permanece aberta.

O usuário confirmou a divisão parcial de quantidades do mesmo material no cancelamento: é possível manter uma unidade e devolver as demais. Registrar quantidades por destino, justificar a parte mantida e gerar os efeitos de estoque correspondentes sem duplicação. Consolidado no arquivo de mudanças do SupriTI.

O usuário definiu uma única descrição justificando todos os materiais mantidos no cancelamento. Não exigir descrição individual por material. Preservar a justificativa junto do conjunto de materiais e quantidades confirmado, com autoria e histórico.

Permanecem necessários: credencial revogável, limites de carga, chave estável de idempotência, prevenção de reserva obsoleta, conversão segura de reserva em consumo e distinção entre recusa e falha transitória. A autenticação entre aplicações não substitui essas garantias.

## Proposta de entrada única e módulos

O usuário afirmou que a identidade de integração não deve poder corrigir permissões. Essa restrição fica registrada como proibição de administrar ou alterar permissões. Não interpretar essa frase automaticamente como autorização ou proibição de corrigir saldo: são operações distintas e a segunda continua sem decisão explícita.

O usuário propôs login único para SupriTI e sistema de chamados, com uma tela de entrada seguida da escolha do módulo. Trata-se de proposta em exploração, não decisão de transformar as aplicações em um único sistema distribuído ou de compartilhar bancos e sessões.

### Avaliação inicial

- Uma entrada comum pode usar uma identidade central para autenticar as pessoas. Cada aplicação mantém suas autorizações e valida o acesso no backend; exibir ou esconder um módulo no portal não substitui essa validação.
- O login único pode reduzir duplicação de credenciais, mas não elimina a representação local da pessoa quando necessária para autoria, histórico e permissões. Associar a identidade central por identificador estável, não somente por nome ou endereço de e-mail mutável. Mudanças nos vínculos existentes precisam preservar a auditoria.
- O login de pessoas e a autenticação entre aplicações resolvem problemas distintos. A fila precisa operar mesmo sem o técnico conectado e após o fim da sessão. Manter identidade própria, restrita e revogável para a comunicação automática, com autoria humana registrada separadamente.
- Uma identidade central cria uma dependência comum: indisponibilidade pode impedir novos logins nos dois módulos. Definir comportamento de sessões já abertas, revogação, desligamento de usuários e acesso administrativo de recuperação. Não pressupor disponibilidade ou mecanismos de contingência sem desenho.
- Uma credencial humana comprometida pode alcançar mais de um módulo, conforme os acessos concedidos. Preservar permissões independentes e definir autenticação e encerramento de sessões adequados ao ambiente.
- Login único não exige banco compartilhado, mesma aplicação ou implantação conjunta. Compartilhar cookie diretamente ou tornar o SupriTI o autenticador improvisado acrescenta acoplamento e exige análise específica; não foi escolhido.
- A migração precisa considerar usuários e históricos existentes no SupriTI, associação das identidades e transição do login atual sem perda de acesso administrativo.

Hipótese em discussão: portal de entrada com dois módulos, identidade humana central e autorização própria em cada módulo; integração de estoque com identidade de serviço separada. Antes de escolher tecnologia, levantar infraestrutura institucional de identidade já disponível e requisitos de hospedagem e operação.

## Avaliação da identidade própria de integração

O usuário propôs que o SupriTI trate as requisições como um caso próprio, com permissões do sistema de chamados, evitando traduzir usuários e perfis entre os sistemas. A proposta está em avaliação, sem aprovação de arquitetura. Não foi realizada revisão Goldfish nesta etapa.

### Problemas e limites

1. **Confiança sem autenticação.** Um campo enviado na requisição dizendo que ela veio do sistema de chamados não comprova a origem. O SupriTI deve autenticar a aplicação remetente no backend e associar suas permissões à credencial verificada. Uma credencial exposta no navegador permitiria uso fora do fluxo esperado.
2. **Permissões amplas.** A autorização local do técnico não obriga o SupriTI a aceitar qualquer operação. O sistema de chamados decide quem pode iniciar ações; o SupriTI limita quais ações a aplicação pode executar e valida saldo, quantidade, produto e reserva. Permitir consumo não implica permitir correção de inventário, estorno ou administração de usuários.
3. **Correção de saldo continua distinta.** A nova identidade pode resolver a autorização do consumo sem duplicar cadastros humanos, mas não resolve quem fará a correção física e cadastral. Liberar correção automática para eliminar uma divergência permitiria à mesma integração alterar a evidência de saldo e consumir sobre ela. Isso exige decisão separada e controle explícito.
4. **Autoria humana.** Uma identidade única não deve apagar quem iniciou cada operação. Preservar identificador externo estável do técnico, sistema de origem, chamado, intervenção e datas, além da identidade executora. O técnico pode ser uma referência externa, sem conta local. O SupriTI receberá essa autoria como informação atestada pela aplicação autenticada; isso não prova, por si só, uma autenticação humana no SupriTI.
5. **Fila e revogação.** O envio pode ocorrer após a retirada de acesso do técnico. É preciso definir se uma intenção de consumo já autorizada e relativa a peça utilizada deve continuar sendo executada, quem pode cancelá-la e como impedir novas intenções sem autorização. Não reavaliar permissões humanas de forma implícita a cada repetição nem ignorar revogações sem regra definida.
6. **Falhas e concorrência.** A identidade própria não elimina resposta perdida, duplicação, reserva que já ocorreu nem envio fora de ordem. Identificar intenção e versão, impedir que uma reserva antiga seja executada depois do consumo e manter reconciliação e idempotência. A execução de consumo com reserva confirmada precisa converter ou encerrar a reserva na operação adequada.
7. **Alcance de uma credencial comprometida.** Limitar operações e materiais autorizados, permitir revogação e troca da credencial e manter limites de carga do lado do SupriTI. A fila do remetente, sozinha, não limita outros clientes nem múltiplos executores mal coordenados.
8. **Mudança real no SupriTI.** As rotas examinadas usam sessão ligada a usuário local e autorização por perfil. `AuthenticatedUser` exige identidade local; `StockMovement.performedById` e `AuditEvent.actorId` referenciam `User`. A solução exige um caminho de autenticação e auditoria adaptado ou uma identidade técnica local restrita. Não basta adicionar um texto indicando a origem. A alternativa de identidade técnica local não exige criar uma conta SupriTI para cada técnico.

### Hipótese para discussão

Separar a identidade da aplicação das identidades humanas. O sistema de chamados controla suas permissões dinâmicas e envia a autoria externa; o SupriTI autentica a aplicação, concede somente operações de integração acordadas e preserva suas regras de estoque. Não traduzir N1/N2/N3 ou grupos para perfis locais. Manter a regularização de saldo fora desse conjunto inicial até decisão explícita. Esta hipótese é uma proposta, não uma autorização de implementação.

## Estimativa inicial de volume

O usuário informou que não existe medição ou quantidade fixa de chamados. Há dias com muitas solicitações e outros com quase nenhuma, em um parque tecnológico defasado. Para planejamento inicial, indicou média de cinco chamados por dia em condições normais. A proporção de chamados com peças, o número de materiais por chamado e o tamanho dos picos continuam desconhecidos.

Essa média não constitui limite de capacidade, máximo diário nem estimativa direta de requisições ao SupriTI. Consultas, reservas, alterações, liberações, consumos e repetições podem gerar várias operações por chamado. O dimensionamento deve contemplar também o acúmulo durante indisponibilidade e sua recuperação gradual. Limites numéricos permanecem pendentes de definição e validação.

## Resolução com integração pendente

O usuário confirmou que o chamado deve ser fechado normalmente quando a peça já foi utilizada e a conexão com o SupriTI falhar. O chamado deve demonstrar a pendência, e o sistema deve continuar tentando concluir a integração. A conexão pendente não pode sobrecarregar o sistema atual. Neste documento, fechar normalmente significa permitir a resolução técnica prevista no planejamento; não define um novo estado de fechamento definitivo nem elimina a janela de reabertura.

### Proteções aprovadas para detalhamento

Após a apresentação das proteções, o usuário respondeu “ok, de acordo”. Ficam aprovadas como diretrizes a fila persistente centralizada, os intervalos crescentes entre tentativas, a pausa dos envios durante indisponibilidade com verificações limitadas, os limites de carga inclusive na recuperação, a retomada após reinício e a prevenção de baixa duplicada. Também foi aceita a distinção entre falhas transitórias e recusas que exigem intervenção. Essa concordância não define capacidade, intervalos numéricos, tecnologia nem comprova o funcionamento da integração.

Os detalhes técnicos a seguir orientam o desenho; mecanismos específicos ainda precisam ser especificados e validados.

- Gravar a resolução e a intenção de consumo de forma atômica no banco local. A tentativa externa ocorre em segundo plano. Se a intenção não puder ser persistida, não apresentar o processo como concluído com pendência recuperável.
- Manter o resultado técnico e a situação da integração separados. Exemplo de apresentação: “Resolvido · Sincronização com SupriTI pendente”. A pendência permanece visível mesmo que o chamado seja reaberto.
- Guardar operações numa fila persistente, com identificador estável por consumo, próxima tentativa e resultado anterior. Não manter uma conexão, temporizador ou processo exclusivo por chamado.
- Limitar tanto a quantidade de envios simultâneos quanto o total de envios por intervalo. Aplicar tempo máximo de espera por requisição. Os valores dependem da capacidade e do volume ainda desconhecidos.
- Aumentar progressivamente o intervalo após falhas transitórias, com pequena variação entre operações e um intervalo máximo. Evitar que todas as operações voltem ao mesmo tempo.
- Após falhas que indiquem indisponibilidade geral, suspender os envios normais para o SupriTI e permitir poucas tentativas de verificação. Retomar gradualmente, sem descarregar toda a fila acumulada de uma vez. Coordenar esse limite entre processos caso haja mais de um executor.
- Manter operações pendentes após reinício e permitir que outro executor retome operações interrompidas, sem duplicar o consumo. A execução precisa de controle de posse temporária e recuperação de operações abandonadas.
- Preservar a mesma chave de idempotência ao repetir a mesma operação. Uma resposta perdida pode significar que a baixa já ocorreu. Confirmar o resultado ou repetir somente com a proteção externa verificada; não gerar outra chave para tentar novamente.
- Separar falha de conexão de recusa de negócio. Saldo insuficiente, reserva expirada e dados inválidos exigem tratamento; não devem produzir tentativas rápidas e infinitas. Credencial inválida também requer correção operacional. Essas pendências permanecem registradas e visíveis, com retomada após a correção adequada.
- Mostrar idade da pendência, última tentativa, próxima tentativa e motivo compreensível. Acompanhar tamanho e idade da fila, tempo de resposta e falhas. Definir responsável pelas pendências que não se resolvem automaticamente.
- Armazenar o necessário para recuperar a operação, com histórico de tentativas limitado ou resumido. A indisponibilidade não pode gerar crescimento ilimitado de logs nem consultas de varredura completa da fila.

As diretrizes de proteção estão aprovadas, ainda sem escolha de tecnologia ou valores numéricos. Em decisão posterior, o usuário autorizou atendimento com reserva pendente por indisponibilidade, conforme seção seguinte.

## Reserva pendente e divergência de estoque

O usuário definiu que a indisponibilidade na reserva deve receber tratamento semelhante à indisponibilidade na finalização: a requisição fica pendente e o atendimento pode prosseguir. Se o chamado for resolvido antes da sincronização, a intenção de reserva pendente deve ser substituída pela intenção de consumo da finalização. Isso não autoriza eliminar o histórico nem presumir que uma requisição já enviada não tenha produzido efeito externo.

A interpretação técnica proposta é substituir somente a ação ainda necessária, preservando operações anteriores e seus resultados. Se uma reserva puder ter sido efetivada sem resposta, será necessário reconciliar seu resultado antes do consumo. A capacidade de converter essa reserva em uso interno ainda exige extensão e validação no SupriTI. Não reenviar a reserva obsoleta após a resolução nem executar duas baixas para a mesma intervenção.

O usuário também definiu que, se o SupriTI responder que não há estoque disponível, o chamado deve permanecer fechado tecnicamente, mostrando um status específico de discordância entre os sistemas. Esse status deve alertar o técnico para regularizar a questão no SupriTI e posteriormente atualizar a situação no novo sistema. Mantém-se o entendimento de resolução técnica, sem redefinir fechamento definitivo ou janela de reabertura.

Nome de interface proposto, ainda não aprovado literalmente: “Resolvido · Divergência de estoque no SupriTI”. A recusa de estoque deve ser distinguida da pendência de conexão. Não declarar consumo confirmado, forçar saldo negativo ou repetir continuamente uma recusa de negócio.

A atualização posterior não deve afirmar sincronização apenas porque o técnico declarou ter corrigido o estoque. É necessário confirmar o resultado externo, por nova tentativa segura ou reconciliação da movimentação que já exista. Se a regularização envolver registrar o consumo manualmente no SupriTI, reconciliar essa movimentação em vez de gerar outro consumo. As rotas e o vínculo verificável necessários ainda não estão definidos.

### Retomada solicitada pelo técnico

O usuário confirmou que o alerta deve oferecer uma opção para o técnico selecionar depois de realizar a retificação no SupriTI e tentar a sincronização novamente. Texto proposto para o botão: “Tentar sincronizar novamente”.

O usuário esclareceu que, nesse fluxo de regularização, o técnico deve apenas arrumar o saldo do SupriTI para que o novo sistema solicite a baixa. Não deve registrar manualmente o consumo como parte dessa regularização. Após a correção, a opção do alerta retoma a mesma intenção de consumo, preservando sua identificação e proteção contra duplicidade. Esse esclarecimento resolve a alternativa de consumo manual no fluxo planejado; as ressalvas anteriores sobre movimentação manual descrevem um risco de desvio, não uma etapa aprovada.

Há uma dependência de autorização a esclarecer: no código examinado, `StockService.validateManualMovement` restringe `INVENTORY_CORRECTION` ao perfil ADMIN. A responsabilidade operacional atribuída ao técnico não concede automaticamente essa permissão no SupriTI. Ainda é necessário decidir se ele acionará um administrador para corrigir o saldo ou se haverá mudança explícita e controlada nas permissões existentes.

O acionamento solicita nova execução pela fila, respeitando os limites de carga e a proteção contra duplicidade acordados. Não limpa a divergência nem marca consumo confirmado por declaração do técnico. Em caso de nova recusa, manter o alerta e atualizar o motivo; em caso de falha de conexão, indicar pendência de conexão e aplicar as tentativas espaçadas. Remover a pendência somente após confirmação verificável do resultado no SupriTI.

Como detalhe técnico proposto, cliques repetidos não devem criar operações de consumo adicionais nem execuções concorrentes da mesma operação. Registrar quem solicitou a retomada e quando.

### Cenários de falha a validar antes da implementação

SupriTI indisponível por vários dias; resposta lenta; resposta perdida após baixa; reinício do novo sistema durante envio; duas instâncias processando a mesma operação; retomada com grande fila; sucesso em apenas parte dos materiais; credencial revogada; reserva expirada antes do consumo; reabertura de chamado ainda pendente. Em todos eles, preservar a intenção registrada, impedir repetição indevida e respeitar os limites de carga.

## Evidências ainda necessárias

- Confirmar se esta cópia corresponde à versão utilizada e se o fluxo real acompanha o código.
- Definir como identificar o técnico humano quando uma integração executar a operação.
- Especificar reserva por chamado, expiração, alteração, liberação e conversão em uso interno, caso essa extensão seja escolhida.
- Validar falhas parciais entre materiais, perda de resposta, repetição e reabertura sem novo consumo das operações anteriores.
- Definir se pendências de estoque impedem a resolução técnica e quem as regulariza.

## Esclarecimento de acesso para correção de estoque — 2026-11-09

O usuário confirmou que o próprio técnico pode acrescentar ou diminuir quantidades no estoque usando suas permissões de acesso no SupriTI. Isso resolve a pendência funcional sobre o responsável pela regularização: o técnico corrige o saldo diretamente no SupriTI e solicita nova sincronização em Chamados; a integração executa o consumo. Não se concede correção de saldo à identidade de integração. A diferença entre esse acesso informado e a restrição a ADMIN observada na cópia examinada deve ser verificada no desenho técnico, sem presumir alteração de permissões ou equivalência com a versão em uso.
