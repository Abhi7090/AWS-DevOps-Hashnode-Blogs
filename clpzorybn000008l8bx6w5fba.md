---
title: "Dockerfile image and push to AWS ECR"
datePublished: Sun Dec 10 2023 16:15:35 GMT+0000 (Coordinated Universal Time)
cuid: clpzorybn000008l8bx6w5fba
slug: dockerfile-image-and-push-to-aws-ecr

---

Step 1: Set Up an Amazon ECR Repository Log in to the AWS Management Console and navigate to the Amazon ECR service. Create a new repository by clicking on “Create repository.” Provide a name and optional description for the repository. Note down the ECR repository Name and AWS region for later use.

Step 2: Configure AWS Credentials Create an AWS Identity and Access Management (IAM) user with the necessary permissions to access the ECR repository. (Your IAM user must have “AmazonEC2ContainerRegistryFullAccess” IAM permissions) Obtain the AWS access key ID and secret access key for the IAM user.

Step 3: Create the Dockerfile and Push to github repository We will create a application on local machine (EC2 Ubuntu machine) and then push to github repository.

1. Create Dockerfile in vim editor
    
2. Workflow of Docker
    
3. Dockerfile -------------&gt; Images ----------------&gt; Containers
    
4. docker build -t java-app:latest .
    
5. docker run -d java-app:latest
    
6. Now to push to dockerhub from local machine EC2 Ubuntu
    
7. docker login -------&gt; dockerhub credentials
    
8. docker tag java-app:latest dockerhubreponame/java-app:latest
    
9. docker push dockerhubreponame/java-app:latest
    
10. Pushing Docker image to GitHub Container Registry Step 4: Create a Personal Access Token (PAT) on GitHub: Go to your GitHub account settings. Click on Developer settings in the left sidebar, click on Personal access tokens and then Generate new token. Give your token a descriptive name, and select the following scopes: read:packages, write:packages, and delete:packages. You might also need to select the repo scope if your image is private. Click Generate token and copy the generated token. Store it securely, as you won't be able to see it again. docker login [ghcr.io](https://private-user-images.githubusercontent.com/77018407/289367442-dedd6d16-460a-4090-ba50-b2b4974367f2.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDIyMjQ5MDcsIm5iZiI6MTcwMjIyNDYwNywicGF0aCI6Ii83NzAxODQwNy8yODkzNjc0NDItZGVkZDZkMTYtNDYwYS00MDkwLWJhNTAtYjJiNDk3NDM2N2YyLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzEyMTAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMjEwVDE2MTAwN1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWI3YjU0NjljMWI2NmQzMjFlYWUzZGYyZTRmNDE5YTkyODMyYmY0NDBmMzgxYjgyZDVkZGRiMWNkZDBhNTVlNTUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.AJOG4TTP-LHujzjH_wBCAzMpFqEcCnyuDSzQh1AzkLs) -u YOUR\_GITHUB\_USERNAME -p YOUR\_PERSONAL\_ACCESS\_TOKEN docker build -t [ghcr.io/githubuser/java-app:latest](https://private-user-images.githubusercontent.com/77018407/289367442-dedd6d16-460a-4090-ba50-b2b4974367f2.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDIyMjQ5MDcsIm5iZiI6MTcwMjIyNDYwNywicGF0aCI6Ii83NzAxODQwNy8yODkzNjc0NDItZGVkZDZkMTYtNDYwYS00MDkwLWJhNTAtYjJiNDk3NDM2N2YyLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzEyMTAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMjEwVDE2MTAwN1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWI3YjU0NjljMWI2NmQzMjFlYWUzZGYyZTRmNDE5YTkyODMyYmY0NDBmMzgxYjgyZDVkZGRiMWNkZDBhNTVlNTUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.AJOG4TTP-LHujzjH_wBCAzMpFqEcCnyuDSzQh1AzkLs) . docker push [ghcr.io/githubuser/java-app:latest](https://private-user-images.githubusercontent.com/77018407/289367442-dedd6d16-460a-4090-ba50-b2b4974367f2.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTEiLCJleHAiOjE3MDIyMjQ5MDcsIm5iZiI6MTcwMjIyNDYwNywicGF0aCI6Ii83NzAxODQwNy8yODkzNjc0NDItZGVkZDZkMTYtNDYwYS00MDkwLWJhNTAtYjJiNDk3NDM2N2YyLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFJV05KWUFYNENTVkVINTNBJTJGMjAyMzEyMTAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjMxMjEwVDE2MTAwN1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWI3YjU0NjljMWI2NmQzMjFlYWUzZGYyZTRmNDE5YTkyODMyYmY0NDBmMzgxYjgyZDVkZGRiMWNkZDBhNTVlNTUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.AJOG4TTP-LHujzjH_wBCAzMpFqEcCnyuDSzQh1AzkLs)
    

Step 5: Configure GitHub Secrets In your GitHub repository, go to “Settings” and navigate to the “Secrets” tab -&gt; ‘Actions’. Add three new secrets: AWS\_ACCESS\_KEY\_ID ,AWS\_SECRET\_ACCESS\_KEY and REPO\_NAME.

Step 6: Pushing Dockerfile from local machine to ECR through AWS CLI

1. sudo apt-get update
    
2. sudo apt-get install awscli
    
3. aws --version
    
4. aws configure
    

How to Create a Repo in ECR :

1. aws ecr create-repository --repository-name &lt;repo\_name&gt; --region &lt;region\_name&gt;
    
2. aws ecr get-login-password --region &lt;region\_name&gt;
    
3. aws ecr --region | docker login -u AWS -p &lt;encrypted\_token&gt; &lt;repo\_uri&gt;
    
4. docker tag &lt;source\_image\_tag&gt; &lt;target\_ecr\_repo\_uri&gt;
    
5. docker push &lt;ecr-repo-url&gt;
    
6. Create ECS Service :
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708184107674/1e80a77f-f0a2-482e-9616-eb2c01b1dbbc.png align="center")
    
7. aws ecs register-task-definition --cli-input-json [file://your\_task\_definition.json](file://your_task_definition.json)
    
8. aws ecs create-cluster --cluster-name your\_cluster\_name
    
9. aws ecs create-service --cluster your\_cluster\_name --service-name your\_service\_name --task-definition your\_task\_definition\_name --desired-count your\_desired\_count #service name should be unique