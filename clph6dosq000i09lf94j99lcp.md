---
title: "Kubernetes"
datePublished: Mon Nov 27 2023 17:20:45 GMT+0000 (Coordinated Universal Time)
cuid: clph6dosq000i09lf94j99lcp
slug: kubernetes

---

Kubernetes is a distributed system with a **<mark>client-server architecture</mark>**. The basic components of a Kubernetes environment are: 

* Control plane: Hosts the components that manage the Kubernetes cluster
    
* Data plane: Machines used as compute resources
    
* Distributed key-value storage system: Keeps the cluster state consistent
    
* Cluster nodes: Also called worker nodes or minions
    

**The Kubernetes architecture also includes:** 

* Master node: Communicates with worker nodes using Kube API-server to kubelet communication
    
* Worker nodes: Can be virtual machines (VMs) or physical machines
    
* Pods: Can contain one or more containers
    
* API (application programming interface) server: Determines if a request is valid and then processes it
    
* etcd: An open-source, strongly consistent, distributed key-value store
    
* # CI/CD with Kubernetes
    
    This guide explains the basic workflow of how Kubernetes can be integrated with the CI/CD pipelines for deployment of an application in a simplified way.
    
    ## What is CI?
    
    CI stands for Continuous Integration. In CI, when people work on a project together, code changes are integrated into a shared repository on a regular basis. This is typically done multiple times a day. Then, automated tests run to check if everything is okay with the new code. This helps catch problems early in the development cycle. If these tests say "Everything is ok!" then the code is allowed to move to the next steps in the pipeline. There are a lot of tools that help in achieving CI/CD, like Jenkins, AWS CodePipeline, etc.
    
    ## What is CD?
    
    CD stands for both, Continuous Delivery and Continuous Deployment. Continuous Deployment extends CI by automating the deployment process. Once the code passes CI stage in the pipeline, it's automatically deployed to a staging environment for further testing. If everything works well in the staging environment, the code is then automatically promoted to the production environment, making the release process more efficient and reducing the risk of manual errors. However, Continuous Delivery means that your code changes are automatically tested and built as part of the CI stage. If everything looks good, the application is all set to be deployed. However, you decide when to actually release it to the end users.
    
    ## Role of Kubernetes in CI/CD
    
    Kubernetes is like a super helper in the process of getting the application ready and out to end users. It helps put the application on different places in the same way every time. This helps make sure everything works well, no matter where it goes.
    
    Let us understand this through a simple workflow of a CI/CD pipeline for a containerized application.
    
    1. **Committing Code**: Developers commit their code changes to a version control system like Git.
        
    2. **Build**: An automated build is triggered in a CI/CD tool like Jenkins to build a Docker image from the application code.
        
    3. **Run Tests**: Automated tests are run on the Docker image to ensure it meets the set functionality/standards.
        
    4. **Push Artifacts**: Once the testing stage is completed, the image is pushed to an Artifact Management System like Artifactory/Dockerhub/Docker Trusted Registry.
        
    5. **Deployment using Kubernetes**: In this stage, we leverage the power of Kubernetes to streamline and automate the deployment process of the application. Let's get into the process by breaking it down into various essential steps.
        
    
    * **Deployment Manifest File**: A Kubernetes deployment script, deployment YAML file, holds the critical configuration instructions that Kubernetes requires to manage and deploy the application. Within this YAML file, we define the application's name, specify the container image to be used, determine the desired number of instances (replicas), and include other essential configurations. This YAML file acts as a comprehensive guide for Kubernetes, detailing how to create, maintain, and update the application's environment. The deployment YAML file is stored in the same repository as the application code. This allows for easy collaboration, versioning, and synchronization between the application code and its deployment configuration.
        
    * **Deployment in Staging Environment**: After generating a new image for the application through CI, the next step is to deploy it to a staging environment within the Kubernetes cluster. This staging environment, often known as "Staging" or "Pre-Prod", closely resembles the production setup but operates in isolation for testing.
        
    * **Testing in Staging Environment**: Automated testing and validation are performed in the staging environment to ensure the new version of the application image works as expected.
        
    * **Production Deployment**: Once the testing in the staging environment passes successfully, the new version of the application can be deployed to the production environment using the same deployment YAML script that guided the staging deployment. This ensures consistent setup in the production environment and the transition from staging to production is seamless.
        
    * ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701099991100/1ee93a84-4e4d-4b95-b1c2-0d8562e92a15.png align="center")
        
    
    ## Conclusion
    
    By automating the above-mentioned steps and using Kubernetes, the CI/CD process becomes more efficient, reliable, and repeatable. It helps development teams release new features faster while maintaining stability.
    
* How to Install Kubernetes of master node and worker node
    
* Launch the two EC2 instance (Master node and Worker node) with same key pair.
    
* # Kubeadm Installation Guide
    
    This guide outlines the steps needed to set up a Kubernetes cluster using kubeadm.
    
    ## Pre-requisites
    
    * Ubuntu OS (Xenial or later)
        
    * sudo privileges
        
    * Internet access
        
    * t2.medium instance type or higher
        
    
    ---
    
    ## Both Master & Worker Node
    
    Run the following commands on both the master and worker nodes to prepare them for kubeadm.
    
    ```dockerfile
    # using 'sudo su' is not a good practice.
    sudo apt update
    sudo apt-get install -y apt-transport-https ca-certificates curl
    sudo apt install docker.io -y
    
    sudo systemctl enable --now docker # enable and start in single command.
    
    # Adding GPG keys.
    curl -fsSL "https://packages.cloud.google.com/apt/doc/apt-key.gpg" | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/kubernetes-archive-keyring.gpg
    
    # Add the repository to the sourcelist.
    echo 'deb https://packages.cloud.google.com/apt kubernetes-xenial main' | sudo tee /etc/apt/sources.list.d/kubernetes.list
    
    sudo apt update 
    sudo apt install kubeadm=1.20.0-00 kubectl=1.20.0-00 kubelet=1.20.0-00 -y
    ```
    
    **Sample Command run on master node**
    
    [![image](https://private-user-images.githubusercontent.com/40052830/261847299-a4e7a4af-31fa-40cf-bb9e-64ba18999cb5.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDcyOTktYTRlN2E0YWYtMzFmYS00MGNmLWJiOWUtNjRiYTE4OTk5Y2I1LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTJjMmQyNjY4MzY5ZDRkZTM5MmEyNThkNjVlYjM5MjgwNzc2OTQ3NDU1MGU1Mzg0MzNlY2RmMDk0YWJhMzFiMDYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.VwJIws7NqLbliT0s7BiKOWYr_fTJkGo-IiIwZ8cmHso align="left")](https://private-user-images.githubusercontent.com/40052830/261847299-a4e7a4af-31fa-40cf-bb9e-64ba18999cb5.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDcyOTktYTRlN2E0YWYtMzFmYS00MGNmLWJiOWUtNjRiYTE4OTk5Y2I1LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTJjMmQyNjY4MzY5ZDRkZTM5MmEyNThkNjVlYjM5MjgwNzc2OTQ3NDU1MGU1Mzg0MzNlY2RmMDk0YWJhMzFiMDYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.VwJIws7NqLbliT0s7BiKOWYr_fTJkGo-IiIwZ8cmHso)
    
    [![image](https://private-user-images.githubusercontent.com/40052830/261847246-acf157b8-5c7b-44e7-91ef-b5437053be60.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDcyNDYtYWNmMTU3YjgtNWM3Yi00NGU3LTkxZWYtYjU0MzcwNTNiZTYwLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTlmMDY0YmNjMGJiZjk0N2Q0M2M5ZGQ4ZTllMTU0Zjc2YWNjMTliMzAyZDUyM2UxMWQxYzE3ZGE5ODQ2YTc0MmUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.2wxouR2WGgyJLnh1qYHSkBjgcUa6d3bw2iyQEcl_guw align="left")](https://private-user-images.githubusercontent.com/40052830/261847246-acf157b8-5c7b-44e7-91ef-b5437053be60.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDcyNDYtYWNmMTU3YjgtNWM3Yi00NGU3LTkxZWYtYjU0MzcwNTNiZTYwLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTlmMDY0YmNjMGJiZjk0N2Q0M2M5ZGQ4ZTllMTU0Zjc2YWNjMTliMzAyZDUyM2UxMWQxYzE3ZGE5ODQ2YTc0MmUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.2wxouR2WGgyJLnh1qYHSkBjgcUa6d3bw2iyQEcl_guw)
    
    [![image](https://private-user-images.githubusercontent.com/40052830/261847209-8f960aae-3706-43cd-bac8-1903fbe8196d.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDcyMDktOGY5NjBhYWUtMzcwNi00M2NkLWJhYzgtMTkwM2ZiZTgxOTZkLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTU3NmJiN2E2M2RkOTgyOGE0MDMwYmJiNzNmZmZmODdlNGQ5MWY3ODFjZjQyYTA2Mjg4NGJmMDMwY2Q3MmY0NGMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.BcR2d0JSvIFlDGSe3Sm1FGdx5Ya7K2yLgXbDtZ9WC44 align="left")](https://private-user-images.githubusercontent.com/40052830/261847209-8f960aae-3706-43cd-bac8-1903fbe8196d.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDcyMDktOGY5NjBhYWUtMzcwNi00M2NkLWJhYzgtMTkwM2ZiZTgxOTZkLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTU3NmJiN2E2M2RkOTgyOGE0MDMwYmJiNzNmZmZmODdlNGQ5MWY3ODFjZjQyYTA2Mjg4NGJmMDMwY2Q3MmY0NGMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.BcR2d0JSvIFlDGSe3Sm1FGdx5Ya7K2yLgXbDtZ9WC44)
    
    ---
    
    ## Master Node
    
    1. Initialize the Kubernetes master node.
        
        ```dockerfile
        sudo kubeadm init
        ```
        
        [![image](https://private-user-images.githubusercontent.com/40052830/261847539-4fed3d68-eb41-423d-b83f-35c3cc11476e.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDc1MzktNGZlZDNkNjgtZWI0MS00MjNkLWI4M2YtMzVjM2NjMTE0NzZlLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTBjNzA3YTY0YmQ2YzVkMzdiM2RkZGQzNTQzMzlkN2E0ZWRhYmIyOTliMjZiNGY3ZjU3NTUyZmQ2MzYxZTA5NTMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.VoN95Xmb594-SoI4R7l87aYVJ2nzH5qS4BtZvt5dlcM align="left")](https://private-user-images.githubusercontent.com/40052830/261847539-4fed3d68-eb41-423d-b83f-35c3cc11476e.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDc1MzktNGZlZDNkNjgtZWI0MS00MjNkLWI4M2YtMzVjM2NjMTE0NzZlLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTBjNzA3YTY0YmQ2YzVkMzdiM2RkZGQzNTQzMzlkN2E0ZWRhYmIyOTliMjZiNGY3ZjU3NTUyZmQ2MzYxZTA5NTMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.VoN95Xmb594-SoI4R7l87aYVJ2nzH5qS4BtZvt5dlcM)
        
        After succesfully running, your Kubernetes control plane will be initialized successfully.
        
        [![image](https://private-user-images.githubusercontent.com/40052830/261847658-760276f4-9146-4bc1-aa92-48cc1c0b13f4.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDc2NTgtNzYwMjc2ZjQtOTE0Ni00YmMxLWFhOTItNDhjYzFjMGIxM2Y0LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWZhNDBmZjZjYWY4OWVhMzUwNjE2Y2ZmOGVmYjAwNDU0NzgxYzUxNTRhNzY5M2Q5MjY3YjM4MGRlMTA0NmE4MzUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.2YVmDyclSk9cHu7Z0Aizu2oAcE4BtDViLnnQUE8FWYk align="left")](https://private-user-images.githubusercontent.com/40052830/261847658-760276f4-9146-4bc1-aa92-48cc1c0b13f4.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDc2NTgtNzYwMjc2ZjQtOTE0Ni00YmMxLWFhOTItNDhjYzFjMGIxM2Y0LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWZhNDBmZjZjYWY4OWVhMzUwNjE2Y2ZmOGVmYjAwNDU0NzgxYzUxNTRhNzY5M2Q5MjY3YjM4MGRlMTA0NmE4MzUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.2YVmDyclSk9cHu7Z0Aizu2oAcE4BtDViLnnQUE8FWYk)
        
    2. Set up local kubeconfig (both for root user and normal user):
        
        ```dockerfile
        mkdir -p $HOME/.kube
        sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
        sudo chown $(id -u):$(id -g) $HOME/.kube/config
        ```
        
        [![image](https://private-user-images.githubusercontent.com/40052830/261847772-f647adc1-0976-490e-b9c9-f6f96908d6fe.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDc3NzItZjY0N2FkYzEtMDk3Ni00OTBlLWI5YzktZjZmOTY5MDhkNmZlLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTAzOGMwY2FlMzUxZTQ3Yzc5MzEzY2U4NTVlOGMyZTQxMWZhYTVhYmY3ODFkZTUyODE2ZjI5YmE1NWNmZmIzY2ImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.5ovrxDQDcvGtyahTQ9xDprjSIf2fJpq3NUmXiaPLmS0 align="left")](https://private-user-images.githubusercontent.com/40052830/261847772-f647adc1-0976-490e-b9c9-f6f96908d6fe.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDc3NzItZjY0N2FkYzEtMDk3Ni00OTBlLWI5YzktZjZmOTY5MDhkNmZlLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTAzOGMwY2FlMzUxZTQ3Yzc5MzEzY2U4NTVlOGMyZTQxMWZhYTVhYmY3ODFkZTUyODE2ZjI5YmE1NWNmZmIzY2ImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.5ovrxDQDcvGtyahTQ9xDprjSIf2fJpq3NUmXiaPLmS0)
        
    3. Apply Weave network:
        
        ```dockerfile
        kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
        ```
        
        [![image](https://private-user-images.githubusercontent.com/40052830/261847852-ec7b4684-7719-4d09-81d8-eee27b98972a.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDc4NTItZWM3YjQ2ODQtNzcxOS00ZDA5LTgxZDgtZWVlMjdiOTg5NzJhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTIxNjA2YzE2N2ZjZjliZTViNDg2ZGYxZDRhNjVlYWE5YjYzNzY5ODBiMmQ0NDU0MTA0NTRiZGU2MDNlODI5Y2UmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.6iizJDGr0-6LmhFiYxJZVHMHIvIh7JaTwfVzHWxqUaA align="left")](https://private-user-images.githubusercontent.com/40052830/261847852-ec7b4684-7719-4d09-81d8-eee27b98972a.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDc4NTItZWM3YjQ2ODQtNzcxOS00ZDA5LTgxZDgtZWVlMjdiOTg5NzJhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTIxNjA2YzE2N2ZjZjliZTViNDg2ZGYxZDRhNjVlYWE5YjYzNzY5ODBiMmQ0NDU0MTA0NTRiZGU2MDNlODI5Y2UmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.6iizJDGr0-6LmhFiYxJZVHMHIvIh7JaTwfVzHWxqUaA)
        
    4. Generate a token for worker nodes to join:
        
        ```dockerfile
        sudo kubeadm token create --print-join-command
        ```
        
        [![image](https://private-user-images.githubusercontent.com/40052830/261848054-0370839b-bbac-415c-9d5a-9ab52cd3108b.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDgwNTQtMDM3MDgzOWItYmJhYy00MTVjLTlkNWEtOWFiNTJjZDMxMDhiLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWQ1NmUxYzM4MGYzYzg5Njg1NTE5ZGYxMmFlYjg5YjFkZjFmMGQxMTI3ZGY4NTFhNzE2ODliY2E3YjgzNzFhNmUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.cMp2GlHeV92-2WDwwcVmi3sAMCT1Yt7gqv0aRPxb7m8 align="left")](https://private-user-images.githubusercontent.com/40052830/261848054-0370839b-bbac-415c-9d5a-9ab52cd3108b.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDgwNTQtMDM3MDgzOWItYmJhYy00MTVjLTlkNWEtOWFiNTJjZDMxMDhiLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWQ1NmUxYzM4MGYzYzg5Njg1NTE5ZGYxMmFlYjg5YjFkZjFmMGQxMTI3ZGY4NTFhNzE2ODliY2E3YjgzNzFhNmUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.cMp2GlHeV92-2WDwwcVmi3sAMCT1Yt7gqv0aRPxb7m8)
        
    5. Expose port 6443 in the Security group for the Worker to connect to Master Node
        
    
    [![image](https://private-user-images.githubusercontent.com/40052830/261848106-b3f5df01-acb0-419f-aa70-6d51819f4ec0.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDgxMDYtYjNmNWRmMDEtYWNiMC00MTlmLWFhNzAtNmQ1MTgxOWY0ZWMwLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWNmNDNkY2NjNmE4MDJjNGVhYzMzMDI5OWFkNzUwNGIyZjZlM2RhOWYzY2NiYWI0NmMzYzM2NTYzNmI2OGIyYTYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.M7iB3g3y6dY17yONqh90QPgK0azwMqQAKwB87gtRd5c align="left")](https://private-user-images.githubusercontent.com/40052830/261848106-b3f5df01-acb0-419f-aa70-6d51819f4ec0.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDgxMDYtYjNmNWRmMDEtYWNiMC00MTlmLWFhNzAtNmQ1MTgxOWY0ZWMwLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWNmNDNkY2NjNmE4MDJjNGVhYzMzMDI5OWFkNzUwNGIyZjZlM2RhOWYzY2NiYWI0NmMzYzM2NTYzNmI2OGIyYTYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.M7iB3g3y6dY17yONqh90QPgK0azwMqQAKwB87gtRd5c)
    
    ---
    
    ## Worker Node
    
    1. Run the following commands on the worker node.
        
        ```dockerfile
        sudo kubeadm reset pre-flight checks
        ```
        
        [![image](https://private-user-images.githubusercontent.com/40052830/261848231-3d29912b-f1a3-4e0b-a6ee-6c9cc5db49fb.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDgyMzEtM2QyOTkxMmItZjFhMy00ZTBiLWE2ZWUtNmM5Y2M1ZGI0OWZiLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTdhMmFjNzFhNmY1NTVmN2IyYmQ5ZGQwOWE5NjRhOGVhODRjOWU1MzMwY2Q0MmFmN2UyMDNlYTk2YzRlOGVjMjEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.8v2m9iPShNZpjmrXxVSiP8VxfBRVyfW0QFsSZNokb2c align="left")](https://private-user-images.githubusercontent.com/40052830/261848231-3d29912b-f1a3-4e0b-a6ee-6c9cc5db49fb.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDgyMzEtM2QyOTkxMmItZjFhMy00ZTBiLWE2ZWUtNmM5Y2M1ZGI0OWZiLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTdhMmFjNzFhNmY1NTVmN2IyYmQ5ZGQwOWE5NjRhOGVhODRjOWU1MzMwY2Q0MmFmN2UyMDNlYTk2YzRlOGVjMjEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.8v2m9iPShNZpjmrXxVSiP8VxfBRVyfW0QFsSZNokb2c)
        
    2. Paste the join command you got from the master node and append `--v=5` at the end. *Make sure either you are working as sudo user or use* `sudo` before the command
        
        [![image](https://private-user-images.githubusercontent.com/40052830/261848429-c41e3213-7474-43f9-9a7b-a75694be582a.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDg0MjktYzQxZTMyMTMtNzQ3NC00M2Y5LTlhN2ItYTc1Njk0YmU1ODJhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTMwMmVlMDUzYjVmMDBlY2U3OWY0YmRiNzYzMDM3NmU3ZWY0NzE2ZjUyNzA4ZjhiOWJkNDJjZjYxNjZkOTBkYTUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.otaMthiiJDb8tb9SDeZCWojwzYKSBB_qRV5WhvAE-Rs align="left")](https://private-user-images.githubusercontent.com/40052830/261848429-c41e3213-7474-43f9-9a7b-a75694be582a.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDg0MjktYzQxZTMyMTMtNzQ3NC00M2Y5LTlhN2ItYTc1Njk0YmU1ODJhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTMwMmVlMDUzYjVmMDBlY2U3OWY0YmRiNzYzMDM3NmU3ZWY0NzE2ZjUyNzA4ZjhiOWJkNDJjZjYxNjZkOTBkYTUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.otaMthiiJDb8tb9SDeZCWojwzYKSBB_qRV5WhvAE-Rs)
        
        After succesful join-&gt;
        
    
    ---
    
    ## Verify Cluster Connection
    
    On Master Node:
    
    ```dockerfile
    kubectl get nodes
    ```
    
    [![image](https://private-user-images.githubusercontent.com/40052830/261848387-4ed4dcac-502a-4cc1-a63e-c9cbb0199428.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDgzODctNGVkNGRjYWMtNTAyYS00Y2MxLWE2M2UtYzljYmIwMTk5NDI4LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTgzNDFkM2ZjMGYzZGM4NzZmZTJjOGYwMzY3MjA3NmVlN2Y3NDc1NWVkODk1MWVlNWU0MGIyYzc4YzY5YmFjYzYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.Ij4Adw0Jc-gsnSng9BenVVVJ9Yh4kbG_WbxXw3mjfT0 align="left")](https://private-user-images.githubusercontent.com/40052830/261848387-4ed4dcac-502a-4cc1-a63e-c9cbb0199428.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDgzODctNGVkNGRjYWMtNTAyYS00Y2MxLWE2M2UtYzljYmIwMTk5NDI4LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTgzNDFkM2ZjMGYzZGM4NzZmZTJjOGYwMzY3MjA3NmVlN2Y3NDc1NWVkODk1MWVlNWU0MGIyYzc4YzY5YmFjYzYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.Ij4Adw0Jc-gsnSng9BenVVVJ9Yh4kbG_WbxXw3mjfT0)
    
    ---
    
    ## Optional: Labeling Nodes
    
    If you want to label worker nodes, you can use the following command:
    
    ```dockerfile
    kubectl label node <node-name> node-role.kubernetes.io/worker=worker
    ```
    
    ---
    
    ## Optional: Test a demo Pod
    
    If you want to test a demo pod, you can use the following command:
    
    ```dockerfile
    kubectl run hello-world-pod --image=busybox --restart=Never --command -- sh -c "echo 'Hello, World' && sleep 3600"
    ```
    
    [![image](https://private-user-images.githubusercontent.com/40052830/261848566-bace1884-bbba-4e2f-8fb2-83bbba819d08.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDg1NjYtYmFjZTE4ODQtYmJiYS00ZTJmLThmYjItODNiYmJhODE5ZDA4LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTY1OWYwYTM3NmUxNTE5ZWExNDY1Mjc0Zjg3NTcxNjc4ZjI3MDEwMTM0ZjU3MjhiZGQ2YTY0YTRhZWQ2Mjc5YTImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.0AD34CDafFka-lt8zL4761Q9LdSBCKvwJIImiJQAXfg align="left")](https://private-user-images.githubusercontent.com/40052830/261848566-bace1884-bbba-4e2f-8fb2-83bbba819d08.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDExMDAzNjcsIm5iZiI6MTcwMTEwMDA2NywicGF0aCI6Ii80MDA1MjgzMC8yNjE4NDg1NjYtYmFjZTE4ODQtYmJiYS00ZTJmLThmYjItODNiYmJhODE5ZDA4LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzExMjclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMTI3VDE1NDc0N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTY1OWYwYTM3NmUxNTE5ZWExNDY1Mjc0Zjg3NTcxNjc4ZjI3MDEwMTM0ZjU3MjhiZGQ2YTY0YTRhZWQ2Mjc5YTImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.0AD34CDafFka-lt8zL4761Q9LdSBCKvwJIImiJQAXfg)
    
    Deployed first Django-app using kubernetes
    
* 1. create directory name project - mkdir project
        
    2. Create three yaml files - pod.yml, deployment.yml, service.yml in the project directory
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701101343127/d617f7d6-e897-441f-a943-f930d396a935.png align="center")
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701101367379/9cf2b1d5-0da3-4143-a464-a285627425b6.png align="center")
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701105100671/b0081ae7-5396-487a-b4f0-e47234e6d89a.png align="center")
        
    3. Run these commands :
        
        kubectl apply -f . (. indicates run pod.yml, deployment.yml and service.yml at a time)
        
    4. We need to create namespace :
        
        kubectl create namespace django-todo-app
        
    5. To see pods running enter this command (see in worker nodes) :
        
        kubectl get pods -n django-app (-n : namespace)
        
    6. If we want to see service proxy :
        
        kubectl get svc -n django-app (-n : namespace)
        
    7. If we want to describe node :
        
        kubectl describe node "node-IP"
        
    8. If we want to scale up replicates (pods=5) in deployment.yaml:
        
        kubectl scale deployment django-todo-app --replicate=5 -n django-app
        
    9. if we want to delete pod :
        
        kubectl delete pod django-todo-app -n django-todo-app
        
        Note : If we delete pod also Website will not crash or it will not effect anything.
        
    10. Copy public ip of worker node and nodeport (30007)
        
        PublicI:30007
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701105579705/a88283ce-d086-4271-9d91-9122d0cfb8c3.jpeg align="center")