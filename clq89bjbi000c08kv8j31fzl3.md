---
title: "Deploy Serverless Application on AWS"
datePublished: Sat Dec 16 2023 16:12:50 GMT+0000 (Coordinated Universal Time)
cuid: clq89bjbi000c08kv8j31fzl3
slug: deploy-serverless-application-on-aws

---

What is serverless ?

Serverless is **a cloud-native development model that allows developers to build and run applications without having to manage servers**. There are still servers in serverless, but they are abstracted away from app development

To deploy a serverless application on AWS, you can use AWS Lambda, AWS API Gateway, and other serverless services. Here's a general outline of the steps:

### **1\. Create an AWS Account:**

If you don't have an AWS account, you'll need to create one at [AWS Console](https://aws.amazon.com/).

### **2\. Set Up AWS CLI:**

Install and configure the AWS Command Line Interface (CLI) on your local machine. This will allow you to deploy and manage resources from your terminal.

### **3\. Develop Your Serverless Application:**

Write your serverless application code. This could be a Lambda function, and you may also need additional resources like DynamoDB tables or S3 buckets, depending on your application.

### **4\. Package Your Application:**

Package your application and its dependencies into a deployment package. This is typically a zip file containing your code and any libraries.

### **5\. Create a Lambda Function:**

Use the AWS Lambda service to create a function. Upload your deployment package, set the runtime, and configure environment variables if needed.

### **6\. Create an API Gateway:**

If your application needs an HTTP API, create an API Gateway. Configure it to trigger your Lambda function when specific endpoints are called.

### **7\. Configure Permissions:**

Ensure that your Lambda function has the necessary permissions to access other AWS services. IAM (Identity and Access Management) roles are used for this purpose.

### **8\. Deploy Your Serverless Application:**

Deploy your application using AWS CLI or other deployment tools. This step will make your Lambda function and API Gateway live.

### **9\. Test Your Application:**

Test your serverless application to ensure that it works as expected. You can use the AWS Management Console, API Gateway console, or tools like Postman.

### **10\. Monitor and Troubleshoot:**

Set up monitoring and logging using AWS CloudWatch to keep track of your application's performance and troubleshoot any issues.

Installation of Serverless in Ubuntu machine

1. Install AWS CLI :
    
    `$ curl "`[`https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip`](https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip)`" -o "`[`awscliv2.zip`](http://awscliv2.zip)`"`
    
    `sudo apt install unzip`
    
    **unzip** [**awscliv2.zip**](http://awscliv2.zip)
    
    sudo ./aws/install
    
    aws --version
    
2. Install NVM on Ubuntu with latest version (above 14.0.0)
    
    ```dockerfile
    sudo apt install curl 
    curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
    source ~/.bashrc  
    nvm install node 
    nvm install 16.0.0
    ```
    
    1. Setting Up Serverless Framework With AWS
        
    2. npm install -g serverless
        
        serverless
        
        Select the project you want to deploy here in this I selected AWS - Python -HTTP API (by pressing down arrow mark)
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702742232876/74711103-bf09-4780-a7ec-edf95969e52b.png align="center")
        
        1. Once the account is created, the CLI will then do one of two things:
            
            1. If you already have AWS credentials on your machine for some reason, you will get prompted to deploy to your AWS account using those credentials. I would recommend saying no at this point and checking out the next step “Setting up provider manually”
                
            2. If you do not have AWS credentials on your machine, the CLI will ask you if you want to set-up an “AWS Access Role” or “Local AWS Keys”. Let's choose the AWS Access Role to continue for now.
                
        2. Give the access key and secret access key in the serverless application to connect the AWS Provider.
            
            When you choose “AWS Access Role” another browser window should open (if not, the CLI provides you a link to use to open the window manually), and this is where we configure our Provider within our dashboard account.
            
            ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702742493448/5e4ece44-3c06-4e26-9a08-1e412e366daf.png align="center")
            
        3. Once everything is done, check the resources are created on AWS Console
            
            ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702742726990/b7e68835-313f-47b9-b845-c4410e6e3f43.jpeg align="center")
            
            ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702742764705/135c7c91-cf21-4ea2-a816-1cf9a8078185.jpeg align="center")
            
            ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702742780947/975c45e0-dbb8-4619-bf18-fa554ac0ae86.jpeg align="center")
            
            ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1702742792382/2cd00b32-5870-497d-85c1-a1913051c747.jpeg align="center")
            
            1. After completing all the tasks, type serverless destroy it will destroy all the resources that we have created.