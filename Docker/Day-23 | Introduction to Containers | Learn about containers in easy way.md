## Understand Virtual Machine to better understand Containers and Dockers

Please watch day-3 concepts of virtual machine before jumping onto this lecture. Because before understanding concept of container or docker , its is required to fully understand the concept of Virtual Machines. Because virtual machines are advancement to Physical server. Similarly, Containers are advancement to VMs

Just an overview of Virtualization, if you bought Physical server or even if laptop, then most of the time you don't use entire resources of it. By resources we mean CPU, RAM etc . One server or one laptop is fine but when you aredealing with 1000s of servers across Organization , resource wastage is big concern. So there was an advancement in technology to fix this problem and there was an inception of Hypervisor

Hypervisor uses concept of vrtualization. Now what is Virtualization?
As name suggests, virtualization helps create Virtual machines on top of your physical servers. So virtual server is basiscally logical separation on top of your physical server. Physical server is something you can see and bought but virtual machine is logical separation where each has its own Operating Systema and on top of this operating system you would run your application. So instead of one physical server where you run one application, you have multiple virtual machines and each VM has its own Operating System on top of which it runs application. This vm provides logical isolation which is created by virtualization platform hypervisor

![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/24.png)

### This VM is very good feature then why do we need to move towards containerization ?


So lets understand. You have Physical server and on top of physical server you have hypervisor. Lets say Physical server is of 100 GB RAM and 100 CPU cores. You know that your team cannot use all 100 CPU cores and 100 GB RAM so lets say you create Hypevisor and create 4 Virtul machines on your physical server. AWS EC2 also has same concept, they buy physical servers and on top of that they install Xen Hypervisors and create EC2 instances. This is the same process on Cloud of installing Zen HV and creating VMs which in cloud its called EC2 instance. On these EC2 instance you install your own OS you want to use and this is great. So on physical server with 100 GB RAM and 100 CPU cores, you create 4 VMs with 25GB RAM and 25 CPU Cores.

But even after moving to virtual machines, you realize that most of the virtual machines or all of the virtual machines are not utilizing the resource sto its fullest capacity. So lets say you have APP1 on VM1 and on the best day APP1 receives maximum threshold load and even after that its using 10GB RAM and some 6 CPU cores. So its wasting some 15 GB RAM and 19 CPU cores. So this is the case when application is running on full capacity. On regular day when it runs even at lower capacity then 10GB RAM and some 6 CPU cores, it would be wasting more resources. So in regular use when load is less, you are wasting more than 50% of resources. Same things happens in EC2 instances. So consider an Organization which is using millions of EC2 instances and these instances are losing lot of resources. Since these are paid EC2 instances, this is heavy loss for the company if full utilization doesn't happen.


<u><b>To Solve this problem inception of Container took place</b></u>

So Containers will fix this problem, they will effectively stop the wastage of resources even in VMs.

So problem of Physical server was solved to some extent by VMs and problem of VMs is solved by Containers.

So lets try to understand architecture of Containers and see how this issue ges fixed.

Containers can be created in two ways

1) On top of VMs
2) Directly on top of physical servers as well

so lets discuss them


Option 2 explanation:

Consider an engineer who has physical IBM server in his organization or data center . He will install an operating system and on top of OS he installs "Docker" or any other containerization platform. So to create containers, you will need a containerizatoin platform. This is just like Hypervisor is needed for creating VMs. So you created docker platfom or containerization platform and on top of it you created multiple containers. So this is one way of creating containers.

Option 1 explanation:

The other way is you create Hypervisor or EC2 instance, on top of Physical server . So you have Physical server on top of which is Hypervisor and on top of which is VM or EC2 instance and on top it there is Docker or other containerization platform.


--> So Containerization platforms do not care if they are containering on top of Physical servers or on top of Virtual Machines. However, more and more Companies will prefer or use option 1 . Why?

Because , when you are maintaining your own Physical servers there is lot of maintenance overhead. There has to be a dedicated team who has to maintain this physical servers whenever there are new patches or whenever there are new fixes or whatever is the case , you have to maintain your datacenter. So you need system administrators.

Whereas if you are moving to AWS or any other cloud provider, or you are creating VMs on top of any Physical servers, your maintenance overhead is less unless you own that physical servers. Thus now a days you would be seeing more and more people moving to model 1 of deploying containers on top of VM to get rid of Physical server maintenance overhead

So in model 1, you wont see Physical server but will see directly VM or EC2 instance provided by AWS and on top of it you will deploy your Containerization platform like Docker and on top of it you will start creating containers.


Now lets understand Container . Docker Containers are very light weight in nature. By light weight , what it means is they don't they have complete Operating system.

So Docker containers on contrary to VMs, do not have complete Operating systems . A VM has complete operating system, and they have complete isolation from other VMs. <b><u>Where as what containers do, they take or utilize the resources from base operating system if installed directly on OS where as if installed on VM, they utilize resources from VM on which they are installed</u></b>

Fo example if your VM is running linux platform and your container needs linux libraries or any dependencies they will use it from VM linux librairies . So this is one of the major thing. So saying that , it doesn't mean that containers do not have operating system at all. <b><u> Containers do have minimal operating system or they will have base image</u></b>


So container is a package or a bundle which is a combination of your application + your application libraries i.e. your application dependencies + system dependencies. Your application might require some system dependencies. Lets say your application might require python , your application might require any system dependencies so what your docker container does is it will be a package of your application libraries and system dependencies. Any other system related packages or any other shared libraries like "etc" will be used from host operating system or if you have VM then from VM OS. So that is the reason why your docker containers are very light in nature.

So for example , you have virtual machine . To take backup of your virtual machine or to create image of your virtual machine, what you will do is you will create a snapshot. So usually these snapshots are 2 GB or 3 GB. Where as with containers, it depends on the type of containers, you might see snapshots falling into size of MB's like 100 MB and if your application container has more dependencies , it might go to 500 MB. But from this what can be concluded is there is significant drop in the size of image or backup. So because the size of the image or containers are light weight in nature, what happens is they are even easy to ship.

What does  shipping means is to move container from one place to another. Lets say you want to deploy this docker container onto container platform or you want to deploy this docker image on K8s , they are very easy to ship and very easy to transfer. So this is one more advantage



> <b><u>So what this implies or refer is Dockers are very light in nature because they only have base operating system and not complete operating system and they use resources from base operating system or VM on which they are running on .</u></b>


But your container, lets say you have a virtual machine and on top of this VM you want to run some 10 containers and one container requires python , second requies node js ,third requires java etc. So what you will do is in your docker image, you can use base image . This base image like I told you it can have all of the system dependencies. So this system dependencies, along with your applications plus your application libraries will form your docker container or your docker image. So never say that docker image or docker container does not have operating system , but they will not have a complete or full operating system . They will have a very minimalistic OS or they will have a very minimalistic system packages/system dependencies. So this is about your docker containers in general. <b><u>So whenever I say docker containers, always understand that I am talking about containers and the concept of containerization</u></b>. The reason why we speak docker container, so that many people will be able to corelate because always most of the times you are reading about containers it is like equivalent to reading about docker containers.


Why docker is very famous?
so docker is containerization plaform. Docker helped the community to write docker images like docker came up with improving this concept of containerization and it create lot of commands like user interface and user experience with docker using docker command became very simple. So to create a container what docker has done is it said that okay you don't have to worry a lot . All you have to do is you can simply write a docker file just like we write any other script files and you can submit this to docker platform or you can submit this to docker engine. This docker engine will convert this docker file into a container image or docker image. Now again using some docker command, you can convert this image into container. 


![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/26.png)


So if somebody asks about the lifecycle of docker or lifecycle of containers, what you will say is the first stage would be to write a docker file and to execute this docker file and to create a image and to execute this image and create a container. So <b><u>if you are using docker platform, there is something called as docker engine which will receive all of this input</u></b> . So you want to convert from docker file to docker image to docker container , you can submit your docker commands to this docker engine and you can create all of these things.

So for your reference when you want to convert your file from docker file to docker image,you will use command <b><u>docker build</u></b> and then to create container out of image you will use a command called <b><u>docker run</u></b> . So this way you can create container from docker image . This is also life cycle of docker


![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/27.png)


Now there is one problem with Docker. So docker is very much dependent on Docker Engine. And this docker engine is a single point of failure. So lets say our docker engine is down for some reason , then it means that all your docker containers will stop working. This is terrible way . Docker community and Organizations are working on this one but as we speak it still remains problem. So if your docker container is going down , all of your docker containers will remain down. To avoid this single point of failure, so whenever docker image gets created there are lot of layers which gets created. And each of this layer takes a lot of space or lot of storage on disk. So to solve this problem , instead of creating a lot of layers, to solve problem of single point of failure and other challenges , Builda said that okay let me create a tool and this toll will help you to solve this problem. So builda will aim to solve the layer problem, single point of failure problem , simplicity also works very well with scopio , podman . So you should also learn Buildah . In Buildah you don't have to write docker files , you can write shell script and in the shell script you can put all the builda commands and it would create an image for you and this image could be docker image, or any other OCI compliant image. So Builda also supports docker image, we can also create docker image using Builda













































































































































































































































































































