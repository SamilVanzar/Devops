In this class we will learn about K8 services. So services are very critical component of K8s. In production, as we discussed we do not deploy a pod, but we usually deploy a deployment as we learned in our last class.

Similarly for each deployment, most of the time you will create a service in the world of K8s. So why do we create a service,what is the importance of this service we will discuss in this class.

Lets understand why do we need service in K8s and what happens if we don't have service in K8s.

So lets discuss about the scenarios of "No Service". So whatever will discuss initially is by considering the fact that there is no service in K8s. So lets see what  will happen

What happens is usually as we discussed in our last class, a K8s Engineer will deploy his pod as deployment in K8s. Deployment would create a replica set and in turn replica set would create a pod.If the replica count is 1 , replica set will create one pod. If the replica's are multiple , then it would create multiple replicas of pod. Lets say we have the requirement of creating 3 replicas. So why do we need multiple replicas of pod. Lets say there is one user who would be accessing application , then you won't need it. But if there are 10 concurrent users simultaneously accessing application so similarly there are 1000's of users who are trying to access application simultaneously like for example: whatsapp at some point of time. So if all the users request would go to same pod 1 , then pod would go down . Hence you create multiple replica's of pod. So the number of replicas of pod depends on the number of users trying to access your application simultaneously. Also, it depends on  the total number of connections that one single pod can take.

So if someone asks you what is the ideal pod size or ideal pod count ? 
What you can say is , it depends on the number of concurrent users and the number of users or the number of requests one replica of your application / pod can handle. So if one replica of your pod/application can handle 10 requests at one time and if you have 100 requests that are coming in , then you have to create 10 pods. So you have to take this decision as devops Engineer as developers you need to sit together and take this decision


Now lets assume that there are 3 pods and out of these 3 pods, one of the pod went down. Containers are ephemeral but the advantage of K8s is that it offers auto-healing feature configured in deployment yaml manifest ( through replica set controller). So replica set will create one replica of pod as soon as pod goes down. However. the problem with this is that as soon as new pod comes up , the IP address is changed.

Lets say that previously the three pods were assigned IP addresses as below

Pod 1 --> 172.16.3.4
Pod 2 --> 172.16.3.5
Pod 3 --> 172.16.3.6

Lets say pod 1 went down and when replica set brings up the replica of pod 1, the IP address of replica of pod 1 is 172.16.3.8 instead of previous IP address which was allocated to pod which was 172.16.3.4. So replica set did bring up the application but the IP address of the application is changed.Since we are currently understanding the scenario where there is no "service" enabled, what will happen is you would need to share this IP address to application teams who access this application using IP address


So as devops engineer what you did was you created a deployment which created replica set which created 3 pods. Now lets say there are three users who are trying to access this application using IP addresses and everything is working fine. Suddenly one of the pod went down and replica set created new pod and that got updated IP address. So the first person who was accessing the pod using IP address 172.16.3.4 would not be able to access the application any longer after replica set creates new pod replica. As devops engineer you will argue no it should work since replica set created new pod replica. However, it should be kept in mind by devops engineer that once replica set creates pod replica , IP address changes and the application user should now access pod replica using updated IP address.

Now if you consider real world it will never work like this,  lets say google.com . google.com will never tell you that try to access my application using IP address 100.64.2.7 and tells other user to access google.com using IP address 100.64.2.8 and like wise google has 100 such replicas, google will never give all 100 users IP addresses to access the application.It never works like this . So the concept is , google use load balancing

So what this concept of load balancing helps is you do not tell users to access application using IP addresses. what you tell users is I will create deployment for application , and on top of this deployment , I will create a service which is also called as "svc". So I will create something called as services and what you ( application user) can do is instead of accessing this specific things of deployment, directly try to access service


![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/7.png)

So now what happens is that user 1 now instead of accessing application using IP address 172.16.3.4 , you changed the behavior by deploying service on top of deployment.

So on top deployment which created replica set which creates pod, now on top of deployment there is creation of service. And what this service does is , it acts as load balancer. It acts as load balancer in K8s , using a component called "kube-proxy". Though we will not discuss kube-proxy here and only discuss service for now. So what end users are asked is instead of accessing application using IP address, access me through service. So users will access application using service. Lets say there is "payment" related service, then instead of accessing the payment servers using IP addresses , users access the application using payment service something like "payment.default.svc". Lets say that this is the name that K8s provided to access services .what K8s does is that as soon as you create service, it creates name of the service "payment" under namespace "default" and .svc for service. You can tell users that you can access my application on something like "payment.default.svc" instead of IP address. So all users would access the application using same "payment.default.svc" . In the backend what happens is this load balancer or service kube-proxy will do is it will forward the requests coming from users to individual pods. So 10 requests from user 1 will be forwarded to pod 1 (172.16.3.4), 10 requests from user 2 will be forwarded to pod 2 (172.16.3.5) and 10 requests from user 3 will be forwarded to pod 3 (172.16.3.6)

![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/8.png)

Now the question is services are just taking the request and simply forwarding to the pod. It will still face same issue as discussed earlier that when pod goes down and replica set creates new pod, the IP address of new pod changes from 172.16.3.4 to 172.16.3.8 and that will impact the end user 1 issue to access the pod using old IP address of 172.16.3.4

Now this is second problem that service solve called "Service Discovery".So what service says is if I keep track of deployment which is creating three pods and if one of the IP address changes and service also repeats the same problem of users unable to reach pod due to IP change then no problem is solved by service. So what service does instead is , I will not bother about IP address at all. I will come up with new process called "Labels and Selectors".So how does service does service discovery is unlike keeping track of IP addresses which changes any number of times and lets say instead of two or three pods, there are 1000s of pods whose IP address tracking is difficult, what service does is I will introduce new mechanism called <u><b>Labels and selectors</b></u> and what it does is for every pod that is getting created, devops or developers does is they create/apply a label . So this label will common for all the pods. Lets say for example, we have application related to payment, we create labels named "payment" .
> So what service does is , I will not care about IP address . I will only watch label called payment

So even if pod goes down 100s of times and have changes to IP addresses 100 times , I don't care since I don't care or look for IP addresses . I just look for label "payment". So even IP address changes when pods goes down and come up , label always remains same. Now why label remains same ??

It is because replica set controller what it will do is it deploy the  pod replica  using yaml it got which has label defined in the yaml metadata in deployment like 
label
app : payment

![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/9.png)


So the final mechanism would be you create a deployment using yaml manifest. So what devops engineer will do is whenever you create this deployment, you provide all the specifications and the names . But along with that inside your metadata of deployment, you create a label. Label is just a tag like

label
app : payment

So what happens is deployment will create replica set lets say for 2 pods and what this replica set will do is it will create pod 1 and pod 2. 
Once these pods are created and you enter the command like

kubectl get pods , what you will see is both the pods have labels something like below

label:
app : payment

so lets say that application pod 1 goes down , the IP address of pod 1 will change but what replica set will say , I have yaml manifest and according to yaml manifest , the pod has to be created with specific label. So even if pod 1 goes down 100 times , all 100 times IP address will change but all time label will remain same as "payment". So what service does is along with providing load balancing, service will keep track of label ,so whenever a new pod is created , it will just track the label of pod and makes application always accessible using label. This is called service discovery

![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/10.png)


So we learnt that service provides 1) Load Balancing 2) Service discovery.

Now the third important thing we will learn which service offers is 
<u><b>Expose to External World</b></u>

So when you create deployment --> it created replica set --> which created pod and each pod has its IP address let say for ex : 172.16.3.4. Now to access this pod , you would need to ssh into minikube or other platform you use and ssh into master node . So what is actually happening is whoever has access of this K8s cluster using minikube or microk8s etc,and can SSH into it and access the application. However , this is not real world scenario.So google cannot ask it customers to ssh into the machine  --> then login into K8s and access my application using this IP address. Anywhere in the world you just access the application on browser using https.

So this is something K8s cannot directly offer you by Deployments. So deployment helps create pods and users can login to K8s and access pods/application using IP addresses .But for users sitting in Italy or Australia cannot do such things . So what services do additionally is a service can expose your application .By exposing it means that service can allow your service to be accessed from outside your K8s cluster. So how service would do is basically, you are provided with 3 options.

Whenever , you are creating K8s manifest in yaml , you are provided with 3 options.You can create service in 3 types

Type 1 : Cluster IP
Type 2 : node Port
Type 3 : Load Balancer


There are other types as well but above 3 are default types

So what happens is when you create a service using Cluster IP , like any service, this will be <b><u>default behavior</u></b> meaning you can access your application inside K8s cluster. So this will only give two benefits of service we discussed which is 1) Load Balancing 2) Service Discovery


But if you create a service of type "Node Port" then what service will do is it will allow access to your service from inside your organization. Anybody inside your organization or anybody within your network like technically they don't have access to your K8s cluster but they have access to your node IP address like worker node or master node. So who ever has access to your node IP addresses , can access your service using "node port"

And finally "Load Balancer" type. So load balancer type is basically your service will expose your application to external world. So lets say you have deployed everything on your EKS Cluster, so if you are creating a service of load balancer type then you will get an elastic load balancer IP address for your specific service and whoever is trying to access can use this ELB from anywhere in the world can access your application using ELB which is Public IP address.

So as we discussed previous service name "payments.default.svc" using ELB or elastic load balancer and it is on any cloud provider then depending on the cloud provider implementation this load balancer will only work on cloud providers .So if you are trying to it on minikube or microk8s <u>by default</u> it will not work. However, there is a way to get it work on minikube as well which we will see later using Ingresses.


so if you create load balancer service type then it means that on your cloud provider their will be a ELB IP address which will be created which is the Public IP address using which you can access your application.If you created service using node port then whoever has access to your AWS or have access to node inside AWS can access the application .And if you deployed service using "cluster IP" nobody can access your application except the ones who has access to your K8s cluster


--> So if we want to understand it using one single diagram the service exposure feature , we can think as below

![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/11.png)

--> You create deployment --> Replica Set --> Pods and all of them are inside a node. Lets say worker node 1. Just for easy understanding. Now there is a customer. Now on top of deployment  you create service. Now lets understand user flow depending on the type of service you create.

Lets say you create a service like "cluster ip" service. Now what this service will do is if you create service using "cluster ip" mode , then it won't be accessible by Customer sitting outside your organization and is only designed for access by users who has access to K8s cluster or whoever has access to the internal network of K8s cluster. 

Now lets say you have created service like "load balancer" type. What will happen is K8s API server will notify AWS server that - Hey EKS I have a service of type "load balancer mode" so if you can provide me Elastic Load Balancer IP address ( which is Public IP address ) . And which component of K8s is doing this ?
There is a component in K8s that we learnt called Cloud Control Manager. This is part of your K8s master node. What is this component? So the K8s cloud Control Manager will generate a Public IP address using AWS implementation and will return a Public IP address. Now what service will do is whoever wants to access this pod, can access using the generated Public IP address. So external user from Internet can access application using generated Public IP address

Finally , you have service type configured as "node port". So when you create service of type "node port" then user who is on the internet would not be able to access the application, but what service would be accessible to anyone who has access to worker node 1. So whoever can access worker node 1 IP address , can access the service. If service is in AWS then whoever has access to EC2 instances can access service.

So if service is configured as "cluster IP" , even though you have access to VPC you would not be able to access the service. Only people who has access to K8s cluster or K8s network or the access of the container inside K8s cluster or containers you have configured only they can access.

So in conclusion below are the 3 advantages that K8s service offers you

1) Load Balancing
2) Service discovery
3) Application exposure to world.










































































































