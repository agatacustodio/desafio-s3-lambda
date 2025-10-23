# Atualização de CRM - Workflow Automatizado com S3 e Lambda

Este repositório contém um workflow baseado nos recursos Amazon S3 e AWS Lambda. Ele consolida os conhecimentos adquiridos e demonstra a automatização do processamento de imagens enviadas por um cliente através de um chatbot e a subsequente atualização no CRM.

O objetivo principal é garantir que, assim que um cliente fizer o upload de uma imagem, o executivo de vendas correspondente receba uma notificação imediata, com a imagem já processada e otimizada, diretamente no seu ambiente de CRM.

## Arquitetura (AWS e CRM)

A arquitetura é baseada em eventos, utilizando o Amazon S3 como gatilho para a execução da função AWS Lambda.

## O que é o Amazon S3?
É um serviço de armazenamento de objetos na nuvem que permite guardar e recuperar qualquer quantidade de dados, com alta escalabilidade, disponibilidade e segurança.

- Armazenamento de objetos: Os dados (como imagens, vídeos, documentos e backups) são armazenados como "objetos" em um formato de armazenamento não hierárquico. 
- Buckets: Para armazenar dados, você cria um "bucket", que é um contêiner para os objetos. Cada nome de bucket deve ser exclusivo na região onde ele é criado. 
- Chaves: Cada objeto dentro de um bucket é identificado por uma "chave" exclusiva, que funciona como um nome de arquivo. 
- Acesso: O acesso aos dados é feito via API, usando o protocolo HTTP, e o gerenciamento é feito através de uma interface web.

## O que é o AWS Lambda?
É um serviço de computação sem servidor que executa código em resposta a eventos, sem a necessidade de provisionar ou gerenciar servidores.

- Sem servidor: Não é necessário gerenciar servidores, o que reduz a sobrecarga administrativa. 
- Orientado a eventos: O código é executado em resposta a eventos específicos, o que o torna ideal para sistemas reativos. 
- Escala automática: O Lambda dimensiona automaticamente a capacidade computacional com base na carga de trabalho recebida, garantindo o desempenho consistente. 
- Pagamento por uso: Você paga apenas pelo tempo de computação utilizado, com cobrança zero quando o código não está em execução. 
- Suporte a diversas linguagens: Suporta várias linguagens de programação, como Node.js, Python, Java e Go. 
- Funções stateless: As funções são projetadas para serem "stateless", o que permite um rápido acionamento sem atrasos na configuração. 

## Caso de Uso: Atualização do CRM com Imagens Enviadas por Clientes

O objetivo é que, assim que o cliente envia uma imagem no chatbot, o CRM seja atualizado com a imagem já processada.

## Componentes Envolvidos

| Componente | Função no Fluxo | 
|------------|-----------------|
| Amazon S3 (Bucket de Entrada) | Onde o chatbot salva a imagem original do cliente. É o **gatilho** do fluxo. |
| AWS Lambda (Função) | Executa o código de processamento da imagem e de notificação. |
| Amazon S3 (Bucket de Saída) | Armazena a imagem após o processamento (ex: redimensionada). |
| CRM API | Recebe o link da imagem e atualiza o perfil do cliente. |

## O Fluxo de Automação (Passo a Passo)

A automação é ativada sempre que um novo arquivo é carregado no S3.

1.  **Gatilho (S3):**
    * O cliente envia a imagem.
    * O Chatbot salva o arquivo (ex: `foto_original.jpg`) no Bucket de Entrada.

2.  **Ação (Lambda):**
    * O S3 detecta a criação do arquivo e aciona a Função Lambda.
    * A Lambda baixa o arquivo, redimensiona-o para uma miniatura e o carrega no Bucket de Saída.

3.  **Resultado (CRM):**
    * A Lambda obtém o link do novo arquivo redimensionado.
    * A Lambda faz uma chamada para a API do CRM para adicionar o link da imagem processada na ficha do cliente.

4.  **Conclusão:**
    * O executivo de vendas é notificado (ou simplesmente vê a imagem otimizada) no HubSpot.
