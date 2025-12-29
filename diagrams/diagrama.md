Arquitetura Serverless - Farmácia Virtual Abstergo Industries

Este diagrama representa o fluxo de dados e a integração entre os serviços AWS selecionados para a otimização de custos e escalabilidade da plataforma.

graph TD
    %% Usuário Final
    User((Usuário / Cliente)) -->|Acessa App/Site| CloudFront[Amazon CloudFront]
    
    %% Camada de Entrega
    CloudFront -->|Requisição de Imagens/Arquivos| S3[Amazon S3]
    CloudFront -->|Requisição de API| APIGateway[Amazon API Gateway]

    %% Camada de Computação (Etapa 1 do Relatório)
    subgraph Computacao_Serverless [AWS Lambda - Processamento de Pedidos]
        APIGateway --> Lambda[AWS Lambda]
    end

    %% Camada de Dados (Etapa 3 do Relatório)
    subgraph Armazenamento_Dados [Persistência de Dados]
        Lambda -->|Consulta Inventário / Salva Pedido| DynamoDB[(Amazon DynamoDB)]
    end

    %% Camada de Objetos (Etapa 2 do Relatório)
    subgraph Armazenamento_Objetos [Arquivos e Mídia]
        Lambda -->|Upload de Receitas| S3
        S3 -->|Busca Fotos de Produtos / Bulas| User
    end

    %% Estilização
    style Lambda fill:#f96,stroke:#333,stroke-width:2px
    style S3 fill:#5b9,stroke:#333,stroke-width:2px
    style DynamoDB fill:#48f,stroke:#333,stroke-width:2px
    style APIGateway fill:#f5f,stroke:#333,stroke-width:1px


Descrição do Fluxo

Acesso: O cliente acessa a interface da farmácia.

Arquivos Estáticos (S3): As fotos dos medicamentos e os PDFs das bulas são carregados diretamente do Amazon S3, reduzindo a carga nos servidores.

Processamento (Lambda): Quando o cliente finaliza uma compra ou anexa uma receita, o API Gateway aciona uma função AWS Lambda.

Banco de Dados (DynamoDB): A função Lambda valida o estoque e registra a transação no Amazon DynamoDB.

Segurança: Toda a comunicação entre esses serviços é controlada por regras do IAM (Identity and Access Management).

Relatório de Arquitetura criado por Leonardo Bento Maria.
