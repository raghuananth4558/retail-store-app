# Retail Store Sample App - GitOps with Amazon EKS Auto Mode
 
![Banner](./docs/images/banner.png)
#


  <strong>
  <h2>AWS Containers Retail Sample</h2>
  </strong>
</div>

This is a sample application designed to illustrate various concepts related to containers on AWS. It presents a sample retail store application including a product catalog, shopping cart and checkout, deployed using modern DevOps practices including GitOps and Infrastructure as Code.

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Branch Strategy](#branch-strategy)
- [Getting Started](#getting-started)
- [GitOps Workflow](#gitops-workflow)
- [EKS Auto Mode](#eks-auto-mode)
- [Infrastructure Components](#infrastructure-components)
- [CI/CD Pipeline](#cicd-pipeline)
- [Monitoring and Observability](#monitoring-and-observability)
- [Cleanup](https://github.com/LondheShubham153/retail-store-sample-app/blob/main/README.md#step-12-cleanup)
- [Troubleshooting](#troubleshooting)

## Overview

The Retail Store Sample App demonstrates a modern microservices architecture deployed on AWS EKS using GitOps principles. The application consists of multiple services that work together to provide a complete retail store experience:


- **UI Service**: Java-based frontend
- **Catalog Service**: Go-based product catalog API
- **Cart Service**: Java-based shopping cart API
- **Orders Service**: Java-based order management API
- **Checkout Service**: Node.js-based checkout orchestration API
- **Caching Service**: Redis


## Application Architecture

The application has been deliberately over-engineered to generate multiple de-coupled components. These components generally have different infrastructure dependencies, and may support multiple "backends" (example: Carts service supports MongoDB or DynamoDB).

![Architecture](https://github.com/aws-containers/retail-store-sample-app/raw/main/docs/images/architecture.png)

| Component                  | Language | Container Image                                                             | Helm Chart                                                                        | Description                             |
| -------------------------- | -------- | --------------------------------------------------------------------------- | --------------------------------------------------------------------------------- | --------------------------------------- |
| [UI](./src/ui/)            | Java     | [Link](https://gallery.ecr.aws/aws-containers/retail-store-sample-ui)       | [Link](src/ui/chart/values.yaml)    | Store user interface                    |
| [Catalog](./src/catalog/)  | Go       | [Link](https://gallery.ecr.aws/aws-containers/retail-store-sample-catalog)  | [Link](src/catalog/chart/values.yaml)  | Product catalog API                     |
| [Cart](./src/cart/)        | Java     | [Link](https://gallery.ecr.aws/aws-containers/retail-store-sample-cart)     | [Link](src/cart/chart/values.yaml)     | User shopping carts API                 |
| [Orders](./src/orders)     | Java     | [Link](https://gallery.ecr.aws/aws-containers/retail-store-sample-orders)   | [Link](src/orders/chart/values.yaml)   | User orders API                         |
| [Checkout](./src/checkout) | Node     | [Link](https://gallery.ecr.aws/aws-containers/retail-store-sample-checkout) | [Link](src/checkout/chart/values.yaml) | API to orchestrate the checkout process |


## Infrastructure Architecture

The Infrastructure Architecture follows cloud-native best practices:

- **Microservices**: Each component is developed and deployed independently
- **Containerization**: All services run as containers on Kubernetes
- **GitOps**: Infrastructure and application deployment managed through Git
- **Infrastructure as Code**: All AWS resources defined using Terraform
- **CI/CD**: Automated build and deployment pipelines with GitHub Actions

![EKS](docs/images/EKS.gif)



## Quick Start

**For a Quick deployment** Follow these steps for a basic deployment:

1. **Install Prerequisites**: AWS CLI, Terraform, kubectl, Docker, Helm
2. **Configure AWS**: `aws configure` with appropriate credentials
3. **Clone Repository**: `git clone https://github.com/raghuananth4558/retail-store-app.git`
4. **Deploy Infrastructure**: Run Terraform in two phases (see [Getting Started](#getting-started))
5. **Access Application**: Get load balancer URL and browse the retail store

**Need advanced GitOps workflow?** See [BRANCHING_STRATEGY.md](./BRANCHING_STRATEGY.md) for automated CI/CD setup.

## Branch Strategy

This repository uses a **dual-branch approach** for different deployment scenarios:

### 🌐 **Public Application (Main Branch)**
- **Purpose**: Simple deployment with public images
- **Images**: Public ECR (stable versions like v1.2.2)
- **Deployment**: Manual control with umbrella chart
- **Updates**: Manual only
- **Best for**: Demos, learning, quick testing, simple deployments

### 🏭 **Production (GitOps Branch)**
- **Purpose**: Full production workflow with CI/CD pipeline
- **Images**: Private ECR (auto-updated with commit hashes)
- **Deployment**: Automated via GitHub Actions
- **Updates**: Automatic on code changes
- **Best for**: Production environments, automated workflows, enterprise deployments

> **📚 For detailed branching strategy, CI/CD setup, and advanced workflows, see [BRANCHING_STRATEGY.md](./BRANCHING_STRATEGY.md)**

## Getting Started

### Prerequisites

1. **Install Prerequisites**: AWS CLI, Terraform, kubectl, Docker, Helm
2. **Configure AWS**: `aws configure` with appropriate credentials
3. **Clone Repository**: `git clone https://github.com/raghuananth4558/retail-store-app.git`
4. **Deploy Infrastructure**: Run Terraform in two phases (see [Getting Started](#getting-started))
5. **Access Application**: Get load balancer URL and browse the retail store

### **Required Tools**

| Tool          | Version | Installation                                                                         |
| ------------- | ------- | ------------------------------------------------------------------------------------ |
| **AWS CLI**   | v2+     | [Install Guide](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) |
| **Terraform** | 1.0+    | [Install Guide](https://developer.hashicorp.com/terraform/install)                   |
| **kubectl**   | 1.33+   | [Install Guide](https://kubernetes.io/docs/tasks/tools/)                             |
| **Docker**    | 20.0+   | [Install Guide](https://docs.docker.com/get-docker/)                                 |
| **Helm**      | 3.0+    | [Install Guide](https://helm.sh/docs/intro/install/)                                 |
| **Git**       | 2.0+    | [Install Guide](https://git-scm.com/downloads) 

Follow these steps to **install Prerequisites:**


### **Quick Installation Scripts**

<details>
<summary><strong>🔧 One-Click Installation</strong></summary>

```bash
#!/bin/bash
# Install all prerequisites

# AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Terraform
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform

# kubectl
curl -LO "https://dl.k8s.io/release/v1.33.3/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/

# Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Helm
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# Verify installations
aws --version
terraform --version
kubectl version --client
docker --version
helm version
```

</details>


## Follow these steps to deploy the application:

### Step 1. Configure AWS with **`Root User`** Credentials:
#

### Create an Access key
![image](docs/images/a01.png)
#

  Ensure your AWS CLI is configured with the **Root user credentials:**

```sh
aws configure
```
### Aws configured and installed all the prerequisites 
![image](docs/images/b02.png)
#

### Step 2. Clone the Repository:

```sh
git https://github.com/raghuananth4558/retail-store-app.git
```
#

### Add timeout = 600 in argocd.tf in terraform folder
![image](docs/images/c03.png)
#

> [!IMPORTANT]
> ### Step 3: Choose Your Deployment Strategy
>
> **For Public Application (Main Branch):**
> - Uses stable public ECR images (v1.2.2)
> - Manual deployment control
> - No GitHub Actions required
> - Skip to Step 4 - infrastructure is ready
>
> **For Production (GitOps Branch):**
> - Uses private ECR with automated CI/CD
> - Requires GitHub Actions setup
> - See [BRANCHING_STRATEGY.md](./BRANCHING_STRATEGY.md) for complete setup


### Step 4. Deploy Infrastructure with Terraform:

> [!IMPORTANT]
> ### Before applying terraform apply to the entire code, we need to create vpc and eks cluster and then map these two with our remaining resources
>
 ```sh
 cd terraform
 terraform init
 terraform apply --target=module.vpc
 ```
 ### terraform init
 ![image](docs/images/d04.png)
 #

 ### Creating a vpc
 ![image](docs/images/e05.png)
 #

 ### Verify if the vpc is created in our aws console
 ![image](docs/images/e06.png)
 #

### Now we will create an eks cluster
```sh
terraform apply --target=module.retail_app_eks
```
### Creating an eks cluster 
![image](docs/images/f06.png)

### Eks cluster created
![image](docs/images/f07.png)

### Verify the eks cluster in aws console
![image](docs/images/f08.png)
#

### Now we can apply terraform to our remaining resources

```sh
terraform apply --auto-approve
```
![image](docs/images/h08.png)
#

This creates the core infrastructure, including:
- VPC with public and private subnets
- Amazon EKS cluster with Auto Mode enabled
- Security groups and IAM roles

And deploys:
- ArgoCD for Setup GitOps
- NGINX Ingress Controller
- Cert Manager for SSL certificates

### The core infrastructure is created
![image](docs/images/i09.png)
#


### Step 5: Update kubeconfig to Access the Amazon EKS Cluster:
```
aws eks update-kubeconfig --name retail-store --region <region>
```
### Kubeconfig update
![image](docs/images/g07.png)
#


> Application is live with Public image:

- Get your ingress EXTERNAL-IP and paste it in the browser to access retail-store application.
    ```sh
    kubectl get svc -n ingress-nginx
    ```

> [!NOTE]
> Let's move forward with GitOps principle utilising Amazon private registry to create private registry and store images.

### Step 6: GitHub Actions (Production branch)

> **Note**: This step is only required if you're using the **Production branch** for automated deployments. Skip this step if using the **Public Application branch** for simple deployment.

For GitHub Actions, first configure secrets so the pipelines can be automatically triggered:

**Create an IAM User, policies, and generate credentials**

### IAM user git-ops admin created
![image](docs/images/L12.png)
#

### Access key generated for the IAM user
![image](docs/images/L13.png)
#

**Go to your GitHub repo → Settings → Secrets and variables → Actions → New repository secret.**


| Secret Name           | Value                              |
|-----------------------|------------------------------------|
| `AWS_ACCESS_KEY_ID`   | `Your AWS Access Key ID`           |
| `AWS_SECRET_ACCESS_KEY` | `Your AWS Secret Access Key`     |
| `AWS_REGION`          | `region-name`                       |
| `AWS_ACCOUNT_ID`        | `your-account-id` |

### Adding a new secret AWS_REGION
![image](docs/images/L14.png)
#

### All the 4 secrets are added in the github
![image](docs/images/L15.png)
#



> [!IMPORTANT]
> Once the entire cluster is created, any changes pushed to the repository will automatically trigger GitHub Actions.

### Few changes made in the src folder
![image](docs/images/m13.png)
#

GitHub Actions will automatically build and push the updated Docker images to Amazon ECR.

![image](docs/images/m14.png)
#

![image](docs/images/n15.png)
#

![image](docs/images/n16.png)
#

# Check the ECR repository for the pushed images
![image](docs/images/o15.png)


### Verify Deployment

Check if the nodes are running:

```bash
kubectl get nodes
```
#

Check if all the pods are running

```bash
kubectl gets pods -A
```

### All the pods are running
![image](docs/images/i10.png)
#

### Step 7: Access the Application:

The application is exposed through the NGINX Ingress Controller. Get the load balancer URL:

```bash
kubectl get svc -n ingress-nginx
```
![image](docs/images/i11.png)
#

Use the EXTERNAL-IP of the ingress-nginx-controller service to access the application.

![image](docs/images/j10.png)
#

### Test the application by placing an order
![image](docs/images/j11.png)

![image](docs/images/j12.png)

![image](docs/images/j13.png)

![image](docs/images/j14.png)

![image](docs/images/j15.png)
#


### Step 8: Argo CD Automated Deployment:

**Verify ArgoCD installation**

```
kubectl get pods -n argocd
```
![image](docs/images/k11.png)
#

### Step 9: Port-forward to Argo CD UI and login:
#


**Port-forward to Argo CD UI**
```
kubectl port-forward svc/argocd-server -n argocd 8080:443 &
```
![image](docs/images/k12.png)
#

**Get ArgoCD admin password**
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath='{.data.password}' | base64 -d
```
![image](docs/images/k14.png)
#


Open your browser and navigate to:
https://localhost:8080
### ArgoCD accessed
![image](docs/images/k13.png)
#

Username: admin 

Password: <output of previous command>

### Step 10: Access ArgoCD UI

Once ArgoCD is deployed, you can access the web interface:

![image](docs/images/k15.png)
#

The ArgoCD UI provides:
- **Application Status**: Real-time sync status of all services
- **Resource View**: Detailed view of Kubernetes resources
- **Sync Operations**: Manual sync and rollback capabilities
- **Health Monitoring**: Application and resource health status

### Step 11: Monitor Application Deployment

```bash
kubectl get pods -n retail-store
kubectl get ingress -n retail-store
```

### Step 12: Cleanup
To delete all resources created by Terraform:
```
terraform destroy --auto-approve
```

![image](docs/images/p16.png)
#


> [!NOTE]
> ECR Repositories you need to Delete it from AWS Console Manually.



## Troubleshooting

### Common Issues

#### **Image Pull Errors**
```
Error: Failed to pull image "123456789012.dkr.ecr.us-east-1.amazonaws.com/retail-store-ui:abc1234"
```
**Solutions**:
1. Ensure you're using the correct branch for your deployment strategy
2. For Production branch: Check GitHub Actions completed successfully
3. For Public Application branch: Verify you're using public ECR images
4. Check AWS credentials and ECR permissions

#### **GitHub Actions Not Triggering**
**Solutions**:
1. Ensure changes are in `src/` directory
2. Verify you're on the `production` branch (gitops)
3. Check GitHub Actions is enabled in repository settings
4. Review [BRANCHING_STRATEGY.md](./BRANCHING_STRATEGY.md) for detailed setup

### Getting Help

- **Basic deployment issues**: Check this README
- **Advanced GitOps issues**: See [BRANCHING_STRATEGY.md](./BRANCHING_STRATEGY.md)
- **Infrastructure issues**: Review Terraform logs
- **Application issues**: Check ArgoCD UI and kubectl logs

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](./LICENSE) file for details.


---

<div align="center">

**🔄 For advanced GitOps workflows, see [BRANCHING_STRATEGY.md](./BRANCHING_STRATEGY.md)**

</div>

