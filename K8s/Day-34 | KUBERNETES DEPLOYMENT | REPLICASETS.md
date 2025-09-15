
-->We had a pending question in Day 33 that if K8s can do things with pod like if you can deploy you application as pod then why do you require deployment ?

--> So what we are going to look at in this chapter is 

**<u>> Difference between deployment and container and pod</u>**

<u>Container</u>:
As we studied in Day 23 to 30 , Lets say you created a container using docker. To run this container, what we usually do is run below command to run container:

docker run -it <followed by name of image> -p -v --network
    
So using above command you can run container on docker platform.
    
+++++++++++++++++++++++++++++++++++++++++++++++++

<u>Pod</u>

--> What K8s says is let me modify the docker command line process and let me bring enterprise model to this. So what K8s says is instead of writing all of these lines in command line , you can create a yaml manifest and inside this yaml manifest you can define all the things you mentioned in docker command . So in yaml file you define what all thngs you need like image, port you want to run this container on, volume , networks in a yaml manifest. So a yaml manifest is nothing but a running specification of your docker container. The only difference is pod can be a single or multiple containers. You can create multiple containers in pod is because lets say you have an application which is dependent on other application without which it cannot run like service mesh or sidecart container. Advantage is if you use a pod which have multiple containers , both of them can share same networking.Both of them can communicate using local host and both can have same storage and volume.
    
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


<u>Deployment</u>:
    
K8s something that allows uses to move from container platforms like docker to K8s . These 2 things are 
    
  1) Auto Healing
  2) Auto scaling

--> Pod does not have this capability of implementing auto healing and auto scaling. So Pod is bit similar to container where it provides yaml specification for running a container. Also pod offers multiple container deployment where shared networks , volumes can be provided.However, pod cannot do auto-healing or auto-scaling
    
These features in K8s are offered by Deployments. So if you want to deploy your applications with zero downtime or with auto healing and auto scaling , then you should never deploy your applications as pod. Instead you should use deployment and what deployment will do is it will deploy pod only but instead of deploying a pod , it will do it will create replica set first and replica set will create pod.
    

--> So what K8s suggests is do not create pod directly. At the end of the pod needs to be created but instead you should create pod using deployment resource. And what this deployment resource do is firstly it will create Replica set. Replica Set is K8s controller. And then replica set will roll out pods.
    
![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/6.png)

--> Now question is why do you need this intermediate resource replica sets?

What deployment does is , inside a deployment you can say the number of replicas of pods you require.

Why is required ?
In some cases, you do not need single replica of a container . Some times load on the container is too high.You might want to expose your application to multiple concurrent users who want to access your application. You can say 100 users should go to pod 1, 100 users should go to replica of pod 1. You can call this HA or Load balancing.
    
    
So what you can do is inside your deployment  yaml manifest , ( deployment is again a yaml manifest) because in K8s everything is yaml manifest . So inside your yaml manifest, you can say replica count as 2.
    
But when you say replica count set as 2, there has to be something in K8s that ensures that okay you said I want 2 replica's. So deployment will create a pod but if we go back to the topic of auto-healing and auto-scaling . What is auto-healing mean ?

You say you want 2 replicas, deployment will say replicas as 2 pods,but what replica set additionally does is because it is K8s controller what it does is it ensures that there are always 2 pods. Even lets say one of the user deletes on the pod, ( lets say accidently user deletes one pod) k8s will say don't worry because you have submitted me a deployment manifest to me , I implement auto-healing using replica set controller. So it will always ensure that there 2 replicas on this controller.( We will also verify this in demo)
    
So end process is you will create a deployment, and this deployment will roll out a replica set and replica set will create number of pods mentioned in deployment yaml manifest. Replica set will ensure what user has provided in yaml manifest, it will ensure to implement the auto-healing capacity. If you say that replica count as 2 or 100 this replica set will always ensure that there are 100 replicas of pod on your cluster . If user deletes one pod and makes it 99, what replica set will do is because deployment told me that pod count has to 100 , let me creat pod so pod count is 100. So this is how zero downtime is received. Tomorrow if you think load is more which 100 pods are unable to manage, you can go to this replica set and update value from 100 to 150. So replica set will realize that there is new update of pod count from 100 to 150, let me create 50 new replica of pods. So this is how deployment works. Deployment create replica sets and this replica set will create pods for you. And this <b>replica set is K8s controller</b>.
    

    
Just like replica set is controller, k8s has lot of controllers and after listening to all lectures you will more acquainted to it. In K8s we deal with lot of controllers. So <b>Controllers are something that maintains the state,it always ensure that the desired state is always present on the actual cluster that is the desired state and actual state on cluster are same. So anything which does this behavior in K8s is controller</b>. So there are some default controllers in K8s and you can also create some custom controllers in K8s like argo cd, admission controllers etc. So if you listen controller , always remember that the state in yaml manifest is maintained, if state in yaml manifest if you define something has to be there , controller will always ensure that in K8s it is always there and is manitained by K8s controller.
    
    
So the popluar interview question from this lecture would be 
    
1) difference between pod vs container vs deployment.
2) Difference between Deployment and replica set .
    
    So replica set is basically a K8s controller, that is the one implementing the auto healing feature of your pods. If the pod is getting killed, or deployment set is telling increase the pod by 1 then replica set is the one implementing this.
    So replica set is the one  implementing auto-healing capability by saying that the actual state in the deployment manifest or the actual state in the deployment should be on the cluster. <b><u>So the desired state in yaml file should always be the actual state of the cluster</b></u>. When deployment  is created, this replica set is created automatically which is responsible for tracking this controller behavior in K8s.
    

    We will now see the implementation of this concept.
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    

    
    
    
    
    
    
    
    
    
    
    
    
    
    
    