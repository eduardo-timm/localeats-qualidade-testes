# Atividade 3: Estratégia e Projeto de Testes do LocalEats

**Equipe:** Eduardo Timm Mendes, Tiago Duarte, Tomas Flores
**Aplicação:** [LocalEats](https://local-eats-unisenac.vercel.app/)

## Tarefa 1: Planejamento dos testes

### 1.1 Objetivo dos testes

Verificar se as funcionalidades de autenticação, busca/filtro de restaurantes e favoritos/pedidos do LocalEats se comportam de forma correta e consistente diante de entradas válidas e inválidas, priorizando os riscos de segurança (senhas fracas), usabilidade (busca) e integridade do pedido (carrinho vazio), sem a necessidade de acessar o código-fonte da aplicação.

### 1.2 Escopo

| Integrante | Funcionalidade incluída | O que será verificado |
|---|---|---|
| Eduardo Timm Mendes | Criar conta / Entrar no sistema | Regras de validação de senha no cadastro e bloqueio de acesso com credenciais inválidas. |
| Tiago Duarte | Explorar, filtrar e pesquisar restaurantes | Combinação entre filtro por especialidade e busca textual, e tratamento de buscas sem resultado. |
| Tomas Flores | Favoritar restaurantes / Fazer pedido | Comportamento do carrinho ao finalizar um pedido vazio versus com itens, e persistência de favoritos. |

| Funcionalidade não incluída | Justificativa |
|---|---|
| Avaliações de restaurantes (estrelas e comentários) | Não é uma funcionalidade listada no escopo do guia da atividade e não está entre as responsabilidades de nenhum integrante da equipe. |

### 1.3 Abordagem

| Item | Decisão da equipe | Justificativa |
|---|---|---|
| Níveis de teste | Sistema | Os fluxos serão verificados de ponta a ponta pela interface, como o usuário final utiliza o LocalEats. |
| Tipos de teste | Funcional | O foco é verificar regras de negócio (validação de senha, combinação de filtros, carrinho vazio), não desempenho ou segurança avançada. |
| Perspectiva caixa-preta ou caixa-branca | Caixa-preta | A equipe não tem acesso ao código-fonte do LocalEats, apenas à interface publicada. |
| Técnicas de teste | Análise de valor limite, tabela de decisão e particionamento de equivalência | Cada integrante aplicou a técnica mais adequada à regra da sua funcionalidade (ver Tarefa 2). |

### 1.4 Ambiente e responsabilidades

| Item | Definição |
|---|---|
| Ambiente necessário | Aplicação em https://local-eats-unisenac.vercel.app/, navegador desktop atualizado (Chrome/Edge), conexão com a internet, uma conta de teste sem dados pessoais reais. |
| Responsáveis pelo planejamento | Eduardo Timm Mendes, Tiago Duarte, Tomas Flores (em conjunto). |
| Responsáveis pela especificação dos casos | Cada integrante pela sua funcionalidade (ver Tarefa 3). |
| Responsáveis pela futura execução | Cada integrante pela sua funcionalidade, com apoio dos demais quando necessário. |

### 1.5 Critérios

| Critério | Definição da equipe |
|---|---|
| Entrada | Aplicação acessível na URL informada, sem contas de teste previamente cadastradas em conflito com os dados usados nos casos. |
| Saída | Todos os 6 casos de teste planejados executados e resultados registrados (aprovado/reprovado) em uma futura rodada de execução. |
| Suspensão | Aplicação indisponível (erro 404/500) ou impossibilidade de criar contas/pedidos de teste. |

## Tarefa 2: Riscos e técnicas de teste

### 2.1 Análise dos riscos

| ID | Integrante | Funcionalidade | Risco | Consequência | Probabilidade | Impacto | Prioridade | Justificativa |
|---|---|---|---|---|---|---|---|---|
| R01 | Eduardo Timm Mendes | Criar conta | O cadastro aceita senhas muito curtas (o campo indica "Min. 3 caracteres", sem exigir letras, números ou símbolos). | Contas ficam vulneráveis a acesso indevido por adivinhação de senha, comprometendo dados do usuário e o histórico de pedidos. | Alta | Alto | Alta | A regra fraca foi observada diretamente no formulário de cadastro; senhas curtas são amplamente exploradas em ataques de força bruta. |
| R02 | Tiago Duarte | Filtrar e pesquisar restaurantes | A combinação entre filtro de categoria e busca textual não retornar os restaurantes esperados (ex.: interseção incorreta). | Usuário deixa de encontrar restaurantes que realmente atendem ao filtro escolhido, prejudicando a experiência de descoberta, que é o propósito central do app. | Média | Médio | Média | Buscar e filtrar são as principais formas de descoberta de restaurantes no LocalEats; um erro de combinação afeta diretamente esse fluxo. |
| R03 | Tomas Flores | Fazer pedido | O sistema permitir finalizar um pedido com o carrinho vazio (sem nenhum item adicionado). | Pedidos vazios ou inválidos podem ser enviados ao restaurante, gerando confusão operacional e possível prejuízo para o estabelecimento. | Média | Alto | Alta | Fazer pedido envolve valor monetário e comunicação com o restaurante; um pedido inválido tem impacto direto no negócio. |

### 2.2 Aplicação da técnica

#### Integrante responsável: Eduardo Timm Mendes

**Nome:** Eduardo Timm Mendes **Funcionalidade:** Criar conta **Risco relacionado:** R01 **Técnica escolhida:** Análise de valor limite

**Por que a técnica foi escolhida?**
A regra de senha é definida por um limite numérico simples (mínimo de 3 caracteres, sem máximo aparente), o que torna a análise de valor limite adequada para verificar exatamente o comportamento nas fronteiras dessa regra.

**Aplicação da técnica**
Valores próximos ao limite mínimo (3 caracteres):
- 1 caractere;
- 2 caracteres;
- 3 caracteres (limite);
- 4 caracteres.

**Casos derivados:** CT01 e CT02.

#### Integrante responsável: Tiago Duarte

**Nome:** Tiago Duarte **Funcionalidade:** Filtrar e pesquisar restaurantes **Risco relacionado:** R02 **Técnica escolhida:** Tabela de decisão

**Por que a técnica foi escolhida?**
O resultado da listagem depende da combinação de duas condições independentes (categoria selecionada e texto de busca preenchido ou não), o que é característico de regras representáveis por tabela de decisão.

**Aplicação da técnica**

| Regra | Categoria selecionada | Texto de busca preenchido? | Resultado esperado |
|---|---|---|---|
| 1 | Todos | Não | Lista todos os restaurantes cadastrados |
| 2 | Específica (ex.: Italiana) | Não | Lista apenas restaurantes da categoria escolhida |
| 3 | Todos | Sim (ex.: "zona sul") | Lista restaurantes cujo texto corresponda à busca, de qualquer categoria |
| 4 | Específica (ex.: Italiana) | Sim (ex.: "zona sul") | Lista apenas restaurantes que atendam simultaneamente à categoria e ao texto buscado |

**Casos derivados:** CT03 e CT04.

#### Integrante responsável: Tomas Flores

**Nome:** Tomas Flores **Funcionalidade:** Fazer pedido **Risco relacionado:** R03 **Técnica escolhida:** Particionamento de equivalência

**Por que a técnica foi escolhida?**
O comportamento do botão "Finalizar Pedido" depende de uma condição simples e categórica — a quantidade de itens no carrinho —, o que se encaixa bem em classes de equivalência válidas e inválidas.

**Aplicação da técnica**

| Classe | Situação | Valor representativo |
|---|---|---|
| Inválida | Carrinho sem nenhum item | 0 itens |
| Válida | Carrinho com pelo menos um item | 1 item |

**Casos derivados:** CT05 e CT06.

## Tarefa 3: Casos de teste e rastreabilidade

### 3.1 Especificação dos casos de teste

**CT01: Rejeitar senha abaixo do limite mínimo no cadastro**
Integrante responsável: Eduardo Timm Mendes Funcionalidade: Criar conta Risco ou requisito relacionado: R01 Técnica utilizada: Análise de valor limite

Pré-condição: O usuário está na tela "Criar Conta", sem estar autenticado.
Dados de entrada: Nome: "Usuário Teste"; E-mail: "teste.valor.limite@teste.com"; Senha: "ab" (2 caracteres).
Passos:
1. Preencher nome e e-mail válidos.
2. Preencher o campo de senha com "ab" (2 caracteres).
3. Clicar em "Registrar".

Resultado esperado: O sistema não conclui o cadastro e informa que a senha não atende ao tamanho mínimo exigido.

**CT02: Aceitar senha exatamente no limite mínimo no cadastro**
Integrante responsável: Eduardo Timm Mendes Funcionalidade: Criar conta Risco ou requisito relacionado: R01 Técnica utilizada: Análise de valor limite

Pré-condição: O usuário está na tela "Criar Conta", sem estar autenticado, com um e-mail ainda não cadastrado.
Dados de entrada: Nome: "Usuário Teste"; E-mail: "teste.valor.limite2@teste.com"; Senha: "abc" (3 caracteres).
Passos:
1. Preencher nome e e-mail válidos.
2. Preencher o campo de senha com "abc" (3 caracteres).
3. Clicar em "Registrar".

Resultado esperado: O cadastro é concluído com sucesso e o usuário é autenticado no sistema.

**CT03: Combinar filtro de categoria com busca textual**
Integrante responsável: Tiago Duarte Funcionalidade: Filtrar e pesquisar restaurantes Risco ou requisito relacionado: R02 Técnica utilizada: Tabela de decisão (Regra 4)

Pré-condição: O usuário está autenticado na tela "Explorar", com a lista completa de restaurantes visível.
Dados de entrada: Categoria: "Italiana"; Texto de busca: "zona sul".
Passos:
1. Selecionar a categoria "Italiana".
2. Digitar "zona sul" no campo de busca.
3. Clicar no botão "Buscar".

Resultado esperado: A lista exibe somente restaurantes que sejam da categoria "Italiana" e estejam localizados na "Zona Sul", simultaneamente.

**CT04: Buscar termo inexistente**
Integrante responsável: Tiago Duarte Funcionalidade: Filtrar e pesquisar restaurantes Risco ou requisito relacionado: R02 Técnica utilizada: Tabela de decisão (Regra 3)

Pré-condição: O usuário está na tela "Explorar", com a categoria "Todos" selecionada.
Dados de entrada: Texto de busca: "xyzinexistente123".
Passos:
1. Digitar "xyzinexistente123" no campo de busca.
2. Clicar no botão "Buscar".

Resultado esperado: A lista de restaurantes fica vazia e o sistema exibe uma mensagem informando que nenhum restaurante foi encontrado.

**CT05: Impedir finalização de pedido com carrinho vazio**
Integrante responsável: Tomas Flores Funcionalidade: Fazer pedido Risco ou requisito relacionado: R03 Técnica utilizada: Particionamento de equivalência (classe inválida)

Pré-condição: O usuário está autenticado e acessou a página de um restaurante, sem adicionar nenhum prato ao pedido.
Dados de entrada: Nenhum item adicionado ao carrinho.
Passos:
1. Acessar a página de detalhes de um restaurante.
2. Não adicionar nenhum prato ao pedido.
3. Procurar e tentar acionar a opção de finalizar pedido.

Resultado esperado: O sistema não permite finalizar um pedido vazio; o botão de finalizar não fica disponível, ou o sistema impede a ação e informa que é necessário adicionar itens.

**CT06: Finalizar pedido com um item no carrinho**
Integrante responsável: Tomas Flores Funcionalidade: Fazer pedido Risco ou requisito relacionado: R03 Técnica utilizada: Particionamento de equivalência (classe válida)

Pré-condição: O usuário está autenticado e acessou a página de um restaurante.
Dados de entrada: 1 item do cardápio adicionado ao carrinho (ex.: "Prato Especial 0").
Passos:
1. Acessar a página de detalhes de um restaurante.
2. Clicar em "Adicionar" em um prato do cardápio.
3. Clicar em "Finalizar Pedido".

Resultado esperado: O pedido é registrado com sucesso, com confirmação visível ao usuário, e passa a aparecer em "Meus Pedidos" com os dados do restaurante e do prato pedido.

### 3.2 Matriz de rastreabilidade

| Integrante | Funcionalidade | Risco ou requisito | Técnica utilizada | Casos de teste |
|---|---|---|---|---|
| Eduardo Timm Mendes | Criar conta | R01: senha fraca aceita no cadastro | Análise de valor limite | CT01 e CT02 |
| Tiago Duarte | Filtrar e pesquisar restaurantes | R02: combinação incorreta entre filtro e busca | Tabela de decisão | CT03 e CT04 |
| Tomas Flores | Fazer pedido | R03: pedido finalizado com carrinho vazio | Particionamento de equivalência | CT05 e CT06 |

Todos os três riscos identificados possuem pelo menos um caso de teste associado; nenhum risco ficou sem cobertura.

## Uso de inteligência artificial

**Ferramenta utilizada:**
Claude (Claude Code / Anthropic).

**Como foi utilizada:**
A IA foi usada para propor os riscos, escolher as técnicas de teste adequadas a cada funcionalidade, derivar os casos de teste e montar o plano simplificado e a matriz de rastreabilidade, a partir do comportamento real observado no LocalEats durante a exploração feita na Atividade 1.

**Uma sugestão que precisou ser alterada ou rejeitada:**
A primeira sugestão de técnica para o risco de Eduardo (R01) foi "particionamento de equivalência" genérico sobre o campo de senha; a equipe ajustou para "análise de valor limite", por ser mais precisa quando a regra é um limite numérico simples (mínimo de 3 caracteres), permitindo testar exatamente a fronteira (2 vs. 3 caracteres) em vez de apenas classes amplas.

**Como as respostas foram verificadas:**
A equipe deve revisar se cada risco realmente representa o que pode dar errado na funcionalidade sob sua responsabilidade, repetir manualmente os passos descritos nos casos de teste (sem registrar o resultado, já que a execução não faz parte desta atividade) e estar preparada para justificar, na defesa técnica, por que a técnica escolhida é adequada e como cada caso deriva dela.
