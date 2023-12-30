---
title: "Jenkins Basic and advance"
datePublished: Wed Nov 22 2023 14:53:05 GMT+0000 (Coordinated Universal Time)
cuid: clp9vwinn003009l42ke097ty
slug: jenkins-basic-and-advance

---

What is Jenkins :  
Jenkins – an open source automation server which enables developers around the world to reliably build, test, and deploy their software.

# **Jenkins Pipeline Tutorial**

In this Jenkins pipeline tutorial, we delve into the following topics:

1. Types of Jenkins Pipeline
    
2. Pipeline as Code Basics
    
3. Building a Basic CI Pipeline as Code for Java App
    
4. Building a Job from Pipeline Code in Source Code Repo
    
5. Executing Parallel Stages in a Pipeline
    
6. Generating Pipeline Scripts & Directives with Jenkins Inbuilt Generators
    

# **Types of Jenkins Pipeline**

There are two types of Jenkins pipeline code.

> *Declarative Pipeline*
> 
> *Scripted Pipeline*

Prepare to be enchanted by the wonders of the Declarative Pipeline, an advanced and extensible version of the scripted pipeline in Jenkins. In this tutorial, we will exclusively focus on this remarkable syntax, which brings a plethora of features and benefits to your Jenkins use cases.

Explore the captivating possibilities that await you. Immerse yourself in the world of Jenkins’s multibranch pipeline, where the declarative pipeline takes center stage with the Jenkinsfile approach.

Are you ready to embark on a journey that will revolutionize your Jenkins pipelines? Let’s dive into this tutorial and discover how to create a stunning pipeline using the declarative pipeline as code, specifically tailored for building **Java Spring Boot applications.**

# **Prerequisites**

> ***Jenkins master***
> 
> ***Jenkins slave*** *node connected to the master*
> 
> ***Access to*** [***Github.com***](https://github.com/spring-projects/spring-petclinic.git) *from your Jenkins server. If you are trying out from your corporate Jenkins setup, you can use your organization’s private git repo.*

Here is the representation of the simple pipeline we are going to build in this tutorial

![](https://miro.medium.com/v2/resize:fit:700/1*g8qoeNwNCd7yN-7X6Tzd6w.png align="left")

Presenting the pipeline code for the above workflow. But first, let’s decipher the meaning behind each block to ensure a clear understanding before setting up the pipeline.

Note: Don’t fret about the DSL (Domain-Specific Language) used in the pipeline code. Read the entire article to discover a simplified approach for generating DSLs effortlessly.

```dockerfile
Pipeline {
    Agent {
        Node {
            Label 'SLAVE01'
        }
    }

    Tools {
        Maven 'maven3'
    }

    Options {
        BuildDiscarder LogRotator(
            DaysToKeepStr: '15',
            NumToKeepStr: '10'
        )
    }

    Environment {
        APP_NAME = "DCUBE_APP"
        APP_ENV = "DEV"
    }

    Stages {
        Stage('Cleanup Workspace') {
            Steps {
                CleanWs()
                Sh 'echo "Cleaned Up Workspace for ${APP_NAME}"'
            }
        }

        Stage('Code Checkout') {
            Steps {
                Checkout([
                    $class: 'GitSCM',
                    Branches: [[name: '*/master']],
                    UserRemoteConfigs: [[url: 'https://github.com/spring-projects/spring-petclinic.git']]
                ])
            }
        }

        Stage('Code Build') {
            Steps {
                Sh 'mvn install -Dmaven.test.skip=true'
            }
        }

        Stage('Printing All Global Variables') {
            Steps {
                Sh 'env'
            }
        }
    }
}
```

Here’s a detailed explanation of each block in the provided Jenkins pipeline code:

`The Pipeline Block`: This block is the root block of the Jenkins pipeline and encapsulates the entire pipeline configuration.

```dockerfile
pipeline {

---<All Pipeline blocks go here>---

}
```

`The Agent Block`: Specifies the agent block where the pipeline will be executed. It supports both static slaves and docker based dynamic slaves. In this case, it uses a specific node with the label 'SLAVE01'.

```dockerfile
   agent {
        node {
            label 'SLAVE01'
        }
    }
```

`Tools Block`: Defines the tools or build environment to be used in the pipeline. Here, it specifies that the 'maven3' tool should be used for the build.

```dockerfile
tools { 
        maven 'maven3' 
    }
```

`Options Block`: Specifies additional options for the pipeline. In this case, it configures the build discarder using the `logRotator` option, which keeps build logs for 15 days and retains the last 10 builds.

```dockerfile
options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '15', 
                    numToKeepStr: '10'
            )
    }
```

`Environment Block`: Defines environment variables that can be used within the pipeline. You can define any number of variables like a key-value pair. Here, it sets two variables: `APP_NAME` with the value "DCUBE\_APP" and `APP_ENV` with the value "DEV".

```dockerfile
   environment {
        APP_NAME = "DCUBE_APP",
        APP_ENV  = "DEV"
    }
```

`Stages`: Contains a list of stages, which represent the major steps or phases in the pipeline.

1. `stage('Cleanup Workspace')`: This stage is responsible for cleaning up the workspace by using the `cleanWs()` step, followed by executing a shell command to print a message indicating that the workspace has been cleaned up.
    
2. `stage('Code Checkout')`: This stage performs the code checkout from a Git repository using the `checkout` step. It specifies the branch to checkout ('\*/master') and the repository URL ('[https://github.com/spring-projects/spring-petclinic.git](https://github.com/spring-projects/spring-petclinic.git)').
    
3. `stage('Code Build')`: This stage executes a Maven build by running the command `mvn install -Dmaven.test.skip=true` using the `sh` step. It skips the execution of tests during the build.
    
4. `stage('Printing All Global Variables')`: This stage prints all the global variables available in the pipeline environment by running the `env` command using the `sh` step.
    

> *Each stage consists of a* `steps` block that contains the specific actions or steps to be performed within that stage. In the following example, we have shown a workplace cleanup step and echoing a variable we defined in the environment block.

Overall, this Jenkins pipeline defines a workflow that includes cleaning up the workspace, checking out code from a Git repository, building the code using Maven, and printing global variables. Lets practically execute this pipeline on a Jenkins server with a slave node.

# **Configure Pipeline as Code Job In Jenkins**

To run the pipeline code provided in this article, we need to configure Maven in the global tool configuration of Jenkins. Here are the steps to do so:

1. Go to “Manage Jenkins” and navigate to “Global Tool Configuration”.
    
2. Look for the Maven section and click on “Maven Installation”.
    
3. Add a new Maven configuration by clicking on the “Add Maven” button.
    
4. Fill in the necessary details for the Maven installation, such as the name (e.g., `maven3`) and the path to the Maven installation on your system.
    
5. Save the configuration.
    

By configuring Maven in this way, we can refer to it in the pipeline code using the tool name “maven3” to ensure it uses the appropriate Maven installation from the global tool configuration.

![](https://miro.medium.com/v2/resize:fit:700/1*DFAGbSURN8o32kaV0UdFpw.png align="left")

> ***Note:*** *We have selected “Install Automatically” option, which will download the selected version every time you execute the job.*

# **Creating & Building a Jenkins Pipeline Job**

Follow the steps given below to create and build our pipeline as code.

**Step 1:** Go to Jenkins home and select “New Item”

![](https://miro.medium.com/v2/resize:fit:700/1*Q7CGVXm9DamEWjTmmqYy2Q.png align="left")

**Step 2:** Give a name, select “Pipeline” and click ok.

![](https://miro.medium.com/v2/resize:fit:700/1*sdzR2bNZZRD16aWVGYJ9ww.png align="left")

**Step 3:** Scroll down to the Pipeline section, copy the whole pipeline code in the script section and save it.

![](https://miro.medium.com/v2/resize:fit:700/1*mri3soa4wYJIjBMgPqomjQ.png align="left")

**Step 4:** Now, click “Build Now” and wait for the build to start.

![](https://miro.medium.com/v2/resize:fit:700/1*gCNosU8GOcffJsuZINwXfw.png align="left")

During the job execution, you can monitor the progress of each stage in the stage view. Below is a screenshot of a successfully completed job. Additionally, you can access the job logs by clicking on the blue icon.

![](https://miro.medium.com/v2/resize:fit:700/1*D943GZKwvVXu4oEEigARmQ.png align="left")

If you have installed the Blue Ocean plugin, you can enjoy a user-friendly interface to view your job status and logs. Simply click on “Open in Blue Ocean” on the left to access the job in the Blue Ocean view, as depicted below.

![](https://miro.medium.com/v2/resize:fit:700/1*5wHv-ij4A9e28JNMLkD-6A.png align="left")

# **Executing Jenkins Pipeline From Github (Jenkinsfile)**

In the last section, we used the pipeline script directly on Jenkins. In this section, we will look at how to execute a pipeline script available in an SCM system like Github.

**Step 1:** Create a Github repo with our pipeline code in a file named `Jenkinsfile`. Or you can use this Github repo for testing. [https://github.com/devopscube/pipeline-as-code-demo](https://github.com/spring-projects/spring-petclinic.git)

**Step 2:** Follow the same steps we used for creating a pipeline job. But instead of entering the code directly into the script block, select the “Pipeline script from SCM” option and fill in the details as shown below.

1. **Definition:** Pipeline script from SCM
    
2. **Repository URL:** [https://github.com/devopscube/pipeline-as-code-demo](https://github.com/spring-projects/spring-petclinic.git)
    
3. **Script Path:** Jenkinsfile
    

![](https://miro.medium.com/v2/resize:fit:700/1*qS5OgkQHahITCIjA8en1vA.png align="left")

**Step 3:** Save the configuration and run the build. You should see a successful build.

# **Executing Jenkins Pipeline Stages In Parallel**

Parallel execution of Jenkins pipeline stages allows for efficient and faster build processes. In scenarios where individual stages are independent and do not rely on each other, executing them simultaneously can significantly reduce build times.

To implement parallelism in Jenkins pipelines using code, the parallel block is utilized. This block enables the definition of multiple stages within a single stage, facilitating parallel execution. Consider the following example, which demonstrates a stage consisting of three parallel stages. You can incorporate the provided code snippet into your existing pipeline to test this functionality.

```dockerfile
stage('Environment Analysis') {

            parallel {

                stage('Priting All Global Variables') {
                    steps {
                        sh """
                        env
                        """
                    }
                }

                stage('Execute Shell') {
                    steps {
                        sh 'echo "Hello"'
                    }
                }

                stage('Print ENV variable') {
                    steps {
                        sh "echo ${APP_ENV}"
                    }
                }

            
            }
        }
```

You can clearly see the parallel execution on blue ocean view.

![](https://miro.medium.com/v2/resize:fit:700/1*o6Gx9W-7lcnAXd7nKXpRIg.png align="left")

# **How to Generate Jenkins Pipeline Scripts?**

Are you wondering if you can generate Jenkins pipeline scripts? Well, the answer is a resounding yes! Jenkins provides a built-in pipeline script generator that makes script generation a breeze. Here’s how it works:

To access the pipeline script generator, simply navigate to the `/pipeline-syntax` path on your Jenkins instance:

```dockerfile
http://<your-jenkins-ip>:port/pipeline-syntax/
```

Alternatively, you can find the syntax generator path within your pipeline job configuration, as shown below.

![](https://miro.medium.com/v2/resize:fit:700/1*3_waqZym-H-Bm7UGpGf_fA.png align="left")

# **Snippet Generator**

Generating Jenkins pipeline scripts becomes effortless with the Snippet Generator. This powerful tool allows you to generate scripts for various steps used within your stages. Let’s take a look at how the Snippet Generator works:

The Snippet Generator provides a user-friendly interface where you can select the desired option from the steps dropdown. Once you’ve made your selection, simply fill in the necessary details and click on the “Generate Script” button. Voila! You now have a script ready to be incorporated into your pipeline. It’s that simple!

![](https://miro.medium.com/v2/resize:fit:700/1*jUFFAjNBpmqlpHaFa_tS-Q.png align="left")

Declarative Directive Generator

# **Declarative Directive Generator**

To streamline the process of generating various options in your pipeline, you can make use of the directive generator. This versatile tool enables you to generate directives such as options, parameters, triggers, and more.

Let’s take a specific example of generating the agent block using the directive generator. By inputting the relevant details and configurations, you can effortlessly generate the agent block that aligns with your requirements.

![](https://miro.medium.com/v2/resize:fit:700/1*wz0teon6a2-2wiVBHQY08Q.png align="left")