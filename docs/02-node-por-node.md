Aqui está uma explicação mais detalhada node por node do workflow.
# Workflow Principal
[imagem workflow principal]

Como é óbvio, o gatilho é a mensagem que o usuário envia por telegram. O telegram é um node nativo do n8n, então não há necessidade de baixar algum community node. Dependendo da forma que você tem instalado o seu n8n, não terá problemas com funcionamento, caso o tenha, recomendo que veja tutoriais no youtube e verifique os acessos do seu firewall.
[ imagem edit field]

O Edit Field é apenas para filtrar os dados do telegram que realmente importam, o message e o id.
[imagem agente principal]

O agente principal nesse caso usa o modelo gpt 4.1 mini. Como é um projeto simples, que será executado algumas vezes por dia internamente por um único usuário, uma memória Simple Memory é suficiente. 

A tool Think serve para ajudar o modelo a realizar os cálculos. Estava tendo problemas com o uso correto da tool Calculadora pelo Agente, ele não usava ordens lógicas para cálculos e às vezes apresentava resultados incorretos, a tool think foi a melhor forma que encontrei para diminuir esses erros.

As três tools seguintes são os sub workflows. Como explicado em /docs/01-visao-geral.md o Agente principal nao sabe de nada, sua única função é interagir com o usuário e chamar as tools correspondentes para ter as informações, registrar e confirmar.

O protocolo de comunicação entre os workflows você irá encontrar em /docs/03-com-multi-agents.md

A estrutura dos prompts voce ira encontrar em /docs/06prompt.md

O workflow fecha com o node de enviar mensagem do telegram.

# SUBWORKFLOW CONSULTA PRECOS
[imagem subworflow consulta precos]

O gatilho aqui é o “When executed by another workflow”, ou seja, quando o agente principal aciona esse subworkflow.
[print da trigger]

O Agente “Consulta Precios” tem as tools necessárias para saber todas as informações para criar o orçamento. Cada tool é um sheets do Google que funciona como banco de dados para ele, o seu cérebro.
Então, o Agente Principal aciona ele dizendo os dados que usuário proporcionou sobre o móvel a ser orçado, o “Consulta Precios” verifica nas tools e responde ao Agente Principal.

# SUBWORKFLOW REGISTRO DE PRESUPUESTO
[imagem subworkflow registro de presupuesto]

O gatilho aqui é o “When executed by another workflow”, ou seja, quando o agente principal aciona esse subworkflow.
[imagem da trigger]

Agora atenção, entenda que o Agente Principal chama o subworkflow com uma string, em /docs/05-erros-e-solucoes.md eu explico o porquê  isso é um problema na hora que o Agente Registro manda esses dados para tabela “Info_Presupuesto”. Então existe a necessidade de fazer um tratamento aqui, o node code.

A função desse code é transformar essa string que vem com todas as informações aglutinadas  em um JSON organizado, com cada campo separado corretamente. O output já sai no formato adequado para o schema do n8n e para o input do Agente Registro.

Depois que isso foi implementado, o workflow passou a registrar os dados na tabela de forma consistente, resolvendo os erros que aconteciam quando tentávamos salvar os valores diretamente a partir da string original.

Para ser sincero, eu não sei nada de JavaScript, estou no básico de Python, mas como a versão do n8n para Python está em beta, não quis arriscar. Então pedi para o Claude me gerar um código e funcionou bem.
Todos os scripts estão em /docs/04-scripts-node-code.md
[imagem aggregate]

O node aggregate serve para juntar os dados que vem do code para facilitar o input do Agente Registro.
[imagem dentro do agente]

Entao o Agente Registro manda para tabela Info_Presupuesto pagina Momentaneo com estes parametros
[imagem info_presupuesto] 

# SUBWORKFLOW CONFIRMAR PRESUPUESTO
[imagem subworkflow confirmar]

O gatilho aqui é o “When executed by another workflow”, ou seja, quando o agente principal aciona esse subworkflow.

O node seguinte é o “Get Momentaneo” (tabela com os dados temporários)

Os dados do orçamento são extraídos na tabela “momentaneo” e leva para um node code que separa cada detalhe do orçamento em diferentes itens, isso é muito importante para não ter erros em casos do orçamento ser com 2 ou mais móveis.
[imagem node code]

Depois disso, o fluxo continua no Google Drive. Lá já existe um documento modelo que serve de base para todos os orçamentos. O workflow faz uma cópia desse template (essa cópia será o arquivo específico daquele cliente) e o node “Update a Document” preenche automaticamente o documento com os dados separados no node code, gerando o arquivo final em .docx.
[imagem copy file]
[imagem doc template]

É no passo “Update a Document” que configuramos exatamente como o documento é preenchido. E é justamente por isso que o node code anterior é tão importante: ele garante que cada item do orçamento esteja separado corretamente, permitindo que o documento seja montado de forma confiável, independentemente da quantidade de móveis, preços, variações ou adicionais.
[imagem update doc]

Por exemplo: imagine um orçamento com dois móveis de configurações diferentes. O documento final só deve exibir os campos relacionados a esses dois itens. Para evitar que [Item 3] ou [Item 4] apareçam no PDF, configuramos o “Update a Document” para exibir ou ocultar cada campo de acordo com a quantidade de itens detectada.
[imagem de comandos no update doc]

Isso deve ser repetido e ajustado para cada parametro, [precio 3], [precio 4], [adicionales 3], [adicionales 4] e assim por diante.
[imagem mais exemplos de comandos update doc]

Aqui está um modelo do comando que você pode adaptar para o seu próprio workflow:

{{ $('Code in JavaScript1').item.json.item3_tipo  !== undefined ? $('Code in JavaScript1').item.json.item3_tipo: "" }}
Então você deve ajustar ele para diferentes contextos, como por exemplo:

{{ $('Code in JavaScript1').item.json.item2_extra_precio !== undefined ? $('Code in JavaScript1').item.json.item2_extra_precio: "" }}

[imagem exemplo de como fica o documento]

Seguindo o fluxo, para converter o .docx em PDF. O node “Share File” dá o acesso ao HTTP Request que realiza a exportação. Depois, o “Upload File” guarda o arquivo na pasta do Drive de preferência
[Imagem Share File]
[Imagem HTTP Request]

Para enviar o documento por email é bem simples. Node “Send a Message” do Gmail, nos campos subject e message, basta arrastar o json correspondente ao documento gerado.
[imagem gmail]

Porque estou mandando por email e não diretamente ao Telegram? Eu tentei, mas aparentemente o node send message do telegram quando usado em subworkflow funciona só quando quer. Muitas vezes nas fases de teste dava erros de autenticação e tratava o restante do fluxo. Não encontrei a solução, decidi colocar o Email.

Por fim, se você leu /docs/01-visao-geral.md sabe que a tabela “Momentâneo” guarda apenas os dados temporários necessários para montar aquele orçamento. Esses dados não devem permanecer ali. Aqui está o motivo:

Na lógica do fluxo, o subworkflow sempre começa extraindo os dados diretamente da tabela “Momentaneo” e enviando tudo para um node code que separa cada item do orçamento. Se essa tabela ainda contiver dados de orçamentos anteriores, o fluxo vai interpretar todas essas informações juntas, como se fossem parte do mesmo pedido. O resultado seria um documento com itens misturados de clientes diferentes (algo completamente incorreto).

Por isso, o segundo node code volta a printar os dados da tabela e salva as informações no “Histórico”, a tabela “Momentaneo” é esvaziada. Assim, ela fica pronta para receber apenas os dados do próximo orçamento, garantindo que cada documento seja gerado de forma limpa e sem contaminação de pedidos anteriores.

[imagem ultima etapa]
[imagem code]

Detalhe importante: O segundo node code também cria um id para cada orçamento para que seja salvo no “Histórico”.
[imagem historico]




