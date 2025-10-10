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















































































