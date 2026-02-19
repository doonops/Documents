# 🚀 Dynamic AWS EKS Setup using Terraform (Production DevOps Style)

This project provisions a **fully dynamic AWS EKS cluster** using Terraform best practices:

* ✅ Variables
* ✅ Data Sources
* ✅ Locals
* ✅ Modular Architecture
* ✅ Reusable Configuration

This is how real DevOps teams structure Terraform for production workloads.

---

## 🧰 Tech Stack

* Terraform
* AWS EKS
* AWS VPC
* AWS CLI
* kubectl

---

## 📌 Infrastructure Overview

This Terraform setup dynamically creates:

* VPC
* Public & Private Subnets
* NAT Gateway
* EKS Control Plane
* Managed Node Group
* IAM Roles
* Security Groups

---

## 📁 Project Structure

```
terraform-eks/
│
├── providers.tf
├── variables.tf
├── data.tf
├── locals.tf
├── main.tf
├── outputs.tf
└── terraform.tfvars
```

---

# 🔧 Prerequisites

Install:

```bash
terraform -version
aws --version
kubectl version --client
```

Configure AWS:

```bash
aws configure
```

Verify identity:

```bash
aws sts get-caller-identity
```

---

# 🌍 providers.tf

```hcl
provider "aws" {
  region = var.region
}
```

---

# 🔢 variables.tf

```hcl
variable "region" {
  description = "AWS region"
  type        = string
  default     = "ap-south-1"
}

variable "cluster_name" {
  description = "EKS cluster name"
  type        = string
  default     = "devops-eks"
}

variable "environment" {
  description = "Environment name"
  type        = string
  default     = "dev"
}

variable "node_instance_type" {
  description = "EC2 instance type for worker nodes"
  type        = string
  default     = "t3.medium"
}

variable "desired_size" {
  description = "Desired number of nodes"
  type        = number
  default     = 2
}
```

---

# 📊 data.tf

```hcl
data "aws_availability_zones" "available" {}

data "aws_caller_identity" "current" {}

data "aws_region" "current" {}
```

---

# 🧠 locals.tf

```hcl
locals {
  name = "${var.cluster_name}-${var.environment}"

  tags = {
    Environment = var.environment
    Terraform   = "true"
    Project     = "EKS-DevOps"
  }
}
```

---

# 🌐 main.tf

## VPC Module

```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "~> 5.0"

  name = local.name
  cidr = "10.0.0.0/16"

  azs = slice(data.aws_availability_zones.available.names, 0, 2)

  private_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
  public_subnets  = ["10.0.101.0/24", "10.0.102.0/24"]

  enable_nat_gateway = true
  single_nat_gateway = true

  tags = local.tags
}
```

---

## EKS Module

```hcl
module "eks" {
  source  = "terraform-aws-modules/eks/aws"
  version = "~> 20.0"

  cluster_name    = local.name
  cluster_version = "1.29"

  vpc_id     = module.vpc.vpc_id
  subnet_ids = module.vpc.private_subnets

  eks_managed_node_groups = {
    default = {
      instance_types = [var.node_instance_type]
      desired_size   = var.desired_size
      min_size       = 1
      max_size       = 3
    }
  }

  tags = local.tags
}
```

---

# 📤 outputs.tf

```hcl
output "cluster_name" {
  value = module.eks.cluster_name
}

output "cluster_endpoint" {
  value = module.eks.cluster_endpoint
}
```

---

# 📝 terraform.tfvars

```hcl
region              = "ap-south-1"
cluster_name        = "my-eks"
environment         = "dev"
node_instance_type  = "t3.medium"
desired_size        = 2
```

---

# ▶️ Deploy Infrastructure

## Initialize

```bash
terraform init
```

## Plan

```bash
terraform plan
```

## Apply

```bash
terraform apply
```

Type:

```
yes
```

⏱ Cluster creation time: ~15–20 minutes

---

# 🔗 Connect kubectl to EKS

```bash
aws eks update-kubeconfig \
  --region ap-south-1 \
  --name my-eks-dev
```

Verify:

```bash
kubectl get nodes
```

---

# 🚀 Deploy Test Application

```bash
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --type=LoadBalancer --port=80
kubectl get svc
```

Copy the `EXTERNAL-IP` and open in browser.

---

# 🧹 Destroy Infrastructure (Important)

```bash
terraform destroy
```

Type:

```
yes
```

This deletes:

* EKS cluster
* VPC
* Subnets
* NAT Gateway
* Load Balancers
* IAM Roles

---

# 🏢 Production Enhancements

In real enterprise setups, teams also implement:

* Remote backend (S3 + DynamoDB)
* GitHub Actions CI/CD
* Helm charts
* ECR integration
* ArgoCD GitOps
* Separate environments (dev/stage/prod)

---

# 📌 DevOps Workflow (Industry Standard)

```
Developer Push
    ↓
GitHub Actions
    ↓
Docker Build
    ↓
Push to ECR
    ↓
Terraform Apply (Infra)
    ↓
Helm Deploy to EKS
```

---

