> What is POD

--> We are moving from Docker from K8s . In K8s smallest unit of deployment is pod. In docker , you build a container and your deploy a container.In k8s also we will use these containers that is deployed in docker because end of the day if its K8s or docker, end goal is to deploy applications in containers. That is the concept of containerization but what K8s says is don't deploy your application/container as it is but deploy it as pod.
But the question is why in K8s we need to deploy container as pod and why can't we deploy it directly as container ?
So by definition pod is described as "how to run a container "

So in docker if you wanted to run a container, you would pass the command something like this as we saw in docker chapter

```
docker run -d <followed by name of image> -p ( for port mapping) -v (volume mount) --network (pass network details)
```

So in docker you pass all the above arguments in command line to run the container.

Where as in K8s , you will pass all these specifications in <b><u>pod.yaml file</u></b>

So in K8s you basically have a wrapper or a concept , that is similar to container but it abstracts the user defined commands in pod.yaml file

So in K8s instead of container , you deploy a pod. A pod can be a single container or multiple containers . ( We will see later why a pod can be multiple containers and what are its advantages but for now lets understand for single container)

--> Assume you are building a pod with one single container, what you will do is similar to docker, pod is exactly like a docker container and the only difference is in pod unlike docker you do not define command in command line like "docker run ........" instead you pass all of them in YAML file

Inside yaml file ,you will say something like 

api version v1
name of the pod
specification
( under specifications you define all details of container)


![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/5.png)

So in K8s you just need to put all the details in yaml file and thats the only difference as compared to running container in docker using command line commands



--> Now why K8s has to deal with such complexity ?

If everything was going fine in docker by running containers , why to increase this complexity of adding yaml files ?

The reality is K8s is enterprise level platform and what it wanted to bring is declarative capabilities or build a standardization

The thing is you put everything in yaml files.You put pod resources , development resources , services they are all written in yaml files only. So you need to master yaml files , that doesn't mean you have to mug up yaml files . Everyone follows the examples in official K8s site and everyone Devops Engineer follows that example. But you need to have an understanding of how yaml files are written only then you become expert in K8s.

Now pod is nothing but one or group of containers. Now why is it group of containers ?

When do you have multiple containers in pod ? 

Consider a scenario where you have an application running inside a container. Now that application needs read some configuration files or user related files from other containers. In such case what most people do, is instead of creating two separate pods , you can have both the containers in one single pod. What pod says is if you put one or 2 or multiple containers inside a pod , what K8s says is I will ensure that both the containers will have some advantages. What are these advantages ??

Group of containers in a single pod , lets say we have container A and container B in one single pod then K8s will allow you shared networking , shared storage. So in this way container A and container B inside a single pod can talk to each other using localhost . So if one container wants to access another container on port 3000 , it can just type localhost:3000.  Also if both of them wants to share some files, then also since both of them are in one single pod they can share the files as well
. So this is one of the reason that people put multiple containers in one pod. But this is still very rare case , the usual case is to create some side car containers or init containers which is advance topics covered in upcoming lessons.

So now we understand that in K8s there is pod and inside pod there is a container. So what K8s does it , it allocates a cluster IP address to the pod and you can access the application running as container inside pod using this pod cluster IP address. So IP addresses are not generated for containers but they are generated for pods.


--> So in simple terms, pod is basically a wrapper that K8s has created for container to make life of Devops Engineers easy.Because when we deal with 100's or 1000's of containers in production, if you have a wrapper like pod which can define everything in a YAML file , a developer can go to github repository and look for pod.From YAML file he will understand everything about the containers like this container is running the application is running on port 80 , it has a volume mount then what is the networking of application and multiple details that you have for containers. So K8s has created a wrapper for it. So most of the cases when you are dealing with the pod, you would deal with single container and you access the container using pod cluster IP address that K8s gave to the pod. 

So who is  giving the IP address to the pod ?
its the Kube Proxy.

This completes the understanding of topic of pod. Now we will move towards deployment of pod.

Before deployment of pod, we need to understand one concept called "kubectl". So kubectl is nothing but docker CLI in K8s. kubectl is command line for K8s which helps you to directly interact with K8s cluster.For ex: You have K8s and inside which you have 10 nodes . So understand how many nodes are there in K8s you can simply use the kubectl command:

kubectl get nodes


Similarly to get how many pods are running you use : kubectl get pods , 
Also to get how many deployments are running you use : 
kubectl get deployments


Similarly if you want to delete a deployment or create a deployment , so for such cases to interact with K8s you have kubectl.

In remaining lecture , we will learn how to install kubectl , create K8 cluster in minukube ( since its free ) and then learn how to deploy a pod.


Important concept that comes after deployment of pod is how to add auto-healing and auto-scaling capabilities to the pod
--> In k8s you have wrapper on top of pod called deployment. So you have to use deployment to use features like auto scaling , auto healing in K8s.
Deployment is just a wrapper on top of pod which is used as way to deploy the application in K8s in real time production scenarios. So you won't be deploying pods in real time prod env but you will be deploying your deployments, daemonsets ,stateful sets and such things which we will learn later in this chapter. But to understand them you need your foundation strong so you need to understand pod first.

Also just to understand that if you ever run into issue with pod , you can debug pod using command:

kubectl describe pod ( to get all information of pod about its config)

kubectl logs pod ( to get all the logs related to pod for tshooting)

--> Also there is kubectl cheatsheet incase you want to know different commands you can execute using kubectl

--> The next thing we want to understand after deploying pod is how to achieve auto healing and auto scaling with pod, In order to get these features in our pod application , you can use the wrapper on top of pod called deployment . So deployment is not much different then pod but just a wrapper. In pod, you created a yaml file and defined kind as pod. While creating deployment, you mention type as deployment. Also we add few more things like template and we say it as pod deployment specification. But in other way deployment is just a wrapper on your pod which is a way to deploy your applications in K8s in real time . So in production env you will not deploy pods, but you will deploy your deployments, stateful sets or daemon sets

--> Also if you want to debug your application , you can use below command

kubectl logs (name of the pod)

This command will help you to see name of the application. Lets say you want to see logs for nginx pod we created , we use command

kubectl logs nginx

--> Also if you want to get all the information about the pod, you can use command

kubectl describe pod

For example of nginx pod we deployed , we can use

kubectl describe pod nginx

















































