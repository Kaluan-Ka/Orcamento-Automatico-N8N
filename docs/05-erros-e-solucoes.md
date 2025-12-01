Alguns erros e bugs foram encontrados durante o desenvolvimento do projeto. A maioria resolvidos com o protocolo de comunicação entre os agentes que você pode encontrar em /docs/03-com-multi-agents.md

# ERRO DE AUTENTICAÇÃO APIS DO GOOGLE

(Erro) Se criar o template do orçamento ou a tabela do banco de dados/info_presupuesto usando aplicativos do Pacote Office (Word/Excel) e só depois fizer o upload desses arquivos para o Google Drive, o Google Workspace não reconhece esses arquivos como documentos nativos. Como consequência a suas APIs do Google terão erro de autenticação ao tentar acessar o arquivo.

(Solução) É necessário criar esses documentos diretamente dentro do Google Workspace (Google Docs ou Google Sheets). Assim, o Google Drive reconhece os arquivos como nativos e suas APIs conseguem acessá-los, modificá-los e atualizá-los sem restrições.


# ERRO ENVIO DO DOCUMENTO DO ORÇAMENTO PELO TELEGRAM

(Erro) O node do telegram no subworkflow as vezes dá erro de autenticação e trava o resto do workflow.

(Solução) Não encontrada.

# ERRO CONSULTA PRECIOS

(Erro) O Agente Principal manda múltiplas solicitações ao subworkflow Consulta Precios, e causa inconsistência nas respostas ao usuário, além de provocar interações desnecessárias.

(Solução) Criação do protocolo de comunicação para o Agente Principal mandar a solicitação em um único input (verificar /docs/03-com-multi-agents.md)

# ERRO REGISTRO DAS INFORMAÇÕES DO ORÇAMENTO

(Erro) O Agente principal manda as informações sem padrão. Isso causa inconsistência no trabalho do Agente Registro Presupuesto.

(Solução) Criação de protocolo de comunicação entre os agentes. O Agente Principal deve mandar as informações em formato específico (verificar /docs/03-com-multi-agents.md). No subworkflow Registro Presupuesto,  um node code recebe o input em string do Agente Principal. A função desse code é transformar essa string que vem com todas as informações aglutinadas  em um JSON organizado, com cada campo separado corretamente. O output já sai no formato adequado para o schema do n8n e para o input do Agente Registro.

# ERRO APRESENTAÇÃO DO CÁLCULO

(Erro) O Agente Principal erra ao apresentar o valor total ao usuário.

(Solução) Especificar no prompt uma ordem lógica de cálculo. Implementar a tool Think para ajudar o Agente nessa tarefa.


# ERRO CÁLCULO DO M2

(Erro) Por vezes o Agente esquece de calcular o valor M2 de materiais específicos

(Solução) Especificar no prompt uma ordem lógica de cálculo. Implementar a tool Think para ajudar o Agente nessa tarefa. Não soluciona 100% o problema, porém quando o usuário pede para realizar o cálculo mais uma vez, volta ao normal.

# ERRO NA APRESENTAÇÃO DE PREÇO DE MOVEIS ESPECIFICOS

(Erro) Por vezes, o Agente Consulta Precios erra na resposta sobre preços de alguns móveis. Ocorre em uma média de uma vez a cada dez.

(Solução) Ainda não testei, é algo que quero tentar em /docs/07-melhorias-futuras.md


  





