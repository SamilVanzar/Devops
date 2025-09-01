# Day 31  KUBERNETES ARCHITECTURE USING EXAMPLES



The reason we discussed we use K8s instead of docker was due to below reasons

- cluster
- Auto scaling
- Auto Healing
- Enterprise grade

We will understand the architecture of K8s using all these 4 points

--> K8s has something called control plane and Data plane

Under Control plane there are multiple components as below

-API server
-etcd
-scheduler
-controller manager
-cloud controller manager

Similarly Data plane also has multiple components like

-Kubelet
-Kube Proxy
-Container run time


--> Simplest thing in docker is container and simplest thing in K8s is pod.We will compare both of these things and understand what happens when container is created in docker and what happens when pod is created in K8s. This will help understand each and every component of K8s and why these many components are required in k8s



--> Lets first understand creation of container in docker

- Consider a VM on which you have installed docker and using docker run created a container
- Now if you run the container , nothing would happen.Lets say you have installed java application container but on the platform you don't have java runtime.This will lead to java application not running.
Similarly, when you run container , you would need something called as container runtime. Without container runtime, container would never run.
Hence in docker , you have a container runtime component that is called as Docker shim. This is something which happens under the hood and not visible to users. Dockershim is responsible for running container in Docker platform.

Similarly in K8s , also need to do similar behavior to run pods but since K8s is advanced concept or because K8s provides you enterprise support with auto healing ,auto scaling and cluster like behavior. what you do with K8s is you create master and you create worker. For simplicity , we will use one master and one worker node component for understanding (though in production you will have multiple masters and multiple workers).
So in K8s, you will not directly send request to worker where pods needs to be deployed but your request goes through master which is control plane.
Now when pod gets deployed into worker node you have component in K8s called Kubelet. 

So what is this kubelet doing ?
Basically, this kubelet is responsible for running your pod.Just as in docker you have docker engine and dockershim. Similarly in K8s you have kubelet which is responsible for maintaining this K8s pod. It always looks for if pod is running or not. If the pod is not running, because k8 has feature called auto-healing, I have to inform k8s that pod is not running please do something. That is why K8s has component called kubelet but even if the pod has to run there needs to be something called container runtime. Even within pod , you have container and in order for container to run you need container run time. But the only difference is in K8s docker is not mandatory. In docker there is dockershim but in K8s you can either use dockershim or you can use containerd , you can use cri-o.These are all competition to dockershim . So docker only has one support called dockershim, where as K8s can support containerd , cri-o,dockershim or any other container runtime which implements K8s container interface. So K8s has a standard called container interface and if some container runtime ( cri-o , dockershim, containerd) if they can implement this container interface or standard K8s has set then K8s allows you to use that container run time.
So we learnt two components here - kubelet (for ensuring pod is always running , if pod goes down kubelet will inform another component which we will study later to restart pod or do something with it), container runtime which we discussed




--> In Docker, you have something called docker zero or default networking in docker called bridge networking. So this networking is mandatory to run yourpod even here in K8s and is called as kube-proxy. So this kube-proxy provides you with networking.So every pod you are creating , every container that you are creating it has to be allocated with IP address and also it needs to be provided with load balancing capabilities. As we discussed in auto-scaling , when you scale your pod instead of one replica if you have two replica's to your pod then there needs to be a component that says send 50% load to this pod and 50% load to other pod. So that component which takes care of this load balancing is kube-proxy.So we learnt about three components:

1) Kubeproxy which provides networking, IP addresses and also the default load balancing capabilities in K8s
2) kubelet:  which is responsible for running your application and if your application is not running or your pod is not running then it informs one of the component in control plane that something is wrong
3) Container run-time: which actually runs your container

![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/3.png)

All the above three components complete data plane / worker architecture of K8s. These are the components of worker node

1) Container runtime -Responsible for running the container
2) kube-proxy - uses ip tables for doing networking
3) kubectl - To make sure container is always up


## Worker Plane / Master Component

This worker node or data plane is responsible for running your application. So the three components we discussed in worker is enough for running your application.

So then the question comes , why do you need control plane or master component ?

--> K8s is enterprise grade solution. It runs in cluster. Now user has created a pod but who will decide where this pod needs to run i.e. pod needs to run on node -1 or node -2 .... or on which node ?
So this is one specific instruction but there can be several such instructions which needs to be decided. So there should be a heart or core component in k8s which has to deal with such kind of instructions when multiple users are trying to access your cluster there needs to be component in K8s which will access all the core components of K8s and takes all the incoming request like SSO configuration or some security related stuff etc. So this core component in K8s which does everything in k8s and that core component is called API server and this component is present in your master component or worker component of K8s


So if someone asks what is the purpose of API Server ?
So an API server is the component which basically exposes your K8s. So this K8s needs to be exposed to the external world. So all the components of data plane is internal to K8s but the heart of the K8s is K8s API server which basically takes all the requests from external world.

Lets say user is trying to create pod, he accesses the API server and from API server ,K8s API server decides that okay node 1 is free but to schedule a component in node your have component in K8s called <b><u>scheduler</b ></b></u>

So what is the responsibility of scheduler ?

--> So scheduler is basically responsible for scheduling your pods or scheduling your resources on k8s.

So who decides the information ? --> API server
Who acts on the information ? --> Scheduler

So we have learnt two things : 1) API server  2) Scheduler

So API server gets instructions to deploy pod and decides on which node to deploy node, once decided lets say node 1 then scheduler takes that instruction from API Server and deploys pod on node1

Now lets say we are deploying production level application on K8s cluster there has to a component inside K8s that access backing service.Something that needs to access backing store of entire cluster information. So in K8s there is a component called <b><u>etcd</u></b>. So etcd is key-value store and the entire K8s information is stored as object or key value pairs inside this etcd. So along with API server and Scheduler we learnt about etcd. Without etcd , you dont have backup of cluster or any cluster related information so if tomorrow if you need to restore cluster , you would backup stored in etcd

And finally you have two more components which are
1) Controller Manager
2) Cloud Controller Manager

Lets put this Cloud Controller Manager aside for a moment and understand controller manager

Now we know K8s support Auto-Scaling. Now to support auto-scaling, K8s needs component which can automatically detect and do some kind of things.So for that K8s has something called controllers.

For example : Replica Set. So replica set is something responsible for managing the states of the pod. So tomorrow lets say one pod is not enough to serve all the requests so I will auto scale the pod to two pods.So there needs to be component in K8s which ensures that both the pods are running, so that is taken care by replica set. In yaml file,you define you need two pods, then replica set controller makes sure that two pods are always running.

Now there has to be component in K8s which ensures that replica set controllers are always running so that component is called as <b><u>Controller Manager</u></b> .So in K8s there are multiple controllers like replica set controller and there has to be a component which ensures that these controllers are running and this component is called Controller Manager


Finally going to the last component called Cloud Controller Manager ( CCM)

- You know that K8s can be run on Cloud platforms like EKS, or AKS or GKE. So lets say are running your K8s on cloud platform like  EKS . So there can be a request to create load balancer , or there is request to create storage . So if you send this request instead to K8s to create load balancer or create storage, K8s needs to be first understand the underlying Cloud provider on top of which K8s is running to understand how to create storage an load balancer on EKS. So K8s has to translate the request from user to API request that your cloud provider understand which in this case is what EKS understands.This mechanism needs to be implemented on your cloud control manager. So what this means is tomorrow there is new cloud service which is launched called "Samil" and you want to deploy K8s on this cloud provider called "Samil" . So what K8s says is that I cannot write logic for all these cloud providers , so I provide you with component called Cloud Control Manager. This CCM is open source utility. This code is available on github. Tomorrow if Samil creates new cloud provider , what Samil can do is he can go this CCM open github repository and write the logic of his Cloud Provider Samil into this CCM . Hence CCM component only exists for Cloud provider and does not exist for on-prem deployments.

So the recap of this chapter is K8s is divided into two main parts:

1) Control Plane and 2) Data Plane


There are 3 K8s Data Plane components which are

- Kubelet
- Kube-Proxy
- Container run-time

Every K8s worker node will have above 3 components

Next you have is Control Plane Components which are

1) API server
2) Scheduler
3) etcd
4) Controller Manager
5) Cloud Controller Manager


So Control Plane is the one which is managing the action and Data Plane is the one which is executing action

![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/4.png)












































































