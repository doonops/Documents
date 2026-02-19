# AWS EKS Cluster Setup — Real-World DevOps Workflow (AWS CLI + eksctl + kubectl)

I’ll show you the real-world DevOps way — using AWS CLI + eksctl + kubectl, because that’s the fastest and industry-standard method.

---

## I’ll cover:

✅ Create EKS Cluster
✅ Access cluster from local CLI
✅ Delete cluster safely

---

## ✅ 1️⃣ Prerequisites (Install First)

Make sure these are installed on your local machine:

```
aws --version
kubectl version --client
eksctl version
```

If not installed:

### Install AWS CLI

https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

### Install kubectl

https://kubernetes.io/docs/tasks/tools/

### Install eksctl

```
choco install eksctl     # Windows (Chocolatey)
# OR
winget install eksctl
```

---

## ✅ 2️⃣ Configure AWS CLI

Login using your IAM user:

```
aws configure
```

Enter:

```
AWS Access Key
AWS Secret Key
Region (ex: ap-south-1)
Output format: json
```

Check identity:

```
aws sts get-caller-identity
```

---

## ✅ 3️⃣ Create EKS Cluster (Simple Method)

This single command creates:

✔ EKS Control Plane
✔ Worker Nodes
✔ VPC + Networking

```
eksctl create cluster \
  --name my-eks-cluster \
  --region ap-south-1 \
  --nodegroup-name my-nodes \
  --node-type t3.medium \
  --nodes 2
```

👉 Creation time: 10–15 minutes

When finished you will see:

```
EKS cluster "my-eks-cluster" created
```

---

## ✅ 4️⃣ Access EKS Cluster from Local CLI

Now connect kubectl to EKS.

Run:

```
aws eks update-kubeconfig \
  --region ap-south-1 \
  --name my-eks-cluster
```

This updates:

```
~/.kube/config
```

Test connection:

```
kubectl get nodes
```

You should see:

```
ip-xxx Ready
ip-xxx Ready
```

🔥 Now your local CLI is connected to AWS EKS.

---

## ✅ 5️⃣ Verify Cluster

Check everything:

```
kubectl get pods -A
kubectl get svc
kubectl cluster-info
```

---

## ✅ 6️⃣ Deploy Test App (Optional but Recommended)

```
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --type=LoadBalancer --port=80
```

---

## ✅ 7️⃣ Delete EKS Cluster (VERY IMPORTANT — Avoid AWS Billing)

To remove EVERYTHING:

```
eksctl delete cluster \
  --name my-eks-cluster \
  --region ap-south-1
```

This deletes:

✔ Control plane
✔ Nodes
✔ Load balancers
✔ CloudFormation stacks

---

## ✅ 1️⃣ Fix kubectl (You already downloaded it)

Right now kubectl is just a file in your home folder.

Run these commands EXACTLY:

```
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

Now test:

```
kubectl version --client
```

✅ It should work.

---

## ✅ 2️⃣ Install eksctl (Correct Way in WSL Ubuntu)

Since apt repo doesn’t have eksctl, install via official tar file.

Run:

```
curl -sL https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz -o eksctl.tar.gz
```

Extract:

```
tar -xzf eksctl.tar.gz
```

Move to path:

```
sudo mv eksctl /usr/local/bin/
```

Verify:

```
eksctl version
```

