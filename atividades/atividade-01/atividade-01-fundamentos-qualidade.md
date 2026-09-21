# Atividade 1: Fundamentos e Características da Qualidade no LocalEats

**Equipe:** Eduardo Timm Mendes, Tiago Duarte, Tomas Flores
**Aplicação analisada:** [LocalEats](https://local-eats-unisenac.vercel.app/)

## Tarefa 1: Fundamentos da qualidade

| Tipo | Necessidade | Interessado | Consequência se não for atendida |
|---|---|---|---|
| Explícita | O sistema deve permitir criar conta e entrar com e-mail e senha. | Usuário final | Sem essa funcionalidade, o usuário não consegue acessar nenhum recurso pessoal (favoritos, pedidos) do LocalEats. |
| Explícita | O sistema deve permitir filtrar e pesquisar restaurantes por especialidade ou localização. | Usuário final | Sem filtro/busca, o usuário precisaria rolar manualmente por todos os restaurantes cadastrados para encontrar o que procura. |
| Implícita | As mensagens do sistema (erros, confirmações) devem ser exibidas no mesmo idioma da interface (português). | Usuário final | Ao testar um login com senha incorreta, o sistema exibiu a mensagem em inglês ("Invalid credentials") no meio de uma interface inteiramente em português, quebrando a consistência e confundindo usuários que não leem inglês. |
| Implícita | Um pedido finalizado deve exibir informações compreensíveis (nome do restaurante e do prato), não apenas identificadores internos. | Usuário final | Em "Meus Pedidos", o pedido registrado exibiu "Restaurante ID: 1" e "1x Item Id #1" em vez dos nomes reais, dificultando a conferência do que foi pedido. |

**Um sistema que implementa todas as funcionalidades explicitamente solicitadas pode, ainda assim, apresentar baixa qualidade?**

Sim. O LocalEats implementa todas as funcionalidades explícitas listadas no enunciado (criar conta, buscar, favoritar, pedir), mas isso não garante qualidade percebida pelo usuário. Um exemplo é a necessidade implícita de consistência de idioma: o sistema é todo em português, mas ao errar a senha a mensagem de erro aparece em inglês ("Invalid credentials"), quebrando a experiência sem que nenhum requisito explícito tenha sido violado. Da mesma forma, o histórico de pedidos mostra "Item Id #1" em vez do nome do prato, o que atende ao requisito explícito de "consultar pedidos", mas não à expectativa implícita de que a informação seja compreensível. Ou seja, cumprir a lista de funcionalidades não é suficiente: a qualidade também depende de necessidades que os usuários não escrevem, mas esperam.

## Tarefa 2: Exploração da aplicação

| Integrante | Funcionalidade | O que foi realizado | O que foi observado | Evidência |
|---|---|---|---|---|
| Eduardo Timm Mendes | Criar conta / Entrar no sistema | Criou uma conta válida (nome, e-mail e senha de 8 caracteres) e, em seguida, tentou entrar novamente com o e-mail correto e uma senha incorreta. | O cadastro foi concluído com sucesso e o usuário foi autenticado automaticamente, sem exigir confirmação de e-mail. Na tentativa de login com senha errada, o sistema bloqueou o acesso e exibiu a mensagem "Invalid credentials", em inglês, destoando do restante da interface em português. | `evidencias/eduardo-criar-conta-sucesso.png`, `evidencias/eduardo-login-senha-invalida.png` |
| Tiago Duarte | Filtrar e pesquisar restaurantes | Aplicou o filtro por especialidade "Italiana" e, em seguida, digitou "zona sul" no campo de busca e pressionou Enter. Repetiu a busca clicando no botão "Buscar". | O filtro por categoria funcionou corretamente, reduzindo a lista para os 3 restaurantes italianos. Pressionar Enter no campo de busca não aplicou o filtro; foi necessário clicar no botão "Buscar" para a busca combinada (categoria + texto) funcionar, retornando apenas 1 restaurante. Uma busca por um termo inexistente ("xyzinexistente123") exibiu corretamente a mensagem "Nenhum restaurante encontrado." | `evidencias/tiago-filtro-italiana.png`, `evidencias/tiago-busca-sem-resultado.png` |
| Tomas Flores | Favoritar restaurante / Fazer pedido | Favoritou o "Restaurante Sabor 0", adicionou um prato ao pedido e finalizou a compra. Depois acessou "Meus Favoritos" e removeu o item favoritado. | O botão mudou para "Favoritado!" e o item passou a aparecer em "Meus Favoritos", inclusive após logout e novo login (persistiu corretamente). O pedido foi concluído sem exigir endereço de entrega ou forma de pagamento, e apareceu em "Meus Pedidos" como "Restaurante ID: 1" / "1x Item Id #1", sem os nomes reais. Ao remover o favorito, a lista ficou vazia sem nenhuma mensagem indicando que não há favoritos (diferente da busca, que mostra "Nenhum restaurante encontrado"). | `evidencias/tomas-favoritar-restaurante.png`, `evidencias/tomas-pedido-realizado.png` |

> **Nota da equipe:** as capturas de tela reais devem ser adicionadas por cada integrante na pasta `evidencias/`, seguindo exatamente os nomes de arquivo indicados na tabela acima, repetindo em sua própria conta as ações descritas (ou capturando as telas durante a própria exploração). O texto de "o que foi realizado/observado" já reflete o comportamento real da aplicação, verificado nesta exploração.

## Tarefa 3: Requisitos e características de qualidade

| Integrante | Requisito de Qualidade | Característica ou subcaracterística | Justificativa | Como avaliar |
|---|---|---|---|---|
| Eduardo Timm Mendes | Todas as mensagens exibidas ao usuário (sucesso, erro, validação) devem estar em português, consistentes com o idioma da interface. | Adequação funcional — mais especificamente, Usabilidade / Reconhecimento de adequação (ISO/IEC 25000) | A mensagem "Invalid credentials" em inglês quebra a consistência textual do produto e pode confundir o usuário que não domina inglês, afetando diretamente a percepção de qualidade sem alterar nenhuma função. | Revisar todas as strings de erro e confirmação exibidas na interface e contar quantas não estão em português; o requisito é atendido quando 100% das mensagens estiverem no mesmo idioma da interface. |
| Tiago Duarte | A busca por restaurantes deve poder ser acionada tanto pelo botão "Buscar" quanto pela tecla Enter no campo de texto. | Usabilidade / Capacidade de operação (operability) | Usuários esperam, por convenção de outros sites de busca, que pressionar Enter em um campo de pesquisa dispare a busca; a ausência desse comportamento aumenta o esforço e pode ser confundida com uma falha do sistema. | Testar o campo de busca digitando um termo válido e pressionando apenas Enter (sem clicar no botão); o requisito é atendido se o resultado filtrado aparecer da mesma forma que ao clicar em "Buscar". |
| Tomas Flores | O histórico de pedidos e a lista de favoritos vazios devem exibir textos legíveis (nome do restaurante/prato, e uma mensagem de "lista vazia"), nunca identificadores internos ou telas em branco sem explicação. | Adequação funcional / Usabilidade — Inteligibilidade | Exibir "Restaurante ID: 1" e "Item Id #1" expõe detalhes internos do sistema ao usuário final, e uma lista de favoritos vazia sem nenhuma mensagem pode ser confundida com um erro de carregamento. | Verificar, após um pedido e após remover todos os favoritos, se as telas mostram nomes reais e uma mensagem de estado vazio (como já ocorre na busca, "Nenhum restaurante encontrado."); o requisito é atendido quando nenhuma tela relevante exibir IDs crus ou ficar em branco sem explicação. |

## Uso de inteligência artificial

**Ferramenta utilizada:**
Claude (Claude Code / Anthropic).

**Como foi utilizada:**
A IA foi usada para explorar a aplicação LocalEats no navegador (criar conta, testar login, filtrar e buscar restaurantes, favoritar, fazer um pedido e consultar o histórico), registrar as observações reais coletadas nesses testes e organizar essas observações no formato exigido pelo guia da atividade (necessidades explícitas/implícitas, tabela de exploração e tabela de requisitos de qualidade).

**Como as respostas foram verificadas:**
A equipe deve revisar cada linha das tabelas, repetir pessoalmente as ações descritas (login inválido, filtro, busca por Enter/botão, favoritar/remover, fazer pedido) na aplicação e confirmar que os comportamentos relatados realmente ocorrem, adicionando as capturas de tela reais na pasta `evidencias/` antes da defesa técnica. Cada integrante deve estar preparado para explicar e justificar, individualmente, a necessidade, a evidência e o requisito de qualidade da funcionalidade pela qual ficou responsável.
