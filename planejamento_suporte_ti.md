# Planejamento do sistema interno de suporte de TI

Documento de continuidade para aplicação da metodologia Elephant Goldfish

Data de consolidação: 4 de novembro de 2026. Versão deste documento: 1.0. Situação: planejamento funcional em andamento, sem autorização implícita para implementação ou alteração dos sistemas existentes.

## 1 Finalidade e limites da consolidação

Este documento reúne o planejamento recuperado do novo sistema interno de chamados, ativos, auditoria de hardware com Wazuh e integração de materiais com SupriTI. Seu objetivo é permitir que outro chat continue o trabalho sem perder decisões e sem transformar recomendações em requisitos aprovados.

A fonte é a conversa “Elaborar sistema de suporte TI”, identificador 6ae34e37-afa0-83e9-9af3-6582e58c89a6, e o pedido atual do usuário. A consulta retornou cinco etapas, incluindo a versão 0.5, o encerramento estruturado da versão 0.6, a integração da versão 0.7, as propostas da versão 0.8 e a última correção sobre SupriTI e revisão final. A ferramenta informou não haver páginas adicionais. As etapas anteriores à versão 0.5 não foram recuperadas; a consolidação não comprova integralidade de todo o histórico original.

Os exemplos de números de chamados, patrimônios, produtos, quantidades e datas apresentados na conversa eram demonstrativos. Não representam cadastros reais. Não houve inspeção do código, banco, API, configuração ou permissões do SupriTI ou do Wazuh.

Neste documento, “confirmado” identifica escolha expressa do usuário; “aceito no contexto” identifica conteúdo da etapa de encerramento abrangido por “o restante estou de acordo completamente”; “proposta” identifica recomendação do assistente sem aprovação inequívoca; “pendente” identifica decisão ou evidência ainda necessária. As decisões mais recentes prevalecem sobre alternativas anteriores.

## 2 Visão do sistema e fronteiras

O sistema pretende reunir atendimento de TI, histórico técnico dos equipamentos, acompanhamento de alterações de hardware e acesso ao estoque de componentes usados nas manutenções. O contexto recuperado menciona uma Prefeitura, secretarias, equipe de TI e a intenção de evitar burocracia percebida no GLPI. Não foram recuperados o nome oficial da organização, dimensão do parque ou requisitos completos de substituição e migração do GLPI.

O novo sistema registra chamados, intervenções, responsáveis, vínculos com equipamentos e referências às operações externas. O SupriTI permanece responsável pelo estoque dos componentes sem patrimônio, incluindo saldo, reserva e movimentações. O Wazuh fornece informações de inventário para comparação. O novo sistema não deve manter uma réplica integral dos logs do Wazuh nem um estoque paralelo independente do SupriTI.

Há três registros relacionados, mas distintos, em uma troca de componente: a intervenção no chamado, o consumo do material no SupriTI e a mudança de inventário observada pelo Wazuh. Uma alteração detectada não comprova, por si só, autorização, consumo ou responsabilidade. Esses vínculos precisam de evidências e classificação.

Movimentação patrimonial e consumo de materiais são processos diferentes. Transferência entre secretarias, envio e retorno de manutenção e baixa patrimonial foram citados como contexto de ativos, mas seus formulários, aprovações e regras não estão detalhados na fonte recuperada.

## 3 Decisões confirmadas

- D01 — Permissões dinâmicas por grupos. O administrador master deve poder conceder e retirar permissões conforme os grupos; N1, N2 e N3 não devem determinar privilégios imutáveis no código.
- D02 — Comparação de hardware no final do dia, com base nas coletas do Wazuh. O usuário propôs comparar a última coleta do dia anterior com a última do dia atual, considerando que os computadores costumam ficar desligados fora do expediente.
- D03 — Economia de armazenamento. Não guardar todo o histórico bruto desses logs no novo sistema; descartar dados sem alteração após análise, preservando o que for necessário para continuar comparando. O ajuste para dias sem coleta é uma proposta descrita adiante.
- D04 — Distribuição híbrida de chamados e possibilidade de abertura pelos próprios técnicos, inclusive ocorrências internas e serviços já executados que precisam ser documentados.
- D05 — Encerramento com registro estruturado e flexível. O técnico registra a solução e o chamado passa a Resolvido sem depender de confirmação prévia do funcionário.
- D06 — Reabertura em até 7 dias após a resolução. A forma exata de contagem ainda precisa ser especificada.
- D07 — Consulta ao estoque SupriTI, lista de materiais por nome e indicação dos itens e quantidades utilizados. A movimentação deve ser entendida como “Uso do setor de Tecnologia” e referenciar o chamado.
- D08 — Selecionar material durante o atendimento deve solicitar sua reserva no SupriTI. A baixa definitiva ocorre somente na finalização do chamado.
- D09 — A integração deve respeitar as regras atuais do SupriTI. Segundo o usuário, quem solicita não pode confirmar e quem confirma não pode solicitar. Essa segregação deve ser preservada e investigada antes de definir o fluxo técnico.
- D10 — Estudar identidade ou tipo de conta específico para integração. Sua existência, viabilidade e permissões não estão aprovadas nem podem ser presumidas.
- D11 — O técnico deve remover manualmente os materiais desnecessários antes de resolver. Não adotar como decisão vigente a liberação automática dos itens não utilizados na revisão final.
- D12 — Antes da finalização, apresentar resumo para confirmar diagnóstico, solução e recursos utilizados. Confirmar inicia a resolução e o pedido de baixa; cancelar retorna ao chamado com os dados preservados.

## 4 Usuários e permissões

O funcionário abre solicitações e acompanha seu atendimento. O técnico também pode abrir chamados, assumir atendimentos e registrar ações conforme suas permissões. O administrador master administra grupos e acessos. N1, N2 e N3 são referências iniciais de organização da equipe, sujeitas à configuração dinâmica.

Foi proposto separar usuário, grupo e permissão, com grupos adicionais como Auditoria, Patrimônio e Somente leitura. Também foram propostos auditoria das mudanças de acesso, proteção do papel master, concessão de master somente por outro master e impedimento de remoção do último administrador master. São recomendações a validar, não regras confirmadas individualmente.

O detalhamento de permissões ainda precisa cobrir visualizar, criar, assumir, redistribuir, escalar, resolver e reabrir chamados; consultar e alterar ativos; classificar eventos de hardware; gerir artigos; consultar estoque; solicitar e cancelar reservas; acompanhar pendências. Ter permissão no novo sistema não implica possuir a permissão correspondente no SupriTI.

Na distribuição híbrida descrita, técnicos autorizados visualizam a fila e assumem atendimentos; redistribuição e escalonamento dependem de permissões. O histórico deve preservar responsáveis anteriores. A atribuição inicial de redistribuição a N3 foi proposta, não constitui privilégio fixo aprovado.

## 5 Fluxo dos chamados

### 5.1 Abertura

O funcionário informa o problema no portal, com título, descrição e vínculo com equipamento quando conhecido. A etapa recuperada descreve sugestão de patrimônio com confirmação, mas o mecanismo de identificação não foi especificado. O técnico pode abrir em nome de um funcionário ou registrar ocorrência interna identificada pela equipe.

Separar a pessoa que criou o registro do solicitante ou beneficiário foi uma proposta para evitar atribuir ao funcionário uma abertura realizada pelo técnico. Origens sugeridas: portal do funcionário, atendimento recebido por técnico, ocorrência interna e monitoramento Wazuh. A conversão de alerta Wazuh em chamado não deve ser interpretada como criação automática já aprovada.

Atendimentos por telefone, presenciais, preventivos, problemas percebidos em visitas e serviços já realizados são exemplos do uso da abertura por técnicos. A política de registro retroativo e seus campos de data permanecem pendentes.

### 5.2 Atendimento e distribuição

O chamado entra na fila da equipe. A descrição recuperada prevê notificação visual e sonora, visualização por técnicos autorizados e possibilidade de assumir o atendimento. O painel dos técnicos foi anunciado como próxima etapa; não existe desenho final aprovado na fonte.

Durante o atendimento, registrar intervenções, diagnóstico, anexos, troca de responsável e materiais quando aplicável. Escalonar entre níveis sem criar outro chamado é uma proposta compatível com o histórico único; regras e notificações ainda precisam ser fechadas.

### 5.3 Campos do encerramento

Os campos abaixo foram apresentados na versão 0.6, seguida da manifestação do usuário de concordância com o restante. São tratados como aceitos no contexto dessa etapa, sujeitos às correções posteriores.

- Título e descrição do problema: obrigatórios na abertura.
- Solicitante ou origem interna: obrigatório na abertura.
- Equipamento: opcional na abertura; confirmar o vínculo na resolução quando aplicável. Foi admitida justificativa quando o equipamento não puder ser identificado.
- Categoria: sugerida ou opcional na abertura; obrigatória na resolução.
- Técnico responsável e solução aplicada: obrigatórios para resolver.
- Diagnóstico técnico: opcional. Aparecer na tela de resumo não o torna obrigatório; quando ausente, indicar que não foi informado.
- Peças substituídas e materiais: preencher quando houver uso ou alteração de componente.
- Imagens, anexos e artigo técnico utilizado: opcionais.

Hardware, software, rede e conectividade, acesso e permissões, impressora e outros foram categorias ilustrativas. A taxonomia definitiva permanece pendente. O registro deve explicar o procedimento realizado, mantendo flexibilidade para intervenções simples.

### 5.4 Revisão e resolução

1. O técnico conclui o registro de diagnóstico, solução, equipamento e materiais.
2. Remove manualmente da lista os itens que não utilizou. A remoção deve acionar o tratamento de liberação de reserva compatível com o SupriTI; a confirmação dessa liberação precisa ser acompanhada.
3. Aciona a opção de resolver. O sistema valida os campos exigidos e apresenta o resumo do chamado, incluindo diagnóstico, solução, equipamento, materiais e quantidades.
4. Se o técnico cancelar a confirmação, retorna ao chamado com os dados preservados. Cancelar essa tela não é cancelar o chamado nem desfazer movimentações anteriores.
5. Se confirmar, inicia a resolução e a solicitação de baixa dos materiais efetivamente listados, respeitando autorizações e regras do SupriTI.
6. A interface deve distinguir resultado técnico e resultado da integração. O comportamento exato diante de baixa pendente ainda exige decisão.

A revisão não libera automaticamente itens considerados não usados. O técnico é responsável por corrigir a lista antes de confirmar. Se uma reserva não puder ser liberada, não declarar que a liberação ocorreu; definir se a pendência impede resolver ou segue para regularização é parte do desenho pendente.

### 5.5 Reabertura e novas ocorrências

O funcionário poderá reabrir em até 7 dias após a resolução quando o problema persistir. A solução anterior e as intervenções devem permanecer no histórico. Depois do prazo, a conversa descreve novo chamado ligado ao equipamento e à ocorrência anterior.

Reabertura não deve desfazer automaticamente consumo de materiais ou movimentação patrimonial já realizada. Esse cuidado foi explicitado na versão 0.7 e deve ser validado no desenho de transações. Novos materiais de uma nova intervenção precisam ser distinguidos dos consumos já confirmados para evitar repetição.

Permanecem pendentes: dias corridos ou úteis, horário de corte, reinício da janela após nova resolução, permissões de reabertura, justificativa obrigatória e eventual exceção fora do prazo. Configuração administrativa do prazo e reabertura excepcional por técnico foram sugestões anteriores, não substituem a decisão de 7 dias.

## 6 Ativos e histórico técnico

O vínculo entre chamado e equipamento permite consultar atendimentos e intervenções anteriores. O técnico confirma o equipamento ao resolver quando a solicitação se refere a um ativo; um patrimônio sugerido pelo usuário não deve ser considerado automaticamente validado.

A linha do tempo proposta reúne chamados, manutenções, substituições de peças, alterações de hardware detectadas e movimentações patrimoniais. Os registros permanecem nos módulos de origem, e o histórico integrado apresenta suas referências. Preservação do histórico por equipamento foi aceita no contexto do encerramento.

Ainda faltam os campos oficiais dos ativos, unicidade e formato do patrimônio, tratamento de ativos sem agente Wazuh, associação entre patrimônio e identificador do agente, equipamentos compartilhados e mudanças de agente. Também faltam os ciclos de transferência, manutenção externa, retorno e baixa patrimonial. O exemplo de patrimônio usado na conversa não define um padrão oficial.

## 7 Wazuh e comparação diária de hardware

### 7.1 Estratégia confirmada e ajuste proposto

A comparação deve ocorrer ao final do dia usando a última coleta disponível do computador no dia atual e a referência anterior. O objetivo é aproveitar o horário em que as máquinas ficam ligadas. O usuário pediu retenção limitada, com descarte dos registros sem alteração após análise.

O assistente propôs um ajuste para máquinas desligadas por um ou mais dias: comparar o último inventário válido anterior com uma coleta nova, em vez de exigir uma coleta do dia imediatamente anterior. Sem esse ajuste, uma máquina ausente na terça poderia voltar na quarta com alterações que não seriam comparadas corretamente. O ajuste é recomendado, mas não foi aprovado expressamente na etapa seguinte.

O horário de 18h foi apenas exemplo. Deve ser definido conforme o expediente e o fuso da organização. Não existe agendamento implementado.

### 7.2 Fluxo proposto para especificação

1. Identificar os equipamentos e consultar a última coleta pertinente no Wazuh.
2. Verificar se a coleta é nova e válida. Sem nova coleta, manter a referência anterior e sinalizar ausência de atualização, sem concluir que não houve mudança.
3. Na primeira coleta, estabelecer referência inicial; o tratamento e eventual revisão dessa primeira referência precisam ser definidos.
4. Comparar os campos de hardware selecionados com a referência válida.
5. Sem diferenças relevantes, atualizar a referência e descartar temporários e relatório detalhado sem mudanças.
6. Com diferenças, registrar evento com equipamento, data da coleta, valores anterior e atual e evidência mínima; encaminhar para classificação técnica.
7. Manter rastreabilidade mínima da execução e dos erros para não confundir falha da rotina com ausência de alterações.

Guardar somente o último inventário de referência, diferenças relevantes e classificação técnica foi a proposta para atender à economia de disco. Não apagar a única referência necessária à próxima comparação. Logs completos existentes no Wazuh seguem a política do próprio Wazuh, ainda não examinada.

### 7.3 Ressalvas

Coletas ausentes, incompletas ou antigas não comprovam retirada de peças. É necessário definir critérios de validade, componentes comparados, normalização de valores, tolerâncias, deduplicação de eventos e classificação de alterações autorizadas ou não explicadas. Comparar somente o último estado diário pode deixar de revelar alterações transitórias ocorridas e revertidas no mesmo dia; esse limite deve ser aceito ou revisto.

Não estão definidos os endpoints, versões, frequência de coleta, forma de autenticação ou retenção quantitativa. A integração não deve presumir capacidade específica da instalação atual. A associação entre um evento de hardware e um chamado não pode basear-se apenas em proximidade de datas sem validação.

## 8 Integração com SupriTI

### 8.1 Regras de negócio e dependências

O SupriTI controla os componentes sem patrimônio individual. A interface do chamado deve consultar o estoque e apresentar nomes dos itens, saldo e quantidade a utilizar. Identificadores internos continuam necessários para distinguir materiais com nomes semelhantes, embora o técnico não precise conhecer códigos.

Ao selecionar item e quantidade durante o atendimento, o sistema solicita reserva ao SupriTI. Somente uma confirmação do SupriTI permite afirmar que o material está reservado. Seleção local ou solicitação enviada não garantem disponibilidade. O saldo deve ser validado na operação de estoque, e não apenas na consulta inicial.

A baixa definitiva deve ser solicitada na finalização, como “Uso do setor de Tecnologia”, com observação identificando o chamado. É preciso verificar se essa expressão corresponde a um tipo de movimentação existente, como ele funciona e quais confirmações exige. Não criar um novo significado de movimentação sem análise.

### 8.2 Segregação de funções e conta de integração

Segundo o usuário, no SupriTI quem pede não pode confirmar e quem pode confirmar não pode solicitar. Essa é uma restrição central do planejamento. Uma conta técnica não pode ser presumida como exceção nem como autorização para executar ambas as etapas.

Antes da arquitetura definitiva, examinar o fluxo atual de solicitação, reserva, retirada, confirmação e consumo; os papéis envolvidos; o significado exato do status Reservado; e as trilhas de auditoria. Verificar se reserva representa separação efetiva de saldo, se há expiração e quais operações podem cancelar ou alterar quantidades.

Estudar uma identidade específica de integração com permissões limitadas e operações rastreáveis. As alternativas podem envolver identidade técnica restrita, etapas com confirmação por pessoas autorizadas ou extensão formal do SupriTI. Nenhuma alternativa foi selecionada. Não presumir conta administrativa, acumulação de papéis ou dispensa de aprovação.

### 8.3 Fluxo funcional vigente

1. Técnico autorizado consulta materiais do SupriTI no contexto do chamado.
2. Seleciona material por nome e informa quantidade.
3. O novo sistema solicita reserva segundo o fluxo permitido no SupriTI.
4. A resposta é registrada e apresentada como confirmada, pendente ou recusada, conforme o resultado real.
5. Durante a intervenção, o técnico ajusta a lista. Alteração de quantidade e remoção exigem tratamento correspondente da reserva, ainda dependente das regras externas.
6. Antes de resolver, o técnico remove manualmente os materiais não utilizados e revisa diagnóstico, solução e materiais restantes.
7. Cancelar a revisão retorna ao chamado. Confirmar inicia resolução e solicitação de consumo das reservas, com referência ao chamado.
8. O novo sistema registra o resultado e o identificador externo, quando disponibilizado. Pendências não podem ser apresentadas como baixa concluída.

Este fluxo descreve a intenção funcional aprovada, não garante que o SupriTI consiga executá-la hoje sem etapas adicionais. A análise do sistema existente pode exigir ajustes no desenho, preservando a segregação.

### 8.4 Dados de integração propostos

Relacionar identificador do chamado, material no SupriTI, quantidade e unidade, reserva, operação, movimentação resultante, técnico, identidade de integração, datas, resultado e mensagem de erro. Quando aplicável, incluir equipamento ou patrimônio. Distinguir autor humano de executor técnico.

A observação da movimentação deve mencionar o chamado. Foram sugeridos equipamento e técnico como contexto adicional. O SupriTI deve idealmente retornar um identificador consultável para permitir reconciliação; essa capacidade ainda não foi comprovada.

Não foram definidos contrato de API, formatos, autenticação, unidades, precisão de quantidades ou limites. O assistente recomendou endpoints autenticados, caso necessários, e evitar alteração direta do banco do SupriTI pelo novo sistema. Essa é proposta de arquitetura a validar.

### 8.5 Falhas e sincronização

O usuário considerou útil o estado Baixa pendente e perguntou se a transação seria retomada automaticamente ou exigiria sincronização manual. O assistente recomendou retomada automática com fila persistente. Não há aprovação inequívoca de toda essa solução técnica; tratá-la como proposta prioritária de validação.

A proposta registra a intenção antes do envio, atribui identificador único, tenta executar, mantém pendências após falhas transitórias e repete com intervalos progressivos. Antes de repetir uma operação cujo resultado é desconhecido, consulta se ela já foi executada. Erros permanentes devem seguir para análise, sem repetição cega.

Idempotência significa que a repetição da mesma operação não produz nova baixa. Essa proteção depende de suporte real no SupriTI ou de mecanismo equivalente verificável. Um identificador gerado somente no novo sistema não garante proteção externa. Se não for possível comprovar o resultado de uma operação, manter a incerteza e exigir reconciliação; não prometer execução exatamente uma vez.

Estados locais sugeridos: Reserva pendente, Reservado, Reserva recusada, Baixa pendente, Consumido e Erro ou divergência. Eles não devem ser confundidos com estados oficialmente disponíveis no SupriTI. Também é necessário definir o acompanhamento de liberação pendente.

Resolver tecnicamente mantendo baixa pendente foi recomendado para não interromper atendimento em indisponibilidade externa. Registrar materiais durante falha de reserva também foi sugerido. Ambos exigem decisão explícita, principalmente quando não há reserva confirmada ou saldo suficiente. Nunca gerar uma baixa fictícia nem assumir saldo negativo autorizado.

### 8.6 Casos ainda sem regra fechada

Reserva antiga, expiração, mudança de quantidade, cancelamento do chamado, atendimento com material já instalado, perda de resposta, consumo parcial de vários itens e liberação recusada precisam de regras. A sugestão anterior de não expirar reservas automaticamente está subordinada ao funcionamento atual do SupriTI e não foi aprovada.

Materiais ausentes do cadastro, peças reaproveitadas, devolução de peças e estornos também precisam ser discutidos. Não apagar uma movimentação confirmada para corrigir um erro; desenhar correção autorizada com trilha de auditoria. A reabertura não é autorização de estorno nem deve reenviar automaticamente o consumo anterior.

## 9 Base de conhecimento

A proposta aceita no contexto do encerramento permite consultar artigos de problemas comuns e vincular o artigo utilizado à solução do chamado. Transformar uma solução em novo artigo foi sugerido para técnicos com permissão, inicialmente mencionando N2 e N3, sempre subordinado à configuração dinâmica.

Permanecem pendentes autoria, revisão, publicação, visibilidade, versionamento, busca e tratamento de informações sensíveis antes de reutilizar conteúdo de chamados. O nome “Problemas comuns” foi uma sugestão de interface, não um requisito visual definitivo.

## 10 Requisitos transversais e modelo conceitual

Como recomendações para o desenho, preservar histórico de mudanças de responsável, resolução, reabertura, alteração de dados e permissões; rastrear solicitações externas e resultados; distinguir permissão local de autorização externa; e apresentar mensagens que indiquem se a operação foi confirmada, recusada ou ficou sem resultado conhecido.

Entidades conceituais sugeridas: usuário, grupo, permissão, chamado, intervenção, resolução, equipamento, evento de hardware, referência de inventário, artigo técnico, material vinculado ao chamado e operação de integração. Identificadores de material, reserva e movimentação do SupriTI devem ser referências externas, não um segundo controle oficial de estoque.

Chamado e integração precisam de estados separados: um atendimento tecnicamente resolvido pode, caso essa política seja aprovada, ter baixa pendente. O catálogo completo de estados e transições não foi aprovado. “Em atendimento”, “Resolvido” e “Reaberto” aparecem no fluxo; cancelamento e fechamento definitivo exigem detalhamento.

Não foram decididos linguagem, framework, banco, hospedagem, login institucional, migração, backup, disponibilidade, desempenho, retenção dos chamados, limites de anexos ou metas de atendimento. Não atribuir valores ou tecnologias a esses requisitos como se já tivessem sido escolhidos.

## 11 Pendências priorizadas

- P01 — Inspecionar SupriTI com prioridade máxima: regras de reserva, segregação, operações disponíveis, confirmações, autorização e auditoria. Evidência esperada: mapa do fluxo atual e matriz de capacidades reais.
- P02 — Decidir a identidade de integração a partir de P01. Definir quem solicita, quem confirma, o que pode ser automatizado e como o técnico humano fica identificado.
- P03 — Validar o contrato de consulta, reserva, alteração, liberação, consumo e consulta de resultado. Comprovar prevenção de duplicidade antes de ativar repetição automática de baixas.
- P04 — Decidir o comportamento durante indisponibilidade: resolver com pendência, permitir uso sem reserva confirmada, retentativas, reconciliação, responsáveis e intervenção manual.
- P05 — Definir atomicidade e falhas parciais: resolução local sem pedido externo, baixa externa sem confirmação local e sucesso de apenas parte dos materiais.
- P06 — Especificar expiração, cancelamento e alteração de reservas conforme o SupriTI, incluindo remoção manual de item e liberação pendente.
- P07 — Validar o ajuste Wazuh para último inventário válido, frequência, horário, ausência de coleta, primeira referência, campos e retenção mínima.
- P08 — Definir associação confiável entre equipamento e agente Wazuh e o processo de classificação de eventos de hardware.
- P09 — Fechar contagem dos 7 dias, responsáveis pela reabertura e tratamento de múltiplos ciclos de resolução.
- P10 — Detalhar matriz dinâmica de permissões e proteção do master, evitando poderes fixos por nível.
- P11 — Desenhar o painel técnico: fila, responsáveis, prioridade, notificações, filtros e acesso ao histórico. Prioridades e metas ainda não foram definidas.
- P12 — Completar regras patrimoniais, base de conhecimento, anexos, segurança, migração e operação. Recuperar documentação anterior se existir para conferir lacunas desta consolidação.

## 12 Cenários de aceite para o próximo desenho

Os cenários abaixo traduzem decisões em verificações de comportamento. Não são testes executados nem evidência de implementação.

- A01 — Um técnico autorizado consegue abrir ocorrência interna, sem atribuir falsamente a criação a um funcionário.
- A02 — O master altera permissões de um grupo e o acesso acompanha a configuração, sem depender de privilégios fixos N1, N2 ou N3.
- A03 — A tentativa de resolver sem solução ou responsável é impedida conforme os campos aceitos no contexto.
- A04 — A revisão final apresenta diagnóstico, solução, equipamento e materiais com quantidades; diagnóstico opcional ausente não se torna impedimento por causa do resumo.
- A05 — Cancelar a revisão retorna ao chamado com dados preservados e não inicia baixa nem resolução por esse cancelamento.
- A06 — O técnico remove manualmente o item não usado antes de confirmar; a tela de revisão não faz essa escolha automaticamente.
- A07 — Selecionar material solicita reserva; nenhuma baixa definitiva ocorre apenas pela seleção. A reserva só é exibida como confirmada após resposta correspondente.
- A08 — Confirmar a revisão inicia a resolução e o pedido de consumo como “Uso do setor de Tecnologia”, com referência ao chamado e respeitando as etapas autorizadas no SupriTI.
- A09 — Uma identidade sem autorização de confirmação não consegue contornar a segregação por meio do novo sistema.
- A10 — O chamado pode ser reaberto dentro do prazo definido de 7 dias, preservando a solução anterior; os limites exatos dependem de P09.
- A11 — Se a repetição automática for aprovada, a perda da resposta após uma baixa efetivada não gera nova baixa; o mecanismo deve ser demonstrado em ambiente de teste.
- A12 — Na comparação diária, dados temporários sem alterações são descartados sem eliminar a referência necessária à próxima execução. O cenário de vários dias sem coleta depende da aprovação de P07.

## 13 Alternativas e ressalvas para continuidade

Permissões fixas por nível foram substituídas pela configuração dinâmica. Baixa imediata ao selecionar material foi superada pela reserva durante atendimento e baixa na finalização. Liberação automática dos não utilizados na revisão foi preterida pela remoção manual. Confirmação prévia do funcionário para resolver foi preterida pela resolução direta pelo técnico com janela de reabertura.

A coleta ou comparação de madrugada foi revista em favor do final do dia, considerando máquinas desligadas. Os detalhes anteriores desse agendamento não foram recuperados. Conta de integração com poderes para solicitar e confirmar não foi aprovada. Retentativa automática, resolução com baixa pendente e ausência de expiração de reserva são propostas, não alternativas definitivamente escolhidas.

O principal risco de implementação é desenhar o estoque apenas pelo comportamento desejado no novo sistema e violar o fluxo existente do SupriTI. Outros riscos são duplicar baixas após falhas de comunicação, consumir materiais de um ciclo anterior ao reabrir, perder a referência de hardware ao economizar disco e tratar ausência de coleta como alteração real.

## 14 Transferência para Elephant Goldfish

Este arquivo é um registro de contexto e decisões para iniciar a metodologia. Não é uma especificação técnica final, um relatório de revisão Goldfish nem uma aprovação para desenvolvimento. Nenhuma revisão por agentes independentes foi realizada nesta consolidação.

O próximo chat deve primeiro ler o documento inteiro, confirmar entendimento das decisões D01 a D12 e investigar P01 a P08 antes de fechar a arquitetura de integração. Lacunas devem permanecer explícitas. Se novas evidências exigirem alterar decisão funcional, apresentar a incompatibilidade e registrar a nova decisão do responsável.

Para o trabalho Elephant, manter um documento de referência com problema, objetivos, limites, decisões, alternativas e questões abertas. Para revisão Goldfish, fornecer o artefato autocontido a revisores em contextos independentes, seguindo as instruções da metodologia disponível no ambiente. Não apresentar uma leitura do mesmo chat como revisão independente.

Uma sequência sugerida é validar as fronteiras de estoque e identidade, fechar as máquinas de estados e os cenários de falha, consolidar os fluxos de chamado e hardware, redigir o desenho técnico e então submetê-lo à revisão. Essa sequência é recomendação de continuidade, não cronograma aprovado.

### Texto para iniciar o próximo chat

Leia o documento Planejamento do sistema interno de suporte de TI como registro de contexto. Continue o planejamento em português segundo Elephant Goldfish. Preserve as decisões confirmadas, diferencie recomendações de requisitos aprovados e não preencha lacunas com suposições silenciosas. Comece pela análise das regras atuais do SupriTI, especialmente reserva e segregação entre solicitação e confirmação. Estude a identidade de integração sem presumir permissões. Mantenha remoção manual dos materiais não usados e revisão final de diagnóstico, solução e materiais; cancelar volta ao chamado com dados preservados. Trate sincronização automática e comportamento com baixa pendente como propostas a validar. Antes de propor implementação, apresente as dependências e evidências necessárias. Este documento foi consolidado de cinco etapas recuperadas; o histórico anterior à versão 0.5 não foi disponibilizado.

## 15 Rastreabilidade das fontes

- Fonte F01 — Etapa da versão 0.5: mensagem do usuário sobre permissões dinâmicas, comparação diária e economia de logs, modelo híbrido e abertura por técnicos. Turno 27b85b63-6ce5-484a-90c9-50d7708e69dc.
- Fonte F02 — Escolha de encerramento estruturado e flexível e proposta de campos, histórico e base de conhecimento. Turno 21e3da55-5928-408a-a4fd-15307b244794.
- Fonte F03 — Integração SupriTI por nome de material, uso do setor de Tecnologia, referência ao chamado, reabertura de 7 dias e resolução direta; concordância geral com o restante da etapa anterior. Turno 391c51fc-6b31-4474-91fb-672ba4d428d0.
- Fonte F04 — Pergunta sobre sincronização, escolha de reserva durante atendimento e baixa somente na finalização; resposta com propostas técnicas de fila, estados e retentativas. Turno e207e6b7-a6e5-45c6-9921-561b4de5aea6.
- Fonte F05 — Correção sobre regras atuais de reserva e segregação do SupriTI, estudo de usuário específico, remoção manual de materiais e resumo de confirmação. Turno a890876d-d100-48c0-9af4-0b08540893ac.
- Fonte F06 — Pedido atual de consolidação em português, editável e Markdown, reiterando as decisões finais e o cancelamento da revisão com retorno ao chamado.

O documento preserva a precedência de F05 e F06 sobre propostas anteriores. Informações que não aparecem nessas fontes permanecem pendentes de levantamento.
