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















































