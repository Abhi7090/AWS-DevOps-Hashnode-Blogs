---
title: "Docker Basic and Advance"
datePublished: Fri Nov 10 2023 13:07:54 GMT+0000 (Coordinated Universal Time)
cuid: closmv11s000409l6dif60wr0
slug: docker

---

## [What is a container ?](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#what-is-a-container-)

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.

Ok, let me make it easy !!!

A container is a bundle of Application, Application libraries required to run your application and the minimum system dependencies.

[![Screenshot 2023-02-07 at 7 18 10 PM](https://user-images.githubusercontent.com/43399466/217262726-7cabcb5b-074d-45cc-950e-84f7119e7162.png align="left")](https://user-images.githubusercontent.com/43399466/217262726-7cabcb5b-074d-45cc-950e-84f7119e7162.png)

## [Containers vs Virtual Machine](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#containers-vs-virtual-machine)

Containers and virtual machines are both technologies used to isolate applications and their dependencies, but they have some key differences:

```plaintext
1. Resource Utilization: Containers share the host operating system kernel, making them lighter and faster than VMs. VMs have a full-fledged OS and hypervisor, making them more resource-intensive.

2. Portability: Containers are designed to be portable and can run on any system with a compatible host operating system. VMs are less portable as they need a compatible hypervisor to run.

3. Security: VMs provide a higher level of security as each VM has its own operating system and can be isolated from the host and other VMs. Containers provide less isolation, as they share the host operating system.
```

1. Management: Managing containers is typically easier than managing VMs, as containers are designed to be lightweight and fast-moving.
    

## [Why are containers light weight ?](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#why-are-containers-light-weight-)

Containers are lightweight because they use a technology called containerization, which allows them to share the host operating system's kernel and libraries, while still providing isolation for the application and its dependencies. This results in a smaller footprint compared to traditional virtual machines, as the containers do not need to include a full operating system. Additionally, Docker containers are designed to be minimal, only including what is necessary for the application to run, further reducing their size.

Let's try to understand this with an example:

Below is the screenshot of official ubuntu base image which you can use for your container. It's just ~ 22 MB, isn't it very small ? on a contrary if you look at official ubuntu VM image it will be close to ~ 2.3 GB. So the container base image is almost 100 times less than VM image.

[![Screenshot 2023-02-08 at 3 12 38 PM](https://user-images.githubusercontent.com/43399466/217493284-85411ae0-b283-4475-9729-6b082e35fc7d.png align="left")](https://user-images.githubusercontent.com/43399466/217493284-85411ae0-b283-4475-9729-6b082e35fc7d.png)

To provide a better picture of files and folders that containers base images have and files and folders that containers use from host operating system (not 100 percent accurate -&gt; varies from base image to base image). Refer below.

### [Files and Folders in containers base images](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#files-and-folders-in-containers-base-images)

```plaintext
    /bin: contains binary executable files, such as the ls, cp, and ps commands.

    /sbin: contains system binary executable files, such as the init and shutdown commands.

    /etc: contains configuration files for various system services.

    /lib: contains library files that are used by the binary executables.

    /usr: contains user-related files and utilities, such as applications, libraries, and documentation.

    /var: contains variable data, such as log files, spool files, and temporary files.

    /root: is the home directory of the root user.
```

### [Files and Folders that containers use from host operating system](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#files-and-folders-that-containers-use-from-host-operating-system)

```dockerfile
    The host's file system: Docker containers can access the host file system using bind mounts, which allow the container to read and write files in the host file system.

    Networking stack: The host's networking stack is used to provide network connectivity to the container. Docker containers can be connected to the host's network directly or through a virtual network.

    System calls: The host's kernel handles system calls from the container, which is how the container accesses the host's resources, such as CPU, memory, and I/O.

    Namespaces: Docker containers use Linux namespaces to create isolated environments for the container's processes. Namespaces provide isolation for resources such as the file system, process ID, and network.

    Control groups (cgroups): Docker containers use cgroups to limit and control the amount of resources, such as CPU, memory, and I/O, that a container can access.
```

It's important to note that while a container uses resources from the host operating system, it is still isolated from the host and other containers, so changes to the container do not affect the host or other containers.

**Note:** There are multiple ways to reduce your VM image size as well, but I am just talking about the default for easy comparision and understanding.

so, in a nutshell, container base images are typically smaller compared to VM images because they are designed to be minimalist and only contain the necessary components for running a specific application or service. VMs, on the other hand, emulate an entire operating system, including all its libraries, utilities, and system files, resulting in a much larger size.

I hope it is now very clear why containers are light weight in nature.

## [Docker](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#docker)

### [What is Docker ?](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#what-is-docker-)

Docker is a containerization platform that provides easy way to containerize your applications, which means, using Docker you can build container images, run the images to create containers and also push these containers to container regestries such as DockerHub, [Quay.io](http://Quay.io) and so on.

In simple words, you can understand as `containerization is a concept or technology` and `Docker Implements Containerization`.

### [Docker Architecture ?](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#docker-architecture-)

[![image](https://user-images.githubusercontent.com/43399466/217507877-212d3a60-143a-4a1d-ab79-4bb615cb4622.png align="left")](https://user-images.githubusercontent.com/43399466/217507877-212d3a60-143a-4a1d-ab79-4bb615cb4622.png)

The above picture, clearly indicates that Docker Deamon is brain of Docker. If Docker Deamon is killed, stops working for some reasons, Docker is brain dead :p (sarcasm intended).

### [Docker LifeCycle](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#docker-lifecycle)

We can use the above Image as reference to understand the lifecycle of Docker.

There are three important things,

1. docker build -&gt; builds docker images from Dockerfile
    
2. docker run -&gt; runs container from docker images
    
3. docker push -&gt; push the container image to public/private regestries to share the docker images.
    

[![Screenshot 2023-02-08 at 4 32 13 PM](https://user-images.githubusercontent.com/43399466/217511949-81f897b2-70ee-41d1-b229-38d0572c54c7.png align="left")](https://user-images.githubusercontent.com/43399466/217511949-81f897b2-70ee-41d1-b229-38d0572c54c7.png)

### [Understanding the terminology (Inspired from Docker Docs)](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#understanding-the-terminology-inspired-from-docker-docs)

#### [Docker daemon](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#docker-daemon)

The Docker daemon (dockerd) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.

#### [Docker client](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#docker-client)

The Docker client (docker) is the primary way that many Docker users interact with Docker. When you use commands such as docker run, the client sends these commands to dockerd, which carries them out. The docker command uses the Docker API. The Docker client can communicate with more than one daemon.

#### [Docker Desktop](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#docker-desktop)

Docker Desktop is an easy-to-install application for your Mac, Windows or Linux environment that enables you to build and share containerized applications and microservices. Docker Desktop includes the Docker daemon (dockerd), the Docker client (docker), Docker Compose, Docker Content Trust, Kubernetes, and Credential Helper. For more information, see Docker Desktop.

#### [Docker registries](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#docker-registries)

A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use, and Docker is configured to look for images on Docker Hub by default. You can even run your own private registry.

When you use the docker pull or docker run commands, the required images are pulled from your configured registry. When you use the docker push command, your image is pushed to your configured registry. Docker objects

When you use Docker, you are creating and using images, containers, networks, volumes, plugins, and other objects. This section is a brief overview of some of those objects.

#### [Dockerfile](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#dockerfile)

Dockerfile is a file where you provide the steps to build your Docker Image.

#### [Images](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#images)

An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

You might create your own images or you might only use those created by others and published in a registry. To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.

## [INSTALL DOCKER](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#install-docker)

A very detailed instructions to install Docker are provide in the below link

[https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)

For Demo,

You can create an Ubuntu EC2 Instance on AWS and run the below commands to install docker.

```plaintext
sudo apt update
sudo apt install docker.io -y
```

### [Start Docker and Grant Access](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#start-docker-and-grant-access)

A very common mistake that many beginners do is, After they install docker using the sudo access, they miss the step to Start the Docker daemon and grant acess to the user they want to use to interact with docker and run docker commands.

Always ensure the docker daemon is up and running.

A easy way to verify your Docker installation is by running the below command

```plaintext
docker run hello-world
```

If the output says:

```plaintext
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied.
See 'docker run --help'.
```

This can mean two things,

1. Docker deamon is not running.
    
2. Your user does not have access to run docker commands.
    

### [Start Docker daemon](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#start-docker-daemon)

You use the below command to verify if the docker daemon is actually started and Active

```plaintext
sudo systemctl status docker
```

If you notice that the docker daemon is not running, you can start the daemon using the below command

```plaintext
sudo systemctl start docker
```

### [Grant Access to your user to run docker commands](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#grant-access-to-your-user-to-run-docker-commands)

To grant access to your user to run the docker command, you should add the user to the Docker Linux group. Docker group is create by default when docker is installed.

```plaintext
sudo usermod -aG docker ubuntu
```

In the above command `ubuntu` is the name of the user, you can change the username appropriately.

**NOTE:** : You need to logout and login back for the changes to be reflected.

### [Docker is Installed, up and running 🥳🥳](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#docker-is-installed-up-and-running-)

Use the same command again, to verify that docker is up and running.

```dockerfile
docker run hello-world
```

Output should look like:

```dockerfile
....
....
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
...
```

### [Clone this repository and move to example folder](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#clone-this-repository-and-move-to-example-folder)

```dockerfile
git clone https://github.com/Abhi7090/Docker-Zero-to-Hero
cd  examples
```

### [Login to Docker \[Create an account with](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#login-to-docker-create-an-account-with-httpshubdockercom) [https://hub.docker.com/](https://hub.docker.com/)\]

```dockerfile
docker login
```

```dockerfile
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: abhireddy934@gmail.com
Password:
WARNING! Your password will be stored unencrypted in /home/ubuntu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

### [Build your first Docker Image](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#build-your-first-docker-image)

You need to change the username accoringly in the below command, t - tag name as latest

```dockerfile
docker build -t namespace/my-first-docker-image:latest .
```

Output of the above command

```dockerfile
    Sending build context to Docker daemon  992.8kB
    Step 1/6 : FROM ubuntu:latest
    latest: Pulling from library/ubuntu
    677076032cca: Pull complete
    Digest: sha256:9a0bdde4188b896a372804be2384015e90e3f84906b750c1a53539b585fbbe7f
    Status: Downloaded newer image for ubuntu:latest
     ---> 58db3edaf2be
    Step 2/6 : WORKDIR /app
     ---> Running in 630f5e4db7d3
    Removing intermediate container 630f5e4db7d3
     ---> 6b1d9f654263
    Step 3/6 : COPY . /app
     ---> 984edffabc23
    Step 4/6 : RUN apt-get update && apt-get install -y python3 python3-pip
     ---> Running in a558acdc9b03
    Step 5/6 : ENV NAME World
     ---> Running in 733207001f2e
    Removing intermediate container 733207001f2e
     ---> 94128cf6be21
    Step 6/6 : CMD ["python3", "app.py"]
     ---> Running in 5d60ad3a59ff
    Removing intermediate container 5d60ad3a59ff
     ---> 960d37536dcd
    Successfully built 960d37536dcd
    Successfully tagged abhishekf5/my-first-docker-image:latest
```

### [Verify Docker Image is created](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#verify-docker-image-is-created)

```dockerfile
docker images
```

Output

```dockerfile
REPOSITORY                         TAG       IMAGE ID       CREATED          SIZE
namespace/my-first-docker-image   latest    960d37536dcd   26 seconds ago   467MB
ubuntu                             latest    58db3edaf2be   13 days ago      77.8MB
hello-world                        latest    feb5d9fea6a5   16 months ago    13.3kB
```

### [Run your First Docker Container](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#run-your-first-docker-container)

it - iterator

```dockerfile
docker run -it namespace/my-first-docker-image
```

Output

```dockerfile
Hello World
```

### [Push the Image to DockerHub and share it with the world](https://github.com/Abhi7090/Docker-Zero-to-Hero/tree/main#push-the-image-to-dockerhub-and-share-it-with-the-world)

```dockerfile
docker push namespace/my-first-docker-image
```

Output

```dockerfile
Using default tag: latest
The push refers to repository [docker.io/abhishekf5/my-first-docker-image]
896818320e80: Pushed
b8088c305a52: Pushed
69dd4ccec1a0: Pushed
c5ff2d88f679: Mounted from library/ubuntu
latest: digest: sha256:6e49841ad9e720a7baedcd41f9b666fcd7b583151d0763fe78101bb8221b1d88 size: 1157
```

## What Are Docker Volumes?

**Volumes** are a mechanism for storing data outside containers. All volumes are managed by Docker and stored in a dedicated directory on your host, usually `/var/lib/docker/volumes` for Linux systems.

Volumes are mounted to filesystem paths in your containers. When containers write to a path beneath a volume mount point, the changes will be applied to the volume instead of the container’s [writable image layer](https://docs.docker.com/storage/storagedriver/#container-and-layers). The written data will still be available if the container stops – as the volume’s stored separately on your host, it can be remounted to another container or accessed directly using manual tools.

Command :

docker volume create --name "name of volume" --opt type=none --opt device=/home/ubuntu/projects/volumes/name of dir to store data --opt o=bind

docker run -d -p 8000:8000 --mount source=name of volume. target=/data django-todo-app:latest

# **Docker Compose**

Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application's services. Then, with a single command, you create and start all the services from your configuration.

Compose works in all environments; production, staging, development, testing, as well as CI workflows. It also has commands for managing the whole lifecycle of your application:

* Start, stop, and rebuild services
    
* View the status of running services
    
* Stream the log output of running services
    
* Run a one-off command on a service
    
    command :
    
    docker-compose down : stop the whole application
    
    docker-compose up : start the whole application
    
    # **Docker Multi-stage builds**
    
    Multi-stage builds are useful to anyone who has struggled to optimize Dockerfiles while keeping them easy to read and maintain.
    
    With multi-stage builds, you use multiple `FROM` statements in your Dockerfile. Each `FROM` instruction can use a different base, and each of them begins a new stage of the build. You can selectively copy artifacts from one stage to another, leaving behind everything you don't want in the final image.
    
    The following Dockerfile has two separate stages: one for building a binary, and another where we copy the binary into.
    
    ```dockerfile
    # syntax=docker/dockerfile:1
    FROM golang:1.21
    WORKDIR /src
    COPY <<EOF ./main.go
    package main
    
    import "fmt"
    
    func main() {
      fmt.Println("hello, world")
    }
    EOF
    RUN go build -o /bin/hello ./main.go
    
    FROM scratch
    COPY --from=0 /bin/hello /bin/hello
    CMD ["/bin/hello"]
    ```
    
    You only need the single Dockerfile. No need for a separate build script. Just run `docker build`.
    
    ```console
    $ docker build -t hello .
    ```
    
    The end result is a tiny production image with nothing but the binary inside. None of the build tools required to build the application are included in the resulting image.
    
    How does it work? The second `FROM` instruction starts a new build stage with the `scratch` image as its base. The `COPY --from=0` line copies just the built artifact from the previous stage into this new stage. The Go SDK and any intermediate artifacts are left behind, and not saved in the final image.
    
    ## [**Name your build stages**](https://docs.docker.com/build/building/multi-stage/#name-your-build-stages)
    
    By default, the stages aren't named, and you refer to them by their integer number, starting with 0 for the first `FROM` instruction. However, you can name your stages, by adding an `AS <NAME>` to the `FROM` instruction. This example improves the previous one by naming the stages and using the name in the `COPY` instruction. This means that even if the instructions in your Dockerfile are re-ordered later, the `COPY` doesn't break.
    
    content\_copy
    
    ```dockerfile
    # syntax=docker/dockerfile:1
    FROM golang:1.21 as build
    WORKDIR /src
    COPY <<EOF /src/main.go
    package main
    
    import "fmt"
    
    func main() {
      fmt.Println("hello, world")
    }
    EOF
    RUN go build -o /bin/hello ./main.go
    
    FROM scratch
    COPY --from=build /bin/hello /bin/hello
    CMD ["/bin/hello"]
    ```
    
    # Two-tier-flask-app
    
* 1. Clone the repo : [https://github.com/LondheShubham153/two-tier-flask-app.git](https://github.com/your-username/your-repo-name.git)
        
    2. Navigate to cd two-tier-flask-app
        
    3. Create two images :
        
        Image 1 :
        
        docker build -t mysql:latest .
        
        docker run -d -p 3306:3306 -e MYSQL\_ROOT\_PASSWORD=test@123 -e MYSQL\_DATABASE=test\_db -e MYSQL\_USER=admin -e MYSQL\_PASSWORD=admin --name mysql-app mysql:latest
        
        Image 2 :
        
        docker build -t flask-app:latest .
        
        docker run -d -p 5000:5000 -e MYSQL\_HOST=mysql-app -e MYSQL-USER=admin -e MYSQL\_PASSWORD=admin -e MYSQL\_DB=test\_db --name flaskapp flask-app:latest
        
    4. Create network for two images :
        
        docker network create -d bridge two-tier-app-nw
        
        docker inspect two-tier-app-nw
        
    5. Now add network to two images due to interaction between two images
        
        docker run -d -p 3306:3306 -e MYSQL\_ROOT\_PASSWORD=test@123 -e MYSQL\_DATABASE=test\_db -e MYSQL\_USER=admin -e MYSQL\_PASSWORD=admin --network two-tier-app-nw mysql:latest
        
        docker run -d -p 5000:5000 -e MYSQL\_HOST=mysql -e MYSQL\_USER=admin -e MYSQL\_PASSWORD=admin -e MYSQL\_DB=test\_db --network two-tier-app-nw flask-app:latest
        
    6. Copy Public IP address :
        
        public-ip:5000
        
    7. connect with mysql database and create a table
        
        docker exec -it "container id" bash
        
        mysql -u root -p (enter root password)
        
        create database mydb;
        
        show databases;
        
        use mydb;
        
        CREATE TABLE messages ( id INT AUTO\_INCREMENT PRIMARY KEY, message TEXT );
        
        select \* from messages
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700316240077/5ab37d93-ec45-4531-9bb1-060df7c0399b.jpeg align="center")
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1700316202989/8f9d1435-6afb-4849-a8a0-25764b25f57c.jpeg align="center")