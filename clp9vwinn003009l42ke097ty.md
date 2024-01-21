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