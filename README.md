# Orçamento-Automatico-N8N
 Geração de orçamento automático feito no N8N, nesse caso, foi feito para uma carpinteria de Buenos Aires, mas pode ser adaptado para diferentes contextos. 

Este repositório não irá fornecer o template em json do projeto, ele serve tanto como documentação pessoal do projeto, tanto para ajudar a estudantes de N8N que queiram melhorar de forma ética na plataforma. Por tanto, aqui você vai encontrar os problemas que encontrei pelo caminho, as soluções e oque eu penso fazer diferente em projetos semelhantes no futuro.

Se você se interessou por este projeto, ficou com alguma dúvida ou tem alguma recomendação para que eu possa melhorar e seguir evoluindo, por favor se comunique comigo mandando um email em kaluanatuomate@gmail.com

## O que é?
Esse é um workflow de geração de orçamento automático feito no N8N, nesse caso, foi feito para uma carpinteria de Buenos Aires, mas pode ser adaptado para diferentes contextos. 

# O que ele resolve?
Em uma breve conversa com o dono de uma carpintaria de Buenos Aires, diagnosticamos que eles perdem muito tempo na criação e geração do documento de orçamentos para passar para aos seus clientes. Horas da semana eram perdidas nessa parte operativa que gerava uma despesa “invisível” para o negócio.

Com esta solução, tudo ficou mais simples. O usuário (pessoa responsável pela criação do orçamento) manda um mensagem no Telegram para um Bot (Agente de IA criado especialmente para o projeto) dizendo “quero criar um novo orçamento”, e com uma breve interação com o bot, o orçamento é criado e o documento em pdf é gerado, tudo resolvido entre 3 a 5 minutos.

# Pré-requisitos

Para criar este workflow foi preciso:

N8N instalado;

Criação de um bot no Telegram; 

Estruturação do banco de dados da carpinteria (Google Sheets);

APIs do Google Drive e Gmail;

API da Open AI para o chat model (GPT 4.1 mini) do AI Agent.

# Como está organizado o repositório

/docs/

. Documentação explicando cada parte do fluxo

. Inclui visão geral, explicações node-por-node, erros e soluções encontrados, etc.

. É onde está o contexto técnico do projeto.

/assets/

. Imagens usados na documentação 

