# Architecture Diagrams

## Microservices Architecture

### Services and its Databases
<img src="./services_databases_diagram.png" alt="Services and its Databases" width="700" />

### Services communication (Order Flow)
<img src="./services_order_flow_diagram.png" alt="Services communication (Order Flow)" width="700" />

## Network Infrastructure
![fiap-irango-infra](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-infra/blob/main/docs/fiap-irango-infra.png?raw=true)

## Database Architecture (RDS and Elasticache)
![fiap-irango-database](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-database/blob/main/docs/fiap-irango-database.png?raw=true)

## Kubernetes Cluster (EKS and ECR)
![fiap-irango-k8s](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-k8s/blob/main/docs/fiap-irango-k8s.png?raw=true)
![fiap-irango-k8s-cluster](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-k8s/blob/main/docs/fiap-irango-k8s-cluster.png?raw=true)

## Auth Service (API Gateway and Amazon Cognito)
![fiap-irango-auth-service](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-auth-service/blob/main/docs/fiap-irango-auth-service.png?raw=true)

## SAGA Pattern (Order Flow)
![fiap-irango-saga-pattern](https://github.com/FIAP-Tech-Challenge-53/fiap-tech-challenge/blob/main/docs/saga_pattern.png?raw=true)


### Por que escolhemos usar uma Saga Coreografada

Escolhemos usar uma **saga coreografada** para a comunicação entre os serviços de **pedido**, **cozinha** e **pagamento** por várias razões relacionadas à descentralização e autonomia dos microserviços:

1. **Autonomia dos Microserviços**:
   - Na saga coreografada, cada serviço reage a eventos emitidos por outros serviços, sem depender de um orquestrador central. Isso proporciona maior autonomia para cada serviço, facilitando o desenvolvimento e a manutenção.

2. **Baixo Acoplamento**:
   - A comunicação entre os serviços é feita através de eventos, o que reduz o acoplamento entre eles. Isso permite que serviços se comuniquem sem depender diretamente uns dos outros.

3. **Escalabilidade**:
   - A coreografia permite que cada serviço escale de forma independente, sem a necessidade de um orquestrador central. Isso é especialmente útil em sistemas onde as operações podem crescer significativamente.

4. **Resiliência**:
   - Cada serviço pode lidar com falhas e eventos de maneira independente. Se um serviço encontrar um problema, ele pode emitir um evento de erro ou compensação, sem que outros serviços sejam diretamente impactados.

5. **Flexibilidade**:
   - A saga coreografada permite que novos serviços sejam adicionados ao sistema com facilidade, apenas consumindo eventos existentes ou emitindo novos eventos conforme necessário.

Essa abordagem ajuda a manter um sistema modular e flexível, alinhado com os princípios de arquitetura de microserviços.

