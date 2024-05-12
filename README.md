# iRango

This repository is a monorepo to iRango project repositories. Proposed as a Tech Challenge for the Software Architecture Postgraduate Course at FIAP.

| Repository | Content |
| :---:   | :---: |
| [fiap-irango-infra](https://github.com/m4tob/fiap-irango-infra) | Repository to configure the network infrastructure and general cloud dependencies to run iRango project. (In AWS using Terraform as IaC) |
| [fiap-irango-database](https://github.com/m4tob/fiap-irango-database) | Repository to configure the databases used in iRango project. (In AWS using Terraform as IaC) |
| [fiap-irango-k8s](https://github.com/m4tob/fiap-irango-k8s) | Repository to configure Kubernetes cluster to handle iRango containers. (In AWS using Terraform as IaC)|
| [fiap-irango-auth-service](https://github.com/m4tob/fiap-irango-auth-service) | Repository to configure AWS Cognito and API Gateway. (Using Terraform as IaC) |
| [fiap-irango-api](https://github.com/m4tob/fiap-irango-api) | Repository containing iRango API main service |

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

We also need create a Secrets Manager with the following ENVs: `SENTRY_DSN`, `DB_HOSTNAME`, `DB_USERNAME`, `DB_PASSWORD`, `REDIS_HOSTNAME`. It will be used in `fiap-irango-k8s` project to deploy API pods.

### Running Project Locally
1 - Run `fiap-irango-infra` terraform files

2 - Run `fiap-irango-database` terraform files

3 - Run `fiap-irango-k8s` terraform files

4 - Build a Docker image in `fiap-irango-api` repository

5 - Apply `fiap-irango-k8s` kubernetes files passing the image built

6 - Run Run `fiap-irango-auth-service` terraform files

### CI/CD
Each repository has at least one workflow file whick is runned with push actions on main branch. Terraform projects has 3 different workflows:
  - Terraform Plan - Check cloud changes and comment on PullRequest. It will run when we open a Pull Request with changes in terraform files.
  - Terraform Apply - Create or change all resources on cloud. It will run when we push on main branch. Also can be run manually
  - Terraform Destroy - Delete all created resources on cloud. It can be run manually onlye.

We also have two releases workflows:
  - [release.yml](https://github.com/m4tob/fiap-irango-api/blob/main/.github/workflows/release.yml) - Build Docker image and push to ECR Repository. Alto trigger the next release workflow.
  - [release-api.yml](https://github.com/m4tob/fiap-irango-k8s/blob/main/.github/workflows/release-api.yml) - Using previous built Docker image, apply Kubernetes api files.
    
