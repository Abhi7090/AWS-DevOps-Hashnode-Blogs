---
title: "Deploying Mario game on Kubernetes"
datePublished: Fri Feb 09 2024 15:58:05 GMT+0000 (Coordinated Universal Time)
cuid: clseu0f1m000109jrgfmdbefg
slug: deploying-mario-game-on-kubernetes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1707490991568/d9fa1abf-cda4-4583-8cc1-8ea8bea23ad2.jpeg

---

## **STEP 1: Launch Ubuntu instance**

1. **Sign in to AWS Console:** Log in to your AWS Management Console.
    
2. **Navigate to EC2 Dashboard:** Go to the EC2 Dashboard by selecting “Services” in the top menu and then choosing “EC2” under the Compute section.
    
3. **Launch Instance:** Click on the “Launch Instance” button to start the instance creation process.
    
4. **Choose an Amazon Machine Image (AMI):** Select an appropriate AMI for your instance. For example, you can choose Ubuntu image.
    
5. **Choose an Instance Type:** In the “Choose Instance Type” step, select `t2.micro` as your instance type. Proceed by clicking "Next: Configure Instance Details."
    
6. **Configure Instance Details:**
    

* For “Number of Instances,” set it to 1 (unless you need multiple instances).
    
* Configure additional settings like network, subnets, IAM role, etc., if necessary.
    
* For “Storage,” click “Add New Volume” and set the size to 8GB (or modify the existing storage to 8GB).
    
* Click “Next: Add Tags” when you’re done.
    

1. **Add Tags (Optional):** Add any desired tags to your instance. This step is optional, but it helps in organizing instances.
    
2. **Configure Security Group:**
    
3. **Review and Launch:** Review the configuration details. Ensure everything is set as desired.
    
4. **Select Key Pair:**
    

* Select “Choose an existing key pair” and choose the key pair from the dropdown.
    
* Acknowledge that you have access to the selected private key file.
    
* Click “Launch Instances” to create the instance.
    

1. **Access the EC2 Instance:** Once the instance is launched, you can access it using the key pair and the instance’s public IP or DNS.
    

Ensure you have necessary permissions and follow best practices while configuring security groups and key pairs to maintain security for your EC2 instance.

## **STEP 2: Create IAM role**

1. Search for IAM in the search bar of AWS and click on roles.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707491310413/a957ddc6-3867-407c-93d8-486cd360ac18.png align="center")

1. Click on create roles
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707491361512/996a7bd7-c5ce-4d55-9337-f8696af58f0f.png align="left")
    
2. Select trusted identity type as AWS Service
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707491434307/dae85817-e0df-4a96-a7c7-d8f6bbd02a7f.png align="left")
    
3. In the add permission section select administrative access and click next
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707491596513/5a1cc2f1-f93b-482a-8d3a-c8a294a20272.png align="left")
    
4. Select use case as EC2 and click on next
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707491527844/e517a03e-cec3-4f33-86cc-ab86df1c482c.png align="left")
    
5. Give role name as MARIO and click on create role
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707492206528/59442d43-b8e1-4097-90b7-0f776021368c.png align="left")
    
6. Now Attach EC2 Instance which we created along with IAM role
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707492358396/77309121-933c-4eeb-bb3d-2059f408509b.jpeg align="left")
    
    1. 1. 1. ## **STEP 3: Cluster provision**
                
                1. Now clone this Repo.
                    
                
                ```dockerfile
                git clone https://github.com/Abhi7090/k8s-mario.git
                ```
                
                ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707493589226/8b89084a-948e-4908-9b14-7448b4acc713.jpeg align="center")
                
                1. change directory
                    
                
                ```dockerfile
                cd k8s-mario
                ```
                
                1. Provide the executable permission to [script.sh](http://script.sh/) file, and run it.
                    
                
                ```dockerfile
                sudo chmod +x script.sh 
                ./script.sh
                ```
                
                ![](https://miro.medium.com/v2/resize:fit:1000/0*BmRxnWYhgpd_RYc4 align="left")
                
                1. This script will install `Terraform, AWS cli, Kubectl, Docker.`
                    
                
                Check versions
                
                ```dockerfile
                docker --version 
                aws --version 
                kubectl version --client 
                terraform --version
                ```
                
                ![](https://miro.medium.com/v2/resize:fit:1000/0*nzcFfKEv3Ki7-zRm align="left")
                
                1. Now change directory into the EKS-TF
                    
                    Run Terraform init
                    
                    `NOTE: Don't forgot to change the s3 bucket name in the backend.tf file`
                    
                
                ```dockerfile
                cd EKS-TF 
                terraform init
                ```
                
                ![](https://miro.medium.com/v2/resize:fit:1000/0*RxGYd-znL8Dkfav_ align="left")
                
                1. Now run terraform validate and terraform plan
                    
                
                ```dockerfile
                terraform validate 
                terraform plan
                ```
                
                ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707493821177/ebbd7128-9927-49a6-b6f6-a9c02150ee35.jpeg align="center")
                
                1. Now Run terraform apply to provision cluster.
                    
                
                ```dockerfile
                terraform apply --auto-approve
                ```
                
                ![](https://miro.medium.com/v2/resize:fit:1000/0*t-rQ3ShygEGesN_D align="left")
                
                1. Completed in 10mins
                    
                
                ![](https://miro.medium.com/v2/resize:fit:1000/0*dohkgMCutkwm1pSz align="left")
                
                1. Update the Kubernetes configuration
                    
                
                Make sure change your desired region
                
                ```dockerfile
                aws eks update-kubeconfig --name EKS_CLOUD --region ap-south-1
                ```
                
                ![](https://miro.medium.com/v2/resize:fit:1000/0*gikOxYGDBjCJctMC align="left")
                
                1. Now change directory back to k8s-mario
                    
                
                Let’s apply the deployment and service
                
                **Deployment**
                
                ```dockerfile
                kubectl apply -f deployment.yaml 
                #to check the deployment 
                kubectl get all
                ```
                
                ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707493938999/b4ed7278-9ef7-4346-b605-5f9cb843de21.jpeg align="center")
                
                1. Now let’s apply the service
                    
                
                ```dockerfile
                kubectl apply -f service.yaml 
                kubectl get all
                ```
                
                ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707493951315/b87d1904-258b-4619-b8a3-f5a934b374a2.jpeg align="center")
                
                1. Now copy Ingress Load Balancer and paste in new tab
                    
                
                ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707493963411/34e3ef44-93f6-4430-9a88-a756dd3221ee.jpeg align="center")
                
                1. Paste the ingress link in a browser and you will see the Mario game.
                    
                
                Let’s Go back to 1985 and play the game like children😀😀
                
                ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707494045950/c20803a1-3a6c-49c8-9afe-167065ab2a21.jpeg align="center")
                
                Let’s remove the service and deployment first
                
                ```dockerfile
                kubectl get all 
                kubectl delete service mario-service 
                kubectl delete deployment mario-deployment
                ```
                
                Let’s Destroy the cluster and other resources
                
                ```dockerfile
                terraform destroy --auto-approve
                ```
                
                ![](https://miro.medium.com/v2/resize:fit:1000/0*6Rom4yuf7x7YBBpR align="left")
                
                After 10mins Resources that are provisioned will be removed.
                
                ![](https://miro.medium.com/v2/resize:fit:1000/0*u_u8UXmvprRmP-eE align="left")
                
                `   `