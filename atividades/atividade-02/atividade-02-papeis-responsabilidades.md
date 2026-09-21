# Atividade 2: Organização da Qualidade no LocalEats

**Equipe:** Eduardo Timm Mendes, Tiago Duarte, Tomas Flores

## Tarefa 1: Diagnóstico da situação

| Problema identificado | Possível consequência para o produto ou para a equipe |
|---|---|
| Não há critérios claros para considerar uma funcionalidade "pronta" (Definition of Done). | Funcionalidades incompletas ou com comportamentos inconsistentes (como mensagens de erro em inglês em uma interface em português) chegam ao usuário final, gerando retrabalho e perda de confiança no produto. |
| A responsabilidade por testar está concentrada apenas no QA. | Desenvolvedores deixam de validar seu próprio código antes de entregar, sobrecarregando o QA e atrasando a identificação de defeitos simples (ex.: campo de busca que não responde à tecla Enter). |
| Defeitos são identificados mas nem sempre são registrados ou acompanhados. | Um mesmo problema pode ser descoberto várias vezes por pessoas diferentes, sem que ninguém saiba se já foi corrigido, priorizado ou descartado, desperdiçando tempo da equipe. |

**A qualidade do LocalEats deve ser responsabilidade exclusiva do profissional de QA?**

Não. QA verifica e aponta problemas, mas não é quem escreve o código, define os requisitos ou decide o que será lançado. Se apenas o QA for responsável pela qualidade, desenvolvedores tendem a relaxar cuidados básicos (como tratar mensagens de erro corretamente) por acreditar que "alguém vai revisar depois". No LocalEats, falhas simples de consistência (idioma da mensagem de erro) ou de usabilidade (busca sem resposta ao Enter) poderiam ser evitadas já na implementação, sem depender de um teste manual do QA. Qualidade é resultado do trabalho conjunto de quem define requisitos, quem implementa e quem valida.

## Tarefa 2: Papéis e competências

| Integrante | Papel analisado | Responsabilidades relacionadas à qualidade | Competências técnicas | Competências comportamentais |
|---|---|---|---|---|
| Eduardo Timm Mendes | Desenvolvedor | Implementar as funcionalidades seguindo os requisitos; validar o próprio código antes de entregar (ex.: mensagens de erro no idioma correto); corrigir defeitos apontados pelo QA ou por outros integrantes. | Conhecimento da linguagem/framework utilizado no LocalEats; testes unitários básicos; leitura e escrita de requisitos técnicos. | Atenção a detalhes; disposição para receber feedback sobre o próprio código; comunicação clara ao reportar dificuldades técnicas. |
| Tiago Duarte | QA / Analista de qualidade | Planejar e executar testes funcionais das telas (ex.: busca, filtro); registrar defeitos encontrados de forma clara e reprodutível; acompanhar se os defeitos registrados foram corrigidos. | Técnicas de teste (particionamento, valor limite, tabela de decisão); ferramentas de registro de defeitos; noções de usabilidade. | Pensamento crítico; paciência para testar casos alternativos e não apenas o caminho feliz; comunicação objetiva ao descrever um defeito. |
| Tomas Flores | Responsável pelo produto | Definir e comunicar os critérios de aceitação de cada funcionalidade (ex.: o que é considerado "pedido concluído com qualidade"); priorizar quais problemas de qualidade devem ser corrigidos antes do lançamento; aprovar a disponibilização de novas versões. | Entendimento do fluxo de negócio do LocalEats (pedidos, favoritos, restaurantes); capacidade de escrever critérios de aceitação testáveis. | Capacidade de priorização; comunicação com toda a equipe; visão do usuário final para equilibrar prazo e qualidade. |

## Tarefa 3: Matriz de responsabilidades

**Papéis definidos pela equipe:** Desenvolvedor (Eduardo), QA (Tiago), Responsável pelo Produto (Tomas).

| Atividade de qualidade | Desenvolvedor | QA | Responsável pelo Produto |
|---|---|---|---|
| Definir critérios de aceitação | C | C | A/R |
| Revisar requisitos | C | C | A |
| Implementar a funcionalidade | R | I | A |
| Revisar o código | R | I | A |
| Criar testes unitários | R | C | I |
| Planejar e executar testes do sistema | C | R | A |
| Registrar e acompanhar defeitos | C | R | I |
| Priorizar a correção dos defeitos | C | C | A/R |
| Aprovar a disponibilização da versão | I | C | A |

**Lacuna ou conflito encontrado:**
"Implementar a funcionalidade" e "Revisar o código" ficam concentradas somente no Desenvolvedor como Responsável, sem um segundo par de olhos técnico (por exemplo, revisão de código por outro desenvolvedor). Em uma equipe de apenas três papéis, isso é um risco: se o único desenvolvedor não perceber um problema (como o idioma da mensagem de erro), ele só será detectado depois, na etapa de testes do QA, quando já seria mais caro corrigir.

**Práticas recomendadas:**

| Prática recomendada | Problema que ajuda a resolver | Papéis envolvidos |
|---|---|---|
| Checklist de "Definition of Done" (ex.: mensagens no idioma correto, estados vazios com texto explicativo, dados exibidos sem IDs internos) revisado antes de qualquer entrega. | Falta de critérios claros para considerar uma funcionalidade pronta. | Desenvolvedor, Responsável pelo Produto |
| Testes exploratórios curtos feitos pelo próprio Desenvolvedor antes de repassar a funcionalidade ao QA, cobrindo pelo menos um caminho alternativo/inválido. | Responsabilidade de testar concentrada apenas no QA; defeitos simples descobertos tarde demais. | Desenvolvedor, QA |

## Uso de inteligência artificial

**Ferramenta utilizada:**
Claude (Claude Code / Anthropic).

**Como foi utilizada:**
A IA foi utilizada para propor a organização de papéis, a matriz RACI e as práticas de QA a partir do contexto do LocalEats descrito no guia da atividade e dos problemas reais observados durante a exploração feita na Atividade 1 (ex.: mensagem de erro em inglês, busca sem resposta ao Enter).

**Como as respostas foram verificadas:**
A equipe deve revisar se a distribuição de papéis faz sentido para o tamanho do trio (3 integrantes), se cada um concorda com o papel atribuído a si e se consegue justificar, durante a defesa técnica, por que classificou cada célula da matriz RACI como R, A, C ou I, e por que a lacuna identificada é relevante.
