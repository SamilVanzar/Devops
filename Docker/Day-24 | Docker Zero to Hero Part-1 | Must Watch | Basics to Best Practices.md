# Repo to learn Docker with examples. Contributions are most welcome.

## If you found this repo useful, give it a STAR 🌠

You can watch the video version of this repo on my youtube playlist. -> https://www.youtube.com/watch?v=7JZP345yVjw&list=PLdpzxOOAlwvLjb0vTD9BXLOwwLD_GWCmC


## What is a container ?

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.

Ok, let me make it easy !!!

A container is a bundle of Application, Application libraries required to run your application and the minimum system dependencies.

![Screenshot 2023-02-07 at 7 18 10 PM](https://user-images.githubusercontent.com/43399466/217262726-7cabcb5b-074d-45cc-950e-84f7119e7162.png)



## Containers vs Virtual Machine 

Containers and virtual machines are both technologies used to isolate applications and their dependencies, but they have some key differences:

    1. Resource Utilization: Containers share the host operating system kernel, making them lighter and faster than VMs. VMs have a full-fledged OS and hypervisor, making them more resource-intensive.

    2. Portability: Containers are designed to be portable and can run on any system with a compatible host operating system. VMs are less portable as they need a compatible hypervisor to run.

    3. Security: VMs provide a higher level of security as each VM has its own operating system and can be isolated from the host and other VMs. Containers provide less isolation, as they share the host operating system.

   4.  Management: Managing containers is typically easier than managing VMs, as containers are designed to be lightweight and fast-moving.



## Why are containers light weight ?

[ A virtual machine is a whether you are creating EC2 instance or virtual machine created on Hypervisor, they have complete operating system.Because of this they are very heavy in nature. Lets say you create EC2 instance so the VMinstance you have created , whether you use it to full extent or whether you don't use it , you still have to pay money for AWS or your cloud provider. On contrary, containers are very light weight in nature which means to say that on a <u><b>specific virtual machine itself you can create multiple containers and if the containers are not running they DON'T USE RESOURCES FROM VM KERNEL </u></b> . So what is kernel ? Kernel is part of your host operating system. Lets say you buy physical infrastruture, or you create EC2 instance on AWS ,it basically has operating system that has a kernel. Now kernel is heart of operating system. Now you install docker on top of it. Now what docker says is whenever it creates containers, it says that okay container you can use all the required resources from the kernel but container itself can only have system dependencies. Why this system dependencies have to be there ? Because container A and container B has to be isolated. So container A can be container of project 1 and container B can be container of project 2. If all of the things, like container 1 is using all the resources from kernel and container 2 is also using all the resources from kernel , then it means to say that some hacker who is on container A can access the application of container B. So there has to be a logical isolation. So even though containers don't have complete OS , but they need to have some part of logical isolation . Without that logical isolation, any people who can login into K8s cluster he can get into all the container that are present on your cluster which is awful hack. So Container A should have logical isolation with Container B . So that can be avoided by using some system dependencies, and rest all of the containers can share resources from kernel or host operating system . <u><b> If containers are running directly on the host operating system, they utilize resources directly from the host OS.But if containers are running inside virtual machines, they utilize the resources allocated to those VMs — and those VM resources, in turn, come from the host operating system (via the hypervisor). Also since containers don't have their own CPU and RAM and its own kernel they are lightweight.</u></b> ]

9:38

Containers are lightweight because they use a technology called containerization, which allows them to share the host operating system's kernel and libraries, while still providing isolation for the application and its dependencies. This results in a smaller footprint compared to traditional virtual machines, as the containers do not need to include a full operating system. Additionally, Docker containers are designed to be minimal, only including what is necessary for the application to run, further reducing their size.

Let's try to understand this with an example:

Below is the screenshot of official ubuntu base image which you can use for your container. It's just ~ 22 MB, isn't it very small ? on a contrary if you look at official ubuntu VM image it will be close to ~ 2.3 GB. So the container base image is almost 100 times less than VM image.

![Screenshot 2023-02-08 at 3 12 38 PM](https://user-images.githubusercontent.com/43399466/217493284-85411ae0-b283-4475-9729-6b082e35fc7d.png)


[ So this Ubuntu container image has all of the files and folders which are listed below under section "Files and Folders in containers base image" .Anybody who is implementing application on this ubuntu base image ,that application will use all of files and folders listed in container image like "/bin" , "var" , "/sbin" , "/usr" , "/root" from the ubuntu official docker image and rest of the things like kernel related things , networking stack related things , file system related things, namespace, control groups, system calls it will use from the host operating system or from the kernel. As you can see from size, container image is 100 times smaller than VM image.So this is very good advantage of docker. Instead of running one Virtual machine what you can do is run 10 or 20 containers on same virtual machine depending on how much resources your containers are utilizing from host operating system.So scenarios where your containers are utilizing more resources from your host operating system it might not be applicable but by default the beauty of containers is if they are not running they will allow other containers to use kernel or host operating system. So thats why you can run "n" number of containers depending on how much resources your containers are using from your host operating system. So what is blocking the same on virtual machine is that your VM has individual operating system , so the kernel and all the resources used by the kernel in VM A do not allow other VMs like VM B  to use its resources ]

To provide a better picture of files and folders that containers base images have and files and folders that containers use from host operating system (not 100 percent accurate -> varies from base image to base image). Refer below.



### Files and Folders in containers base images

```
    /bin: contains binary executable files, such as the ls, cp, and ps commands.

    /sbin: contains system binary executable files, such as the init and shutdown commands.

    /etc: contains configuration files for various system services.

    /lib: contains library files that are used by the binary executables.

    /usr: contains user-related files and utilities, such as applications, libraries, and documentation.

    /var: contains variable data, such as log files, spool files, and temporary files.

    /root: is the home directory of the root user.
```

--> So above are the basic files and folders forms a basic logical isolation from one container to another container. That means a container cannot share this files and folders with abnother container. If a container is sharing this files and folders , it means you are compromising security of your container. So these files and folders are not shared from kernel and are part of your base image.


### Files and Folders that containers use from host operating system

```
    The host's file system: Docker containers can access the host file system using bind mounts, which allow the container to read and write files in the host file system.

    Networking stack: The host's networking stack is used to provide network connectivity to the container. Docker containers can be connected to the host's network directly or through a virtual network.

    System calls: The host's kernel handles system calls from the container, which is how the container accesses the host's resources, such as CPU, memory, and I/O.

    Namespaces: Docker containers use Linux namespaces to create isolated environments for the container's processes. Namespaces provide isolation for resources such as the file system, process ID, and network.

    Control groups (cgroups): Docker containers use cgroups to limit and control the amount of resources, such as CPU, memory, and I/O, that a container can access.
    
```
[ Above are the files that container use from host operating system or from the kernel. Because "/etc" ,"/root" , "/var" are not the only required files and folder that can form your complete virtual machine or complete virtual image or your operating system. There has to be lot and lot more files so that other files which are described above list "host file system" , " networking stack", "system calls" , "name spaces" ," control groups" they are all part of kernel]


It's important to note that while a container uses resources from the host operating system, it is still isolated from the host and other containers, so changes to the container do not affect the host or other containers.

**Note:** There are multiple ways to reduce your VM image size as well, but I am just talking about the default for easy comparision and understanding.

so, in a nutshell, container base images are typically smaller compared to VM images because they are designed to be minimalist and only contain the necessary components for running a specific application or service. VMs, on the other hand, emulate an entire operating system, including all its libraries, utilities, and system files, resulting in a much larger size. 

I hope it is now very clear why containers are light weight in nature.



## Docker


### What is Docker ?

Docker is a containerization platform that provides easy way to containerize your applications, which means, using Docker you can build container images, run the images to create containers and also push these containers to container regestries such as DockerHub, Quay.io and so on.

In simple words, you can understand as `containerization is a concept or technology` and `Docker Implements Containerization`.

[ Analogy : "Docker is Hypervisor and Containers are VM.Just like Hypervisor are used to implement VMs, similarly Docker is used to implement containers "]

[ Another question comes to mind which is are their any other platforms apart from Docker to implement containerization ?
Answer is Yes . Podman is another containerization platform and good competitor to docker platform. <u>It also addresses some complicated problems of Docker like docker is a centralized platform where it has only one docker daemon </u> but do not go to podman directly . Learn Docker first as it is very easy to start with and understand and troubleshoot the issues.Once concept of docker are comfortable , you can go to podman , builah ,scopio etc]

### Docker Architecture ?

![image](https://user-images.githubusercontent.com/43399466/217507877-212d3a60-143a-4a1d-ab79-4bb615cb4622.png)

The above picture, clearly indicates that Docker Deamon is brain of Docker. If Docker Deamon is killed, stops working for some reasons, Docker is brain dead :p (sarcasm intended).

### Docker LifeCycle 

We can use the above Image as reference to understand the lifecycle of Docker.

There are three important things,

1. docker build -> builds docker images from Dockerfile
2. docker run   -> runs container from docker images
3. docker push  -> push the container image to public/private regestries to share the docker images.

![Screenshot 2023-02-08 at 4 32 13 PM](https://user-images.githubusercontent.com/43399466/217511949-81f897b2-70ee-41d1-b229-38d0572c54c7.png)



### Understanding the terminology (Inspired from Docker Docs)


#### Docker daemon

The Docker daemon (dockerd) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services. 
[docker daemon receives commands from docker client and executes them. When they execute and they create docker image , docker containers and finally using this docker daemon itself you can push the docker images to your docker registry. So docker daemon is the heart of docker. If docker daemon goes down , technically your docker will stop functioning or your containers will stop working. Because containers are running against this docker daemon and docker daemon is the one listinening to these containers. ]


#### Docker client

The Docker client (docker) is the primary way that many Docker users interact with Docker. When you use commands such as docker run, the client sends these commands to dockerd (docker daemon), which carries them out or performs them. The docker command uses the Docker API. The Docker client can communicate with more than one daemon.
[Docker client is docker CLI. As a user you will use this docker CLI to execute docker commands which are received by the docker daemon and docker daemon ensures to create your requirements. For ex: If you will ask "docker build" then docker daemon will create docker image for you. If you use command "docker run" , docker daemon will create container for you. If through docker client you execute command called "docker pull", then docker daemon will receive the command and will pull the containers from registry.]


#### Docker Desktop

Docker Desktop is an easy-to-install application for your Mac, Windows or Linux environment that enables you to build and share containerized applications and microservices. Docker Desktop includes the Docker daemon (dockerd), the Docker client (docker), Docker Compose, Docker Content Trust, Kubernetes, and Credential Helper. For more information, see Docker Desktop.


#### Docker registries

Registry is a platform that is used to share your docker images. Registry can be Private registry or public registry. Even inside your public registry, you can create private repositories.

A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use, and Docker is configured to look for images on Docker Hub by default. You can even run your own private registry.

When you use the docker pull or docker run commands, the required images are pulled from your configured registry. When you use the docker push command, your image is pushed to your configured registry.
Docker objects

When you use Docker, you are creating and using images, containers, networks, volumes, plugins, and other objects. This section is a brief overview of some of those objects.

[ So Docker registry is nothing but place where you store docker images . So you share your docker images with other person , organizations, or other people in world . So how do you share these things ?
So there has to be a centralized place . So this centralized place is called docker registry. One such registry is dockerhub. What is dockerhub?
Dockerhub is a place where you share your docker images with external world. Similarly you can create your own private registry as well since dockerhub is public registry. So in our example of ubuntu , ubuntu as an organization has created official "ubuntu" docker image and pushed it to dockerhub. So anyone in the world who wants to use "ubuntu" docker image, can download from dockerhub . So users on top of docker base image can add their application and run docker container. Just like dockerhub there is one more public registry called "quay.io". Even in gihub you can create private registry and share it with certain set of people.
One common question that comes up is difference between github and dockerhub?
So github is where you save your sourcecode . <u><b>Github a version control system platform of a sourcecode whereas dockerhub again is a version control platform for your docker images</b></u>]



[The concept of containers is complicated, once you understand the container concept, other things are just using docker platform to create those things.If you understand the concept of container, docker is just a platform. Docker platform will allow you to create docker images , containers and all these things. So you can just think of it as CLI tool or daemon service that is receiving your request.
So what is lifecycle of this daemon service ?
- You as docker user will write a docker file. So what is docker file?
-So docker file is basically a set of instructions.Like you tell docker daemon that okay get me a base image like in previous example we have ubuntu base image. So let say we request in docker file : Get me a ubuntu base image and inside that put my provided source code and once source code is placed, run this specific commands like if its node.js then run nodejs commands, if its python then run pip command ,so that all the application libraries are installed inside this docker image and finally execute the set of commands so that application can get executed/started. Now using docker CLI , you submit this docker file to docker daemon using docker build command and it creates Docker image. What is docker image ? It can be considered as snapshot in VM. This image can be executed or docker image can be run , once this docker image is run using docker run command,what happens is docker daemon received docker run command and it creates docker container for you. This docker container is your final output which has application libraries, system dependencies and your application itself. You can share this docker container to anybody in world using Public Registry and they can simply download this container and they can run your application anywhere. Like they can run your application anywhere even without downloading a single dependency because docker container / docker image already have all of those things. They can simply download your docker image and run/execute that docker image and create docker container and <u>they can run it on any platform</u>. So this is the beauty of it. On contrary if you didn't use containers/docker then you have to manually download the application, you will create an EC2 instance, on this EC2 instance you will run the application. Once you run the application then you will expose the port and lot of other things to finally run your application on Vm or EC2 instance. So this problem is solved by docker. Apart from docker containers being light in weight and all other things the other important thing docker solves is the complexities. So instead od user doing 100 manual actions, user writes one docker file which can contain an application and anybody who executes this docker image everything is created out of the box. So your docker container is also helping you in efficiency]


#### Dockerfile

Dockerfile is a file where you provide the steps to build your Docker Image. You can understand it as set of instructions that you are sharing with docker daemon.


#### Images

An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

You might create your own images or you might only use those created by others and published in a registry. To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.



[ Below we will see simple example to print "Hello World" using docker. If lets say we don't use docker and had been using VM then for such simple python program we had to create VM/EC2 instance,even if they want to run this on their laptop, even then they had to install python , after installing python , they have to install pip . So on VM after creating VM instance ,  they have to select required VM image for that , after that they have to install python 3 , then they have to install pip dependencies, then they have to ensure python 3 is working or not. But the advantage of docker is lets say you want to share this "hello world" application with QA team , you don't have to do all these things. They can just download the docker image and then perform "docker run" command and they can test the application ]

## INSTALL DOCKER

A very detailed instructions to install Docker are provide in the below link

https://docs.docker.com/get-docker/

For Demo, 

You can create an Ubuntu EC2 Instance on AWS and run the below commands to install docker.

```
sudo apt update
sudo apt install docker.io -y
```


### Start Docker and Grant Access

A very common mistake that many beginners do is, After they install docker using the sudo access, they miss the step to Start the Docker daemon and grant acess to the user they want to use to interact with docker and run docker commands.

Always ensure the docker daemon is up and running.

A easy way to verify your Docker installation is by running the below command

```
docker run hello-world
```

If the output says:

```
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied.
See 'docker run --help'.
```

This can mean two things, 
1. Docker deamon is not running.
2. Your user does not have access to run docker commands.


### Start Docker daemon

You use the below command to verify if the docker daemon is actually started and Active

```
sudo systemctl status docker
```




If you notice that the docker daemon is not running, you can start the daemon using the below command

```
sudo systemctl start docker
```


### Grant Access to your user to run docker commands

[ This is another drawback of docker . You can only use root user to run docker commands. So docker daemon needs root user access . So you need to add regular user to sudo group to execute docker commands. Now this itself is a drawback that root needs to be run as "root" because if someone gets access to docker daemon they can easily hack or get access of the entire sytem. This is another drawback of docker along with docker being monolithic. By monolithic , it means single process that is if docker goes down, all the containers would go down]


To grant access to your user to run the docker command, you should add the user to the Docker Linux group. Docker group is create by default when docker is installed.

```
sudo usermod -aG docker ubuntu
```

In the above command `ubuntu` is the name of the user, you can change the username appropriately.

**NOTE:** : You need to logout and login back for the changes to be reflected.


### Docker is Installed, up and running 🥳🥳

Use the same command again, to verify that docker is up and running.

```
docker run hello-world
```

Output should look like:

```
....
....
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
...
```


## Great Job, Now start with the examples folder to write your first Dockerfile and move to the next examples. Happy Learning :)

### Clone this repository and move to example folder

```
git clone https://github.com/iam-veeramalla/Docker-Zero-to-Hero
cd  examples
```

### Login to Docker [Create an account with https://hub.docker.com/]

```
docker login
```

```
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: abhishekf5
Password:
WARNING! Your password will be stored unencrypted in /home/ubuntu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

### Build your first Docker Image

You need to change the username accoringly in the below command

```
docker build -t abhishekf5/my-first-docker-image:latest .
```

Output of the above command

```
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

### Verify Docker Image is created

```
docker images
```

Output 

```
REPOSITORY                         TAG       IMAGE ID       CREATED          SIZE
abhishekf5/my-first-docker-image   latest    960d37536dcd   26 seconds ago   467MB
ubuntu                             latest    58db3edaf2be   13 days ago      77.8MB
hello-world                        latest    feb5d9fea6a5   16 months ago    13.3kB
```

### Run your First Docker Container

```
docker run -it abhishekf5/my-first-docker-image
```

Output

```
Hello World
```

### Push the Image to DockerHub and share it with the world

```
docker push abhishekf5/my-first-docker-image
```

Output

```
Using default tag: latest
The push refers to repository [docker.io/abhishekf5/my-first-docker-image]
896818320e80: Pushed
b8088c305a52: Pushed
69dd4ccec1a0: Pushed
c5ff2d88f679: Mounted from library/ubuntu
latest: digest: sha256:6e49841ad9e720a7baedcd41f9b666fcd7b583151d0763fe78101bb8221b1d88 size: 1157
```

### You must be feeling like a champ already 