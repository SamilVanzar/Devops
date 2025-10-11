# Custom Resource Definition, Custom Resources and Custom Controller

--> In this chapter we will learn about above topics

--> In level overview, in K8s cluster there are some resources which comes out of the box like lets say : for example you can create deployment resource like once you write a deployment.yaml file , a deployment is created for you and an application is created for you which is taken care by the controller in K8s or you have service in K8s or you have pod , config maps or secrets which are all out of the box / native K8s resources.

But apart from these out of the box resources, what K8s says is if you want to extend the APt of K8s or you want to introduce new resource to K8s, 

why you want to introduce newsource in K8s ? Because you feel that the functionality you need in K8s is not supported by any of the native or out of box resources. For example : you feel that K8s does not support advance Security Capability . For ex: You have resource like Kube Hunter or Kuberno or Kube bench so all of these things addresses security related problems. So they say that you want to introduce new resource into K8s or you have applications like ArgoCD . What they say is introduce GitOPS capabilities to K8s or what they say is you want applications like Flux , Spingear . So you have 100's of applications which are not out of box. You can find such applications in CNCF. CNCF is all about the K8s controller like custom K8s controller. You want to introduce new resource to K8s or you want to expand the resources of K8s or you want to extend the API of K8s , during that time you use CRD ,custom resources and Custom controller. This is the high level overview

So what happens is there are two actors here 
1) Devops Engineer , 2) User

So Deploying the CRD and the custom controller is the responsibility of Devops engineer and deploying the custom resource can be the action of the user or can be the action of Devops Engineer as well.

So we will understand the concept of CRD , Custom resource and custom controller along with who implements them.

So why you want to extend the API of K8s which we learnt was because whenever you want to introduce new resource to K8s like Argo CD , Key Clock or any resource which is not natively available in K8s , in that scenario you need custom Resource, Custom Resource Definition and a Custom Controller

Lets understand each one of them and deep dive on each of them

--> Lets say you have K8s cluster and have applications deployed as K8s deployment , service , ingress controller as well, config maps. Also user is able to access the application using ingress controller.Traffic is flowing inside and outside of K8s cluster . But after a while , what Devops Engineering team said is let me explore K8s more . There is world beyond K8s , like you have realized like there is something called ISTIO which is adds service mesh based capabilities to K8s or you have realized you have an application called Argo CD which adds GITops capabilities to your K8s cluster or you have realized that there is an application called Keyclock which provides features of Identity and Access Management or OUTA capabilities to your K8s cluster. So similarly there are multiple applications that you have realized that are used to enhance the capabilities of your K8s cluster. So you have realized that there is a world beyond the native K8s applications. So then the question comes how does K8s support this external resources because this resources keeps growing. So there are multiple resources / multiple companies that says that we will provide advance capabilities to K8s cluster like use our services to get X feature on K8s , use our services to get Y feature on K8s , use our services to get z feature on K8s . So lot of companies comes to K8s and tells we can provide different features to K8s . So how K8s handle this ?

K8s cannot go into each of these applications and create logic for all these thousands of applications into K8s. K8s has accomodated logic for Deployment, K8s has accomodated logic for services , logic for config maps, secrets and have them as native services. But if K8s will start creating logic for such hundreds of external applications as well , its impossible for the creators of K8s teams as these applications are like ten of thousands and would be very difficult for the creators of the K8s to create logic or configurations for all these application inside K8s .

So what K8s said is okay there are said resources or native resources with K8s which we support out of the box , <b><u>if you want to add additional capabilities to K8s , what we will do is we will allow the users to extend the API of K8s</u></b>

So what k8s is saying that we will allow to extend the capabilities of K8s ( extend the API of K8s) is that you can add new API Resource  Using this resource you can ask your customer or anyone  who wants to use ISTIO. So what K8s is saying to ISTIO people is that you can created and you can extend K8s like whoever the developer or users are , you can ask them to deploy few resources and in this K8s clusters can be extended but we from K8s team are not going to support it or create resources.

To extend the API or to extend the resources or capabilities of API there are 3 resources in K8s

1) CRD - Custom Resource Definition
2) CR - Custom Resource
3) Custom Controller

Lets dig dive in details about each of them to understand

1) CRD ( Custom Resource Definition):

For example there is a company called  Istio , and istio says that Istio can enhance the capability of K8s, so K8s people are saying Istio to create a CRD. 

What is CRD ?
 --> Defining a new type of API to K8s. 
 And how do you define is <b><u> to submit Custom Resource Definition</u></b> to K8s.
 
 So people of istio will create new CRD and in this CRD , its a yaml file and in this yaml file you will define things like what a user can create like 
For example : You are istio user who is creating deployment.yml file and in this deployment.yml file you defined few things what is the API, what is kind or what is spec, what is template ,what pod ,what is container , but beyond this how would K8s understands that whatever the deployment.yml you have created is correct or not . So K8s will have a <b><u>template</u></b> which has a template which has all the field related to deployment.yml , like tomorrow you can add volume mounts or mounts or new field like XYZ. So k8s says that immediately when you create new field called XYZ in deployment.yml , you will get an error. 
What error you will get ? --> Field XYZ unknown or some error

So how K8s is throwing this error is K8s knows what is definition of deployment , out of this definition you can use whatever is defined in definition or omit whatever is defined and you don't need but this is the standard definition that K8s has . 

Similarly, even for the custom resource ( what is custom resource ? it is something custom made or new or what is not available  inbuilt in K8s) but before anyone submits , K8s asks you to extend or define a new type of API to K8s using the CRD ( Custom Resource Definition) where people of Istio will provide a complete yaml file which will have all the possible options that they support. 


#### <b><u>So a CRD is yaml file which is used to introduce a new type of API to K8s and that will have all the fields that a user can submit to custom resources.</b></u>

So if you take a resource named Deployment.yml and further deployment.yml you have Resource Definition inside your K8s. so deployment.yml is general <u><b>RESOURCE</b></u> of K8s and the fields inside deployment.yml is general <u><b>RESOURCE DEFINITION</b></u> of K8s. But because we are dealing with custom resources that's why we call it as Custom Resource Definition. And whatever user is submitting , we call it as Custom Resource.


![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/12.png)


Lets understand this concept in detail by comparing CRD with deployment.yml itself for easy understanding

For ex: You are user who is creating "deployment.yml" . Inside this deployment.yml file you define "API Version : Apps V1" , "Kind:" , "Metadata:" , "Spec:" etc. But how does K8s understand that your yaml definition is correct or not . So like I told you, this yaml file is <b><u>KUBERNETES RESOURCE</u></b> you are creating . Similarly K8s has <u><b>RESOURCE DEFINTION</b></u> in the API Server or K8s control Manager. So what does this resource definition do , is it will validate if the Resource you created is right or wrong. So the Resource Definition in K8s will try to understand if the user created resource is correct or not.

Similarly, even in <b><u>Custom Resource Definition</u></b>, its defintion for custom resource you are adding to enhance the behavior of K8s or to extend the API of K8s. So even in that what user will do is , as a user he will create a <b><u>custom resource</u></b> . So lets take same example of ISTIO. ISTIO has a custom resource called Virtual Service. So what user will do is it will define "API Version :" , "Kind:" , "Metadata:" , "Spec:" etc which you can get from the Resource yaml file of Istio service. So all the yml file you defined as virtual Resource is <b><u>Custom Resource</u></b>. Now who will validate this custom resource ??
So this Custom Resource is validated against a <b><u>Custom resource definition that is CRD</u></b> that Istio people have created and as Devops engineer you can deploy this custom resource onto your K8s cluster so that your K8s cluster is extended . So the two functionality of CRD is to extend the facility of K8s API and also to validate. So right now you have understood the difference. If I try to compare this with native K8s resource with custom resource of K8s the process is same . On K8s you are creating deployment.yml file and on the contrary Istio or third party service people will create Custom Resource which will be validated against the Custom Resource defintion


![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/13.png)

Once you think that this is done , but in reality this is not done yet. So user has created a custom resource validated against custom resource definition . So we already learnt both these things CR and CRD. This Custom resource is created inside your K8s cluster. if you think this is done then its incorrect. It doesn't complete here. Because you have created a custom resource but if you take the same example of deployement , you might have created deployment.yml inside your K8s cluster which is validated against the deployment Resource Definition . After that , you will know that inside K8s there is something called as <b><u>Deployment Controller</u></b>. So this deployment controller is the one that is taking care of creating Replica Set and replica set controller will create a pod .So there is a process that is happening. So who is doing this ? A <b><u>K8s controller</u></b> is doing this. So similarly here , there has to be a custom controller or you can call it as a custom k8s controller. So this is the flow .
So there has to be a custom K8s controller that is already deployed inside your K8s cluster. So that once you deploy your custom resource, this controller will watch for the Custom resource and it will perform some action.

![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/14.png)

So lets create one more diagram and understand the flow. So if there is a K8s Cluster. So first of all Devops Engineer, what they will do is

Step 1 : If Organization decides to use Istio, then Devops Engineer will first deploy CRD ( Custom Resource Definition).How they will deploy this ?
They will go the Istio document, they will find what is CRD and they will deploy either using the plain K8s manifest or using the helm chart or they can deploy using operator.

So the Devops Engineer deployed the new CRD because we are talking about Istio lets call is as Virtual Service CRD is deployed on to your K8s cluster.

Now there is another actor here and this actor is nothing but a user. A user can be Developer or Devops Engineer or any user. Now what this user will do ? Again he will also go to istio docs and because he wants to use the capabilities of Istio inside the cluster he will create a <u><b> Custom Resource</b></u>. What is this Custom Resource ? Let's say he has name space called Abhishek. So inside this name space "Abhishek" , he will create istio "Virtual service" Custom Resource lets call is "VS". So he has created "VS" custom resource. Now like I told you , before it gets created the API Server or someone will intercept this request and they will try to validate this against virtual service CRD and if the request is correct then the request will passthrough else reqest will fail. So this is the process that will happen. Lets say user has created a proper Custom Resource after following the documentation which is validated and deployed inside your K8s cluster but till here you have just defined a custom resource , it will just there and won't do anything. Lets say for example , you defined an ingress resource without ingress controller , what will happen ? nothing will happen. Like we discussed in previous class ingress resource if of no use.

Similarly you have just deployed a Custom Resource . if you deploy a deployment there is a deployment controller which is taking care or which is doing something for you . But here the custom resource is being watched by no one. So if nobody is watching it, nothing will happen. So someone has to watch this custom resource. So again the action too here for Devops Engineer would be to deploy custom controller.

So how is this custom controller is deployed ??
Devops engineer will either go to the documentation and will either deploy it using the helm chart or plain manifest or operator or whatever process Devops Engineer wants to follow in the Organization.

So again, he can create this custom controller across the cluster or he can create for the specific namespace depending on the feature that controller support. Lets say ,because we are dealing with Abhishek namespace, devops Engineer will deploy a custom controller under Abhishek namespace. So now this Custom Resource is verified by this  controller and controller will perform required action. In this case what is the required action , the required action is istio i.e service mesh or virtual service or east - west traffic or whatever. So this istio controller which you deployed will read the custom resource and will perform the action. So whenever you are getting confused with respect to custom resources or custom resource definition , the simplest thing you will do is try to understand it with native K8s resources itself. Because whether it is a native resource like deployment or custom resource the only difference is in case of custom resource you will deploy all the required resources whereas in case of deployment there are these resources available out of box in K8s cluster. But the steps are common. For any custom resource or for any applications like istio or argo cd the steps are common. that is the

Step 1: would be to deploy CRD to extend the capability of your K8s cluster
Step 2: You have to deploy Custom Controller
Step 3: User who wants to use this feature on their K8s cluster like you might have 100s of namespaces but only 20 might want to use this feature. So whoever the users or whoever the namespace that they want to use, what they will do is they will deploy the custom resources.

So similarly when you compare it with deployment, by default inside K8s cluster you have a Resource definition for deployment . As a user you are creating a deployment in K8s which is validated against Resource Definition of your K8s and instead of the custom controller, for deployment inside your K8s , you have a native K8s controller. So this is a concept of Custom Resource, Custom Resource Definition and a Custom Controller.


Now some interesting points just for understanding . How can one write a custom controller ??

So the very popular way of writing a custom controller is using a GoLang. You can write using Python , you can write using Java as well but the community or the very popular medium of writing a custom K8s controller is using a Golang. One of the primary reasons is because K8s application itself is written in Golang. So one of the popular K8s API is Client-Go . Now you have Client-Python , Client- Java and everything but you know initially when K8s was developed, to extend the capabilities of K8s  , it had something called "Client-Go" which will allow you to interact with the K8s API. So whenever I am saying , you want to extend the capabilities of K8s that means to say that user has to interact with the K8s API , just like kubectl interacts with the K8s API , whenever you want to write a custom controller, you have to or you might want to talk to your K8s , for that inside the K8s API Server there is a component called "Client-Go" . So this Client-Go will allow you to talk to K8s API Server. So initially it was only "Client-Go" but later point of time like now you can write it in Python , write it in Java, because K8s have API supported for Python, Java for different things. That is fine but because the community started with Go and K8s itself is written in Go , the entire CNCF ECO System because of the features of GoLang like concurrency or easy way of writing it and all these things we will learn when we try to learn Golang but for now because initially Community had started in Go Language and Client support is very well so its very good community for Client-Go and CNCF Eco System with Go Language so all of these custom controllers are written in Go language. So to write custom controller the preferred way is using Go. 

And how do write a custom controller ?

In a very high level without going much into details, because details would need another entire lecture and need knowledge of Golang. So what you will do is you will use Go Language as your programming language and if you have your K8s cluster or API server then there is component called "Client-Go" . You will interact with "client-go" and the entire process depends on setting the watchers and listers. So what you do is by default this client-go or by default your K8s will be watching for a set of watchers like there is a deployment watcher , there is a service watcher . So whenever you are creating or whenever you are performing any of the actions like "update" , "delete" , "Create" so what happens is there is inbuilt watchers that K8s has created for this resources. So whenever one of these actions is performed, K8s will come to know using these watchers. But if you want to write a system K8s controller , then you have to create new watchers. So early when I started writing K8s controller in 2015 the frameworks were not strong. There were not many frameworks so you had to create your own watchers for everything right from scratch. But right now you have many framworks like one of the popular one is you can use the K8s Controller runtime. So this is one is supported by K8s itself. It is a golang based K8s package so using this also you can setup your watchers. Like lets say what people at  Istio might have done , lets say they have setup watchers for let say for virtual service so any action like lets say user are creating , deleting or updating so there are watchers which are created for this virtual service and these watchers will notify client-go. In Client-Go there is a package called "Reflector" ( we will not go into details of it) , so using this Reflector you know what you will do is whenever you understand that a new virtual service is created , you can place it in FIFO queue so you will put that in worker queue and you will start reading each of the element or each of the objects in the worker queue and like watchers will identify and will put that in the worker queue and then you will start processing each and every resource. So this way once your controller starts processing each and every object in the queue then it starts creating the required functionality in K8s , in this case it will start creating virtual service configuration on your K8s cluster. So this very high level concept if want to write a custom controller. If you want to understand more you can go for the sample K8s custom controller whose link is
https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbVRyYi1FTndmcHJnZzVoa1Zfdk1oaGxPZEVpQXxBQ3Jtc0tsbFNjUDhsb0QtR3NlTV9FR2tpM3BhOTdVbDVxNDR1Y0VQSzdFUmJXbUZNcjY1SXNxeFM1aEdzT0x0VFU1NGp5ZDhEbnZLbm9BdHo1LU4yYjNUZ1l4OTl2V1k2d1ZsVVgzdnNUMENBWnhSdEF6bzFjZw&q=https%3A%2F%2Fgithub.com%2Fkubernetes%2Fsample-controller&v=alGEPSQxbLg

This will help to understand how to write custom controller which K8s supports some document as well. And go for the "controller-runtime" as your medium for writing. If you want to write operators then you can use "operator.sdk" as well. Devops Engineers don't write custom-controller or CRDs. But if you are in K8s Developer role and in your org if you are required to write a new K8s Custom controller then you might have to know all of these things


![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/15.png)

30:00 after this practical example of how K8s controller looks like through demo

































































































































































































