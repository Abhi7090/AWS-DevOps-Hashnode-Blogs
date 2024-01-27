---
title: "Three Tier Application Deployment on Kubernetes"
datePublished: Wed Jan 24 2024 15:07:43 GMT+0000 (Coordinated Universal Time)
cuid: clrrx605e000009jsexcag995
slug: three-tier-application-deployment-on-kubernetes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1706105377788/b0209ea8-99d7-4822-9a40-f0be2415bd03.jpeg

---

## Prerequisites

1. Basic knowledge of Docker, and AWS services.
    
2. An AWS account with necessary permissions.
    
3. Clone the git : [https://github.com/Abhi7090/TWSThreeTierAppChallenge.git](https://github.com/Abhi7090/TWSThreeTierAppChallenge.git)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706107312005/f3e7c08d-b7fb-47ce-8e07-43dcefb27818.jpeg align="center")
    

Step 1 : IAM Configuration

1. Create a user `eks-admin` with `AdministratorAccess`.
    
2. Generate Security Credentials: Access Key and Secret Access Key.
    
3. Make new project directory in ubuntu machine and note that all the installation should be done with outside project directory
    

Step 2 : Create EC2 instance with AMI machine t2.micro and add ports in security groups for backend deployment : 8080, frontend deployment : 3000

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706106132563/60cbf5ad-4bbd-4b40-b562-3d900ac2714c.jpeg align="center")

Step 3 : Install AWS CLI v2

```dockerfile
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install -i /usr/local/aws-cli -b /usr/local/bin --update
aws configure
```

Step 4 : Install Docker

```dockerfile
sudo apt-get update
sudo apt install docker.io
docker ps
sudo chown $USER /var/run/docker.sock
```

Step 5 : Install kubectl

```dockerfile
curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client
```

Step 6 : Install eksctl

```dockerfile
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```

Step 7 : Create ECR images for both frontend and backend

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706107456648/34325186-c02f-47ba-b567-dd2bbdc1da52.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706107517662/0ba3bfa5-fb95-49c7-91c2-7484091c7fc1.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706107535604/f31edeb7-722a-4529-9a12-0c1f26e12120.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706107547788/66d99f11-0536-42c9-95dd-8e2389aa778b.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706107560574/7902bcab-7948-41ec-8cdf-ce2076668503.jpeg align="center")

Step 8 : Setup EKS Cluster in the region region us-west-2 with t2.medium with min nod 2 and max with 2

```dockerfile
eksctl create cluster --name three-tier-cluster --region us-west-2 --node-type t2.medium --nodes-min 2 --nodes-max 2
aws eks update-kubeconfig --region us-west-2 --name three-tier-cluster
kubectl get nodes
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706107585851/a9b06646-6971-4a6f-9eb8-eee112acc984.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706107595743/bb873476-a841-40aa-92bb-1f44686cb043.jpeg align="center")

Step 10 : Create namespace and Run Manifests files

```dockerfile
kubectl create namespace workshop
kubectl apply -f .
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706107810084/c236559a-f0a6-4cea-862e-03b3303d3ae5.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706107818397/10efa79e-3452-4a3e-97e0-35f9af0f122f.jpeg align="center")

Step 11 : Install AWS Load Balancer

```dockerfile
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json
aws iam create-policy --policy-name AWSLoadBalancerControllerIAMPolicy --policy-document file://iam_policy.json
eksctl utils associate-iam-oidc-provider --region=us-west-2 --cluster=three-tier-cluster --approve
eksctl create iamserviceaccount --cluster=three-tier-cluster --namespace=kube-system --name=aws-load-balancer-controller --role-name AmazonEKSLoadBalancerControllerRole --attach-policy-arn=arn:aws:iam::626072240565:policy/AWSLoadBalancerControllerIAMPolicy --approve --region=us-west-2
```

Step 10 : Install Helm and Deploy AWS Load Balancer Controller

```dockerfile
sudo snap install helm --classic
helm repo add eks https://aws.github.io/eks-charts
helm repo update eks
helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=my-cluster --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller
kubectl get deployment -n kube-system aws-load-balancer-controller
kubectl apply -f full_stack_lb.yaml
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706107858900/7374ce10-c21e-41ed-ae9d-b0c68c09a173.jpeg align="center")

Step 11 : Add ECR images (frontend and backend) from AWS ECR to the frontend-deloyment.yaml and backend-deployment.yaml, Check the backend service is connected with Database or not

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706107958235/901c5b14-3e56-4e7e-834c-df07632376bc.jpeg align="center")

Step 12 : Check with the ingress alb address and edit the address in the full\_stack\_lb.yaml (host) and frontend\_deployment.yaml (env value), apply these yamle files again (kubectl apply -f frontend\_deployment.yaml and full\_stack\_lb.yaml)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706108051625/9cd358bc-955a-42ae-93a9-f2ae0b9083d1.jpeg align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706108308663/d7b434e5-a90b-4303-b5c8-6c50976fa661.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706108347032/72287dba-ac1c-43b2-9362-fad97d783b32.png align="center")

Step 13 : Copy the Ingress application load balancer address and paste in the new tab

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706108422910/88c20dd3-2e8c-49cb-85fb-516df232ccd7.jpeg align="center")

Step 14 : Check the data in the Mongo database

```dockerfile
kubectl exec -it mongodb-797968fbc5-v99wx -n workshop -- /bin/sh
mongo
show dbs
use todo
db.tasks.find()
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1706108540735/479c7a11-279d-4e28-817d-55913a7b82a8.jpeg align="center")

Step 15 : Clean or delete the EKS cluster

```dockerfile
eksctl delete cluster --name three-tier-cluster --region us-west-2
```