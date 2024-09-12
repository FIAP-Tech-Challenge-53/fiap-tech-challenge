# iRango

This repository is a monorepo to iRango project repositories. Proposed as a Tech Challenge for the Software Architecture Postgraduate Course at FIAP.

## Terraform Repositories
| Repository | Content |
| :---   | --- |
| [fiap-irango-infra](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-infra) | Repository to configure the network infrastructure and general cloud dependencies to run iRango project. (In AWS using Terraform as IaC) |
| [fiap-irango-database](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-database) | Repository to configure the databases used in iRango project. (In AWS using Terraform as IaC) |
| [fiap-irango-k8s](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-k8s) | Repository to configure Kubernetes cluster to handle iRango containers. (In AWS using Terraform as IaC)|
| [fiap-irango-auth-service](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-auth-service) | Repository to configure AWS Cognito and API Gateway. (Using Terraform as IaC) |

## Application Repositories
| Repository | Content |
| :---:   | :---: |
| [fiap-irango-order-api](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-order-api) | Service to handle information related to Consumidor, Produto, and Pedido |
| [fiap-irango-payment-api](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-payment-api) | Service to handle information related to Pagamento |
| [fiap-irango-cook-api](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-cook-api) | Service to handle information related to Cooking process |

## Archictecture Diagrams
[Archictecture Diagrams](./docs/architecture-diagrams.md)

### Cloning repository
```bash
git clone --recurse-submodules git@github.com:m4tob/fiap-tech-challenge.git
```

### Updating submodules
```bash
git submodule update --init --recursive
```

### Dependencies
First of all, we need create an S3 bucket to store Terraform state. This bucket must be configured in each main.tf file.

We also need create a Secrets Manager with the name `fiap-irango-secrets-api` in JSON format with the following ENVs: `SENTRY_DSN`, `DB_HOSTNAME`, `DB_USERNAME`, `DB_PASSWORD`, `DB_PAYMENT_HOSTNAME`, `REDIS_HOSTNAME`, `MONGO_HOSTNAME`, `MONGO_DATABASE`, `MONGO_USERNAME`, `MONGO_PASSWORD`, `MONGO_ATLAS_ORG_ID`, `MONGODB_ATLAS_PUBLIC_KEY`, `MONGODB_ATLAS_PRIVATE_KEY`, `PERSONAL_ACCESS_TOKEN`, `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`. It will be used by some terraform workflows and also in `fiap-irango-k8s` project to deploy API pods. Ex:
```json
{
  "SENTRY_DSN": "https://public@sentry.example.com/1",
  "DB_HOSTNAME": "fiap-irango-rds.rds.amazonaws.com",
  "DB_USERNAME": "root",
  "DB_PASSWORD": "password",
  "DB_PAYMENT_HOSTNAME": "fiap-irango-payment-rds.rds.amazonaws.com",
  "REDIS_HOSTNAME": "fiap-irango-cache.cache.amazonaws.com",
  "MONGO_HOSTNAME": "fiap-irango-cluster.mongodb.net",
  "MONGO_DATABASE": "irango_cook",
  "MONGO_USERNAME": "root",
  "MONGO_PASSWORD": "password",
  "MONGO_ATLAS_ORG_ID": "XXXXXXXXXXXXXXXXXXXXXXXX",
  "MONGODB_ATLAS_PUBLIC_KEY": "public_key",
  "MONGODB_ATLAS_PRIVATE_KEY": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "PERSONAL_ACCESS_TOKEN": "ghp_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  "AWS_ACCESS_KEY_ID": "XXXXXXXXXXXXXXXXXXXX",
  "AWS_SECRET_ACCESS_KEY": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
}
```

### Running Project Locally
1 - Run `fiap-irango-infra` terraform files

2 - Run `fiap-irango-database` terraform files

3 - Run `fiap-irango-k8s` terraform files

4 - Build a Docker image in `fiap-irango-order-api`, `fiap-irango-payment-api` and `fiap-irango-cook-api`, repositories. Export the Image URI as envs.

5 - Apply `fiap-irango-k8s` kubernetes passing secrets and Image URI envs using `envsubst` [README.md](`https://github.com/FIAP-Tech-Challenge-53/blob/main/README.md#without-make`).

6 - Run `fiap-irango-auth-service` terraform files

### CI/CD
Each repository has at least one workflow file whick is runned with push actions on main branch. Terraform projects has 3 different workflows:
  - Terraform Plan - Check cloud changes and comment on PullRequest. It will run when we open a Pull Request with changes in terraform files.
  - Terraform Apply - Create or change all resources on cloud. It will run when we push on main branch. Also can be run manually
  - Terraform Destroy - Delete all created resources on cloud. It can be run manually onlye.

We also have two releases workflows:
  - [release.yml (order)](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-order-api/blob/main/.github/workflows/release.yml) - Build order-api Docker image and push to ECR Repository.
  - [release-api.yml (order)](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-k8s/blob/main/.github/workflows/release-order.yml) - Using previous built Docker image, apply Kubernetes api files.
  - [release.yml (payment)](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-payment-api/blob/main/.github/workflows/release.yml) - Build payment-api Docker image and push to ECR Repository.
  - [release-payment.yml (payment)](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-k8s/blob/main/.github/workflows/release-payment.yml) - Using previous built Docker image, apply Kubernetes api files.
  - [release.yml (cook)](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-cook-api/blob/main/.github/workflows/release.yml) - Build cook-api Docker image and push to ECR Repository.
  - [release-cook.yml (cook)](https://github.com/FIAP-Tech-Challenge-53/fiap-irango-k8s/blob/main/.github/workflows/release-cook.yml) - Using previous built Docker image, apply Kubernetes api files.
    
