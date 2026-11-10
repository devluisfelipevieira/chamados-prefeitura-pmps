# Sistema de Chamados da Prefeitura — conceitos consolidados

- **Autor:** Luis Felipe Vieira
- **Data:** 2026-11-10
- **Situação:** brief funcional para revisão do autor e preparação do desenho técnico. Não houve revisão Goldfish deste documento. Não constitui autorização para implementação.
- **Idioma:** português, conforme a continuidade do planejamento solicitada pelo autor.

## Visão geral

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
