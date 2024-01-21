---
title: "Jenkins Basic and advance"
datePublished: Wed Nov 22 2023 14:53:05 GMT+0000 (Coordinated Universal Time)
cuid: clp9vwinn003009l42ke097ty
slug: jenkins-basic-and-advance

---

What is Jenkins :  
Jenkins – an open source automation server which enables developers around the world to reliably build, test, and deploy their software.

# Install Jenkins, configure Docker as agent, set up cicd, deploy applications to k8s and much more.

## AWS EC2 Instance

* Go to AWS Console
    
* Instances(running)
    
* Launch instances
    

[![Screenshot 2023-02-01 at 12 37 45 PM](https://user-images.githubusercontent.com/43399466/215974891-196abfe9-ace0-407b-abd2-adcffe218e3f.png align="left")](https://user-images.githubusercontent.com/43399466/215974891-196abfe9-ace0-407b-abd2-adcffe218e3f.png)

### Install Jenkins.

Pre-Requisites:

* Java (JDK)
    

### Run the below commands to install Java and Jenkins

Install Java

```dockerfile
sudo apt update
sudo apt install openjdk-11-jre
```

Verify Java is Installed

```dockerfile
java -version
```

Now, you can proceed with installing Jenkins

```dockerfile
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

**Note:** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.

* EC2 &gt; Instances &gt; Click on
    
* In the bottom tabs -&gt; Click on Security
    
* Security groups
    
* Add inbound traffic rules as shown in the image (you can just allow TCP 8080 as well, in my case, I allowed `All traffic`).
    

[![Screenshot 2023-02-01 at 12 42 01 PM](https://user-images.githubusercontent.com/43399466/215975712-2fc569cb-9d76-49b4-9345-d8b62187aa22.png align="left")](https://user-images.githubusercontent.com/43399466/215975712-2fc569cb-9d76-49b4-9345-d8b62187aa22.png)

### Login to Jenkins using the below URL:

http://:8080 \[You can get the ec2-instance-public-ip-address from your AWS EC2 console page\]

Note: If you are not interested in allowing `All Traffic` to your EC2 instance 1. Delete the inbound traffic rule for your instance 2. Edit the inbound traffic rule to only allow custom TCP port `8080`

After you login to Jenkins, - Run the command to copy the Jenkins Admin Password - `sudo cat /var/lib/jenkins/secrets/initialAdminPassword` - Enter the Administrator password

[![Screenshot 2023-02-01 at 10 56 25 AM](https://user-images.githubusercontent.com/43399466/215959008-3ebca431-1f14-4d81-9f12-6bb232bfbee3.png align="left")](https://user-images.githubusercontent.com/43399466/215959008-3ebca431-1f14-4d81-9f12-6bb232bfbee3.png)

### Click on Install suggested plugins

[![Screenshot 2023-02-01 at 10 58 40 AM](https://user-images.githubusercontent.com/43399466/215959294-047eadef-7e64-4795-bd3b-b1efb0375988.png align="left")](https://user-images.githubusercontent.com/43399466/215959294-047eadef-7e64-4795-bd3b-b1efb0375988.png)

Wait for the Jenkins to Install suggested plugins

[![Screenshot 2023-02-01 at 10 59 31 AM](https://user-images.githubusercontent.com/43399466/215959398-344b5721-28ec-47a5-8908-b698e435608d.png align="left")](https://user-images.githubusercontent.com/43399466/215959398-344b5721-28ec-47a5-8908-b698e435608d.png)

Create First Admin User or Skip the step \[If you want to use this Jenkins instance for future use-cases as well, better to create admin user\]

[![Screenshot 2023-02-01 at 11 02 09 AM](https://user-images.githubusercontent.com/43399466/215959757-403246c8-e739-4103-9265-6bdab418013e.png align="left")](https://user-images.githubusercontent.com/43399466/215959757-403246c8-e739-4103-9265-6bdab418013e.png)

Jenkins Installation is Successful. You can now starting using the Jenkins

[![Screenshot 2023-02-01 at 11 14 13 AM](https://user-images.githubusercontent.com/43399466/215961440-3f13f82b-61a2-4117-88bc-0da265a67fa7.png align="left")](https://user-images.githubusercontent.com/43399466/215961440-3f13f82b-61a2-4117-88bc-0da265a67fa7.png)

## Install the Docker Pipeline plugin in Jenkins:

* Log in to Jenkins.
    
* Go to Manage Jenkins &gt; Manage Plugins.
    
* In the Available tab, search for "Docker Pipeline".
    
* Select the plugin and click the Install button.
    
* Restart Jenkins after the plugin is installed.
    

[![Screenshot 2023-02-01 at 12 17 02 PM](https://user-images.githubusercontent.com/43399466/215973898-7c366525-15db-4876-bd71-49522ecb267d.png align="left")](https://user-images.githubusercontent.com/43399466/215973898-7c366525-15db-4876-bd71-49522ecb267d.png)

Wait for the Jenkins to be restarted.

## Docker Slave Configuration

Run the below command to Install Docker

```dockerfile
sudo apt update
sudo apt install docker.io
```

### Grant Jenkins user and Ubuntu user permission to docker deamon.

```dockerfile
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```

Once you are done with the above steps, it is better to restart Jenkins.

```dockerfile
http://<ec2-instance-public-ip>:8080/restart
```

The docker agent configuration is now successful.

### **Jenkins Master (Server)**

Jenkins’s server or master node holds all key configurations. The Jenkins master server is like a control server that orchestrates all the workflow defined in the pipelines. For example, scheduling a job, monitoring the jobs, etc.

### **Jenkins Agent**

An agent is typically a machine or container that connects to a Jenkins master and this agent executes all the steps mentioned in a Job. When you create a Jenkins job, you have to assign an agent to it. Every agent has a label as a unique identifier.

When you trigger a Jenkins job from the master, the actual execution happens on the agent node that is configured in the job.

A single, monolithic Jenkins installation can work great for a small team with a relatively small number of projects. As your needs grow, however, it often becomes necessary to scale up. Jenkins provides a way to do this called “master to agent connection.” Instead of serving the Jenkins UI and running build jobs all on a single system, you can provide Jenkins with agents to handle the execution of jobs while the master serves the Jenkins UI and acts as a control node.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701436120051/f9cb918f-d452-47c9-857d-42408bd12c5d.png?auto=compress,format&format=webp align="left")

**Pre-requisites**

Let’s say we’re starting with a fresh Ubuntu 22.04 Linux installation. To get an agent working make sure you install Java ( same version as Jenkins master server ) and Docker on it.

Note:- While creating an agent, be sure to separate rights, permissions, and ownership for Jenkins users.

### Task: 1

* Create an agent by setting up a node on Jenkins
    
* Create a new AWS EC2 Instance and connect it to the master (Where Jenkins is installed)
    
* The connection of the master and agent requires SSH and the public-private key pair exchange.
    
* Verify its status under the "Nodes" section.
    

**Step: 1**

Create two EC2 instances one for the master and the other one for the agent.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701437065761/158ef9be-aefc-49da-a827-10bb406265c1.png?auto=compress,format&format=webp align="left")

**Step: 2**

Generate SSH keys on Jenkins-master EC2 instance (`ssh-keygen -t ed25519`)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701449565679/50514887-e282-497a-ba60-2ece0e093f27.png?auto=compress,format&format=webp align="left")

**Step: 3**

Add public key from Jenkins-master instance to Jenkins-agent instance under location .ssh/authorized\_keys

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701449630743/256e58a7-61f1-4ee1-b040-b4f16450420e.png?auto=compress,format&format=webp align="left")

**Step: 4**

Try to connect Jenkins-agent from Jenkins-master

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701443109877/86c3eba0-7af8-44d3-9fca-124cc5301fcb.png?auto=compress,format&format=webp align="left")

**Step: 5**

Install Java on the Jenkins-agent [**installation link**](https://www.jenkins.io/doc/book/installing/linux/#debianubuntu)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701443661736/6758beb7-89ed-4883-a7fc-f522708fd0e9.png?auto=compress,format&format=webp align="left")

**Step: 6**

Create an agent by setting up a node on Jenkins

Go to the Jenkins dashboard, and click on Manage Jenkins --&gt; Set up agent

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701444282327/058d951a-d88f-4c19-91be-ffc87029d476.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701447233659/0cffea6a-92b8-4bf8-8c51-0155b3d840f9.png?auto=compress,format&format=webp align="left")

Select the launch method as **Launch agents via SSH** Give the public IP of the agent in the host field.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701447356928/1a5b16fb-d44c-4f4b-a8c7-9619bdc4f2eb.png?auto=compress,format&format=webp align="left")

Click on **Add** under Credentials.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676134842070/596827e9-8f87-44f0-80a7-7ce59ba50b5e.png?auto=compress,format&format=webp&auto=compress,format&format=webp align="left")

Add the private key that we created in a Jenkins-master instance using ssh-keygen

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676135332215/af4bbc7c-cf2c-41bd-aa43-2356b3d37d86.png?auto=compress,format&format=webp&auto=compress,format&format=webp align="left")

Below we select Ubuntu for credentials

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701449519431/ffad0239-eecb-491e-a6f8-f7566258a8de.png?auto=compress,format&format=webp align="left")

Click on **save** which will create a node.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701449754330/4cec5259-eb65-4133-8fdc-501ec4c8587e.png?auto=compress,format&format=webp align="left")

### Task: 2

* Run your previous Jobs on the new agent
    
* Use labels for the agent, your master server should trigger builds for the agent server.
    

**Step: 1**

Create a new job and give it to any name, select **Pipeline** and save.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701450479618/bbfa3ce2-4519-4af4-8a2f-882a0756c020.png?auto=compress,format&format=webp align="left")

**Step: 2**

Give a general description.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701451553442/886a80f1-5ad5-4998-aafa-4beb320ba7cb.png?auto=compress,format&format=webp align="left")

In the pipeline script set the **dev-server** as follows and click on save

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701451622253/8f51b6cb-2b0b-4990-98f7-acc7d6969448.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701452007018/1aee3a57-b2a9-4aa2-9734-55258ab13171.png?auto=compress,format&format=webp align="left")

on the console output we can see on pipeline running on the **dev-server**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701452103946/7e308ff9-f008-464e-bd2f-10d0036ecd0d.png?auto=compress,format&format=webp align="left")

**Create a connection to your Jenkins job and your GitHub Repository via GitHub Integration.**

Webhooks provide a way for notifications to be delivered to an external web server whenever certain actions occur on a repository or organization.

**For GitHub-Webhook:**

1. Open the GitHub repository.
    
2. Go to “settings” and then to “hooks”.
    
3. Click the “Add webhook” button. Enter “**&lt;Jenkins url&gt;//github-webhook/**” Payload URL. Click on Add webhook.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701348890906/9ea0e9ba-c252-4820-9ebb-5676798bb0c9.png?auto=compress,format&format=webp align="left")

### 💼**Task: 2**

* In the Execute shell run the application using Docker compose
    
* You will have to make a Docker Compose file for this Project
    
* Run the project
    

**Steps:**

For the GitHub integration plugin

Click on Manage Jenkins &gt;Manage plugin &gt; Available plugin&gt; GitHub integration &gt; Install it

Create ssh public and private keys

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701349637698/99bf16c9-86d9-490a-9f46-c91b45de2ad7.png?auto=compress,format&format=webp align="left")

Go to GitHub account &gt; Account Settings &gt;SSH and GPG keys&gt;Enter public ip and add it

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701349735068/63973433-7589-46b3-ba23-9af73cd0a4b5.png?auto=compress,format&format=webp align="left")

1. Click on New Item on left panel &gt;Enter item name and select freestyle project options &gt;click on Ok
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701349878056/025f8f3c-db7b-4158-84d2-9d0402d3e8d7.png?auto=compress,format&format=webp align="left")
    
2. Enter description &gt; select checkbox Github project &gt; enter github url of project
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701350114658/5a53db70-ab85-4187-b8be-c7b813faf8ef.png?auto=compress,format&format=webp align="left")

1. In Source Code Management &gt; select Git &gt; enter github url&gt; In credentials click on Add
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701350800898/8ee1c54c-0e8e-403b-85f9-608e8a6856ae.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701350827923/0675ffe9-9de1-4e1d-857f-03653b65cbdc.png?auto=compress,format&format=webp align="left")

1. Select Github hook trigger for GITScm polling in Build trigger
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701350913062/a2acf04e-3cae-4da8-a5db-3231e5c8df20.png?auto=compress,format&format=webp align="left")

1. In build steps &gt;select Execute shell &gt; Enter commands of docker compose and save the configuration
    

**COPY**

**COPY**

```dockerfile
echo "……Devops CI/CD Started………………"

docker-compose up -d
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701351448058/e96ac538-6588-4f9c-8830-4a64dd0f0cc2.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701351466835/314f4fef-19a5-4d35-8d34-5e82f8a3f87f.png?auto=compress,format&format=webp align="left")

1. Check the web-hook connection of the Github project, it should have a green checkmark.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701351724184/7bdf2c00-8b5c-404b-9806-6f2c0d190011.png?auto=compress,format&format=webp align="left")

1. Access the URL
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701351954495/79b62422-f69e-4582-a257-0840fa4024ac.png?auto=compress,format&format=webp align="left")
    
2. Now make changes in code push to GitHub Master branch & jenkins will automatically build the code
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701364483554/d75b78a1-2aee-46c1-8b9d-836989ea57f7.png?auto=compress,format&format=webp align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1701365241203/8e7b5465-52b7-41e1-8b6b-1d8f0fd755fc.png?auto=compress,format&format=webp align="left")