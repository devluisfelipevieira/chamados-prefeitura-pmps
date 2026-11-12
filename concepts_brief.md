# Sistema de Chamados da Prefeitura — conceitos consolidados

- **Autor:** Luis Felipe Vieira
- **Data:** 2026-11-10
- **Situação:** brief funcional para revisão do autor e preparação do desenho técnico. Não houve revisão Goldfish deste documento. Não constitui autorização para implementação.
- **Idioma:** português, conforme a continuidade do planejamento solicitada pelo autor.

## Visão geral

### Retomada em outro computador — 2026-11-12

O autor pediu guardar a questão de classificação inicial por formulário e sincronizar o repositório. Continuar o plano técnico com `egm-draft` e o ponto de retomada no início de `design_draft.md`. A questão pendente é escolher entre categoria fixa por formulário, categoria sugerida conforme respostas ou permitir ambos os modos. Não houve escolha nem aprovação do bloco atual de formulários. As aprovações parciais anteriores estão registradas no rascunho; não há autorização de implementação.

### Revisão de escopo: abertura simples e comunicação pelo canal existente

O autor confirmou a base de conhecimento dentro de Atendimento, incluindo artigos e vínculos às soluções de chamados, sem serviço adicional. Autoria, revisão, publicação, visibilidade e versionamento continuam por definir; escolher o serviço responsável não aprova implicitamente esses fluxos.

O autor confirmou que Atendimento centraliza as notificações internas dos técnicos, reunindo os avisos dos três serviços no mesmo painel. Materiais e Inventário encaminham avisos por sua API, respeitando a propriedade exclusiva das tabelas. Preservar leitura, destinatários, retenção e remoção já aprovados. As notificações não substituem os resultados e as pendências oficiais das operações; contrato, prevenção de duplicidade e recuperação de entrega permanecem abertos.

O autor definiu que contas, login, grupos, permissões e sessões ficam no serviço Atendimento, sem criar um serviço de Identidade separado. Materiais e Inventário validam acessos técnicos pela API de Atendimento, sem acesso direto às tabelas de identidade. O contrato e as credenciais entre serviços permanecem por detalhar. A indisponibilidade de Atendimento pode afetar essa validação, sem autorizar contornar o login ou aceitar sessões não verificadas.

Por solicitação do autor, adotar o comportamento de sessões encontrado na cópia de SupriTI: 30 minutos de inatividade e limite absoluto de 8 horas por padrão, configuráveis; sessão no servidor, token em cookie protegido e verificação de conta ativa, revogação e expiração nas requisições protegidas. A renovação por atividade não ultrapassa o limite absoluto. Esses são padrões do código examinado, não valores comprovados da configuração de produção. Manter sessões próprias de Chamados e a revogação imediata já aprovada; a responsabilidade pela validação entre os serviços ainda será definida.

Redefinir a senha ou desativar a conta de um técnico encerra imediatamente todas as suas sessões abertas. Recusar novas requisições com sessões revogadas e exigir novo login; troca posterior de senha ou reativação não restaura sessões antigas. Não confundir essa revogação com cancelamento de operações de materiais já autorizadas, que seguem as regras de continuidade e suspensão administrativa existentes.

O administrador cria as contas dos técnicos, com login e senha próprios do sistema, sem autocadastro público da equipe. Fornecer senha temporária e exigir sua troca no primeiro acesso antes de permitir o uso das funções do painel. Em caso de esquecimento, o administrador redefine a senha com outra temporária, novamente sujeita à troca obrigatória no próximo acesso. Não depender de recuperação automática por e-mail. Integração com AD fica para uma etapa futura. Essa autenticação é independente do formulário público do solicitante e mantém as permissões por grupos já definidas. Gestão e revogação de sessões permanecem por detalhar.

O formulário de abertura sem login será acessível pela internet, inclusive fora da rede da Prefeitura. O painel técnico também será acessível pela internet, mas com login obrigatório e aplicação das permissões do usuário autenticado. Essas decisões não concedem consulta pública a chamados, anexos, dados de contato ou cadastros internos e não implicam expor banco, filas ou todas as APIs. Mecanismo de autenticação, sessão, publicação HTTPS, domínio, controles de acesso à infraestrutura e proteção contra envios abusivos ainda serão definidos.

Após discutir adoção pelos funcionários, o autor escolheu simplificar a participação do solicitante: formulário de abertura sem login, destinado ao registro do chamado; esclarecimentos, acompanhamento e pedidos de reabertura continuam pelo WhatsApp/Chatwoot, e o técnico atualiza o chamado no sistema. Não implementar chat próprio nem exigir conta do solicitante nesta etapa. O sistema concentra registro, organização e controle do atendimento, sem substituir integralmente o canal atual.

Esta decisão mais recente substitui as regras anteriores de portal autenticado do solicitante, notificações internas destinadas a ele e ações de acompanhamento, complemento, cancelamento ou pedido de reabertura realizadas diretamente por ele no sistema. As notificações internas da equipe técnica permanecem. Preservar as regras técnicas de prazo, histórico e controle de reabertura que forem compatíveis; a forma de registrar e avaliar uma solicitação recebida pelo canal externo ainda precisa ser detalhada. Não presumir integração automática com Chatwoot, envio automático de mensagens ou criação de chamados a partir das conversas.

O autor confirmou os campos da abertura simplificada: nome, setor, telefone/WhatsApp, assunto, descrição e patrimônio opcional, além de anexo opcional de imagem, como foto do equipamento ou captura do erro. Nome, setor e contato são informados pelo solicitante, sem exigir conta. A relação dos campos essenciais com os formulários configuráveis por categoria ainda precisa ser definida. Sem autenticação, dados informados não comprovam autoria; não expor histórico ou cadastros internos no formulário público. Quantidade, formatos e tamanho máximo das imagens ainda precisam ser definidos.

O sistema deve reunir o atendimento de TI, o histórico técnico dos equipamentos e o acompanhamento de materiais usados nas intervenções. O objetivo é permitir que solicitantes registrem problemas e que a equipe organize, execute e documente os atendimentos sem acrescentar confirmações ou aprovações sem utilidade prática.

Chamados registra o atendimento e a intenção das operações de materiais. O SupriTI continua sendo a fonte oficial de estoque, reservas e movimentações. O Wazuh fornece inventários para detectar alterações de hardware. Um chamado fechado pode manter pendências de integração visíveis e rastreáveis, sem apresentar uma movimentação não confirmada como concluída.

## Fontes e precedência

Este brief consolida [planejamento_suporte_ti.md](./planejamento_suporte_ti.md), [analise_integracao_supriti.md](./analise_integracao_supriti.md), [mudancas_necessarias_supriti.md](./mudancas_necessarias_supriti.md) e as decisões da conversa de 10 de novembro de 2026. Os registros mais recentes substituem as propostas anteriores sobre o mesmo assunto. Pendências antigas já respondidas não devem ser reabertas apenas porque permanecem escritas nos documentos históricos.

A análise de SupriTI documentada nas fontes foi estática, sobre a cópia em `C:/Users/luis.felipe/Desktop/codex/solicitaTEC`; não comprovou a versão em produção. Este brief não representa uma nova inspeção desse código nem testes de integração. O histórico anterior à versão 0.5 do planejamento não foi integralmente recuperado.

## Análise

O principal risco é confundir três fatos diferentes: a conclusão técnica do atendimento, o resultado de uma movimentação de estoque e uma alteração detectada no equipamento. Cada um precisa de seu próprio registro e evidência. Fechar um chamado não comprova uma baixa no SupriTI; detectar uma mudança de hardware não comprova quem a autorizou.

A continuidade durante falhas externas evita bloquear o atendimento, mas cria trabalho de acompanhamento. Por isso, pendências precisam sobreviver a reinícios, indicar resultados por material e permitir recuperação sem consumo duplicado. Reservas sem expiração preservam peças para o atendimento, mas podem reter saldo por tempo prolongado; os participantes devem acompanhar seu destino.

Permitir atuação de vários técnicos facilita continuidade e também cria disputas de edição. As regras aprovadas exigem rejeitar alterações de materiais baseadas em dados antigos e impedir a confirmação de uma revisão final desatualizada. Para decisões de reabertura, a primeira ação válida registrada prevalece.

Formulários configuráveis atendem categorias diferentes, mas exigem preservar a versão usada por cada chamado. Filtros pessoais ajudam a equipe, mas não podem esconder os chamados novos que aguardam triagem. O círculo piscante sinaliza chegada; detalhes de acessibilidade e apresentação ainda precisam ser definidos no desenho.

O resultado pretendido é um atendimento simples de abrir e acompanhar, com histórico confiável e integração recuperável. A solução ainda precisa demonstrar esse resultado nos cenários de falha e concorrência; não há evidência de execução ou desempenho neste brief.

## Objetivos e limites

### Dentro do escopo

- Abertura por solicitantes e técnicos, inclusive em nome de um solicitante ou para ocorrências internas.
- Formulários visuais configuráveis por categoria, painel da equipe, prioridade interna, filtros pessoais e notificações internas.
- Participação conjunta de técnicos, intervenções, revisão final, fechamento, cancelamento e reabertura com histórico.
- Integração com SupriTI para consulta, reserva, ajuste, liberação, consumo e tratamento de devoluções no cancelamento.
- Histórico por equipamento e comparação diária de hardware com dados do Wazuh.
- Permissões dinâmicas por grupos e consulta à base de conhecimento, respeitando o grau de definição descrito abaixo.

### Escopo futuro para discussão: alerta de inatividade de equipamentos

O autor solicitou registrar o monitoramento de equipamentos sem atividade por um período configurável pelo administrador, tendo 35 dias como referência inicial proposta. A ideia é consultar os registros do Wazuh e comparar a última atualização ou atividade com o limite configurado, gerando um alerta de possível sumiço quando o tempo decorrido ultrapassar esse limite. Esse monitoramento complementa a comparação de hardware para identificar possíveis mudanças não autorizadas.

A funcionalidade fica anotada para discussão futura, sem implementação nesta etapa. Ainda será necessário definir qual registro do Wazuh representa atividade, frequência de verificação, tratamento de equipamentos desativados, ausência de registros e falhas de coleta, além dos destinatários e do encerramento dos alertas. A falta de atualização é um indício para investigação, não confirmação de desaparecimento.

### Fora do escopo atual

- Estoque oficial paralelo dentro de Chamados ou cópia integral dos logs brutos do Wazuh.
- Confirmação de conclusão pelo solicitante, aceite de compliance e confirmação automática após sete dias.
- Rascunhos do formulário de abertura.
- Notificações por e-mail, SMS, WhatsApp ou outros canais externos.
- Login único nesta etapa; integração com AD fica para o futuro.
- Controle, por Chamados, da avaliação e entrada em estoque de peças retiradas dos equipamentos. O técnico trata eventual reaproveitamento diretamente no SupriTI.
- Implementação de um canal alternativo de comunicação ou novas etapas de contestação da recusa de reabertura.

## Decisões funcionais vigentes

### 1. Acessos e participação técnica

O administrador master configura permissões por grupos. N1, N2 e N3 não devem determinar privilégios fixos no código. As autenticações humanas de Chamados e SupriTI permanecem separadas.

Um chamado pode ter vários técnicos participantes, com responsabilidade compartilhada por reservas e pendências. Qualquer técnico pode se incluir e retirar outro participante. Não retirar o último participante de um chamado já assumido: incluir o substituto antes. Isso não exige participante técnico na abertura de um chamado ainda em triagem.

Na desativação de um técnico, verificar os chamados aos quais está vinculado. Se ele for o único técnico ou o último participante ainda ativo, o administrador deve escolher um novo técnico para assumir antes de concluir a desativação. Aplicar essa verificação também aos chamados fechados ou cancelados com pendências de integração, evitando que fiquem sem acompanhamento ativo. Havendo outros técnicos ativos participantes, esta regra não exige substituição adicional. Esta definição mais recente substitui tanto a exigência anterior de substituição em todos os chamados abertos quanto a dispensa anterior para encerrados com pendências.

Se o administrador escolher outra pessoa como substituta, enviar ao técnico escolhido uma notificação padrão informando que foi vinculado ao chamado pelo administrador. Se o próprio administrador assumir, não enviar essa notificação a ele. Preservar autoria histórica e registrar a atribuição administrativa; a substituição não reabre o chamado nem repete suas operações. Operações já autorizadas continuam com a autoria original preservada.

Na desativação por suspeita de uso indevido, o administrador pode selecionar operações pendentes para suspensão. Administradores analisam e liberam ou cancelam essas intenções. Suspender não desfaz efeitos externos nem resolve um resultado desconhecido. Registrar que o autor estava ativo na autorização não lhe devolve acesso atual.

### 2. Abertura e formulários

O administrador configura visualmente um formulário por categoria: campos, tipos, ordem, obrigatoriedade e condições de exibição conforme outras respostas. A página do chamado apresenta o resumo dos dados preenchidos pelo solicitante. Cada chamado preserva a versão do formulário usada na abertura, inclusive campos posteriormente alterados ou removidos. Não haverá rascunho manual nem automático para retomada posterior. Se ocorrer erro de validação ou envio, preservar os dados na página aberta para correção ou nova tentativa, sem criar rascunho persistido. O autor confirmou essa distinção após o achado F4: evitar a carga adicional de rascunhos sem obrigar a redigitar a tentativa atual. Isso não garante recuperação após fechar ou recarregar a página.

**Proposta ainda não confirmada tipo a tipo:** texto curto e longo, seleção única e múltipla, caixas de confirmação, sim/não, números, datas e horários, contatos, endereços de site e anexos. Seletores de equipamento, setor e usuário precisam ser especificados como referências a cadastros. A relação entre campos essenciais do sistema e campos removíveis pelo administrador também permanece aberta.

### 3. Painel e prioridades

Separar chamados novos, no topo, dos chamados em andamento. Os novos ficam aguardando triagem, sem prioridade padrão. Cada novo chamado tem um pequeno círculo verde piscante à esquerda, apenas visual, sem ação de clique. Ele continua piscando até um técnico assumir; abrir os detalhes não interrompe o indicador.

Exibir como informações principais: título, categoria, data de abertura, prioridade e solicitante, com setor e usuário. Clicar no chamado abre sua página; o técnico assume e edita a partir dela. Ao assumir, o chamado sai imediatamente da triagem e passa para os em andamento. Se não houver prioridade escolhida, aplicar média.

O técnico avalia a prioridade. A ordem é crítica, alta, média e baixa; dentro de cada nível, os mais antigos primeiro. Qualquer técnico pode alterar a prioridade sem justificativa e sem se tornar participante apenas por essa ação. Registrar autor, data e valores anterior e novo no histórico interno. O solicitante não vê a prioridade nem sua posição na fila.

Os técnicos veem todos os chamados da equipe e podem combinar e salvar filtros, como categoria, prioridade e participante. Manter o último filtro de cada técnico ao retornar; sem filtro anterior, mostrar todos. Os filtros afetam apenas os chamados em andamento: a área de triagem continua mostrando todos os novos.

### 4. Atendimento, revisão e fechamento

Preservar intervenções, participantes e histórico do equipamento. A solução aplicada é exigida para resolver; o diagnóstico é opcional. O vínculo com equipamento deve ser confirmado quando aplicável. Os campos herdados do planejamento inicial precisam ser conciliados com o configurador sem eliminar silenciosamente as exigências da resolução.

Antes de fechar, apresentar revisão de diagnóstico, solução e materiais. O técnico remove manualmente materiais não usados; a revisão não decide sua liberação automaticamente. Cancelar a tela de revisão retorna ao chamado com os dados preservados. Confirmar atesta a solução e o uso dos materiais listados. Sem materiais, confirmar apenas a solução, sem sugerir consumo inexistente.

O técnico fecha o chamado imediatamente. Não há botão de confirmação do solicitante nem contador de aceite. A janela de reabertura não torna o fechamento provisório. Preservar os materiais e quantidades confirmados; uma intervenção posterior não edita nem repete os consumos anteriores.

Se outro técnico alterar materiais, solução, diagnóstico ou vínculo de equipamento durante a revisão, impedir a confirmação antiga, atualizar o resumo e exigir nova conferência antes do fechamento. Validar no processamento da confirmação que o conteúdo atestado ainda corresponde ao apresentado; uma tentativa desatualizada não inicia fechamento nem consumo. A extensão à solução, ao diagnóstico e ao equipamento foi confirmada pelo autor após o achado F2 da revisão Goldfish. Em alterações simultâneas do mesmo material, rejeitar o salvamento desatualizado, mostrar o estado atual e preservar a alteração pretendida para comparação, sem aplicá-la automaticamente.

### 5. Reabertura após fechamento por resolução

O solicitante pode pedir reabertura em até sete dias corridos contados do fechamento mais recente. Seleciona um motivo padrão ou informa justificativa personalizada, sem obrigação de preencher ambos. O pedido não reabre automaticamente: qualquer técnico pode aceitar ou recusar. Até o aceite, o chamado permanece fechado.

O pedido fica visível a todos os técnicos no painel, mas apenas os participantes recebem sua notificação. Quem aceita e reabre torna-se participante automaticamente, mantendo os anteriores. A reabertura retorna o chamado a Em atendimento. A recusa exige motivo visível ao solicitante e registrado no histórico; mantém o fechamento definitivo, sem nova contestação no sistema. A regra de permitir novo pedido após cancelamento pelo solicitante não se aplica à recusa técnica.

Um pedido apresentado dentro do prazo continua válido para avaliação posterior aos sete dias. O solicitante pode acrescentar informações enquanto aguarda avaliação, sem nova notificação e sem alterar a data original. Pode cancelar o pedido pendente; atualizar o painel e remover seu aviso, sem enviar outro. Após esse cancelamento, pode fazer outro pedido apenas dentro da janela original.

A primeira ação válida registrada prevalece entre aceite, recusa e cancelamento, inclusive em telas desatualizadas. Informar o estado atualizado a quem tentar uma ação posterior. A decisão técnica remove o aviso pendente dos demais participantes e envia ao solicitante a notificação padrão do resultado.

Se houver pedido pendente, a reabertura direta por técnico equivale ao aceite desse pedido e recebe os mesmos efeitos: encerrar a pendência como aceita, reabrir em Em atendimento, incluir o técnico como participante se necessário, remover os avisos pendentes e notificar o solicitante. Não deixar pedido pendente após essa reabertura nem duplicar efeitos ou notificações. Por equivaler ao aceite, essa ação participa da mesma regra de primeira ação válida registrada em concorrência com recusa ou cancelamento.

O autor confirmou que, mesmo após uma recusa já registrada, outro técnico pode reabrir diretamente o chamado dentro dos sete dias corridos do fechamento mais recente. A recusa encerra aquele pedido e mantém o chamado fechado naquele momento, mas não bloqueia essa ação técnica posterior. Preservar a recusa, seu motivo e sua autoria no histórico; registrar a reabertura como uma nova ação, com o motivo exigido para reabertura técnica, sem converter retroativamente o pedido recusado em aceito. Aplicar as regras de participação, estado e notificações da reabertura direta. Não reiniciar a janela apenas porque houve recusa.

Qualquer técnico também pode reabrir diretamente conforme a janela e o motivo já definidos. Após nova resolução, começa outra janela de sete dias. Fora da janela, oferecer novo chamado vinculado, com dados do equipamento e confirmação da criação, sem herdar participantes, reservas ou consumos nem notificar antigos participantes apenas pelo vínculo.

### 6. Cancelamento do chamado

O solicitante pode cancelar sem motivo obrigatório somente antes de um técnico ser vinculado. Esses chamados não permanecem no histórico; o mecanismo de remoção ainda precisa ser desenhado. Qualquer técnico pode cancelar mediante descrição obrigatória do motivo, mesmo sem participar, sem inclusão automática como participante. Notificar o solicitante e, quando o autor não participa, os participantes.

Cancelamentos por técnicos preservam histórico, materiais e resultados de integração. As decisões de 9 de novembro permitem reabertura por técnicos e solicitante em sete dias corridos do cancelamento, com motivo padrão ou personalizado, retorno a Em atendimento e preservação dos participantes. **A avaliação técnica do pedido aprovada posteriormente foi definida para chamados resolvidos; sua aplicação aos cancelados por técnicos ainda precisa ser confirmada.** Após o prazo, oferecer novo chamado vinculado.

### 7. Notificações

Usar somente o próprio sistema: ícone com indicador de não lidas, persistência durante desconexão e retenção de 30 dias desde a criação, tanto para lidas quanto não lidas. Clicar marca como lida e abre o chamado conforme as permissões vigentes. Remover uma notificação não apaga o histórico nem resolve pendências.

Novos chamados notificam todos os técnicos, sem torná-los participantes. Ao assumir, remover o aviso de entrada na fila dos demais e notificar o solicitante. Notificar o solicitante da conclusão e dos resultados dos pedidos de reabertura. Manter as regras específicas de notificação dos participantes na reabertura e no cancelamento.

### 8. Materiais e SupriTI

Criar fluxo próprio de Chamados, separado das solicitações comuns do SupriTI e de suas aprovações, mas disputando o mesmo saldo oficial. Autenticar a aplicação sem exigir conta SupriTI para cada técnico; Chamados autoriza o técnico e SupriTI valida a operação de estoque. O mecanismo de credencial ainda não foi escolhido. A integração não administra permissões nem recebe implicitamente poder de corrigir saldo.

Selecionar material solicita reserva, sem reduzir estoque físico. Reservas de Chamados não expiram por idade. Alterar quantidade ou remover material trata o ajuste ou a liberação correspondente. O consumo ocorre no fechamento após revisão. Informar condição nova ou recondicionada e respeitar seu saldo, sem substituição automática.

Indisponibilidade ao reservar, consumir ou liberar não impede atendimento, fechamento ou cancelamento. Persistir as intenções e mostrar pendência. Se o fechamento ocorrer antes da reserva sincronizar, a intenção vigente passa a consumo, com reconciliação quando houver resultado desconhecido. Não deixar uma reserva antiga reaparecer depois do consumo.

Usar fila persistente, tentativas espaçadas, pausa durante indisponibilidade e recuperação gradual com carga limitada. Identificar operações para que repetição ou perda de resposta não gere duplicidade. Sucesso parcial preserva itens confirmados e mantém pendências apenas nos demais. Falta de saldo ou item desativado gera divergência específica, sem forçar saldo negativo nem insistir continuamente em recusa de negócio.

O técnico corrige saldo diretamente no SupriTI com suas permissões e solicita nova sincronização em Chamados. Corrigir saldo não é lançar manualmente a mesma saída: o consumo permanece com a integração. O alerta só desaparece após resultado externo verificável. A diferença entre as permissões informadas pelo autor e a restrição a ADMIN na cópia examinada exige validação.

No cancelamento, identificar materiais mantidos e devolvidos, inclusive divisão de quantidade do mesmo item. Uma justificativa conjunta explica os materiais mantidos. Consumir os mantidos sem repetir baixas já feitas; liberar reservas ou registrar devoluções conforme o efeito externo real. Não criar entrada física para peça que só estava reservada. Peça nova devolvida por não ter sido efetivamente usada retorna como nova. Peças retiradas dos equipamentos seguem o processo independente descrito nos limites.

Bloquear desativação de produto com reserva ativa no SupriTI, mostrando ID do chamado e data da reserva. Se o item for desativado antes de uma reserva pendente chegar, comunicar a recusa e manter a divergência. Preservar aplicação executora, autor humano estável, chamado, intervenção, material, quantidade, condição, operação e resultado.

### 9. Equipamentos, Wazuh e conhecimento

Manter histórico técnico por equipamento, relacionando chamados e intervenções. Comparar inventários Wazuh ao final do dia e economizar armazenamento, sem guardar todo o histórico bruto no novo sistema.

**Propostas pendentes de aprovação:** comparar com o último inventário válido quando houver dias sem coleta; manter referência mínima e diferenças relevantes; classificar alterações e associá-las a intervenções mediante evidência. Horário, componentes, normalização, primeira coleta e critérios de validade ainda precisam ser definidos. Ausência de coleta não comprova ausência de mudança; a comparação diária pode perder alterações revertidas no mesmo dia.

A consulta a artigos e seu vínculo à solução foi aceita no contexto do planejamento original. Autoria, publicação, revisão, visibilidade e criação de artigo a partir de uma solução continuam por definir. Transferência patrimonial, manutenção externa, retorno e baixa também não têm fluxo fechado.

## Alternativas consideradas

| Alternativa | Decisão do autor e motivo registrado |
| --- | --- |
| Confirmação de conclusão pelo solicitante e aceite automático | Retirada pelo autor: não tem utilidade prática para fechar um chamado que já está concluído e acrescenta processos. |
| Salvar abertura como rascunho | Retirada explicitamente pelo autor; não foi apresentado motivo adicional. |
| Reabertura imediata pelo solicitante após resolução | Substituída por pedido sujeito a aceite técnico. O autor definiu esse controle, sem justificativa adicional registrada. |
| Notificar toda a equipe de cada pedido de reabertura | O autor escolheu visibilidade geral no painel e aviso apenas aos participantes. |
| Notificar complementos do pedido | Recusada pelo autor: apenas atualizar o pedido. |
| Prioridade padrão na chegada | O autor escolheu aguardar triagem; média só é aplicada ao assumir sem definição. |
| Solicitante visualizar prioridade | Recusada pelo autor: classificação interna feita pela equipe técnica. |
| Expirar reservas de Chamados por idade | O autor escolheu reservas sem validade temporal e acompanhamento pelos participantes. |
| Aprovação humana adicional para consumo de Chamados | O autor escolheu fluxo próprio equivalente à autonomia do uso interno, separado das solicitações comuns. |
| Bloquear atendimento por indisponibilidade do SupriTI | O autor escolheu continuidade com pendência persistida e sincronização posterior. |
| Liberar automaticamente materiais supostamente não usados na revisão | O autor manteve remoção manual e confirmação explícita da lista final. |
| Canais externos e login único agora | Adiados: notificações ficam internas e AD é integração futura. |

Durante a abertura do plano técnico, o autor escolheu serviços separados para favorecer desenvolvimento cooperativo por mais pessoas, em vez de uma aplicação única com módulos internos. O código dos serviços de Chamados ficará em um único repositório; isso não define compartilhamento de banco ou implantação conjunta. Fronteiras, propriedade dos dados, banco, hospedagem, protocolo, credencial e tecnologia de fila ainda não foram escolhidos. A saída manual atual do SupriTI não demonstrou suportar a conversão de reservas próprias de Chamados; essa limitação é evidência da análise, não rejeição de um mantenedor externo.

## Questões abertas e próximos passos

### Decisão de linguagem: Rust

O autor escolheu Axum como framework HTTP dos três serviços Rust, mantendo APIs HTTP/JSON. O acesso ao PostgreSQL será feito com SQLx e SQL escrito diretamente, sem ORM. Versões, pools, migrações e implementação da fila permanecem por definir.

O autor escolheu Rust para os serviços de backend Atendimento, Materiais e Inventário, buscando economia de recursos e aprendizado. A equipe domina principalmente JavaScript, Python e Java. Considerar o esforço de aprendizado e validar o consumo real na VM, sem presumir que a linguagem isoladamente garante o orçamento de recursos. Axum e SQLx com SQL direto foram escolhidos posteriormente; as demais bibliotecas e a tecnologia da interface permanecem abertas.

### Decisão de implantação: servidor próprio

O autor escolheu uma VM Debian em servidor próprio da Prefeitura, com um container por serviço de Chamados, e informou capacidade inicial de 4 GB de RAM, 4 vCPUs e 100 GB de armazenamento no total. O banco compartilhado e a fila persistente também ficarão nessa mesma VM. Todos compartilham sua disponibilidade e seus recursos; persistência da fila não oferece recuperação automática diante de perda do armazenamento da VM. Ferramenta de containers, publicação da interface, forma de execução do banco e da fila, distribuição de recursos, persistência, backup e recuperação ainda precisam ser definidos. A suficiência para a carga real não foi validada e esta decisão não estabelece alta disponibilidade.

### Decisão do plano técnico: banco compartilhado

O autor confirmou comunicação por APIs HTTP com mensagens JSON entre os três serviços, mantendo consultas e alterações na interface do proprietário dos dados. O trabalho persistido em filas internas será encaminhado pelas APIs, sem leitura direta de tabelas alheias. Autenticação, proteção do transporte e contratos permanecem por definir; a escolha não obriga esperar a conclusão externa para responder ao usuário nem presume compatibilidade dos contratos externos do SupriTI e do Wazuh.

O autor escolheu implementar as filas persistentes somente no PostgreSQL, sem RabbitMQ ou outro intermediário nesta etapa. Cada serviço mantém propriedade exclusiva de suas tabelas de tarefas; a fila não autoriza acesso direto aos dados dos demais serviços. Encaminhamento entre serviços, processamento concorrente, recuperação, tentativas e biblioteca ou implementação ainda serão definidos. Manter a distinção entre material aguardando identificação humana e operação apta ao processamento automático.

O autor definiu PostgreSQL como banco compartilhado, por ser a tecnologia utilizada pela equipe. Versão, organização de schemas e tabelas, credenciais, migrações e limites de conexão permanecem abertos. Manter as fronteiras de leitura e escrita por serviço já aprovadas.

O autor escolheu um banco compartilhado entre os serviços Atendimento, Materiais e Inventário, com escrita exclusiva: cada serviço altera apenas suas próprias tabelas. Tanto mudanças quanto consultas a dados de outro serviço passam pela interface responsável, sem acesso direto às tabelas alheias. O banco do SupriTI permanece externo; seu estoque não será incorporado ao banco de Chamados. A escolha cria uma dependência comum, sem garantir isolamento diante de falha do banco. A atribuição concreta das tabelas, as credenciais e a coordenação de migrações ainda precisam ser definidas.

### Decisão inicial do plano técnico: três serviços

O autor confirmou o uso de fila persistente no fluxo de materiais provisórios com uma condição explícita: sem produto oficial identificado, o registro aguarda ação humana e não gera tentativas de baixa. Após a seleção do produto pelo técnico, preparar a operação para processamento automático conforme a intenção vigente, sem antecipar consumo antes da revisão e do fechamento ou do cancelamento aprovado. Identificação pendente e operação pronta para processamento são situações distintas. Tecnologia da fila, propriedade do registro provisório e persistência/publicação sem perda ainda precisam ser definidas; esta escolha não torna automaticamente todos os processos assíncronos.

Materiais registrados provisoriamente sem produto oficial associado aparecem na seção independente de pendências com o aviso “Identificação de material pendente”, inclusive em chamados fechados. A identificação encerra esse tipo de pendência, mas não comprova baixa nem elimina outras pendências de integração do material.

Se o serviço de Materiais estiver indisponível, permitir ao técnico registrar no chamado os materiais utilizados mesmo sem consultar o catálogo, mantendo a sincronização pendente sem bloquear o fechamento. Não apresentar reserva ou consumo como confirmado. O autor confirmou os campos provisórios: nome ou descrição, quantidade e condição (novo ou recondicionado). Após a recuperação, o técnico seleciona o produto correto do catálogo antes da baixa; não associar automaticamente por nome. Preservar os dados originais e a autoria e data da associação. O mecanismo de persistência e encaminhamento ainda precisa ser definido.

Qualquer técnico pode desativar e reativar equipamentos, sem concessão específica adicional para essas ações. Preservar autoria, data e histórico, mantendo a execução pela interface do serviço de Inventário.

O chamado guarda apenas o identificador do equipamento, sem cópia de patrimônio, descrição ou resumo do cadastro. Os detalhes são consultados pela interface de Inventário. Durante uma falha desse serviço, manter o identificador e sinalizar a indisponibilidade dos detalhes; não converter um vínculo existente em pendência de associação apenas por não conseguir consultá-lo. Equipamentos fora de uso são desativados, preservando cadastro, identificador e histórico para consulta nos chamados antigos. Não permitir sua seleção em novos chamados enquanto estiverem desativados; a reativação torna a seleção novamente disponível, preservando o mesmo cadastro e seus vínculos. O formato do identificador permanece aberto.

O autor escolheu Atendimento (chamados e formulários), Materiais (integração SupriTI) e Inventário (equipamentos e Wazuh), no mesmo repositório, para desenvolvimento cooperativo com três pessoas. A justificativa é facilitar o isolamento e o diagnóstico de falhas de materiais e inventário. A separação não garante disponibilidade por si só: contratos e tratamento de indisponibilidade ainda serão desenhados. Propriedade dos dados e localização de funções transversais continuam abertas.

Se Inventário ficar indisponível, o atendimento e o fechamento podem continuar usando o vínculo de equipamento já registrado. A consulta ao histórico do equipamento e ao Wazuh fica temporariamente indisponível, sem apagar o vínculo nem interpretar a falha como inexistência de dados. O autor confirmou que também será possível abrir um chamado sem selecionar equipamento durante essa falha e realizar o vínculo após a recuperação. A contingência não deve ser bloqueada pela obrigatoriedade normal do seletor no formulário. Se o serviço terminar antes da recuperação, permitir fechar sem vínculo, registrando a indisponibilidade como justificativa e mantendo a associação pendente para regularização posterior. Registrar o vínculo posterior com autoria e data, sem apagar o histórico nem exigir reabertura apenas para essa regularização. O local de acompanhamento dessa pendência ainda precisa ser definido.

### Decisão após revisão Goldfish: seção de pendências de integração

O usuário definiu uma seção à parte, independente, para localizar chamados com pendências de integração, inclusive os já fechados ou cancelados. A presença nessa seção depende da pendência de integração, não de o atendimento continuar aberto nem da existência de uma notificação. Em conjunto com a substituição obrigatória do último técnico ativo, essa decisão trata o acompanhamento e a descoberta das pendências apontados em F1 do relatório Goldfish. O autor confirmou posteriormente que a mesma seção também deve mostrar a associação de equipamento ainda não regularizada após falha de Inventário, identificada como “Vínculo de equipamento pendente”. Manter os tipos distintos: regularizar o vínculo não elimina pendências de materiais e a recuperação do serviço não comprova a associação. Os demais detalhes de apresentação dessa seção ainda serão desenhados.

1. Confirmar com o autor este brief, especialmente a distinção entre reabertura de resolvidos e de cancelados, e a separação entre campos essenciais e configuráveis. Completar os tipos de campo e as regras de edição ou troca de categoria após abertura.
2. Consolidar a matriz de permissões e os estados do atendimento, pedido de reabertura e integração. Definir apresentação de pedidos pendentes no painel sem presumir que filtros possam ocultá-los. Fixar fuso e instante limite da janela de sete dias.
3. Validar a instalação real do SupriTI e definir contrato de consulta, reserva, ajuste, liberação, consumo, devolução e consulta de resultados, com origem e autoria. Escolher credencial, representação de movimentações e compatibilidade com solicitações comuns.
4. Definir os mecanismos de persistência e concorrência entre resolução local, fila e operações externas; limites de carga, tempos de espera, recuperação, suspensão e reconciliação. Não escolher esses mecanismos silenciosamente durante implementação.
5. Fechar regras de ativos, associação ao agente Wazuh, componentes comparados e retenção. Completar a base de conhecimento e os fluxos patrimoniais ou delimitar sua entrega posterior.
6. Definir implantação, migração, segurança, anexos, retenção dos chamados e histórico, backup, recuperação e metas mensuráveis. A média mencionada nas fontes de cinco chamados por dia não é limite de pico nem volume de operações.
7. Após revisão e autorização do autor, submeter o artefato a Goldfishes em contextos independentes. Depois, redigir `design_draft.md` por seções com decisões arquiteturais explícitas do autor. Nenhuma revisão independente foi executada para produzir este brief.

## Aprovação opcional de uma ação no chamado — decisão de 2026-11-12

Escopo funcional aprovado pelo autor em 2026-11-12, com autorização para continuar o plano técnico, sem autorização de implementação.

O técnico pode solicitar a análise de outro técnico que ele designar antes de executar uma ação específica no chamado. Registra por escrito a ação proposta, as informações disponíveis, os prós, os contras e os riscos conhecidos. A pessoa designada lê o conteúdo e aprova ou reprova. O objetivo é documentar a comunicação dos riscos e a decisão de quem autorizou a continuidade, inclusive quando essa pessoa ocupa cargo superior; a hierarquia não define, por si só, uma permissão no sistema.

O autor escolheu bloquear apenas a ação submetida: enquanto aguarda decisão ou após reprovação, essa ação não pode prosseguir. Outros trabalhos no chamado podem continuar. A aprovação permite a continuidade da ação avaliada, sem executá-la automaticamente e sem determinar diretamente o fechamento do chamado. A etapa é opcional no fluxo geral; uma vez solicitada, aplica-se o impedimento à ação correspondente.

Registrar no histórico a solicitação, o solicitante, a pessoa designada e sua decisão. O conteúdo avaliado, as datas e a identificação autenticada precisam ser detalhados no desenho para que se possa distinguir o que foi aprovado de alterações posteriores. A intenção de oferecer evidência administrativa não constitui garantia de isenção de responsabilidade. O mecanismo de aceite e eventual documento ou assinatura digital ainda não foram definidos.

O autor definiu a ação como uma etapa escrita e documental, sem vínculo obrigatório com uma operação funcional do sistema. Exemplos: implantação de um sistema em um setor ou de um servidor Samba. A solicitação documenta a proposta e os riscos apresentados à pessoa designada; a aprovação registra que ela autorizou a implementação com conhecimento dos riscos descritos. O impedimento aplica-se à continuidade da etapa documentada, sem bloquear o restante do chamado nem controlar tecnicamente a execução externa. Não criar bloqueios de consumo de materiais ou de alteração de cadastros por inferência dessa decisão.

O autor escolheu aprovação ou reprovação pela conta autenticada da pessoa designada. Registrar sua identidade, a decisão, a data e a hora, vinculadas ao conteúdo avaliado. Não exigir nova confirmação de senha nem certificado digital para esse aceite. Aplicar as validações de sessão e conta ativa já definidas. Trata-se de um aceite autenticado registrado no sistema; não declarar que houve assinatura com certificado digital.

O autor escolheu preservar cada versão da proposta. Alterações geram uma nova versão e exigem nova aprovação; uma decisão anterior não autoriza o texto alterado. Preservar o conteúdo de cada versão e suas decisões, com autoria e datas, sem sobrescrever a evidência anterior. Vincular cada aprovação ou reprovação à versão efetivamente avaliada. O impedimento documental da ação volta a valer para a versão alterada até sua aprovação.

O autor definiu que a decisão deve recair sobre a versão atual. Se a proposta mudar durante a leitura, recusar a tentativa de aprovação ou reprovação da versão substituída, apresentar o conteúdo atual e exigir nova conferência antes de decidir. Verificar a versão no processamento da decisão, sem depender apenas do estado exibido na página. A tentativa desatualizada não constitui aprovação nem reprovação de nenhuma versão; preservar as decisões que já haviam sido validamente registradas antes da alteração.

O autor definiu comentário opcional nas decisões e acrescentou aprovação com ressalva e reprovação com sugestão. Oferecer quatro resultados: aprovação, aprovação com ressalva, reprovação e reprovação com sugestão. Preservar o resultado escolhido e o comentário, quando informado, vinculados à versão avaliada. A sugestão não altera automaticamente a proposta nem autoriza sua execução. Se incorporada ao texto, aplica-se a regra de nova versão e nova aprovação. Ainda definir o efeito da aprovação com ressalva sobre a autorização de continuidade e como representar uma ressalva ou sugestão sem texto, sem presumir obrigatoriedade de comentário.

O autor definiu que aprovação com ressalva autoriza a continuidade da ação na versão avaliada, sem comprovação de condições nem novo aceite. A ressalva permanece como observação documental. Isso não autoriza versões futuras alteradas da proposta, que continuam sujeitas a nova aprovação. O comentário permanece opcional conforme decisão anterior; ainda definir a apresentação de ressalvas ou sugestões quando não houver texto.

O autor esclareceu a obrigatoriedade por resultado: aprovação e reprovação simples permitem comentário opcional; aprovação com ressalva e reprovação com sugestão exigem comentário descrevendo o que deve mudar. Essa definição substitui a generalização anterior de comentário opcional em todas as decisões e resolve a apresentação sem texto: não permitir registrar resultados com ressalva ou sugestão sem comentário preenchido. Validar essa exigência no servidor. A aprovação com ressalva continua autorizando a continuidade sem novo aceite, conforme decisão anterior; não criar uma etapa adicional de comprovação das mudanças por inferência.

O autor definiu que qualquer outro técnico ativo pode ser designado para avaliar, sem permissão específica adicional de aprovação e sem exigência de cargo superior. O solicitante não pode designar a si próprio. A decisão cabe à pessoa designada, autenticada e com conta ativa; a elegibilidade dos demais técnicos não os autoriza a decidir em seu lugar. Não exigir participação prévia no chamado apenas para ser escolhido como avaliador.

O autor definiu que qualquer técnico com acesso ao chamado pode criar uma solicitação de aprovação, sem exigir que seja participante. Essa possibilidade respeita o acesso já autorizado ao chamado e não concede acesso a chamados antes inacessíveis. Registrar a autoria da solicitação independentemente da lista de participantes.

O autor definiu que, antes da decisão, o técnico que criou a solicitação de aprovação ou um administrador pode substituir o avaliador. Aqui, solicitante significa o autor dessa solicitação técnica, não a pessoa que abriu o chamado público. O novo avaliador deve ser outro técnico ativo, respeitando a proibição de autodesignação do autor. Registrar quem efetuou a troca, a data e a hora, o avaliador anterior e o novo. Após a troca, o avaliador anterior não pode decidir a solicitação. A troca não apaga versões ou decisões históricas nem altera uma decisão já registrada.

O autor definiu que o autor da solicitação de aprovação ou um administrador pode cancelá-la enquanto estiver pendente. Preservar a solicitação e suas versões no histórico, registrando quem cancelou, a data e a hora. Uma solicitação cancelada não aceita decisão posterior. Cancelamento não é aprovação nem comprova autorização para executar a ação proposta. O efeito sobre a continuidade documental da etapa ainda precisa ser definido.

O autor definiu que cancelar uma solicitação pendente retira a exigência de aprovação e permite a continuidade documental da etapa. Registrar explicitamente quem retirou a exigência, a data e a hora, preservando proposta, versões e histórico. Apresentar o resultado como exigência retirada, sem aprovação do avaliador; não classificar a proposta como aprovada. Essa retirada pode ser feita pelo autor da solicitação ou por um administrador, conforme a permissão de cancelamento já definida, e não se aplica a solicitações já decididas. Esta definição resolve o efeito do cancelamento anteriormente aberto.

O autor definiu que o fechamento do chamado exige resolver todas as solicitações de aprovação pendentes: registrar uma das quatro decisões ou retirar a exigência pelas pessoas autorizadas. Enquanto houver solicitação pendente, impedir o fechamento, inclusive se a pendência surgir durante a revisão final. Uma reprovação resolve a solicitação, mas não autoriza executar a ação reprovada; por si só, não impede fechar o atendimento. Esta regra é uma condição específica de fechamento acrescentada ao caráter documental da etapa.

O autor definiu que cancelar o chamado cancela automaticamente o processo de aprovação ainda pendente. Registrar o cancelamento das solicitações como consequência do cancelamento do chamado, com autoria e data da ação de origem, preservando propostas, versões e decisões anteriores. Impedir decisões posteriores nessas solicitações. Distinguir este encerramento por cancelamento do chamado da retirada individual de exigência para continuar uma etapa: o cancelamento do chamado não autoriza sua execução. Não apagar o histórico nem presumir reversão de implementações externas já realizadas. As regras vigentes de liberação, consumo, devolução e acompanhamento de materiais no cancelamento continuam aplicáveis; cancelar o fluxo de aprovação não comprova o cancelamento de efeitos externos ou de pendências de integração.

O autor definiu notificações internas ao avaliador quando a análise for solicitada e ao autor da solicitação quando houver decisão, em qualquer dos quatro resultados. Aplicar as regras vigentes de destinatários, leitura, retenção e remoção de avisos de ações que deixarem de ser aplicáveis. A notificação não substitui o histórico nem comprova leitura da proposta. Não acrescentar envio por canais externos. Avisos específicos de troca de avaliador, nova versão e cancelamento ainda precisam ser detalhados.

O autor confirmou que trocar o avaliador ou criar nova versão atualiza a pendência e notifica o avaliador atualmente responsável sobre a análise vigente. Remover avisos anteriores que perderam validade, sem apagar o histórico. Após troca, a pendência pertence ao novo avaliador; após alteração do texto, refere-se à nova versão. Nenhum aviso antigo autoriza decidir sobre versão substituída ou por avaliador removido. No cancelamento, a regra geral já aprovada exige remover avisos de ação pendente que deixaram de ser aplicáveis; eventual aviso informativo específico de cancelamento ainda não foi definido.

O autor definiu que qualquer técnico com acesso ao chamado pode editar a proposta e gerar nova versão, identificando explicitamente quem fez a alteração e quando. Preservar o conteúdo anterior, a autoria de cada versão e as decisões correspondentes; editar não transfere a autoria original da solicitação nem as permissões de cancelamento e troca do avaliador. Aplicar as regras já definidas de nova aprovação, atualização da pendência e notificação do avaliador. Ainda definir o tratamento quando o próprio avaliador altera a proposta que lhe foi submetida, para não presumir autorização para aprovar o próprio texto.

O autor esclareceu a exceção à edição por qualquer técnico com acesso: o avaliador designado não edita diretamente a proposta que está avaliando. Para autorizar a continuidade indicando mudanças, usa aprovação com ressalva, descrevendo-as no comentário obrigatório e preservando o texto avaliado. Pode também registrar reprovação com sugestão quando não autorizar a continuidade. Os demais técnicos com acesso podem criar novas versões com autoria registrada, sujeitas a nova análise. Aplicar a restrição ao avaliador atualmente designado, inclusive no servidor. Esta decisão resolve o tratamento da edição pelo próprio avaliador anteriormente aberto.

O autor escolheu apresentar o fluxo somente no histórico do chamado, como linha do tempo, sem seção própria de aprovações. Mostrar em sequência quando a análise foi solicitada e quando houve decisão, identificando envolvidos, datas, horários e versão avaliada. Registrar também novas versões com autoria, trocas de avaliador, retirada de exigência e cancelamento decorrente do chamado. Permitir consultar o texto de cada versão e o comentário da decisão a partir do respectivo evento. Exibir a situação atual da solicitação junto ao registro correspondente, distinguindo pendência, os quatro resultados e as duas formas de cancelamento, sem alterar o significado histórico dos eventos anteriores. Manter as notificações internas já aprovadas.

O autor escolheu priorizar a decisão nas disputas com alterações: enquanto o avaliador estiver analisando, impedir edições da proposta e exigir liberação antes de editar. Essa restrição é temporária e não elimina a possibilidade de os demais técnicos criarem versões fora do período de análise. Não usar apenas a notificação como prova de início da análise. Ainda definir o início explícito, a liberação e a recuperação do bloqueio em caso de abandono, perda de sessão ou indisponibilidade do avaliador. Preservar a validação da versão atual ao registrar a decisão. A prioridade não define, por si só, a ordem entre decisão, troca de avaliador e cancelamento.

O autor definiu que abrir a solicitação pelo avaliador designado inicia a análise e bloqueia a edição da proposta, sem botão separado de início. Abrir o chamado ou receber a notificação não basta: o início corresponde à abertura da solicitação pendente pelo responsável atual, com sessão válida. A consulta por outros técnicos não inicia esse bloqueio. Registrar o início da análise com identidade, data e hora na linha do tempo. O início não equivale à aprovação nem comprova concordância com os riscos. O bloqueio se restringe à proposta, preservando o restante do atendimento.

O autor definiu liberar a edição ao sair da visualização da solicitação sem decidir. A saída encerra a análise em curso, não cancela a solicitação nem retira a exigência de aprovação; a solicitação permanece pendente e a ação continua documentalmente impedida. Registrar o encerramento da análise na linha do tempo. Uma abertura posterior pelo avaliador inicia novo período de análise sobre a versão atual. Prever recuperação por prazo quando o navegador ou a conexão não comunicar a saída, sem depender exclusivamente de eventos de fechamento da página; o prazo ainda será escolhido pelo autor. Ao decidir, encerrar o bloqueio da análise e aplicar o resultado registrado, preservando as regras de versionamento.

O autor definiu expirar o bloqueio de análise após 1 minuto sem comunicação com a visualização do avaliador. Esse prazo recupera saídas não comunicadas e perdas de conexão; não é limite total de leitura nem altera os prazos de sessão. Após expirar, a solicitação continua pendente e a edição pode prosseguir. Antes de aceitar decisão de uma visualização cujo bloqueio expirou, exigir retomada da análise e conferência da versão atual. Ainda detalhar o mecanismo de renovação e o tratamento de múltiplas visualizações.

O autor permitiu múltiplas visualizações simultâneas da mesma solicitação pelo avaliador. Manter o bloqueio de edição enquanto ao menos uma visualização estiver ativa e se comunicando. Sair de uma delas encerra apenas sua participação no bloqueio; cada visualização sem comunicação expira após 1 minuto, e a edição é liberada quando não restar nenhuma ativa. Aceitar somente a primeira decisão válida registrada para a versão pendente; as demais visualizações devem apresentar o resultado atual e não registrar outra decisão. Validar sessão, designação, versão e estado pendente no servidor. Ainda detalhar o mecanismo de renovação, sem modificar esses comportamentos.

O autor confirmou que o autor da solicitação ou um administrador pode cancelá-la ou trocar o avaliador mesmo durante a análise, enquanto ela continuar pendente. Registrar a ação no histórico e encerrar os períodos de análise ativos. Cancelar retira a exigência conforme regra já definida; trocar mantém a solicitação pendente, notifica o novo avaliador e exige nova abertura para iniciar sua análise. Invalidar as visualizações anteriores para decisão e renovação do bloqueio, inclusive se a mesma pessoa voltar a ser designada posteriormente; exigir nova abertura válida. Se uma decisão já tiver sido registrada, essas ações não podem alterar seu resultado. A ordem entre ações efetivamente simultâneas ainda precisa ser definida.

O autor definiu que, na disputa entre decisão e intervenção de cancelamento ou troca de avaliador, prevalece a primeira ação válida registrada. A operação seguinte deve conferir o estado atualizado e ser recusada se incompatível: decisão já registrada impede cancelar ou trocar nessa solicitação; cancelamento impede decisão posterior; troca invalida a tentativa do avaliador anterior. Não usar o horário de clique do navegador como critério. A validação e o registro precisam ocorrer de forma indivisível no serviço responsável. Preservar a regra de bloqueio de edição durante análise e a exigência de resolver pendências no fechamento, verificando ambas no processamento das operações.

O autor confirmou notificar o avaliador quando a solicitação for cancelada. Remover o aviso de análise pendente e enviar notificação interna informativa de cancelamento, preservando o evento na linha do tempo. Aplicar tanto à retirada individual de exigência quanto ao cancelamento automático pelo cancelamento do chamado, distinguindo o motivo para não sugerir autorização de continuidade no segundo caso. Respeitar as regras gerais de destinatários e retenção já aprovadas.

O autor escolheu renovar o bloqueio por interação na visualização do avaliador: clicar, digitar ou rolar. Não renovar apenas porque a página permanece aberta. Após 1 minuto sem interação comunicada e aceita pelo servidor, expirar a participação dessa visualização no bloqueio, mesmo durante leitura silenciosa. Com múltiplas visualizações, manter o bloqueio enquanto ao menos uma permanecer ativa segundo essa regra. A sessão autenticada continua sujeita aos prazos próprios; interação não restaura sessão revogada ou expirada. Depois de perder o bloqueio, a visualização precisa retomar a análise e conferir a versão atual antes de decidir; não permitir que um sinal atrasado restaure automaticamente uma análise invalidada por troca, cancelamento ou decisão. Esta definição substitui a formulação anterior genérica de comunicação e resolve o mecanismo funcional de renovação. O mecanismo técnico será detalhado no plano de implementação. Não presumir bloqueio automático de ações executadas fora do sistema.

## Decisão de interface — 2026-11-12

O autor aprovou em 2026-11-12 as definições de interface e publicação abaixo, autorizando a continuidade do plano técnico sem autorização de implementação. Permanecem abertas as escolhas técnicas não resolvidas explicitamente.

O autor escolheu interface executada no navegador consumindo APIs, com código no mesmo repositório dos serviços. Os serviços mantêm autenticação, permissões, validações, controle de versões e transições concorrentes. Preservar o cookie protegido de sessão e não enviar credenciais de integração ao navegador. Prever carregamento, indisponibilidade e recuperação de falhas de envio sem duplicar efeitos nem criar rascunhos persistentes por inferência. Framework, linguagem, publicação dos arquivos e roteamento entre serviços permanecem abertos.

O autor escolheu TypeScript como linguagem da interface, resolvendo essa pendência. Framework e ferramenta de construção continuam abertos. A tipagem não verifica respostas HTTP em tempo de execução; detalhar o alinhamento com os contratos Rust e a validação dos dados recebidos.

O autor escolheu React com TypeScript para a interface por já ter experiência com React. Preservar a execução no navegador consumindo APIs. Ferramenta de construção, roteamento, componentes, formulários e gerenciamento de dados remotos continuam por definir; não presumir um framework de servidor ou renderização no servidor a partir dessa escolha.

O autor escolheu um proxy no mesmo endereço da interface para encaminhar `/api/atendimento`, `/api/materiais` e `/api/inventario` aos respectivos serviços. Preservar validações nos serviços, consulta de identidade à API de Atendimento e propriedade exclusiva dos dados. O proxy não autoriza operações e constitui dependência comum do acesso pelo navegador. Software do proxy, publicação dos arquivos React, HTTPS, caminhos e limites operacionais ainda serão detalhados.

O autor definiu servir os arquivos estáticos construídos da interface React no próprio proxy, sem container separado para a interface. As APIs continuam encaminhadas aos respectivos serviços. Detalhar publicação, rotas de navegação, atualização e recuperação dos arquivos; software e execução do proxy e ferramenta de construção permanecem abertos. Essa escolha não exige servidor Node.js em produção apenas para entregar a interface.

O autor informou existir Traefik em uso na Prefeitura e escolheu encerrar o HTTPS nessa infraestrutura. O tráfego segue para o proxy do projeto, que serve React e encaminha as APIs; não presumir remoção desse proxy ou instalação de novo Traefik na VM. Confirmar domínio, certificados, rede, confiança nos cabeçalhos encaminhados e proteção do trecho interno, ainda sem escolha entre HTTP e HTTPS nesse trecho. Preservar cookies Secure e controles de acesso; a configuração existente não foi inspecionada.

Após inspeção estática dos arquivos locais de SupriTI, o autor escolheu seguir seu modelo: HTTPS no Traefik e HTTP em rede interna restrita até o proxy do projeto, substituindo a preferência inicial por HTTPS também internamente. Não foi verificada produção. O trecho HTTP não protege contra observação ou manipulação do tráfego interno; detalhar isolamento, acesso restrito ao proxy e confiança nos cabeçalhos encaminhados. Não estender essa escolha automaticamente a outras integrações.

O autor informou que o Traefik está em outra VM. O trecho HTTP até o proxy do projeto atravessa a rede entre VMs; seu isolamento ainda precisa ser confirmado. Verificar segmentação e restrição de entrada ao Traefik, sem tratar essas restrições como criptografia do caminho. A escolha por HTTP interno permanece vigente.

O autor confirmou uma VLAN exclusiva de servidores entre as VMs do Traefik e do projeto. Registrar essa segmentação como redução de exposição, sem criptografia ou comprovação de isolamento de todos os servidores. Manter HTTP interno conforme escolha vigente; confirmar regras de entrada restritas ao Traefik e proteção de APIs e banco antes de finalizar a implantação.

O autor aprovou firewall permitindo acesso à porta do proxy do projeto somente a partir da VM do Traefik, negando as demais origens. Registrar como requisito de implantação ainda não aplicado ou verificado. Confirmar o endereço efetivo de origem e o comportamento da publicação de portas dos containers; essa restrição não substitui autenticação nem criptografia.

## Organização dos schemas PostgreSQL — 2026-11-12

O autor aprovou as definições de banco e implantação em 2026-11-12, incluindo schemas por serviço, migrações na inicialização, atualização manual sem pipeline, construção local e Docker Compose, e autorizou continuar o plano técnico. As pendências explicitamente registradas continuam abertas; não houve autorização de implementação.

O autor escolheu um schema por serviço no banco compartilhado: `atendimento`, `materiais` e `inventario`, com credenciais próprias e permissões restritas. Preservar acesso a dados alheios somente pelas APIs, sem consultas diretas entre schemas. Detalhar permissões dos objetos existentes e futuros; isolamento por schema não elimina a dependência comum da instância PostgreSQL. Credenciais e coordenação das migrações permanecem por definir.

O autor substituiu a etapa separada de migração por migrações automáticas na inicialização de cada serviço, antes de atender requisições, somente em seu próprio schema. Falhas impedem a inicialização; coordenar execuções concorrentes e preservar compatibilidade com versões em uso. Voltar o código não desfaz o banco. Ferramenta, credenciais e recuperação permanecem por definir.

O autor retirou o pipeline automatizado e escolheu simplificar a atualização para operação manual dos containers, com migrações na inicialização. Não exigir plataforma ou executor de pipeline. Definir procedimento curto de atualização e verificação, ferramenta de execução e entrega das imagens, mantendo atualização por serviço.

O autor escolheu construir as imagens na própria VM Debian a partir do código, durante atualizações manuais, sem exigir registro externo de imagens. Construção Rust e da interface compartilham o orçamento total de 4 GB de RAM, 4 vCPUs e 100 GB com os serviços e o banco; verificar consumo e detalhar paralelismo e cache. Preservar a versão em uso se a construção falhar. A ferramenta de containers permanece aberta.

O autor escolheu Docker Compose para construir e executar os containers na VM Debian, seguindo o modelo operacional do SupriTI. Preservar atualização manual, construção local e migrações na inicialização por serviço. Detalhar serviços, redes, volumes, limites de recursos e verificações de saúde, sem expor banco ou backends por inferência. A entrada pelo proxy permanece restrita à VM do Traefik.

## Armazenamento de anexos — 2026-11-12

O autor aprovou em 2026-11-12 as definições de anexos e retenção registradas abaixo e autorizou continuar o plano técnico. As especificações técnicas explicitamente pendentes continuam abertas; não houve autorização de implementação.

O autor escolheu volume persistente na própria VM para anexos, integrado ao Docker Compose e com acesso controlado pelo sistema. Informou possibilidade de ampliar o armazenamento futuramente; manter 100 GB como capacidade inicial total até confirmação de expansão. Definir limites de upload, acompanhamento do disco e backup fora da VM, com consistência entre arquivos e banco. Persistência do volume não substitui backup.

O autor escolheu limites separados de anexos: na abertura pública, até 3 imagens de no máximo 5 MB cada, com anexo opcional e até 15 MB de arquivos. Esse limite não se aplica ao tamanho total do chamado. Validar no servidor e compatibilizar o limite de requisição do proxy com arquivos, campos e metadados. Formatos, tratamento de conteúdo, controles de abuso e limites dos envios posteriores permanecem abertos.

O autor definiu imagens, PDF, CSV e Excel como tipos iniciais de anexos posteriores dos técnicos, ainda sem limites quantitativos ou formatos concretos de imagem e Excel. Acrescentou vídeo opcional à abertura pública, além das até 3 imagens de 5 MB cada. Os 15 MB referem-se somente às imagens, não mais ao total da abertura com vídeo. Quantidade, tamanho, duração, formatos de vídeo e combinação com imagens permanecem por definir; não presumir conversão automática ou vídeo nos anexos posteriores.

O autor definiu até 1 vídeo de 30 MB na abertura pública, além das até 3 imagens de 5 MB cada, permitindo combinação e até 45 MB de anexos por abertura. Os anexos permanecem opcionais e esse total não limita o chamado inteiro. Validar no servidor e compatibilizar limites HTTP do Traefik e do proxy com arquivos, campos e metadados. Formatos de vídeo, eventual duração máxima e tratamento continuam abertos.

O autor definiu até 5 arquivos de 10 MB cada por envio posterior dos técnicos, para imagens, PDF, CSV e Excel, com até 50 MB de arquivos por envio. O limite é separado da abertura pública e não limita o total acumulado do chamado. Não presumir vídeo nos envios posteriores. Formatos concretos, tratamento de conteúdo e eventual limite acumulado permanecem abertos.

O autor escolheu não limitar o total acumulado de anexos por chamado, mantendo os limites de cada envio e os tipos autorizados. Acompanhar o uso total do disco e detalhar falhas por falta de espaço, sem declarar disponível arquivo não persistido nem presumir exclusão automática de anexos históricos. A capacidade inicial da VM permanece finita; retenção e proteção contra abuso continuam abertas.

O autor definiu JPEG, PNG e WebP como formatos de imagem permitidos na abertura pública e nos anexos posteriores dos técnicos, respeitando os limites de cada envio. Validar conteúdo real, sem depender apenas da extensão ou do tipo declarado; não presumir conversão ou compressão automática.

O autor definiu XLSX e XLS como formatos Excel permitidos nos anexos posteriores dos técnicos, preservando os limites por envio e sem incluir XLSM. XLS pode conter macros; não presumir ausência de conteúdo ativo pela extensão. O recebimento não deve executar macros ou fórmulas. Detalhar validação do formato real e política de conteúdo ativo.

O autor definiu MP4 e MOV para o vídeo opcional na abertura pública, limitado a 1 arquivo de 30 MB. Não incluir outros formatos ou vídeo posterior por inferência. Validar conteúdo real; o contêiner não garante reprodução de todos os codecs no navegador. Visualização, codecs e eventual conversão permanecem abertos.

O autor definiu que o vídeo da abertura não terá duração máxima: aplicar somente o limite de 1 arquivo MP4 ou MOV de 30 MB. Essa decisão resolve a duração anteriormente aberta e não exige conversão automática.

O autor escolheu visualização de imagens e tentativa de reprodução dos vídeos no navegador, com download disponível inclusive quando o vídeo não puder ser reproduzido. Não exigir conversão automática. PDF, CSV, XLSX e XLS ficam para download, sem visualizador embutido. Controlar acesso no servidor para visualização e download; envio público não concede consulta anônima aos anexos ou ao histórico.

O autor escolheu exclusão de anexos após prazo configurável do encerramento, com preservação por tempo indeterminado de arquivos considerados importantes pelo técnico como evidência histórica. Registrar remoções e alterações da marca de preservação com autoria e data; arquivos preservados não são excluídos automaticamente pelo prazo. Definir permissões, prazo inicial, efeito de reabertura, concorrência e tratamento dos backups, sem presumir exclusão imediata das cópias de recuperação.

O autor definiu que qualquer técnico com acesso ao chamado pode marcar ou desmarcar a preservação indeterminada de anexos, sem exigir participação ou privilégio administrativo adicional. Registrar autoria, data e hora da alteração e validar acesso no servidor. Ainda definir a exclusão de arquivo desmarcado após vencimento do prazo; desmarcar não restaura arquivos removidos.

O autor escolheu 90 dias após o encerramento como prazo padrão configurável para excluir anexos não preservados, mantendo o registro da remoção no histórico. Permissões de configuração, efeito sobre chamados já encerrados e reabertura permanecem por definir.

O autor definiu que a reabertura antes da exclusão suspende a remoção automática por prazo enquanto o chamado estiver aberto e reinicia a contagem no próximo encerramento. Preservar as marcas de permanência indeterminada e não recuperar por inferência arquivos já removidos. Verificar o estado atual do chamado no processamento da exclusão.

O autor definiu que retirar a preservação de um anexo com prazo vencido o torna elegível para a próxima limpeza, com aviso explícito antes de confirmar a desmarcação, sem reiniciar o prazo. Exigir chamado encerrado e prazo vigente vencido; em chamado aberto, manter suspensão até o próximo encerramento. Registrar desmarcação e exclusão separadamente e conferir as condições atuais antes da remoção.

O autor definiu que somente administradores podem alterar o prazo de retenção dos anexos, com padrão de 90 dias, mantendo a marcação de preservação disponível a qualquer técnico com acesso ao chamado. Ainda definir aplicação de mudanças do prazo a chamados já encerrados.

O autor definiu aplicar mudanças do prazo de retenção a todos os chamados encerrados, recalculando o vencimento desde o último encerramento com o prazo vigente. Reduções podem tornar anexos não preservados elegíveis para a próxima limpeza; aumentos não recuperam arquivos removidos. Preservar as exceções de chamado aberto e arquivo marcado por tempo indeterminado. Informar o efeito de reduções ao administrador antes de salvar e registrar autoria e valores anterior e novo da configuração.

O autor escolheu limpeza automática de anexos vencidos uma vez por dia, verificando chamado encerrado, prazo vigente e ausência de preservação antes de remover. Registrar remoções confirmadas e detalhar horário, recuperação de falhas e coordenação entre arquivos e banco. A limpeza não implica exclusão instantânea no vencimento ou remoção de cópias de backup.

## Backup externo — 2026-11-12

O autor aprovou em 2026-11-12 as definições de backup, considerando a delimitação posterior de geração e download locais, envio externo fora do sistema, agenda e retenção escolhidas, acesso administrativo e pausa de alterações. Especificações técnicas pendentes continuam abertas; não houve autorização de implementação.

O autor escolheu serviço de armazenamento externo à VM para backups, cobrindo banco e anexos de forma consistente. Identificar serviço, acesso, proteção, periodicidade, retenção, metas de recuperação e verificação de restauração. Exclusão de anexos na aplicação não remove automaticamente cópias de backup. Não houve autorização para contratação ou transferência de dados nesta etapa.

O autor informou que existe conta de nuvem utilizada para arquivos de backup e aceitou envio manual nesta etapa, sem integração automática ou contratação nova. Gerar cópia consistente de banco e anexos e conferir a transferência; arquivo somente na VM não constitui backup externo concluído. Frequência, responsabilidade, retenção, proteção e verificação de restauração permanecem abertas.

O autor confirmou geração e retenção semelhantes ao SupriTI examinado: backup completo automático diário; aos domingos, copiar o backup concluído do dia como semanal; manter os últimos 7 diários e 4 semanais, excluindo excedentes por categoria. Não combinar os diários ou apagá-los todos ao gerar o semanal. O envio à nuvem continua manual; a retenção na nuvem permanece aberta. Definir horário, fuso, consistência entre banco e anexos, falhas e verificação de recuperação; não presumir a configuração UTC do código de SupriTI como escolha deste projeto.

O autor definiu geração diária dos backups às 02h no fuso `America/Sao_Paulo`, incluindo a identificação do domingo para a cópia semanal. Não herdar a contagem em UTC do código de SupriTI nem presumir configuração de horário pelo painel.

O autor delimitou a responsabilidade do sistema à geração local dos backups e sua disponibilização para download. Envio à nuvem, frequência desse envio, confirmação de transferência e retenção externa ficam na operação manual da equipe, fora do fluxo do sistema. Preservar agenda e retenção locais já escolhidas e não declarar backup externo concluído apenas pela geração ou pelo download.

O autor definiu download dos backups somente para administradores, com verificação de sessão e autorização no servidor, sem arquivos publicamente acessíveis. Registrar o download no histórico administrativo sem expor conteúdo ou credenciais.

O autor escolheu pausar temporariamente alterações durante o backup, mantendo consultas. Coordenar gravações dos três serviços, anexos e tarefas automáticas antes da cópia, com liberação segura após conclusão ou falha. Detalhar sessões, operações em andamento e retomada das integrações: pausar localmente não desfaz efeitos já enviados ao SupriTI nem interrompe o Wazuh. Não confundir botões desabilitados com pausa efetiva no servidor. O mecanismo técnico permanece por definir.

## Proteção da abertura pública — 2026-11-12

O autor aprovou em 2026-11-12 as regras vigentes: somente limite por IP, sem CAPTCHA, com 10 chamados criados e 30 tentativas inválidas em contadores separados e janelas móveis de 10 minutos. Autorizou continuar o plano técnico, sem implementação. As especificações técnicas explicitamente pendentes continuam abertas.

**Decisão vigente:** o autor retirou o CAPTCHA do escopo atual e manteve somente o limite por IP. Essa definição substitui as escolhas anteriores de CAPTCHA externo e Cloudflare Turnstile registradas abaixo. Não exigir integração ou credenciais de provedor nesta etapa. Definir quantidade e janela do limite considerando IP compartilhado e origem validada pela cadeia de proxies confiáveis.

O autor escolheu limite de frequência por IP e CAPTCHA, validados no servidor, para a abertura pública. Considerar IP compartilhado por funcionários e identificar origem apenas por proxies confiáveis. Definir mecanismo de CAPTCHA, limites, janela, falhas e momento de controle dos uploads; a proteção não comprova identidade do solicitante.

O autor escolheu CAPTCHA por serviço externo, com validação pelo backend e segredo fora do navegador. Provedor, configuração, dados envolvidos, acessibilidade e tratamento de indisponibilidade permanecem abertos. Não houve autorização de contratação.

O autor escolheu Cloudflare Turnstile como serviço externo de verificação da abertura pública, com token validado pelo backend de Atendimento, segredo fora do navegador e limite por IP complementar. Não exigir alteração do DNS ou substituição do Traefik. Configuração, testes, falhas e valores dos limites permanecem por detalhar; nenhuma conta ou chave foi criada.

O autor escolheu até 10 chamados por IP a cada 10 minutos na abertura pública, sem CAPTCHA. IPs compartilhados compartilham o limite; verificar origem por proxies confiáveis e aplicar no servidor. Ao recusar, informar restrição e preservar o preenchimento na página, sem criar chamado. Detalhar janela, contagem de tentativas inválidas, concorrência e proteção dos uploads.

O autor escolheu janela móvel: cada abertura considera os chamados criados pelo IP nos 10 minutos anteriores, com no máximo 10 no intervalo, sem reinício por blocos de relógio. Coordenar criações concorrentes e informar a espera sem expor chamados de outros usuários do mesmo IP. Proteção de tentativas inválidas e uploads permanece por definir.

O autor definiu que somente chamados criados consomem a cota de 10 por IP em janela móvel de 10 minutos. Falhas de validação de campos ou arquivos não consomem essa cota. Definir separadamente a frequência de tentativas e proteção dos uploads, sem transformar envios inválidos em chamados.

O autor definiu limite separado de 30 tentativas inválidas por IP em janela móvel de 10 minutos, por erros de campos ou arquivos. Não incluir chamados criados nessa cota, nem inválidos na cota de 10 chamados. Ao atingir o limite de inválidos, restringir novos envios temporariamente, informando espera e preservando preenchimento. Falhas do sistema e recusas pelo próprio limite não são novos erros de validação. Detalhar uploads interrompidos e controle do recebimento antes da validação.

## Configurador visual e triagem guiada — 2026-11-12

O autor esclareceu que o configurador visual já previsto deve criar perguntas simples e caminhos condicionais para orientar a triagem, sem exigir conhecimento de categorias técnicas do solicitante. As escolhas podem sugerir classificação, mas o técnico define ou corrige a categoria efetiva. Preservar respostas e versão originais; respostas sobre alcance do problema são indícios, não diagnóstico comprovado.

O autor substituiu a proposta de uso híbrido por dois tipos no mesmo configurador: **uso externo**, para o solicitante, e **uso interno**, para os grupos da equipe. Não oferecer formulário de uso mútuo nesta etapa nem expor formulários internos na abertura pública. Preservar respostas e versões de cada formulário separadamente. Associação a grupos, mapeamento de caminhos para sugestões e efeito da reclassificação sobre campos técnicos permanecem por definir; manter os campos básicos da abertura já aprovados.

O autor definiu que um formulário interno pode ser associado a vários grupos, compartilhando a mesma configuração sem cópias por grupo. A associação controla quem pode usar esse formulário para abrir um chamado, não a atribuição do atendimento nem a consulta das respostas. Validar o grupo no servidor na abertura; após a criação, aplicar as permissões de acesso ao chamado. Preservar a versão usada e não conceder acesso a chamados pela associação do formulário.

## Cenários para validar o desenho futuro

- Novo chamado permanece visível na triagem apesar do filtro pessoal; abrir detalhes não o assume; assumir aplica média quando necessário e o retira da triagem.
- Mudança no formulário não altera respostas anteriores; campos condicionais e obrigatoriedade funcionam na versão do chamado.
- Pedido dentro dos sete dias pode ser aceito depois; complementos não notificam; cancelamento remove seu aviso; ações concorrentes produzem uma única decisão.
- Fechamento sem materiais não cria consumo; revisão desatualizada não fecha nem movimenta estoque.
- Falha ou resposta perdida na reserva, baixa ou liberação pode ser recuperada sem duplicidade, inclusive após reinício e sucesso parcial.
- Cancelamento com três peças, uma mantida e duas devolvidas, trata cada quantidade conforme a movimentação real e aceita justificativa conjunta.
- Desativação de técnico preserva autorização histórica, exige substituição nos chamados abertos e permite suspender somente as operações escolhidas.
- Produto com reserva não é desativado; produto desativado antes da sincronização retorna divergência identificável.
- Ausência de coleta Wazuh não gera conclusão falsa; a referência necessária à comparação não é apagada para economizar armazenamento.

Estes são critérios para desenho e testes futuros, não verificações já executadas.
