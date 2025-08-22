# AWS Serverless Order Processing

Projeto final do **Programa AWS Developer** da Escola da Nuvem, implementando um sistema de processamento de pedidos **orientado a eventos** usando serviços totalmente gerenciados da AWS.

Arquitetura principal:
- Entrada de pedidos via **API Gateway** e via **arquivos S3**
- Validação com **AWS Lambda**
- Processamento assíncrono com **SQS FIFO/Standard**
- Orquestração com **EventBridge**
- Persistência em **DynamoDB**
- Alertas de erro via **SNS**
- Resiliência com **DLQs**
- Infraestrutura como Código com **CloudFormation**

---

## 🧱 Arquitetura Geral
![Arquitetura Geral](assets/architecture-diagram.png)

---

## 📌 Ingestão de Pedidos via API e EventBridge

**Componentes:**
- API Gateway (rota `/pedidos` POST)  
- Lambda de pré-validação  
- Fila SQS FIFO  
- EventBridge Custom Bus  

**Diagrama e prints:**

![API Gateway](assets/day1/api-gateway.png)  
![Lambda Pré-Validação](assets/day1/lambda-pre-validacao.png)  
![SQS FIFO](assets/day1/sqs-fifo.png)  
![Event Bus](assets/day1/event-bus.png)  

---

## 📌 Ingestão de Arquivos via S3 e Rastreamento

**Componentes:**
- Bucket S3 recebendo arquivos `.json`  
- DynamoDB para histórico  
- SNS notificando falhas  
- Lambda de validação com variáveis de ambiente  

**Diagrama e prints:**

![Bucket S3](assets/day2/s3-bucket.png)  
![DynamoDB Controle](assets/day2/dynamodb-controle.png)  
![SNS Tópico](assets/day2/sns-topic.png)  
![Lambda Validação S3](assets/day2/lambda-s3-validation.png)  

---

## 📌 Processamento Central e Persistência

**Componentes:**
- EventBridge Rule para roteamento de pedidos validados  
- SQS Pedidos Pendentes  
- Lambda Processa-Pedidos  
- DynamoDB com pedidos processados  

**Diagrama e prints:**

![EventBridge Rule](assets/day3/event-bridge-rule.png)  
![SQS Pedidos Pendentes](assets/day3/sqs-pedidos-pendentes.png)  
![Lambda Processa Pedidos](assets/day3/lambda-processa-pedidos.png)  
![DynamoDB Pedidos](assets/day3/dynamodb-pedidos-db.png)  

---

## 📌 Cancelamento e Alteração + DLQs

**Componentes:**
- EventBridge Rules CancelarPedido e AlterarPedido  
- Filas SQS específicas (com DLQs)  
- Lambdas para cancelar ou alterar pedidos  
- DynamoDB refletindo status alterado/cancelado  

**Diagrama e prints:**

![EventBridge Rules Cancelar/Alterar](assets/day4/eventbridge-rules-cancel-altera.png)  
![SQS Cancelamento](assets/day4/sqs-cancela.png)  
![SQS Alteração](assets/day4/sqs-altera.png)  
![Lambda Cancela Pedido](assets/day4/lambda-cancela.png)  
![Lambda Altera Pedido](assets/day4/lambda-altera.png)  

---

## ✅ Prova de Funcionamento

- Pedido recebido via EventBridge → SQS  
- Processamento pela Lambda  
- Persistência no DynamoDB (`PEDIDO_PROCESSADO`)  

![Logs Processa Pedido](assets/proof/logs-processa-pedido.png)
