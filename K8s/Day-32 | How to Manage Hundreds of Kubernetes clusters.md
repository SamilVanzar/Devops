# Day 32 - K8s Production System

In this class we will learn how devops Engineer manage the lifecycle like creation , upgradation , configuration and deletion of K8s cluster in production

Why we need this ?
- Today we see most the teaching platforms shows K8s working in minikubes , microk8s etc which are development environment.These platforms are not used in production env
- However, the real time jobs which needs Devops Engineer , Devops admin would be to deploy and manage infrastructure and if infra uses K8s you are expected to know the lifecycle of K8s like creation , upgradation , configuration and deletion of K8s cluster. Hence we should what kind of K8s organization uses , what distribution is used and how organization manage them

- Lets try to understand why minikube, microk8s etc are local K8s clusters ?

This is because they are not full blown K8 clusters. You need k8s extablished in HA which is not supported by above dev env platforms. Also they do not support any issues you run into production if you use these platforms and they just say that they are not prod ready K8s.



> Lets understand how Devops engineers manage production K8s systems

In order to understand this , you would first need to understand what is K8s distribution and what are popular K8s distribution

( In interviwes ,people will usually ask which K8s distribution you used , did you manage them , installed them . They won't be interested in knowing about minikube or microk8s)

So what is distribution ?
Any open source platform lets say linux , people have developed distribution on top of it. For ex : Amazon linux distribution, Red Hat linux distribution , CentOS , Ubuntu . So people have taken open source linux platform and enhance it , create wrappers on top of it or create our own distribution on it.
Similary, K8s is open source container orchestration solution platform. So people have identified that lets create distribution on top of it.For ex : what Amazon has done is it has come up with its own managed K8s service called EKS. Similarly,Red hat has come up with its own distribution called Openshift. VMWare has something called Tanzu. There is something called Rancher labs which have created Rancher. All of these are distributions of K8s. So if you know architecture , concept, how K8s works then you almost know all the tools.Because they are not building anything new but they are building user experience on top of it or better customer experience like you say you create ticket with EKS , you know yous issue would be fixed asap by Amazon because you are paying for EKS. But on the other hand if you create EC2 instance and install K8s on top of it on your own then Amazon will help you if you face any issues with EC2 instance but not K8s because that is an add-on installed by you.If you want K8s support, you can buy our K8s service EKS.


Now if the interviewer ask which K8s distribution you have worked ?

You can say that you have worked on K8s . Though it doesn't have an official support or paid support but that doesn't mean people doesn't use K8s. Lets say your organization have 100s of teams with 10000 Engineers working and if you get EKS license for all 10K people, your org will run out of resources. So in the staging or pre-production env organizations where they want to test they use K8s. Also, there can be organization which can use K8s on production.Not every organizations have timelines like if you an issue is faced it needs to be fixed in 1 hour. So such organizations where there are no strict environment, can still continue to use K8s in production.

So K8s is top most distribution that people use in production. After that people use openshift distro .After that people use Rancher. Then there is something called VMWare Tanzu . Then comes EKS, AKS , GKE , Docker K8s Engine. Thus above all are K8s distribution which can be used in prod and not minikube , microk8s etc

Now what is the difference between installing K8s vs installing minikube ?
- When you install K8s it means you are installing K8s with all the capabilities for an enterprise. For ex: minukube when you install on local machine it would only need 2 CPU and 4GB RAM where as when you install K8s in production you would need a lot of capabilities since etcd itself will take a lot of memory , storage also if you install on AWS your ebs volume would consume a lot of storage also lot of other resources are used if you K8s in full capacity. 

Also one more question is what is difference between K8s and EKS ?

- If you deploy a couple of EC2 instances and deploy K8s on top of it and make a cluster then you can say that you are managing this K8s cluster.Amazon will not provide any support for K8s issues
- On the other hand, if you use EKS you get support from Amazon 
thats the only difference

You can consider EKS as distribution of K8s provided by Amazon which has some wrappers , some plugins ,some CLIs but at the end of the day it is same.


> Now lets move on the topic about K8s Engineers manage 100s of clusters ?

- One of the most widely used tools is called kops (also called K8s operations)
- Now K8s engineers have to deal with installation , upgrades , modifications , deletion so this entire lifecycle needs to be managed by K8s Engineer and KOPS helps in managing it. Hence, KOPS is the most widely used tool for managing K8s . Now KOPS is useful for managing K8s . However, for other distributions apart from K8s like Rnacher , openshift , EKS they have their own tools for managing lifecycle like for ex: ansible scripts are used for managing openshift. However, KOPS is used for managing K8s distributions

So in your interview you can say that you are using KOPS for managing K8s.Next is the demo for managing K8s lifecycle and needs to be done practically so there are no written notes for it






















































































































