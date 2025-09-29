## What is Ingress

--> In previous class we studied K8 services which offered Service Discovery, Load Balancing , Exposure of application to internet

> Now the question is why you need ingress or what problem is it solving ?

Before 2015 Dec, before K8s version 1.1 , ingress was not available. So people were using K8s without ingress. Meaning people were using K8s with service concept. So people used to create Deployment, which created pod. Also because you use Deployment , you get features like auto-healing and auto-scaling. On top of pod, you create service so you can expose your application within K8s cluster or outside K8s cluster using load balancer mode of service.

### But there are some practical problems that people realize after using K8s.


#### <u>Problem 1</u>

-->So when people migrated to K8s , they migrated from legacy systems like people use to have virtual machines or physical servers on top of that they hosted their applications. And then people, used to use load balancer. These load balancers were like nginx, F5 load balancers etc on their Virtual machines or physical servers. So these are all enterprise load balancers. Enterprise load balancers means they offer really good load balancing services like ratio-based load balancing in which you can set ratio to send 3 requests to Pod 1 and 7 requests to Pod 2 (there are no pods inside virtual machines but this example was used solely for understanding) . You can use sticky session which means if requests from user 1 goes to server 1,all the requests from user 1 needs to go to server 1 only. There is one more load balancing type called path based load balancing , domain based load balancing , whitelising which means only specific Public IP addresses are allowed to access server , blacklisting which means specific IP addresses or specific Country based traffic is not allowed. So all these load balancing is something enterprise load balancers support. Now the problem was when these people who used these load balancers on virtual machines and migrated to K8s, initially they were happy that K8s provided auto healing , auto scaling , automatic service discovery, exposing services to external world. People used to create deployment and have services on top of it using which they exposed their applications to external world. But then they realized that service was providing load balancing feature for exposing the application was simple round robin load balancing feature. This round robin load balancing feature allowed was that if 10 external requests comes for applications, kube-proxy creates Ip tables and sent 5 requests to pod 1 and other 5 requests to pod 2. So in K8s they are only getting one load balancing feature of Round Robin vs in Enterprise Vms they used to get a number of different load balncing features which we saw earlier. Hence, people in general were not much happy with using K8s.


#### <u>Problem 2</u>

==> Load Balancer Mode

Lets say Company as big as Amazon which has 1000's of services, for each services when they create service type as Load Balancer Mode , for every static IP address that they received they were charged for every Static Public IP . So if there are 1000's of services that you require on your K8s , Cloud provider charged a lot. This was other problem vendors were facing after migrating to K8s.


In the previous example, People used to create one load balancer in VMs even if they have one server, two servers or 3 servers. They would configure load balancer in a way that if request comes for amazon.com/xyz forward it to server 1 , if request comes for amazon.com/abc forward request to server 2 and so on. So they only exposed load balancer with just 1 static public IP address and then from load balancer , using same static IP they would do path based load balancing like 1.1.1.1/abc forward traffic to server 1, 1.1.1.1/mno forward to server 2 and so on. So when setup was on VM ,companies only needed 1 Public IP address but when they migrated to K8s for each service you needed static IP address so for 100's of services you were charged for 100 static IP addresses.


#### So there are 2 basic problems faced by Customers after moving to K8s which are 1) Enterprise & TLS Load Balancing 2) Service type as Load Balancer and get static IP address for each service then Service provider will charge you for each static IP address.


These problems were acknowledged by K8s. Until then what happened was that openshift (K8s Distribution of redhat) they had something called "openshift routes" which is similar to K8s ingress. Since many users are complaining about this valid problem and is already implemented by  openshift , so K8s decided to implement "ingress"

#### <b>So we will allow the user of K8s to create a resource called ingress   and what load balancer providers like nginx, F5, Ambassador were informed to do was to create ingress controller.</b>

Since K8s cannot create ingress controller for bunch of service providers what they did is they asked service providers to create ingress controllers . K8s only allowed its users to create a resource called ingress.

> Now what is Ingress Controller ?

Lets say you are creating ingress resource on your K8s cluster, and if you say I need patch based routing than you realized you are missing path based routing on K8s which you used to get in VMs. So through ingress feature in K8s , you can come to K8s cluster then create an ingress resource and you can say that I want to create path based routing. So K8s says okay create a yaml file and inside yaml file create and inside that yaml file create configuration for path based routing. <b>However , who will implement this ?? Who will decide which load balancer to use ??</b>. Since there are 100's of load balancers in the market and some might come up in future too. So K8s said that we cannot support and create logic for all these 100s of load balancers in K8s Master or K8s API server. Instead, K8s gave this responsibility to load balancer providers to create logic for load balance controllers based on the logic behind existing load balancer already present. So what does ingress controller mean ? Lets say you want to create specific load blancing capability using nginx load balancer , so the nginx load balancer would a nginx company will write a nginx ingress controller and as K8s user on this K8s cluster , you will just <b><u>deploy the ingress controller created by nginx</u></b> using helm charts or yaml manifest. Once you deploy the ingress controller, K8s adminsitrator will also create ingress yaml resource for their K8s services. So this Ingress controller will watch for the ingress resource and it will provide you the path based routing

![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/11.png)

So to wrap up and revise lets summarize this concept again

--> Lets say this you have K8s cluster. You are creating a pod and writing a yaml manifest for it. Now what will happen is there is a component called Kubelet . What Kubelet will do is , it will deploy your pod on one of the worker node. So Kubelet also sits on worker node and API server will notify kubelet using scheduler that okay a pod is created and Kubelet will deploy a pod and similarly lets say you are creating a service yaml manifest. There is kubeproxy which will update iptable rules. So for every resource you are creating in K8s there is a component watching for that resource and is performing the required action.

So similarly , even if you are creating ingress in K8s, there has to be a resource or component of controller who watch for this ingress. So this was the problem. So K8s said,okay I can create ingress resource but if I have to create logic for all the load balancers in the market, like nginx, F5 , Ambassador etc , it is technicall impossible to do it. So I will come up with an architecture where user will create an ingress resource. Load balancing companies like nginx, F5 etc they will write their own ingress controllers and they will place these ingress controllers on GitHub and they will provide the steps on how to install these ingress controllers using helm charts or any other ways and as a User along with  creating ingress resource, user also has to deploy ingress controller. So it is upto the Organization which ingress controller they want to use. Ingress controller at the end of the day is just a load balancer. Sometimes Ingress controller can be load balancer + API Gateway as well which provides additional capabilities. So at the end of the day as user what you have to do is on your K8s cluster , pre-requisite is deploy an ingress controller ( you can deploy ingress controller which you used in your VM environment. Lets say you used nginx ingress controller in VM environment, you will deploy nginx ingress controller on your K8s env). So you will go to nginx Github page and you will deploy nginx-controller on to the K8s cluster.After that you will create ingress resource depending upon the capabilities your need. If you need path based routing , you will create one kind of ingress. If you need TLS based ingress, you will create other kind of ingress. If you need host based , you will create host based ingress resource etc. So this is one time activity where Devops Engineer have to decide what kind of ingress controller they want i.e. which load balancer they want like nginx ,F5. Then Devops Engineer will go to their Githubs page , find the steps on how to deploy this ingress controller , once they realize how to deploy them , after that it can be one service , 2 service or 100s of services , they will just write one ingress resource. Once they write the ingress resource, like ingress does not need to be 1:1 mapping . Like you can create one ingress and you can handle 100s of services using paths. You can say if path is "/a" go to service 1. If path is "/b" go to service 2 , if path is "/c" go to service 3 etc. So we understood the topic here . What is ingress controller , why was it introduced etc

So the major thing you have to understand is

Problem 1 ingress is solving is K8s services did not had enterprise level load balancing capability. So might say to move to K8s because its lightweight and few other things but without security, without good load balancing capbilities , no one will move to K8s. So that is why they introduced Ingress

Problem 2 , if you are creating service using load balancer mode, you had to use dedicated Public static IP for each service and Cloud provider was charging for each and every static Public IP address.

These two problems are solved by Ingress.

After understanding these two problems, you have to understand how to install ingress like if you just followed the presentation until here, and you go to your K8s cluster ,find one ingress example, get yaml file and you create just ingress resource. If you do this nothing will happen because you don't have ingress controller on your cluster. So if the ingres controller is missing . then you even if you create one ingress, 2 ingress or 100 ingress nothing will happen because the ingress is of no use without ingress controller and what is ingress controller  , it is a load balancer you choose based on your requirement from services like nginx, F5 etc. So if you want nginx load balancer, you use nginx ingress controller. If you want to use F5 load balancer ,you use F5 nginx ingress controller etc. Once you create ingress controller , and once ingress resource is created , ingress controller will watch for ingress resource and they what they will do is they will provide required capability like path based load balancing, host based load balancing etc

Next slides from 22:30 is practical implementation of theory we learnt.









































































