# Day 30 Introduction to K8s

Docker is container platform & K8 is container orchestration platform - Textbook definition

### <u>Practical Understanding of K8s</u> ###

One of the things we know about containers is that they are ephemeral in nature meaning they are short lived. In easier terms it means containers can die and revive anytime

![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/1.png)


As shown in diagram , there is VM platform on top of which we have docker applciation running which is creating 100s of container.
Now since container uses resources from host operating system and assume that first container is consuming a lot of resources which is causing container 99 to run out of resources and die. Or maybe that container 1 is taking so much resources that it doesn't let container 99 to be scheduled.Hence if container doesn't have enough resources of if container image is not pulled from registry , container immediately dies which is why they are short lived

Now how exactly we ran into this problem or why this is problem ?

Because there is only one host as we can see in image . In that host kernel , each process has priority. There is priority assigned to each process and since container is already running there is process ID allocated to it. Now since there is lot of memory allocated to this process and there is no more memory left to create new process , container 100 doesn't get created or immediately dies.Because there is only one host on top of which docker is running and that host itself is running out of resources which is leading to our inability of creating new container/ container dying. Hence we have learnt one problem here which is **<u>single host</u>**

>  #### <u>Now we know one of the problem of docker is that container is present/limited on single host which is impacting other containers</u>




**Next problem i.e problem number 2**

As discussed above if the container goes out of resources, it will die. This will cause container to be inaccessible. Also, once the resources is available , container won't come back up automatically. Container needs to be manually started or human intervention is required. This problem is called auto-healing. Also there are several hundred reasons that container can go down. Consider, there is a system using 10K containers , it is not feasible that a user can monitor these many containers manually.

![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/2.png)

You just can't keep running "docker ps" command and see which containers are in running state. Hence there needs to be something called auto-healing which docker in itself doesn't have it. Hence , the second problem we know is **<u>auto-healing</u>**



**Next problem i.e. problem number 3**

<u>**Auto-Scaling**</u>


Consider a running container which is utilizing 4 CPU and 4GB RAM. This container  can serve 10000 requests of external users. For ex : container belonging to mobile shop service customers web requests. Now lets say its black friday and suddenly customers request increased from previous 10K to 100K . Now since resources are limited or for easy to understand consider 4 CPU and 4 GB RAM are the maximum resources which can used by one container. So the solution would be to manually bring up 10 such containers which can satisfy customer requirements of 100K web requests.Also think about how to balance this traffic among 10 servers. You cannot have one server handle 10K requests and tell you to go to next server IP where another 10K requests would be served and so on.You would need a load balancer to balance traffic automatically among these 10 new containers. So you just visit xyz.com and it points to load balancer which doesn't care of containers are brought up manually or automatically. Load balancer will see that I have 10 containers so let me equally split the load among these 10 containers.
So there has to be a mechanism like load balancer which can split the load  automatically. This mechanism is missing in docker and that is a big problem


**Next problem i.e. problem number 4**

Docker is very minimalistic and simple platform . By default, docker doesn't support any of the enterprise level application. For an enterprise grade solution you need a lot of features like below

- Load Balancers
- Firewall
- Auto scale 
- Auto heal
- Support API gateways

etc

However , docker doesn't support any of the above enterprise grade standards.Hence it is very simple of minimalistic platform which can be used for testing or college project.


 Lets write all the four problems and see how K8s solve each of them
 
 1) Single host based
 2) Auto Scaling
 3) Auto Scaling
 4) Enterprise level support
 
 There are 100s of such problems but lets start by how K8s fix above problems and then as we continue our journey , we will see other problems getting fixed by K8s
 
 
1) Single host based problem

- By default K8s is cluster . Cluster meaning it is a group of nodes. Previously docker was installed on a single host/machine. However, K8s is installed as master/node architecture

[ Master/Node Architecture: This architecture is created using clusters, meaning we install K8s we create one master node and multiple nodes. ( though you can install k8s on single node but that would be your staging/development env and not enterprise production). So in production env regardless of you deploy K8s in standalone or High avalilability , K8s are always deployed in cluster ]

Now the advantage of deploying K8s in cluster helps to fix our single host based problem identified in docker.
Lets go back to the same example we discussed while understanding single host problem of single host where container 99 was dying due to container 1 using lot of resources.
Now in K8s since there are multiple nodes, if due to container 1 , container 99 is dying , K8s will immediately put container 99 in second worker node.This will allow container 99 to run smoothly. Similarly, if there is any faulty node which would impact running pods , K8 will move all the pods from faulty node to other normal worker node.Thus K8 cluster like architecture helps solve single node problem


2) Auto Scaling problem


Now K8s have something called as replication sets. With the presence of replica set. So lets say you have container C1 running which can serve 10K requests and due to some festival there are now 100K requests. What you can do in K8s there are yaml files so you can go to replication set.yaml file or deployement.yaml file and set/increase the replicas from 1 to 10. [yaml is just normal json file / indentation file]. Since we know that tomorrow traffic going to increase we increase the replicas. This is manual way. Now K8s also support HPA which is Hortizontal Pod Auto_scaler using which you can just set when load increases automatically increase pods. You can set limit that whenever load on any pod reaches threshold of 80% , just spin up one more container. In such cases, it will automatically keep on spinning new containers.This helps to achieve auto-scaling


3) Auto healing

Proposed outcome is whenever there is damage,K8s should automatically heal the damage. So K8 should control and fix the damage. So k8 will either control or fix the damage ( though most of the time it will control the damage). Now lets consider that a container is going down for any reason, K8s has a feature called Auto Healing where even before container goes down, K8s will start a new container. In K8s there is something called API Server.Whenever this API server receives a signal that container is going down, immediately it will roll out a new container before the previous container actually goes down. So end user doesn't know that container has gone down and new container has come up. They will enjoy using application seamlessly.This helps us to achieve new feature called auto healing


4) Enterprise Grade / Enterprise Nature

 As we discussed by default , docker doesn't support firewalls, load balancer etc which caused it not be used by enterprise.However, K8s is originated from Google tool Borg. Hence, K8s is part of Borg. So people at google developed k8s as enterprise level orchestration platform to overcome the shortfall of docker which was just a container platform. Also they made K8 open source with all the enterprise ready tools inbuilt that an organization can use like whitelist , blacklist , firewalls, load balancers, API gateways etc































