# Mudanças necessárias no SupriTI para integração com Chamados

Data de atualização: 2026-11-09
Situação: requisitos em planejamento. Este documento não autoriza implementação nem constitui desenho técnico final ou revisão Goldfish.

## Substituição do último técnico ativo na desativação — decisão após revisão Goldfish

Na desativação de um técnico, verificar os chamados aos quais está vinculado. Se ele for o único técnico ou o último participante ainda ativo, o administrador deve escolher um novo técnico para assumir antes de concluir a desativação. A regra inclui chamados fechados ou cancelados com pendências de integração; não deixar essas pendências sem acompanhamento ativo. Havendo outros participantes técnicos ativos, não exigir substituição adicional por esta regra.

Se o substituto não for o próprio administrador que realiza a desativação, enviar a ele uma notificação padrão informando que foi vinculado ao chamado pelo administrador. Preservar a autoria histórica das operações e registrar a atribuição. A substituição não reabre o chamado nem repete operações de estoque.

Esta decisão mais recente substitui a exigência anterior de substituto em todos os chamados abertos mesmo com outros participantes e a dispensa anterior de substituição nos encerrados com pendências. Resolve a responsabilidade ativa levantada no achado F1; a forma de localizar essas pendências no painel ainda precisa ser definida.

## Seção independente de pendências de integração — decisão após revisão Goldfish

O usuário definiu uma seção à parte, independente, para os chamados com pendências de integração, incluindo chamados fechados ou cancelados. A descoberta dessas pendências não depende de notificações nem de o chamado estar em andamento. A seção acompanha a pendência real, sem reabrir o atendimento apenas para exibi-la. Esta decisão complementa a substituição do último técnico ativo e resolve a definição funcional de descoberta das pendências levantada em F1; detalhes de apresentação permanecem para o desenho.

## Nova conferência após alteração dos dados da revisão final — decisão após revisão Goldfish

O usuário confirmou que alterações na solução, no diagnóstico ou no vínculo de equipamento enquanto a revisão final estiver aberta devem receber a mesma proteção já definida para materiais: impedir a confirmação do resumo anterior, apresentar o conteúdo atualizado e exigir nova conferência antes do fechamento. Validar a atualidade do conteúdo no processamento da confirmação, sem depender apenas da interface. Uma tentativa desatualizada não inicia fechamento nem consumo. Esta decisão resolve a lacuna funcional apontada em F2 do relatório Goldfish.

## Reabertura direta equivale ao aceite do pedido pendente — decisão após revisão Goldfish

O usuário definiu que a reabertura direta por um técnico, quando houver pedido de reabertura pendente, deve ser entendida como aceite desse pedido. Aplicar os mesmos efeitos do aceite: encerrar o pedido como aceito, retornar o chamado a Em atendimento, incluir o técnico como participante se necessário, remover os avisos pendentes e enviar a notificação padrão ao solicitante. Evitar duplicação de efeitos e notificações.

Como aceite do mesmo pedido, a reabertura direta participa da regra de primeira ação válida registrada diante de recusa ou cancelamento concorrentes. Esta decisão resolve a interação com pedido pendente apontada em F3. A possibilidade de reabrir diretamente após uma recusa já registrada ainda precisa ser definida.

## Reabertura técnica após recusa de pedido — decisão após revisão Goldfish

O usuário confirmou que, após um técnico recusar o pedido de reabertura, outro técnico pode reabrir diretamente o chamado dentro dos sete dias corridos contados do fechamento mais recente. A recusa encerra o pedido e mantém o chamado fechado naquele momento, mas não impede a reabertura técnica posterior dentro da janela vigente. Essa definição esclarece o alcance do fechamento definitivo por recusa descrito anteriormente.

Preservar a recusa e seu motivo, autoria e data; registrar a reabertura posterior como nova ação, com o motivo exigido para técnicos, sem transformar retroativamente o pedido recusado em aceito. Aplicar as regras já acordadas de retorno a Em atendimento, inclusão do técnico e notificações. A recusa não reinicia nem amplia a janela de sete dias. Esta decisão completa a interação funcional apontada em F3 do relatório Goldfish.

## Preservação da tentativa de envio sem rascunhos — decisão após revisão Goldfish

O usuário confirmou que, diante de erro ao enviar o formulário, os dados devem permanecer na página aberta para correção ou nova tentativa, sem criar rascunhos para o usuário. A exclusão de rascunhos busca evitar carga desnecessária no sistema. Não implementar salvamento manual ou automático para retomada posterior; preservar a tentativa atual na interface não constitui rascunho persistido nem garante recuperação após fechar ou recarregar a página. Esta decisão esclarece F4 do relatório Goldfish.

## Objetivo e fontes

Permitir que o sistema de Chamados consulte materiais, reserve peças durante o atendimento, libere reservas e registre consumo no SupriTI. O SupriTI continua sendo a fonte oficial dos saldos e das movimentações. Não criar estoque paralelo no sistema de Chamados.

Este arquivo consolida as decisões desta discussão e as diferenças encontradas por leitura estática da cópia em `C:/Users/luis.felipe/Desktop/codex/solicitaTEC`. Não houve alteração nesse repositório, execução de movimentações ou validação em produção.

Referências de contexto: `planejamento_suporte_ti.md` e `analise_integracao_supriti.md`. Em caso de conflito, as decisões mais recentes registradas aqui prevalecem sobre propostas anteriores no assunto da integração. Outros assuntos do projeto continuam nos documentos de origem.

## Decisões confirmadas

1. Manter autenticações humanas separadas nos dois sistemas. Login único fica para futura integração com AD.
2. Autenticar a aplicação Chamados no SupriTI sem exigir uma conta SupriTI para cada técnico. O mecanismo de credencial ainda será escolhido.
3. Chamados administra as permissões dos seus técnicos. O técnico pode consumir peças sem aprovação de outra pessoa, como no uso interno atual.
4. Criar um fluxo próprio de reserva e consumo de Chamados, independente do ciclo de solicitações comuns, suas aprovações e sua confirmação de recebimento. Independência do processo não exige duplicar o estoque nem dispensa a concorrência com os demais consumidores de saldo.
5. Reservas de Chamados não possuem prazo de validade. Seu encerramento depende do ciclo do chamado, sem expiração automática por idade. A remoção manual de material não utilizado continua solicitando liberação da reserva correspondente.
6. Selecionar material solicita reserva. Consumo definitivo ocorre na finalização do chamado, após revisão pelo técnico. Cancelar a tela de revisão preserva os dados e não inicia resolução nem consumo por esse cancelamento.
7. O técnico remove manualmente os materiais não utilizados antes de finalizar. A revisão não libera automaticamente itens considerados desnecessários.
8. Indisponibilidade na reserva não impede o atendimento: a operação fica pendente. Se o chamado for resolvido antes da sincronização, a intenção vigente passa de reserva para consumo.
9. Indisponibilidade no consumo não impede a resolução técnica. O chamado mostra a pendência e a integração tenta novamente com carga limitada.
10. Falta de saldo informada pelo SupriTI mantém o chamado resolvido com alerta específico de divergência. Não apresentar consumo como confirmado nem forçar saldo negativo.
11. Na regularização acordada, corrigir apenas o saldo no SupriTI; o consumo será solicitado pela integração. Não lançar manualmente a mesma saída como parte desse fluxo.
12. O alerta no sistema de Chamados permite solicitar nova sincronização após a correção. A pendência só desaparece após confirmação verificável do resultado externo.
13. Usar fila persistente, tentativas espaçadas, pausa durante indisponibilidade, recuperação gradual e proteção contra baixa duplicada. Valores numéricos e tecnologia ainda não foram escolhidos.
14. A identidade de integração não pode administrar ou alterar permissões. A permissão para corrigir saldo não foi aprovada; não confundir essa operação com alteração de permissões.
15. No cancelamento, perguntar ao técnico se as peças foram devolvidas ao estoque e realizar a operação correspondente ao registro existente. Na resolução normal, apresentar mensagem de confirmação com o resumo dos itens usados e perguntar se o técnico confirma seu uso. O esclarecimento mais recente distingue a confirmação de uso na resolução da pergunta sobre devolução discutida no cancelamento. A resposta por material, as quantidades parciais e os efeitos exatos do cancelamento ainda precisam ser detalhados. Não devolver saldo de peças que nunca tiveram saída registrada.
16. O cancelamento permite manter todos ou alguns dos itens. O técnico deve justificar por que manteve ou utilizou esses materiais. Registrar a baixa correspondente vinculada ao chamado cancelado, juntamente com a justificativa. O cancelamento não implica liberação de todos os materiais nem impede consumo justificado. Se a baixa já estiver confirmada, preservar seu vínculo e acrescentar a justificativa sem descontar novamente. A divisão de quantidade dentro de um mesmo item ainda precisa ser definida.
17. O cancelamento pode ser concluído mesmo com o SupriTI indisponível, da mesma forma que a resolução. Persistir as operações de estoque necessárias e mostrar a pendência vinculada ao chamado cancelado. Aplicar as mesmas proteções de carga, repetição segura, distinção de divergência e retomada pelo alerta. Não apresentar baixa, liberação ou devolução como confirmada antes do resultado externo.
18. No cancelamento, permitir divisão da quantidade de um mesmo material entre parte mantida e parte devolvida. Exemplo confirmado: de três memórias, manter uma e devolver duas. A parte mantida exige justificativa e consumo vinculado ao chamado cancelado; a parte devolvida recebe liberação ou devolução conforme o estado da movimentação. A soma das quantidades deve corresponder à quantidade em tratamento, sem valores negativos ou duplicação.

## Diferenças em relação ao código examinado

| Situação atual | Mudança necessária |
| --- | --- |
| Rotas de estoque usam sessão de usuário local e perfis TECHNICIAN ou ADMIN. | Oferecer autenticação e autorização próprias para a aplicação Chamados. |
| INTERNAL_USE faz saída imediata, com `reservedDelta: 0`. | Implementar reserva vinculada ao chamado e sua conversão em consumo. |
| Reservas existentes pertencem a solicitações comuns e têm expiração e prorrogação próprias. | Manter reservas de Chamados fora desse ciclo e das suas rotinas de expiração. |
| O saldo reservado agregado já protege a disponibilidade. | Incorporar as novas reservas ao controle oficial de disponibilidade, mantendo sua propriedade identificável. |
| Autoria da movimentação referencia usuário local. | Registrar aplicação executora e autoria humana externa, sem duplicar contas dos técnicos. |
| Saídas manuais já têm chave de idempotência persistida. | Estender a proteção às novas operações e oferecer consulta verificável de resultados. |
| Correção de inventário é restrita a ADMIN no serviço atual. | Resolver quem fará a regularização; não conceder esse poder implicitamente à integração ou ao técnico. |

Fontes principais: `apps/api/src/stock/stock.controller.ts`, `stock.service.ts`, `dto/create-stock-movement.dto.ts`, `apps/api/src/requests/requests.controller.ts`, `requests.service.ts`, `apps/api/src/auth/session.guard.ts`, `roles.guard.ts` e `apps/api/prisma/schema.prisma`.

## Alterações funcionais necessárias no SupriTI

### Identidade e autorização da integração

Reconhecer a aplicação remetente por credencial verificada no backend. Não aceitar uma declaração de nível de autorização no corpo da requisição como prova de origem. Restringir essa identidade às operações acordadas e permitir sua revogação. A credencial deve permanecer entre backends.

O SupriTI continua validando suas regras de estoque. Não precisa reproduzir os grupos N1, N2 e N3 ou autenticar a pessoa no sistema de Chamados, mas deve guardar a autoria atestada pela aplicação autenticada. Escolher entre identidade técnica local restrita ou adaptação explícita do modelo de identidades e auditoria.

### Reserva própria de Chamados

Registrar reserva com origem, identificadores do chamado e da intervenção, material, quantidade, estado e referências das operações. O modelo e os nomes finais ainda serão definidos. Não reutilizar o status de uma solicitação comum como se fosse uma reserva de Chamados.

Uma reserva confirmada reduz a disponibilidade e aumenta a quantidade reservada, sem diminuir o saldo físico. Deve disputar o mesmo saldo oficial com as solicitações comuns e outras saídas. Verificar disponibilidade dentro da transação, inclusive em acessos concorrentes.

Não expirar reservas de Chamados por tempo. As rotinas de expiração das solicitações comuns não podem liberá-las. Idade elevada pode ser exibida para acompanhamento, sem produzir liberação automática; a interface de acompanhamento ainda é proposta a validar.

### Alteração e liberação

Permitir alteração de quantidade com validação do saldo adicional e liberação da diferença quando houver redução. A remoção manual de material solicita liberação de sua reserva, preservando a trilha de auditoria. Uma reserva já consumida não pode ser simplesmente liberada; correção de consumo é outra operação autorizada.

O usuário determinou consulta ao técnico sobre devolução das peças no contexto do cancelamento e esclareceu que a resolução normal deve perguntar pela confirmação de uso. O cancelamento deve considerar o destino das peças antes da operação. Cancelamento ou abandono ainda exige detalhamento; não presumir liberação automática por ausência de atividade.

No cancelamento, permitir identificar quais itens foram mantidos e quais retornaram ao estoque. Para os mantidos, exigir uma única descrição do técnico explicando por que todos eles foram mantidos ou usados e registrar o consumo associado ao chamado cancelado. Essa justificativa conjunta foi confirmada pelo usuário; não exigir justificativas individuais. Preservar autor, materiais, quantidades e descrição, vinculando-a ao conjunto confirmado no cancelamento.

Para itens mantidos com reserva ativa, converter a reserva correspondente em consumo; sem reserva confirmada, seguir o tratamento de consumo e reconciliação já descrito. Para consumo já confirmado, não repetir a saída. Para os devolvidos, considerar o estado real: liberação quando houver apenas reserva, devolução quando houver saída confirmada e reconciliação se o resultado for desconhecido. Esses efeitos precisam de operações e autorizações específicas no contrato futuro.

O usuário confirmou posteriormente que o cancelamento segue o mesmo tratamento da resolução diante de indisponibilidade: concluir o cancelamento técnico, persistir as operações necessárias e sinalizar a integração pendente. As tentativas automáticas respeitam os limites acordados. Recusas de negócio devem mostrar divergência e permitir retomada após regularização, sem esconder o estado cancelado do chamado nem repetir operações já efetivadas.

Para especificar a operação, distinguir reserva sem consumo, consumo confirmado e operação com resultado desconhecido. Liberar reserva não aumenta estoque físico; registrar devolução de consumo já confirmado pode aumentá-lo após validação da entrada física e das permissões. Se a peça foi utilizada sem baixa sincronizada e voltou ao estoque, reconciliar as intenções pendentes sem gerar entrada fictícia. São distinções técnicas para o desenho, não uma máquina de estados já aprovada. Não repetir baixa ou devolução ao tentar novamente.

### Consumo na resolução

Apresentar ao técnico a revisão final já acordada, com diagnóstico, solução e resumo dos materiais e quantidades, incluindo mensagem que peça confirmação do uso dos itens listados. O texto final da interface não foi escolhido. Confirmar inicia a resolução e a intenção de consumo; cancelar a confirmação retorna ao chamado com dados preservados, sem iniciar consumo por esse cancelamento. Não liberar automaticamente itens não usados: sua remoção manual deve ocorrer antes. Se não houver materiais, o tratamento da confirmação de uso ainda precisa ser definido sem sugerir consumo inexistente.

Quando existir reserva confirmada, consumir a quantidade correspondente e reduzir saldo físico e reservado de forma atômica. Impedir uso da reserva de outro chamado ou intervenção. Não utilizar diretamente a saída manual atual esperando que ela reduza a reserva, pois esse caminho não faz a conversão.

Quando a reserva comprovadamente não tiver ocorrido, atender à intenção final de consumo com validação atômica da disponibilidade. Se não houver saldo, responder com recusa de negócio identificável.

Se o resultado da reserva for desconhecido, consultar ou reconciliar antes de decidir qual caminho executar. O fechamento pode ocorrer enquanto uma tentativa de reserva está em andamento; preservar o resultado dessa tentativa e impedir que uma operação antiga volte a criar reserva após o consumo. O mecanismo de ordenação ou versão ainda precisa ser especificado.

O fechamento não autoriza consumir silenciosamente toda reserva remanescente: a lista final revisada define os materiais usados; o técnico deve remover os demais previamente. O tratamento de divergência entre lista final e reservas confirmadas ainda requer detalhamento.

### Resultados, repetição e recuperação

As operações devem ter identificador estável, resultado persistido e meios de consulta após perda de resposta. Repetir a mesma operação não pode gerar outra reserva, liberação ou baixa. Reutilizar identificador com conteúdo incompatível deve ser recusado. Definir os campos que compõem essa comparação.

Responder de forma distinguível a sucesso, resultado já existente, falta de saldo, reserva incompatível, dados inválidos, falha de autenticação e indisponibilidade. Não misturar status de atendimento com status de estoque. O contrato e os códigos finais não estão escolhidos.

Manter confirmação externa consultável, inclusive identificadores de reserva e movimentação. A reabertura do chamado não deve repetir consumos anteriores; novos materiais de nova intervenção recebem operações próprias.

### Autoria e auditoria

Guardar separadamente a identidade da aplicação executora e o identificador estável do técnico no sistema de Chamados, acompanhado do nome registrado no momento. Guardar referências de chamado, intervenção, operação, material, quantidade, datas e resultado. Nome ou login sozinho não é chave de identidade suficiente.

O nome externo é informação atestada por Chamados, não comprovação independente de login humano no SupriTI. Preservar histórico e rastrear alterações e retomadas. Não expor credenciais em logs.

### Consulta e acompanhamento

Disponibilizar consulta autenticada de materiais com identificadores, nomes, disponibilidade e dados necessários à operação. Definir elegibilidade para o fluxo de Chamados e tratamento de produtos inativos, kits e estados Novo ou Recondicionado.

Permitir localizar reservas e movimentações pela origem e referências externas. Um painel de reservas de Chamados, com pendências e reservas antigas, é proposta para operação; seu desenho e permissões ainda não foram aprovados.

### Proteção do SupriTI e compatibilidade

Aplicar limites de carga da integração também no SupriTI. O remetente terá fila persistente, paralelismo limitado, tempo máximo de requisição e intervalos crescentes, evitando tempestade de retomadas. Dimensionar sem tratar a média estimada de cinco chamados por dia como máximo diário ou número de operações.

Preservar o fluxo de solicitações comuns. As novas reservas devem ser isoladas quanto ao ciclo de vida, mas integradas aos saldos e às transações. Reutilizar regras comuns de consistência quando adequado, evitando duas implementações divergentes de cálculo de estoque.

Revisar relatórios, saldos agregados, estornos, auditoria e migração para contemplar as reservas e consumos de Chamados. Definir se a saída terá tipo próprio ou se o processo próprio produzirá INTERNAL_USE com origem estruturada. Essa representação não foi decidida.

## Responsabilidades no sistema de Chamados

Autenticar e autorizar o técnico; persistir intenção e resolução local de forma atômica quando aplicável; substituir intenção de reserva ainda pendente pela de consumo sem apagar histórico; executar fila com limites; mostrar conexão pendente ou divergência; permitir nova tentativa pelo alerta; preservar identificação de operações e não repetir consumos em reaberturas.

O botão de nova tentativa não limpa alertas por declaração do técnico, não contorna limites de carga e não deve gerar múltiplas execuções da mesma operação. Registro de quem acionou a retomada é detalhe proposto para auditoria.

## Questões ainda abertas

- Quem pode corrigir o saldo no SupriTI, dado que a regra atual exige ADMIN? A resposta sobre proibir alteração de permissões não resolveu a correção de saldo.
- Como encerrar reservas em cancelamento, abandono ou exclusão lógica de chamado conforme o destino informado das peças?
- Como tratar quantidade alterada ou material removido enquanto uma reserva está em processamento?
- Como tratar resolução quando uma liberação solicitada ainda não foi confirmada?
- Como tratar sucesso de apenas parte dos materiais sem repetir os demais?
- Como tratar produto indisponível, inativo, kit ou conservação diferente no consumo?
- Qual a representação dos novos tipos, estados e referências externas e qual o contrato das operações?
- Como validar referências do chamado sem depender de uma conexão de retorno em cada movimentação?
- Como tratar consumo já autorizado se o técnico perder a permissão antes de a fila executar?
- Quais limites, intervalos, responsáveis e indicadores serão usados na operação?
- Como implantar e migrar sem alterar indevidamente as solicitações comuns existentes?

## Cenários de aceite para o futuro desenho

1. Uma reserva de Chamados permanece válida após sete dias, sem ser atingida pela expiração das solicitações comuns.
2. Reservar não reduz estoque físico; consumir a reserva reduz os saldos físico e reservado uma única vez.
3. Duas operações concorrentes não reservam ou consomem o mesmo saldo disponível além do permitido.
4. A remoção manual libera somente a reserva correspondente; cancelar a revisão não inicia consumo.
5. Reserva pendente seguida de resolução não deixa reserva antiga criada depois da baixa.
6. Resposta perdida após reserva ou consumo pode ser reconciliada sem duplicação.
7. Reinício e processamento concorrente recuperam pendências preservando uma única execução efetiva.
8. Falta de saldo retorna divergência identificável; correção seguida de nova tentativa conclui a mesma intenção de consumo.
9. Indisponibilidade prolongada e recuperação com fila acumulada respeitam limites de carga.
10. Autoria externa é preservada sem exigir conta SupriTI para cada técnico; requisição sem credencial válida é recusada.
11. Credencial de integração não altera permissões; correção de inventário não é liberada implicitamente.
12. Reabertura não desfaz nem repete a baixa de uma intervenção anterior.
13. Solicitações comuns continuam funcionando com suas próprias aprovações, reservas, expiração e confirmação de recebimento.
14. Cancelamento com alguns itens mantidos exige justificativa e registra somente os consumos correspondentes, vinculados ao chamado cancelado; itens já consumidos não sofrem nova baixa.
15. Cancelamento durante indisponibilidade preserva o estado cancelado e as intenções de estoque, mostra pendência e permite sincronização posterior sem duplicação.
16. Cancelamento de três unidades com uma mantida e duas devolvidas gera consumo somente da unidade mantida e o tratamento correto das outras duas, sem criar entrada física quando houver apenas reserva.
17. Cancelamento com vários materiais mantidos aceita uma única justificativa cobrindo o conjunto, preservando as quantidades de cada material e sem exigir descrições individuais.

Os cenários são critérios para especificação e validação futura, não testes executados.

## Esclarecimento sobre regularização pelo técnico — 2026-11-09

O usuário confirmou que o próprio técnico consegue corrigir o estoque usando suas permissões de acesso no SupriTI, acrescentando ou diminuindo quantidades. Portanto, no fluxo de divergência, o técnico responsável regulariza o saldo diretamente no SupriTI e depois solicita nova sincronização em Chamados. Não é necessário prever o acionamento obrigatório de outro administrador nesse fluxo.

Esta confirmação resolve a questão funcional sobre quem corrige o saldo. Não concede correção de saldo à identidade de integração nem altera automaticamente perfis ou permissões. A restrição a ADMIN observada na cópia de código continua sendo uma evidência daquela cópia; verificar sua correspondência com os acessos e a versão em uso durante o desenho técnico.

A regularização corrige apenas o saldo. O consumo vinculado ao chamado continua sendo executado pela integração, preservando a mesma intenção e a proteção contra duplicidade. A pendência só desaparece após confirmação externa verificável.

## Retirada de permissão após autorização da baixa — 2026-11-09

O usuário confirmou que uma baixa já autorizada, relativa a material utilizado e pendente de sincronização, deve continuar sendo processada mesmo se o técnico perder a permissão, mudar de função ou tiver a conta desativada antes da execução da fila. A retirada de acesso bloqueia novas operações desse técnico; não desfaz o uso físico da peça nem cancela automaticamente a intenção de consumo já registrada.

Preservar a autoria humana original e o registro da autorização da operação. A continuidade usa a identidade própria da aplicação e mantém as validações de estoque, a proteção contra duplicidade e os limites de carga acordados. Esta decisão não autoriza novas ações pelo técnico sem acesso nem define o tratamento de reservas ainda não consumidas ou a revogação da credencial da aplicação.

Este esclarecimento resolve, para baixas já autorizadas de materiais utilizados, a questão aberta sobre perda de permissão antes da execução da fila.

## Acompanhamento de reservas em chamados parados — 2026-11-09

O usuário definiu que o próprio técnico responsável pelo chamado acompanha as peças reservadas quando o chamado fica parado por muito tempo e decide seu destino. A idade da reserva não provoca liberação automática; permanece a decisão de não expirar reservas de Chamados por tempo. As operações decorrentes seguem os fluxos acordados de remoção, resolução ou cancelamento, conforme o destino das peças.

Ainda é necessário definir quem assume esse acompanhamento quando o técnico responsável deixa a equipe ou perde o acesso. A continuidade das baixas já autorizadas não resolve a responsabilidade pelas reservas ainda não consumidas.

## Continuidade por outros técnicos — 2026-11-09

O usuário definiu que qualquer técnico pode se incluir em um chamado e realizar as alterações necessárias. Assim, se o técnico original sair da equipe ou perder o acesso, outro técnico pode se incluir no chamado e continuar o atendimento, inclusive acompanhar e tratar as peças reservadas conforme os fluxos acordados. Não exigir participação do técnico original para essa continuidade.

Preservar no histórico a autoria de cada alteração e os participantes anteriores. A inclusão de outro técnico não transfere a autoria das operações passadas nem desfaz baixas já autorizadas. Esta decisão trata do acesso aos chamados; não concede automaticamente permissões adicionais no SupriTI.

Ainda é necessário esclarecer se um chamado pode ter vários técnicos participantes simultaneamente ou se a inclusão substitui o responsável atual, e como fica definida a responsabilidade pelo acompanhamento das pendências.

## Participação conjunta de técnicos — 2026-11-09

O usuário confirmou que um chamado pode ter vários técnicos participantes simultaneamente. Ao se incluir, o técnico participa junto com os atuais; sua entrada não remove nem substitui automaticamente outro participante. Um técnico também pode retirar outro técnico do chamado quando necessário.

A retirada da participação no chamado não equivale à desativação da conta ou à revogação de permissões gerais. Preservar o histórico de participação e a autoria das alterações e operações anteriores. Esta confirmação resolve a dúvida sobre inclusão conjunta ou substituição do responsável, mas ainda não define quem acompanha as pendências quando houver vários participantes.

## Responsabilidade compartilhada entre participantes — 2026-11-09

O usuário definiu que todos os técnicos participantes do chamado compartilham a responsabilidade pelo acompanhamento das reservas e das pendências de integração. Não exigir um responsável principal para esse acompanhamento. Esta decisão complementa a participação conjunta: a inclusão não remove os demais e um técnico pode retirar outro quando necessário, preservando o histórico.

Ainda é necessário definir o tratamento de um chamado com reservas ou pendências quando o último técnico participante for retirado, para não presumir uma regra de bloqueio ou de atribuição automática.

## Permanência de pelo menos um técnico — 2026-11-09

O usuário confirmou que deve permanecer pelo menos um técnico participante no chamado. Impedir a retirada do último participante; para substituí-lo, incluir outro técnico antes de retirar o anterior. Isso mantém o acompanhamento compartilhado das reservas e pendências sem exigir responsável principal.

A confirmação foi dada no contexto de retirada de participantes, especialmente quando há reservas ou pendências. Não define atribuição obrigatória na abertura: o planejamento ainda prevê chamados novos aguardando na fila antes de serem assumidos. O tratamento de desativação da conta do único participante também precisa ser definido separadamente, pois bloquear sua retirada do chamado não mantém seu acesso ativo.

## Desativação de técnico com chamados abertos — 2026-11-09

O usuário definiu que, antes de desativar a conta de um técnico, o sistema deve verificar se ele participa de chamados abertos. Se houver, quem realiza a desativação deve informar qual técnico assumirá esses chamados. Oferecer duas formas: atribuir todos ao mesmo técnico ou escolher um técnico para cada chamado específico. A indicação é obrigatória mesmo quando o chamado já possui outros participantes, conforme a regra informada de verificar os chamados abertos do técnico.

Concluir a desativação somente após registrar a atribuição exigida para todos os chamados abertos envolvidos. Preservar os demais participantes e o histórico de participação e autoria do técnico desativado. A atribuição não cria um responsável principal: os técnicos participantes continuam compartilhando o acompanhamento. Não interromper baixas já autorizadas apenas pela desativação do autor.

Ainda é necessário esclarecer o alcance sobre chamados resolvidos ou cancelados que mantenham reservas ou pendências de integração; a expressão chamados abertos não os inclui automaticamente.

## Pendências em chamados resolvidos ou cancelados após desativação — 2026-11-09

O usuário definiu que chamados resolvidos ou cancelados com pendência de integração com o SupriTI não exigem indicação de técnico substituto na desativação. Devem concluir a sincronização preservando a autoria do técnico desativado, por meio da identidade própria da aplicação e com as proteções já acordadas. A conta humana desativada não precisa voltar a ter acesso para a fila executar essas operações.

O usuário acrescentou que o status de ativo deve ser conservado de antes da ativação. O sentido exato dessa frase aguarda esclarecimento; não alterar a regra de acesso nem definir uma cópia histórica do status com base em interpretação silenciosa.

## Autorização histórica e suspensão por suspeita de uso indevido — 2026-11-09

O usuário esclareceu que conservar o status de ativo significa registrar que o técnico estava ativo quando autorizou a operação. A conta permanece atualmente desativada e sem acesso. O registro histórico serve como evidência da autorização original, não como permissão vigente para novas ações. Este esclarecimento resolve a frase ambígua registrada na seção anterior.

O usuário aprovou que a desativação ofereça uma opção de suspender operações pendentes por suspeita de uso indevido. Essa opção constitui exceção à continuidade normal da sincronização já autorizada. A suspensão não deve apagar as intenções nem apresentar operações como concluídas, desfazer movimentações confirmadas ou reativar a conta.

Ainda precisam ser definidos o alcance da suspensão, quem pode analisar e liberar ou cancelar as operações suspensas e o tratamento de operações em andamento cujo resultado externo seja desconhecido. Não presumir que suspender novos envios cancela uma movimentação já recebida pelo SupriTI.

## Análise administrativa de operações suspensas — 2026-11-09

O usuário definiu que os administradores do sistema podem analisar operações suspensas por suspeita de uso indevido e decidir pela liberação ou pelo cancelamento. Essa competência não exige reativar a conta do técnico autor. Preservar a autoria e a autorização originais e registrar a decisão administrativa, seu autor e sua data.

Liberar permite retomar a operação com as validações, os limites de carga e a proteção contra duplicidade já acordados. Cancelar uma intenção pendente não desfaz automaticamente uma movimentação externa confirmada. Operações em andamento ou com resultado desconhecido exigem reconciliação antes de afirmar que não produziram efeito. O alcance da suspensão ao selecionar a opção na desativação ainda precisa ser esclarecido.

## Seleção das operações a suspender — 2026-11-09

O usuário definiu que, ao optar pela suspensão por suspeita de uso indevido durante a desativação, o administrador deve escolher quais operações pendentes autorizadas pelo técnico serão suspensas. Não suspender automaticamente todas as operações desse técnico. As operações não selecionadas continuam o processamento normal, preservando a autoria e a autorização históricas.

Registrar a seleção e a autoria da decisão administrativa. A suspensão preserva as intenções para análise posterior pelos administradores e não desfaz efeitos externos já confirmados. Se uma operação selecionada estiver em andamento ou tiver resultado desconhecido, reconciliar o resultado antes de afirmar cancelamento ou ausência de efeito no SupriTI.

Esta decisão resolve o alcance da suspensão: por operação escolhida pelo administrador.

## Sucesso parcial na baixa de materiais — 2026-11-09

O usuário confirmou que, quando o SupriTI efetivar a baixa de alguns materiais e recusar outros, devem ser preservadas as baixas confirmadas e exibidas pendências apenas nos itens restantes. Não desfazer os consumos confirmados nem repeti-los nas tentativas seguintes. O chamado mantém seu resultado técnico, conforme as regras de resolução ou cancelamento já acordadas.

Acompanhar o resultado de cada material e suas operações. Para itens recusados, mostrar a divergência e permitir a regularização e a retomada acordadas; para falhas de conexão, aplicar o tratamento de indisponibilidade. Resultado desconhecido exige reconciliação antes de nova execução que possa duplicar a baixa. A situação geral da integração só pode indicar conclusão quando não houver itens pendentes, suspensos ou com divergência ainda não resolvida.

Esta decisão resolve a questão funcional sobre sucesso de apenas parte dos materiais; o contrato técnico de resposta por operação ainda será especificado.

## Resolução com liberação de reserva pendente — 2026-11-09

O usuário confirmou que a indisponibilidade do SupriTI ao liberar uma reserva de material removido não impede a resolução técnica do chamado. Persistir a intenção de liberação, apresentar a pendência e tentar sincronizar posteriormente, com as proteções de carga, recuperação e repetição segura já acordadas.

A lista final revisada define os materiais a consumir. A quantidade removida permanece destinada à liberação, sem ser consumida apenas porque sua reserva ainda consta no SupriTI. Não apresentar essa quantidade como liberada ou disponível antes da confirmação externa. Preservar e acompanhar separadamente os resultados de consumo e de liberação, reconciliando operações com resultado desconhecido para evitar duplicidade ou reserva remanescente indevida.

Esta decisão resolve a questão funcional sobre resolver um chamado enquanto uma liberação solicitada ainda não foi confirmada.

## Bloqueio de desativação de produto com reserva — 2026-11-09

O usuário definiu que o SupriTI não deve permitir desativar um item enquanto houver reserva ativa vinculada a ele, até a conclusão do tratamento dessa reserva. A tentativa de desativação deve informar o motivo do bloqueio e indicar onde o item foi reservado, apresentando o ID do chamado e a data da reserva. Se houver reservas em vários chamados, apresentar as referências correspondentes.

A conclusão técnica do chamado não basta para remover o bloqueio quando a reserva ainda aguarda consumo ou liberação no SupriTI. O bloqueio depende do encerramento efetivo da reserva, inclusive nos fluxos com sincronização pendente. Validar a ausência de reservas no momento da desativação, de forma consistente com reservas concorrentes; uma verificação apenas na interface não é suficiente.

Esta decisão impede a desativação de produto com reserva já registrada. Ainda não define o tratamento de uma intenção de reserva que esteja apenas na fila de Chamados e ainda não tenha sido registrada no SupriTI quando o produto for desativado.

## Item desativado antes da sincronização — 2026-11-09

O usuário definiu que, se a reserva ainda não tiver sido registrada no SupriTI e o item for desativado antes da sincronização, a tentativa de baixa deve retornar erro e informar que, ao tentar sincronizar, o SupriTI respondeu que o item não está à disposição.

Manter o resultado técnico do chamado e apresentar a pendência de integração correspondente ao material, sem declarar consumo confirmado. Tratar a recusa como indisponibilidade do item, distinta de falha de conexão, seguindo o fluxo já acordado de regularização e nova tentativa. Não reativar o produto automaticamente nem repetir continuamente a recusa. Preservar as baixas já confirmadas dos demais materiais.

## Condição do material: novo ou recondicionado — 2026-11-09

O usuário confirmou que o técnico deve informar se o material utilizado é novo ou recondicionado. A reserva e a baixa no SupriTI devem corresponder à condição selecionada, respeitando o saldo disponível dessa condição. Não substituir automaticamente material novo por recondicionado ou vice-versa.

Preservar a condição junto ao material e à quantidade na intenção de operação, na revisão final, nos resultados da integração e no histórico. No cancelamento com devolução parcial, manter a identificação da condição de origem das quantidades tratadas. Esta decisão não define reclassificação de condição em uma devolução nem autoriza conversão automática entre condições.

## Condição de retorno de peça nova no cancelamento — 2026-11-09

O usuário esclareceu que a peça nova devolvida no cancelamento retorna como nova, pois não chegou a ser usada efetivamente. Não reclassificá-la automaticamente como recondicionada apenas por ter sido separada para o atendimento.

Se houver apenas reserva, liberar a quantidade na condição nova sem gerar entrada física fictícia. Se a baixa já estiver confirmada, a devolução correspondente retorna à condição nova, conforme o fluxo de cancelamento acordado e sem duplicidade. Este esclarecimento descreve peças sem uso efetivo; não estabelece a condição de retorno de peças efetivamente utilizadas.

## Peças retiradas dos equipamentos: fora do fluxo de estoque de Chamados — 2026-11-09

O usuário definiu que o sistema de Chamados não deve controlar a avaliação, o destino ou a entrada em estoque das peças retiradas de computadores durante a manutenção. O próprio técnico avalia se a peça é reutilizável e realiza uma eventual entrada no SupriTI de forma independente do processo do chamado.

Não exigir essa avaliação ou entrada para resolver ou cancelar o chamado; não gerar automaticamente entrada, devolução ou reclassificação de estoque a partir da retirada de um componente do equipamento. Esta delimitação não elimina o registro técnico da intervenção já previsto no planejamento. Também não altera o tratamento acordado para materiais retirados do estoque para o atendimento e devolvidos no cancelamento, que continuam no fluxo de integração.

## Alterações simultâneas no mesmo material — 2026-11-09

O usuário aprovou que, se dois técnicos alterarem simultaneamente o mesmo material no chamado, o sistema impeça um salvamento baseado em dados anteriores a uma alteração já salva por outro técnico. Atualizar os dados apresentados e avisar que outro técnico alterou o material, pedindo conferência antes de salvar novamente.

Manter visível a alteração pretendida pelo segundo técnico para que ele possa compará-la com o estado atual, sem aplicá-la automaticamente nem sobrescrever silenciosamente a alteração anterior. Somente a alteração aceita deve produzir uma nova intenção de ajuste de reserva ou consumo. A validação deve ocorrer também no backend; a atualização da tela, sozinha, não impede concorrência.

Esta decisão trata de alterações concorrentes no mesmo material. O mecanismo técnico de controle de versão e a ordenação das operações já enviadas ao SupriTI continuam para o desenho técnico.

## Alteração de materiais durante a revisão final — 2026-11-09

O usuário confirmou que, se outro técnico alterar os materiais enquanto a revisão para resolução estiver aberta, o sistema deve impedir a confirmação baseada no resumo anterior. Apresentar o resumo atualizado e exigir nova conferência antes de confirmar a resolução e as intenções de consumo correspondentes.

A tentativa baseada no resumo desatualizado não deve iniciar resolução nem consumo. Preservar os dados já salvos e informar que os materiais foram alterados desde a abertura da revisão. Validar a atualidade do resumo no backend no momento da confirmação, para que uma alteração concorrente não seja aceita entre a conferência e a gravação da resolução. O mecanismo técnico será definido no desenho.

## Materiais confirmados e novas intervenções após resolução — 2026-11-09

O usuário confirmou que, após a resolução do chamado, o técnico já atestou o uso dos itens listados. Os materiais, suas condições e quantidades confirmados naquela intervenção ficam bloqueados para edição retroativa. Qualquer alteração posterior deve gerar uma nova intervenção, preservando integralmente o registro anterior.

Novas intervenções não substituem nem repetem os consumos já confirmados. Operações de integração ainda pendentes continuam vinculadas à intervenção original; a sincronização e a atualização de seus resultados não constituem edição do uso atestado. Esta decisão não autoriza corrigir silenciosamente uma intenção pendente nem apagar o histórico de operações suspensas ou canceladas administrativamente.

A exigência de nova intervenção está confirmada. Ainda é necessário esclarecer se registrar essa intervenção requer reabrir o chamado conforme a janela de sete dias ou se existe um fluxo posterior distinto.

## Nova intervenção e prazo de reabertura — 2026-11-09

O usuário confirmou que, para registrar nova intervenção após a resolução, é necessário reabrir o chamado dentro do prazo de sete dias. Após esse prazo, abrir outro chamado vinculado ao anterior. Preservar as intervenções e os materiais confirmados anteriormente, sem repetir suas baixas.

Esta decisão resolve o fluxo de acesso à nova intervenção após a resolução. A forma de contar os sete dias, inclusive dias corridos ou úteis e instante limite, ainda precisa ser definida.

## Contagem do prazo de reabertura — 2026-11-09

O usuário definiu que o prazo de reabertura é de sete dias corridos, contados a partir da resolução, incluindo fins de semana e feriados. Após o prazo, registrar novo chamado vinculado ao anterior, conforme decisão já confirmada.

Ainda é necessário definir se uma nova resolução após reabertura inicia outra janela de sete dias. O instante limite e o fuso de referência serão explicitados no desenho do fluxo.

## Reinício da janela após nova resolução — 2026-11-09

O usuário confirmou que, quando um chamado reaberto for resolvido novamente, começa uma nova janela de sete dias corridos para reabertura, contada a partir dessa nova resolução. Preservar no histórico as resoluções, reaberturas e intervenções anteriores. A nova janela não altera nem repete as operações de estoque das intervenções anteriores.

## Permissão e justificativa de reabertura por técnicos — 2026-11-09

O usuário definiu que qualquer técnico pode reabrir qualquer chamado, exigindo justificativa para a reabertura. Não restringir a ação aos técnicos que já participavam do chamado. Preservar a justificativa, a autoria e a data no histórico.

Mantém-se o prazo confirmado de sete dias corridos a partir da resolução mais recente; após esse prazo, abrir outro chamado vinculado ao anterior. A expressão qualquer chamado não foi interpretada como exceção ao prazo nem como autorização para reativar chamados cancelados. A possibilidade de reabertura pelo solicitante, prevista no planejamento original, ainda precisa ser confirmada neste detalhamento.

## Reabertura pelo solicitante com motivos predefinidos — 2026-11-09

O usuário confirmou que o solicitante pode reabrir o próprio chamado dentro da janela de sete dias corridos a partir da resolução mais recente. Oferecer motivos predefinidos para seleção, incluindo “O mesmo problema persiste”. Registrar o motivo selecionado, a autoria e a data da reabertura no histórico.

A lista completa dos motivos e a exigência ou não de descrição complementar ainda precisam ser definidas. Não presumir que a seleção exige texto livre adicional. Mantém-se a justificativa obrigatória para reabertura por técnicos, conforme decisão anterior.

## Motivo suficiente para reabertura pelo solicitante — 2026-11-09

O usuário confirmou que, para o solicitante reabrir o próprio chamado, basta selecionar um motivo padrão ou informar uma justificativa personalizada. São alternativas: não exigir ambas nem descrição complementar quando houver motivo padrão selecionado. Sem nenhuma das alternativas preenchida, não permitir a reabertura.

Manter a janela de sete dias corridos após a resolução mais recente e registrar no histórico o motivo escolhido ou o texto personalizado, com autoria e data. Este esclarecimento resolve a exigência de descrição complementar para o solicitante; a lista completa de motivos padrão permanece para definição.

## Motivos predefinidos também para técnicos — 2026-11-09

O usuário confirmou que os técnicos também devem dispor de motivos padrão selecionáveis para facilitar a reabertura. A justificativa obrigatória pode ser atendida pela seleção de um motivo padrão ou pelo preenchimento de justificativa personalizada, sem exigir ambos. Sem motivo selecionado ou texto personalizado, não permitir a reabertura.

Preservar no histórico o motivo efetivamente informado, a autoria e a data. Mantêm-se as regras de que qualquer técnico pode reabrir qualquer chamado resolvido dentro da janela de sete dias corridos após a resolução mais recente. A lista completa de motivos padrão ainda será definida.

## Participantes e notificações na reabertura e no chamado vinculado — 2026-11-09

O usuário definiu que a reabertura dentro da janela de sete dias corridos mantém os técnicos participantes anteriores vinculados ao chamado e os notifica da reabertura.

Quando houver criação de um novo chamado vinculado ao anterior, não herdar automaticamente os técnicos participantes nem notificá-los apenas por esse vínculo. O vínculo preserva a referência ao histórico anterior; a participação no novo chamado segue seu próprio fluxo de atendimento.

## Inclusão do técnico que reabre o chamado — 2026-11-09

O usuário confirmou que, quando um técnico que ainda não participa do chamado realizar sua reabertura, ele deve ser incluído automaticamente como participante. Manter os demais participantes, sem substituição automática, e registrar a inclusão e a reabertura no histórico. Aplicam-se o prazo e a exigência de motivo já acordados.

## Novo chamado vinculado após o prazo de reabertura — 2026-11-09

O usuário confirmou que, ao tentar reabrir um chamado após os sete dias corridos, o sistema deve oferecer a criação de um novo chamado vinculado ao anterior. Copiar os dados do equipamento e permitir descrever o problema atual antes de confirmar a criação.

Não criar o novo chamado automaticamente pela tentativa de reabertura. Manter o chamado anterior e seu histórico preservados. Conforme decisão anterior, o novo chamado não herda automaticamente os técnicos participantes nem os notifica apenas pelo vínculo. Não copiar materiais como novos consumos ou reservas a partir da intervenção anterior.

## Estado do chamado após reabertura — 2026-11-09

O usuário confirmou que a reabertura dentro do prazo retorna o chamado diretamente ao estado “Em atendimento”, mantendo os técnicos participantes e notificando-os conforme já acordado. Se a reabertura for realizada por técnico ainda não participante, incluí-lo automaticamente sem retirar os demais.

Preservar as resoluções e intervenções anteriores e os resultados ou pendências de integração associados a elas. A mudança para “Em atendimento” não desfaz nem repete operações de estoque anteriores.

## Revisão final de chamado sem materiais — 2026-11-09

O usuário confirmou que, quando o chamado não tiver materiais, a revisão final deve apresentar a confirmação da solução, sem perguntar sobre uso de peças nem sugerir consumo inexistente. Preservar os demais dados e as validações de resolução já acordados, incluindo diagnóstico opcional e equipamento quando aplicável. Confirmar a resolução não gera intenção de consumo se não houver materiais.

A ausência de materiais na lista final não elimina eventuais liberações de reserva já pendentes, que continuam sendo acompanhadas e sincronizadas conforme decisão anterior.

## Permissão de cancelamento pelo solicitante e pelos técnicos — 2026-11-09

O usuário definiu que o solicitante pode cancelar o próprio chamado somente antes de um técnico ser vinculado a ele. Após a vinculação de um técnico, somente um técnico pode cancelar o chamado. Validar essa condição no momento do cancelamento, inclusive se a inclusão de um técnico ocorrer enquanto a tela do solicitante estiver aberta.

Continuam aplicáveis ao cancelamento as regras de destino dos materiais, justificativa para itens mantidos e acompanhamento da integração já acordadas. Ainda é necessário esclarecer se o técnico que cancela precisa participar do chamado ou se qualquer técnico pode realizar essa ação.

## Cancelamento por qualquer técnico com motivo obrigatório — 2026-11-09

O usuário definiu que qualquer técnico pode cancelar o chamado, mesmo sem participar dele, desde que descreva por que o cancelou. Exigir descrição do motivo e preservá-la no histórico com a autoria e a data. Não presumir inclusão automática do técnico como participante apenas por cancelar.

O motivo do cancelamento é exigido mesmo quando não houver materiais. Quando houver materiais mantidos, permanece também a exigência já acordada de uma justificativa conjunta para sua manutenção ou uso. Não presumir que os dois registros exigem textos separados; a forma de apresentação ainda será definida.

Mantém-se a regra de que o solicitante só pode cancelar antes da vinculação de um técnico. Os efeitos de estoque e as pendências do cancelamento seguem as decisões anteriores.

## Cancelamento pelo solicitante sem motivo obrigatório — 2026-11-09

O usuário definiu que o solicitante não precisa informar motivo para cancelar o próprio chamado antes da vinculação de um técnico. Registrar o cancelamento com autoria e data, sem exigir justificativa. Após a vinculação de um técnico, o solicitante não pode cancelar; qualquer técnico pode fazê-lo mediante descrição obrigatória do motivo, conforme decisão anterior.

## Reabertura e histórico de chamados cancelados — 2026-11-09

O usuário definiu que somente chamados cancelados por técnicos podem ser reabertos. Chamados cancelados pelo solicitante antes do atendimento não devem ser mantidos como histórico, com o objetivo de evitar acúmulo desnecessário no sistema. Esta decisão substitui, para esse cancelamento pelo solicitante, a orientação anterior de preservar o cancelamento no histórico do chamado.

Preservar o histórico dos chamados cancelados por técnicos, incluindo motivo, autoria, materiais e resultados ou pendências de integração já acordados. A regra de cancelamento pelo solicitante continua limitada ao período anterior à vinculação de um técnico.

O mecanismo de remoção dos chamados cancelados antes do atendimento e de seus dados associados será definido no desenho técnico, sem presumir que apenas ocultar registros reduz armazenamento. Ainda é necessário definir o prazo de reabertura dos chamados cancelados por técnicos e o marco inicial desse prazo.

## Prazo de reabertura após cancelamento por técnico — 2026-11-09

O usuário confirmou que chamados cancelados por técnicos podem ser reabertos em até sete dias corridos a partir do cancelamento. Essa regra é distinta da janela contada da resolução e não se aplica aos chamados cancelados pelo solicitante antes do atendimento, que não permanecem no histórico conforme decisão anterior.

A reabertura preserva o histórico e não desfaz nem repete automaticamente operações de estoque do cancelamento. Ainda é necessário esclarecer quem pode reabrir um chamado cancelado por técnico: apenas técnicos ou também o solicitante.

## Reabertura pelo solicitante após cancelamento por técnico — 2026-11-09

O usuário confirmou que o solicitante também pode reabrir o próprio chamado quando ele tiver sido cancelado por um técnico, dentro dos sete dias corridos a partir do cancelamento. Técnicos continuam podendo reabrir conforme as permissões já acordadas.

Aplicar a exigência de motivo padrão selecionado ou justificativa personalizada já definida para a reabertura, preservando autoria, data e motivo no histórico. Essa possibilidade não se aplica aos chamados cancelados pelo solicitante antes do atendimento, que não permanecem no histórico.

## Estado e motivo na reabertura de chamado cancelado — 2026-11-09

O usuário confirmou que, quando o solicitante reabrir um chamado cancelado por técnico dentro dos sete dias corridos, o chamado deve voltar para “Em atendimento”, mantendo e notificando os técnicos participantes. O solicitante deve informar por que está reabrindo o chamado cancelado; não permitir reabertura sem motivo.

Permanece a regra anteriormente acordada de registrar motivo padrão selecionado ou justificativa personalizada. A confirmação atual reforça a obrigatoriedade de explicar a reabertura, sem definir expressamente que apenas texto personalizado seria aceito para chamados cancelados.

## Motivo padrão suficiente na reabertura de chamado cancelado — 2026-11-09

O usuário confirmou que selecionar um motivo padrão é suficiente para o solicitante explicar a reabertura de um chamado cancelado por técnico. Não exigir texto personalizado adicional. Continua disponível a alternativa de justificativa personalizada, conforme regra geral já acordada. Sem motivo padrão selecionado nem justificativa personalizada, impedir a reabertura.

Esta confirmação resolve a dúvida sobre exigir texto livre especificamente para reabrir chamados cancelados. Mantêm-se o prazo de sete dias corridos a partir do cancelamento, o retorno para “Em atendimento”, a preservação dos participantes e sua notificação.

## Novo chamado vinculado após prazo do cancelamento — 2026-11-09

O usuário confirmou que, após sete dias corridos do cancelamento por técnico, uma tentativa de reabertura deve oferecer a criação de um novo chamado vinculado ao anterior. Aplicar o fluxo já acordado: trazer os dados do equipamento, permitir descrever o problema atual e exigir confirmação da criação, sem herdar automaticamente técnicos, reservas ou consumos anteriores nem notificar os antigos participantes apenas pelo vínculo.

Preservar o chamado cancelado e seu histórico. Esta opção não se aplica aos chamados cancelados pelo solicitante antes do atendimento, que não permanecem no histórico.

## Notificação de cancelamento por técnico não participante — 2026-11-09

O usuário confirmou que, quando um técnico que não participa do chamado o cancelar, os técnicos participantes devem ser notificados do cancelamento e do motivo informado. Preservar no histórico a autoria, a data e a descrição obrigatória do motivo, conforme as regras já acordadas. O cancelamento não inclui automaticamente seu autor como participante.

## Notificação do solicitante sobre cancelamento por técnico — 2026-11-09

O usuário confirmou que o solicitante deve receber uma notificação com o motivo sempre que um técnico cancelar seu chamado, independentemente de o técnico participar ou não dele. Manter a descrição obrigatória do motivo e seu registro no histórico com autoria e data. Os canais de notificação ainda serão definidos.

## Canal de notificações — 2026-11-09

O usuário definiu que as notificações devem ocorrer apenas dentro do próprio sistema. Aplicar esse canal aos avisos de chamados, reaberturas e cancelamentos já acordados. Não prever envio por e-mail, SMS, WhatsApp ou outros canais externos no escopo atual.

A apresentação, a indicação de leitura e a retenção das notificações internas ainda serão detalhadas. A decisão sobre o canal não altera quais eventos e destinatários devem ser notificados.

## Ícone e persistência das notificações internas — 2026-11-09

O usuário confirmou que as notificações recebidas enquanto estiver desconectado devem ser guardadas como não lidas e disponibilizadas no próximo acesso. A interface deve ter um ícone de notificações, tomando como referência o padrão de aplicativos como Mercado Livre, com indicação de avisos não lidos e acesso à lista de notificações.

A referência é ao padrão de interação, sem exigir reprodução exata da interface de outro aplicativo. Manter as notificações apenas no próprio sistema, conforme decisão anterior. O critério de marcação como lida e o prazo de retenção ainda serão definidos.

## Abertura e leitura das notificações — 2026-11-09

O usuário confirmou que clicar em uma notificação deve marcá-la como lida e abrir o chamado correspondente. Atualizar o indicador de não lidas. A abertura do chamado deve respeitar as permissões vigentes de acesso; a notificação não concede permissões adicionais.

## Retenção das notificações internas — 2026-11-09

O usuário definiu retenção de 30 dias para as notificações internas. Aplicar o prazo aos avisos lidos e não lidos, contados da criação de cada notificação. Após o prazo, remover a notificação; a remoção do aviso não apaga o histórico do chamado nem resolve ou oculta a pendência de integração registrada no próprio chamado.

## Notificação de novos chamados na fila — 2026-11-09

O usuário definiu que todos os técnicos devem receber uma notificação quando um novo chamado entrar na fila. Não restringir esse aviso a um grupo específico. Utilizar as notificações internas já acordadas, com indicador de não lidas, abertura do chamado ao clicar e retenção de 30 dias. O recebimento do aviso não inclui automaticamente o técnico como participante do chamado.

## Remoção do aviso da fila após um técnico assumir — 2026-11-09

O usuário definiu que, quando um técnico assumir um novo chamado, a notificação de entrada na fila deve desaparecer para os demais técnicos. Atualizar também seus indicadores de notificações não lidas, quando aplicável. Essa remoção atinge o aviso de novo chamado, não o registro do chamado nem outras notificações sobre ele.

O chamado continua acessível aos técnicos conforme as regras já acordadas, inclusive para inclusão de outros participantes. Não interpretar a remoção da notificação como restrição de acesso ou exclusividade de atendimento.

## Notificação ao solicitante quando o chamado for assumido — 2026-11-09

O usuário confirmou que o solicitante deve receber uma notificação interna quando um técnico assumir seu chamado. Aplicar o comportamento já acordado de indicador de não lidas, abertura do chamado e marcação como lida ao clicar, com retenção de 30 dias.

## Mensagem de conclusão e confirmação do solicitante — 2026-11-10

O usuário definiu que o solicitante deve receber uma mensagem de conclusão com opções de confirmar a conclusão ou pedir a reabertura. A confirmação do solicitante tem finalidade de compliance e não condiciona a resolução: o chamado é finalizado pelo técnico sem aguardar esse aceite.

Na ausência de confirmação expressa, a confirmação deve ocorrer automaticamente em sete dias. Distinguir no histórico a confirmação expressa do solicitante da confirmação automática pelo decurso do prazo; não atribuir ao solicitante uma ação que ele não realizou.

Ainda é necessário esclarecer o efeito do pedido de reabertura sobre essa confirmação automática. O marco e a contagem do prazo de confirmação também precisam ser explicitados, sem presumir que sejam idênticos aos da janela de reabertura anteriormente definida.

## Aceite técnico do pedido de reabertura — 2026-11-10

O usuário definiu que o pedido de reabertura do solicitante deve notificar o técnico e depender do aceite dele para reabrir efetivamente o chamado. O envio do pedido não altera por si só o estado resolvido do chamado. Esta decisão substitui, para reabertura solicitada pelo solicitante após resolução, qualquer interpretação anterior de reabertura imediata.

Ainda precisam ser definidos o técnico destinatário quando houver vários participantes, o tratamento da recusa e o efeito de um pedido aguardando avaliação sobre a confirmação automática de conclusão em sete dias. Não estender esta decisão automaticamente à reabertura feita por técnicos ou à de chamados cancelados.

## Pedido de reabertura interrompe a confirmação automática — 2026-11-10

O usuário definiu que, quando o solicitante pedir a reabertura, o contador de sete dias para confirmação automática da conclusão deve ser desconsiderado. O pedido elimina a contagem vigente, em vez de apenas pausá-la; não confirmar automaticamente a conclusão pelo vencimento desse prazo enquanto o pedido aguarda avaliação técnica. O chamado permanece resolvido até o aceite técnico da reabertura, conforme a decisão anterior.

O comportamento da confirmação de compliance em caso de recusa do pedido ainda precisa ser definido. Esta decisão trata do contador de confirmação automática e não altera, por si só, a janela permitida para solicitar reabertura.

## Recusa de reabertura encerra o chamado definitivamente — 2026-11-10

O usuário definiu que, se o técnico recusar o pedido de reabertura, o chamado deve ser fechado em definitivo, sem reiniciar o contador de confirmação de conclusão nem criar novas etapas de contestação ou aprovação no sistema. Técnico e solicitante dispõem de um canal alternativo de comunicação para tratar a questão; esta decisão não inclui a implementação desse canal no sistema.

Preservar no histórico o pedido, a decisão técnica, a autoria e as datas. O fechamento por recusa não deve ser apresentado como confirmação expressa do solicitante. Ainda precisam ser definidos o técnico responsável pela decisão quando houver vários participantes e a comunicação da recusa ao solicitante.

## Aceite da reabertura por técnico não participante — 2026-11-10

O usuário definiu que qualquer técnico pode reabrir o chamado, inclusive quem não participou do atendimento anterior. Ao aceitar o pedido e efetivar a reabertura, o técnico deve ser automaticamente associado como participante, caso ainda não participe. A decisão não restringe o aceite aos participantes anteriores e reafirma a inclusão automática do técnico que reabre, já registrada anteriormente.

Esta resposta esclarece quem pode aceitar e efetivar a reabertura; não estabelece que qualquer técnico também possa recusar o pedido. A abrangência da permissão de recusa e os destinatários da notificação do pedido ainda precisam ser definidos.

## Recusa de reabertura por qualquer técnico — 2026-11-10

O usuário confirmou que qualquer técnico pode recusar o pedido de reabertura e fechar o chamado definitivamente, inclusive quem não participou do atendimento anterior. Registrar a autoria e a data da decisão. A inclusão automática como participante está definida para quem efetiva a reabertura; não estendê-la automaticamente a quem recusa.

Esta decisão resolve a abrangência da permissão de recusa. Os destinatários da notificação do pedido de reabertura ainda precisam ser definidos.

## Visibilidade e notificação do pedido de reabertura — 2026-11-10

O usuário definiu que o pedido de reabertura deve ficar visível para todos os técnicos no painel, mas a notificação do pedido deve ser enviada apenas aos técnicos participantes do chamado. A visibilidade no painel permite que qualquer técnico aceite ou recuse o pedido, conforme as permissões já acordadas; não é necessário notificar toda a equipe.

Esta decisão resolve os destinatários do aviso do pedido. A comunicação ao solicitante sobre o aceite ou a recusa ainda precisa ser definida.

## Notificação do resultado do pedido de reabertura — 2026-11-10

O usuário definiu que o solicitante deve receber a notificação padrão do sistema quando seu pedido de reabertura for aceito ou recusado. Aplicar as regras já acordadas das notificações internas: indicador de não lidas, persistência durante desconexão, abertura do chamado e marcação como lida ao clicar, com retenção de 30 dias. O aviso deve comunicar o resultado efetivo: chamado reaberto no aceite ou fechado definitivamente na recusa.

## Motivo obrigatório na recusa de reabertura — 2026-11-10

O usuário confirmou que o técnico deve informar um motivo obrigatório ao recusar o pedido de reabertura. O motivo deve ficar visível ao solicitante e ser preservado no histórico junto à autoria e à data da decisão. Aplicar a notificação padrão do resultado já acordada; a recusa fecha o chamado definitivamente, sem nova etapa de contestação no sistema.

## Contagem do prazo de confirmação automática — 2026-11-10

O usuário confirmou que os sete dias para confirmação automática são dias corridos, contados a partir do momento em que o técnico resolve o chamado. A resolução é imediata e independe do aceite do solicitante. Se o solicitante pedir reabertura, desconsiderar essa contagem, conforme decisão anterior; não confirmar automaticamente a conclusão enquanto o pedido aguarda avaliação.

## Simplificação do fechamento: sem confirmação do solicitante — 2026-11-10

O usuário revisou a decisão sobre confirmação de conclusão: o técnico fecha o chamado em definitivo, sem depender de confirmação do solicitante. Excluir o botão de confirmação, o aceite de compliance, a confirmação automática e seu contador. As decisões anteriores que descrevem esses mecanismos ficam substituídas por esta decisão. A passagem de sete dias não exige uma nova operação de fechamento.

O solicitante recebe a notificação de conclusão e pode solicitar reabertura durante sete dias corridos, contados do momento do fechamento pelo técnico. O pedido depende do aceite de um técnico para efetivar a reabertura; até esse aceite, o chamado continua fechado. Qualquer técnico pode aceitar ou recusar. Quem reabrir passa automaticamente a participante. A recusa exige motivo visível ao solicitante e preservado no histórico, mantendo o fechamento definitivo.

Preservar a visibilidade do pedido para todos os técnicos no painel, a notificação apenas aos participantes e a notificação padrão ao solicitante sobre aceite ou recusa. O técnico também pode reabrir o chamado conforme as regras já acordadas. O prazo de sete dias limita a apresentação do pedido pelo solicitante; não é um contador de confirmação e não encerra automaticamente um pedido já apresentado que ainda aguarde avaliação.

## Validade do pedido apresentado dentro do prazo — 2026-11-10

O usuário confirmou que um pedido de reabertura apresentado pelo solicitante dentro dos sete dias corridos continua válido mesmo que o técnico só o avalie e aceite depois desse prazo. Usar o momento de apresentação do pedido para verificar sua tempestividade; o fim da janela não impede o aceite posterior nem expira automaticamente o pedido pendente.

## Complementação do pedido de reabertura pendente — 2026-11-10

O usuário confirmou que o solicitante pode complementar o motivo do pedido de reabertura com novas informações enquanto ele aguarda avaliação técnica. A complementação pertence ao mesmo pedido e não altera a data de apresentação usada para verificar o prazo de sete dias. Preservar o motivo original e registrar os complementos com autoria e data no histórico.

## Complementos sem nova notificação — 2026-11-10

O usuário definiu que os complementos do solicitante apenas atualizam o pedido de reabertura existente, sem gerar novas notificações. Preservar o registro dos complementos e a data original de apresentação do pedido, conforme a decisão anterior.

## Decisões simultâneas sobre o pedido de reabertura — 2026-11-10

O usuário confirmou que, se dois técnicos tentarem decidir o mesmo pedido simultaneamente, prevalece a primeira decisão registrada. Informar ao outro técnico que o pedido já foi avaliado, sem sobrescrever a decisão nem repetir seus efeitos. Garantir essa regra no processamento da decisão, inclusive quando os técnicos estiverem com telas desatualizadas; não depender apenas do bloqueio de botões na interface.

## Cancelamento de pedido de reabertura pendente pelo solicitante — 2026-11-10

O usuário confirmou que o solicitante pode cancelar seu pedido de reabertura enquanto ele ainda aguarda avaliação técnica. O cancelamento encerra o pedido, mantendo o chamado fechado, e deve ser preservado no histórico com autoria e data. Ainda precisa ser definido se o solicitante poderá apresentar outro pedido dentro da janela original de sete dias após cancelar o anterior.

## Novo pedido após cancelamento pelo solicitante — 2026-11-10

O usuário confirmou que, após cancelar seu pedido de reabertura pendente, o solicitante pode apresentar outro pedido se ainda estiver dentro dos sete dias corridos contados do fechamento original. O cancelamento e o novo pedido não reiniciam nem ampliam essa janela. Preservar o pedido cancelado no histórico e tratar o novo pedido pelo fluxo de avaliação técnica já definido. Esta permissão não se aplica a pedidos recusados por técnico, cuja recusa mantém o fechamento definitivo conforme decisão anterior.

## Atualização do painel e remoção do aviso após cancelar pedido — 2026-11-10

O usuário definiu que, quando o solicitante cancelar um pedido de reabertura pendente, basta atualizar o painel e remover a notificação de reabertura correspondente, sem gerar outra notificação. Remover o aviso dos destinatários e atualizar seus indicadores de não lidas quando aplicável. Preservar o pedido cancelado e seu histórico; a remoção atinge somente o aviso daquele pedido, não outras notificações sobre o chamado.

## Remoção do aviso pendente após decisão técnica — 2026-11-10

O usuário confirmou que, quando um técnico aceitar ou recusar o pedido de reabertura, a notificação pendente desse pedido deve desaparecer para os demais participantes. Atualizar os indicadores de não lidas quando aplicável e refletir o resultado no painel. Preservar o histórico do pedido e a notificação padrão do resultado enviada ao solicitante; não remover outras notificações do chamado.

## Cancelamento e decisão técnica simultâneos — 2026-11-10

O usuário confirmou que a primeira ação registrada prevalece quando o cancelamento do pedido pelo solicitante concorrer com o aceite ou a recusa por um técnico. Efetivar somente a primeira ação válida e informar à outra pessoa o estado atualizado do pedido, sem sobrescrever o resultado nem repetir seus efeitos. Aplicar a regra no processamento, inclusive com telas desatualizadas, conforme a proteção já definida para decisões técnicas simultâneas.

## Informações principais no painel dos técnicos — 2026-11-10

O usuário definiu as informações principais que devem aparecer para cada chamado no painel dos técnicos: título do chamado, categoria, data de abertura, prioridade e solicitante, incluindo setor e usuário. A ordenação inicial dos chamados e as regras de definição da prioridade ainda precisam ser esclarecidas.

## Ordenação inicial da fila dos técnicos — 2026-11-10

O usuário confirmou que a fila deve respeitar primeiro a hierarquia de prioridade, exibindo os níveis mais prioritários acima. Dentro de cada nível, ordenar pela data de abertura, com os chamados mais antigos primeiro. Os níveis de prioridade e as regras para atribuí-los ainda precisam ser definidos.

## Definição técnica e visibilidade da prioridade — 2026-11-10

O usuário definiu que a prioridade deve ser analisada e atribuída pelo técnico. O solicitante não deve visualizar a prioridade atribuída nem sua posição na ordenação por prioridade. A prioridade é uma informação interna da equipe técnica e orienta a fila conforme a ordenação já acordada. Os níveis de prioridade e o tratamento inicial de chamados ainda não avaliados continuam pendentes de definição.

## Chamados novos aguardando triagem — 2026-11-10

O usuário definiu que os chamados novos devem aparecer como “Aguardando triagem” até a avaliação técnica, sem receber uma prioridade padrão. Após a atribuição de prioridade pelo técnico, aplicar a ordenação por hierarquia de prioridade e, dentro de cada nível, pela data de abertura mais antiga. A posição ou seção dos chamados aguardando triagem no painel ainda precisa ser definida.

## Divisão do painel entre chamados novos e em andamento — 2026-11-10

O usuário definiu que a tela principal dos técnicos deve ter áreas distintas para chamados novos e chamados em andamento. Reservar o topo do painel para os chamados novos, que aguardam triagem. Exibir nesses chamados um pequeno círculo verde piscando no canto esquerdo, indicando a chegada de um novo chamado. Conforme correção do usuário, o círculo é apenas um indicador visual, não um botão, e não executa ação ao clicar. O momento em que o indicador deixa de piscar ainda precisa ser esclarecido.

Manter as informações principais de cada chamado já acordadas. Não atribuir prioridade padrão aos chamados novos; a ordenação por prioridade aplica-se após a avaliação técnica.

## Duração do indicador de novo chamado — 2026-11-10

O usuário definiu que o círculo verde deve continuar piscando até um técnico assumir o chamado. Apenas abrir os detalhes para consulta ou triagem não interrompe o indicador. Ao assumir, o círculo deixa de piscar; a remoção da notificação de entrada na fila para os demais técnicos segue a regra já acordada.

## Prioridade média ao assumir sem definição explícita — 2026-11-10

O usuário definiu que, quando um técnico assumir o chamado sem informar a prioridade, o sistema deve atribuir prioridade média por padrão. A definição explícita de prioridade não é obrigatória para assumir. Antes de ser assumido ou receber avaliação de prioridade, o chamado novo permanece aguardando triagem, sem prioridade padrão, conforme a regra anterior. A prioridade continua sendo informação interna da equipe técnica.

## Níveis de prioridade — 2026-11-10

O usuário definiu quatro níveis de prioridade: baixa, média, alta e crítica. Na fila priorizada, aplicar a hierarquia crítica, alta, média e baixa, com os chamados mais antigos primeiro dentro de cada nível. Manter média como padrão ao assumir sem definição explícita e a prioridade oculta ao solicitante.

## Alteração de prioridade por qualquer técnico — 2026-11-10

O usuário definiu que qualquer técnico pode alterar a prioridade de um chamado em andamento, inclusive quem não participa dele. Não restringir essa ação aos participantes. A alteração deve refletir a nova prioridade na ordenação da fila, mantendo essa informação oculta ao solicitante. Esta decisão não inclui automaticamente o autor da alteração como participante.

## Alteração de prioridade sem justificativa — 2026-11-10

O usuário definiu que a alteração de prioridade não exige justificativa. Registrar no histórico interno quem alterou, quando e os níveis anterior e novo, preservando a restrição de visibilidade da prioridade ao solicitante. Não acrescentar campo obrigatório de motivo ou etapa de aprovação para essa alteração.

## Visualização da equipe e filtros no painel — 2026-11-10

O usuário definiu que, na área de chamados em andamento, cada técnico deve visualizar inicialmente todos os chamados da equipe, com opção de usar filtros programáveis. Não limitar a visualização inicial aos chamados de que participa. O significado e o funcionamento dos filtros programáveis ainda precisam ser esclarecidos, incluindo os critérios configuráveis e eventual salvamento de combinações.

## Combinação e salvamento de filtros por técnico — 2026-11-10

O usuário confirmou que cada técnico pode combinar critérios de filtro, como prioridade, categoria e participante, e salvar essas combinações para reutilizar no painel. Os critérios mencionados não constituem uma lista exaustiva. O comportamento de aplicação dos filtros salvos ao acessar novamente o painel ainda precisa ser definido.

## Persistência do último filtro utilizado — 2026-11-10

O usuário definiu que, ao voltar ao painel, cada técnico deve encontrar o último filtro que utilizou ainda aplicado. A preferência pertence ao técnico. A visualização inicial de todos os chamados aplica-se quando ainda não houver filtro anterior; nas visitas seguintes, prevalece o último filtro utilizado.

## Triagem independente dos filtros do painel — 2026-11-10

O usuário definiu que os chamados aguardando triagem não devem ser afetados pelos filtros do painel. A área reservada no topo continua mostrando todos os chamados aguardando triagem, independentemente da combinação de filtros aplicada ou restaurada para o técnico. Aplicar os filtros à área de chamados em andamento, preservando a separação entre as duas áreas.

## Transferência imediata da triagem ao assumir — 2026-11-10

O usuário confirmou que, quando um técnico assumir um chamado, ele deve sair imediatamente da área de triagem e passar à área de chamados em andamento. Aplicar a prioridade escolhida pelo técnico ou média por padrão, a ordenação e os filtros vigentes dessa área. Atualizar o indicador de novo chamado e as notificações conforme as regras já acordadas.

## Acesso à página do chamado pelo painel — 2026-11-10

O usuário esclareceu que, quando um chamado chega, o técnico clica nele no painel e é direcionado à página do chamado. A partir dessa página, realiza as edições necessárias e assume o atendimento. Não é necessário acrescentar uma abertura automática de detalhes depois de assumir, pois o técnico já está na página do chamado. Manter a distinção já acordada: apenas abrir a página não equivale a assumir o chamado nem interrompe o círculo verde piscante.

## Resumo apresentado na página do chamado — 2026-11-10

O usuário definiu que a página do chamado deve apresentar o resumo conforme preenchido pelo solicitante. A definição dos campos do formulário de abertura deve orientar esse resumo, sem presumir informações adicionais fornecidas pelo solicitante.

## Formulário de abertura configurável pelo administrador — 2026-11-10

O usuário definiu que os campos preenchidos pelo solicitante ao abrir um chamado devem ser configuráveis pelo administrador. O resumo apresentado ao técnico deve refletir os dados preenchidos nesse formulário. O alcance da configuração, incluindo obrigatoriedade e eventual variação por categoria, ainda precisa ser esclarecido.

## Formulários configuráveis por categoria — 2026-11-10

O usuário definiu que o formulário de abertura deve ser totalmente configurável pelo administrador conforme a categoria escolhida. Cada categoria pode ter sua própria configuração de campos; não limitar o sistema a um único formulário global. O resumo do chamado deve refletir os dados efetivamente preenchidos no formulário da categoria. Os recursos específicos do configurador ainda precisam ser detalhados, sem interpretar “totalmente programável” como autorização implícita para executar código arbitrário.

## Configurador visual de formulários por categoria — 2026-11-10

O usuário confirmou que a configuração dos formulários deve ocorrer por uma interface visual para o administrador. Permitir adicionar campos, escolher o tipo de cada campo, definir sua ordem e marcar quais são obrigatórios, com configuração própria por categoria. Os tipos de campo disponíveis ainda precisam ser definidos.

## Preservação do formulário dos chamados já abertos — 2026-11-10

O usuário confirmou que alterações administrativas no formulário não devem modificar os campos nem as respostas dos chamados já abertos. Cada chamado deve preservar a versão do formulário utilizada na abertura, permitindo interpretar suas respostas mesmo após alteração ou remoção de campos na configuração da categoria. Novas configurações aplicam-se às novas aberturas, sem migração automática dos chamados existentes.

## Tipos de campo do formulário de chamados — 2026-11-10

O usuário solicitou os tipos de campo de um formulário de chamados padrão. Como tradução inicial desse pedido para o configurador, considerar: texto curto; texto longo; seleção única em lista ou botões de opção; seleção múltipla; caixa de confirmação e sim/não; número inteiro ou decimal; data, hora e data com hora; e-mail; telefone; endereço de site; e anexos, incluindo imagens e documentos. Esta enumeração é uma proposta de detalhamento do pedido amplo, não uma lista de tipos individualmente confirmados pelo usuário.

Seletores vinculados aos cadastros do sistema, como equipamento, setor e usuário, devem ser diferenciados de listas de opções livres para preservar a referência ao cadastro. O alcance desses seletores e a exibição condicional de campos ainda precisam ser definidos.

## Exibição condicional de campos no formulário — 2026-11-10

O usuário confirmou que o administrador deve poder configurar campos condicionais na interface visual: a exibição de um campo pode depender da resposta dada em outro campo. Por exemplo, mostrar o campo de patrimônio do equipamento quando uma resposta indicar problema de hardware. O exemplo ilustra o recurso e não estabelece um campo ou uma regra obrigatória para todas as categorias. Preservar essas condições na versão do formulário vinculada ao chamado.

## Formulário de abertura sem rascunho — 2026-11-10

O usuário retificou a decisão anterior: não haverá opção de rascunho no formulário de abertura. Não implementar salvamento manual ou automático de rascunhos para retomada posterior. O chamado é registrado quando o solicitante envia o formulário, entrando na triagem e gerando as notificações conforme as regras já acordadas.
