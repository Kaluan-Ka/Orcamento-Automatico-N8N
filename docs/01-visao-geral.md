# Uma breve passagem pelo fluxo geral do processo
Orccamento-Automatico-N8N/assets/imagens/agente-principal.png

1. O usuário envia a mensagem para o bot no Telegram, esse é o Trigger. 
2. Um node set para separar dois campos de dados: message e chat_ID.
3. O agente principal do workflow, ele é responsável por interagir com o usuário e chamar os subworkflow para conseguir as informações necessárias. Esse Agente não sabe de nada sobre os móveis, preços, variações etc. Para saber sobre os preços ele pergunta do agente “Consulta Precios”, para registrar o orçamento ele chama o agente “Registro de Presupuesto”, para gerar o documento em pdf ele chama o subworkflow “Confirmar Presupuesto”. Além disso, ele tem uma calculadora entre as tools para fazer cálculos necessários.
4. Como é um projeto simples, que será executado algumas vezes por dia internamente por um único usuário, uma memória Simple Memory é suficiente.
5. O chat model escolhido foi o GPT 4.1 mini.
6. Finaliza mandando a mensagem ao usuário

[imagem subworflow precio]

Este é o subworkflow que tem todo o banco de dados necessário para a criação do orçamento, ele é responsável em responder ao agente principal sobre os tipos de móveis, variações, dimensões, materiais, adicionais e preços.
Então o usuário manda uma mensagem para bot dizendo que quer criar um novo orçamento, o bot responde pedindo os dados necessários (o móvel, as dimensões, etc). O usuário responde e o Agente Principal manda um prompt único de input para o subworkflow com as informações necessárias para saber os preços e começar definitivamente o seu trabalho.

[imagem subworkflow registro]

Ok, então o usuário já mandou os dados, o Agente Principal já verificou com o subworkflow os preços, já calculou e mandou ao usuário o detalhe completo do orçamento pelo Telegram. 
O seguinte passo cabe ao usuário verificar se tudo parece bem. Logo após a verificação feita, o usuário diz “registrar presupuesto” e Eero chama ao subworkflow “Registro de Presupuesto”.

1. O Agente Principal manda toda a informação em um único prompt em string ao subworkflow.
2. O node code serve para separar as informações que vem em string, em dados separados em uma tabela. O motivo de fazer isso você vai entender no arquivo /docs/02-node-por-node.md
3. Node Aggregate para voltar a juntar os dados. O motivo de fazer isso você vai entender no arquivo /docs/02-node-por-node.md
4. Agente de IA que sua única função é registrar as informações que estarão presentes no documento em pdf do orçamento.

[imagem subworkflow confirma presupuesto]

Logo após o registro do orçamento, o usuário deve dizer ao Bot que quer “confirmar presupuesto” e então, o agente principal chama ao subworkflow “confirma presupuesto”
Aqui não tem nenhum Agente de IA para realizar o trabalho, mais detalhes sobre a configuração desses nodes você vai encontrar em /Docs/02-node-por-node.md Mas para dar um panorama geral, funciona assim: 
1. Os dados do orçamento são extraídos na tabela “momentaneo” e leva para um node code que separa cada detalhe do orçamento em diferentes itens, isso é muito importante para não ter erros em casos do orçamento ser com 2 ou mais móveis.
2. Depois que o code separa todos os itens do orçamento, o fluxo continua no Google Drive. Lá já existe um documento modelo que serve como base para todos os orçamentos. O workflow faz uma cópia desse template (essa cópia será o documento específico do cliente) Em seguida, o node “Update a Document” preenche essa cópia com as informações reais do orçamento, gerando o arquivo final em formato .docx.
3. Nesse ponto o documento de orçamento já está criado, então falta converter em PDF. O node “Share File” dá a permissão ao “HTTP Request” que exporta em PDF e faz o upload no drive.
4. Documento criado, resta mandar um email ao usuário no node “Send a message”.
5. A tabela “Momentâneo” guarda apenas os dados temporários necessários para montar aquele orçamento. Esses dados não devem permanecer ali (o porque disso você vai entender melhor em /docs/02-node-por-node.md), porém ainda são dados importantes para se guardar para análises futuras dentro do negócio. Por isso, antes de limpar a tabela, um node code printa novamente todas as informações e o fluxo envia para a tabela “Histórico”, onde ficam armazenados de forma permanente. Depois disso, o workflow apaga a tabela “Momentâneo” para deixá-la pronta para o próximo orçamento.
