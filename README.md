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

### Running Project Locally
1 - Run `fiap-irango-infra` terraform file

2 - Run `fiap-irango-database` terraform file

3 - Run `fiap-irango-k8s` terraform file