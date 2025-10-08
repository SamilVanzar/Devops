# RBAC

It is very simple yet complicated topic. Simple because it is very simple topic to understand but if not implemented properly then it becomes very complicated to debug issues and it becomes very complicated for your organizations because K8s RBAC is directly related to Security. If something is related to security that itself means its very important. You need to understand concept of RBAC more than how to create service account, role binding because it takes very less time to configure service account , role binding and attach pod to it but this lecture we will not focus much on those things instead firstly we will understand the concept of RBAC how and why it is very important and after that we will discuss what is role , what is serivce account , what is service binding.

### K8s RBAC can be broadly divided into two parts:

Part 1: Users Management
Part 2: Service accounts or how services manage access in K8s that can be any application you are running on K8s

#### Users Management

If you have K8s cluster, lets say you are using K8s in minikube or K8s in Kind or any other K8s cluster . Out of the box you get Administrative access to cluster because they are your local K8s cluster and you have been playing around this local K8s cluster for learning K8s. But when you try to run K8s in Organizations , the very first thing you do as K8s adminstrator or K8s Engineer your primary responsibility would be to define access.
So if there is Dev team , QA team , so how do you define what access developers have on the cluster and what access should this QA Engineer have on this cluster. Its not that any QA Engineer can access K8s cluster and delete any resources lets say for example Kube-System namespace or lets say they delete something related to etcd. So these things become worse if someone comes and deletes something related to etcd then it becomes very difficult for your Devops Engineer or Devops team to get back to the original state of these things.

So effectively how you solve this problem is by using RBAC.That means to say "<b><u>Role Based Access control</u></b>" . What this means is depending on the role of the person you define access. So this is one part of RBAC . So how you are going to manage the access to users for Cluster that is one part of RBAC

Second part of RBAC is how you are going to deal with service account.Lets say you have created a pod, through a deployment or any other sources, you created a pod.Now, what access this pod needs to have on this K8s cluster. Should pod be having access to config maps, should pods be having access to secrets. Lets say as part of your application , you need pods to have access to config maps or lets say as part of your application you need pods to have access to read secret or as part of your application can you delete lets say you have deployed a pod and what is pod is doing is lets say this pod is malicious and what this pod is doing is deleting some content related to API server or accidently it is removing some files on your system so how do you restrict this ?? So Similar to user management, you can also manage the access to your services or for applications running on your server using RBAC

So two primary responsibilities of RBAC is "user management" as well as managing the services running on the cluster.

Now how this is done is on broad level before jumping into depth of how do you manage all of these things 
> On broad level in K8s you have 3 major things for managing the RBAC's

1) Service Accounts or Users
2) K8s Roles or Cluster Roles
3) Role binding or Cluster Role Binding

So we will discuss the difference between Roles / Cluster Roles and Role binding / Cluster Role Binding but first understand that this are the high level 3 things that can define RBAC in K8s

<u>BUT</u>

How do you create users in K8s ?

So at the starting of lecture we learnt there are 2 major things 1) Users and 2) Service Accounts. But how do you create users ??

But the question is how do you create users ? We have been using minikube for training , so if you go inside minikube can you create a user ? 
Let say in personal laptop which is linux laptop , I can use command like "user add" and using this command I can create user on my linux system and can share this access and share this user lets say Dev User with Dev team and they login with Dev credentials and perform bunch of actions. But the question is how do you create user in K8s ?

You can't use linux command "user add" to create user in K8s. So what K8s says is <b><u>K8s does not deal with user management but K8s offloads the user management to Identity Providers </u></b>. So this is very important to understand because service accounts is something you can create on K8s , even in your minikube you can login and create service accounts. But this part is very important to understand. Lets say your organization is using EKS or lets say your Organization is using AKS or open shift so how do you create this users ? How do you tell that Devops Engineer should login with this username. Lets say your Devops team has 10 users , you want to create 10 users for these Devlopers and each Devloper should only have relavant access. So Devloper should people should not be able to delete the resources , QA people at most should only read the resources and read the logs for example. And Devops Engineers might want to do the administration of Cluster. So how do you do all of these things ? So thats what K8s says is I am not going to create/manage the users , I will offload the user management to Identity Providers.

Lets say now a days you are using any  applications the fundamental thing you might have notices is Most of these applications have option like
"Login with Instagram" or "Login with Google" you don't even have to create account with these applications. Lets say a person has downloaded an application from Play store and most of the time you get this option to "Login with Google" , "Login with Instagram" or "Login with facebook" and what happens is this person gets access to this application <b><u>WITHOUT CREATING A USER</u></b>. So this is what exactly K8s does. So K8s says is I will offload the user managment. So in K8s you all know there is a component called API Server, so you can pass certain flags to the API server ( flags are simple and we will discuss because as we discussed earlier never worry about syntax or don't worry about yaml look like). In K8s, API server works as your OAUTH server ( we will discuss about this). So you can offload your user management to Identity Provider.

What are some of the popular Identity Providers ?

--> lets say you are using this K8s cluster on AWS and lets say this is your EKS K8s cluster. So why can't you use IAM users ? thats what K8s says.If you are using EKS platform what K8s says is you can use your IAM users and using your IAM users you can log into K8s. So in between what  you have to create is IAM Provider or IAM OAuth Provider . And using this IAM , person will login into K8s cluster and already you have created users and groups in IAM. So if you have a user and if your user belongs to a Group in IAM, so as you login into K8s , you get to login using the same username and same Group. So this is how K8s offloads the User Management to External Identity Providers. So this concept is same regardless if you use EKS , AKS , Openshift etc. So depending on the Identity Provider your Organization is using, this might change. For ex: Your Organization might be using LDAP , your Organization might be using OKTA , your organization might be using SSO . So you can use all of these things. So K8s natively supports all of these things. It is upto you how you want to use this Identity Providers , how you want to configure this IDP , how you want to create users inside this Identity Provider. You can also use some Identity Brokers like Key Clock. Key Clock is very popular , many people manage their K8s Identity Management or user management using Key clock as broker , you can use all of these things.

Lets say you create K8s cluster on Amazon , you can go to EKS and you can integrate EKS with Key Clock and using key clock you can connect with your GitHub. So in your Github you can create User Management. As part of Github you can collaborate with 100's of people and you can create collaborators , you can create users in your Organizations , which user has what access in your Github. So using Key Clock you can connect to your EKS and you can get all of these users from Github. This is how K8s offloads the user managment


![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/12.png)


Now the second part is Service Account Management

--> Service accounts are just yaml file . Everybody, can create service accounts. So there is no difference with respect to service account.

 So if you understood how to create users and I will show you how to create service accounts . So service account is just like creating a pod. You can just create serviceaccount.yaml and inside your serviceaccount.yaml you will just define what should be your API version , what should be your kind, what should name of your service account.

But then comes an interesting part. Lets say you logged in as user, or your application is running  as service account. By default even if you are creating a pod, you might have this question , these days I am using this pod by default a pod comes with service account even if you are creating service account or not , <b> <u> a service account gets created automatically and using this service account itself, K8s pod for whatever application you are running will be talking to API server </u></b>. To that matter of fact , connecting with any resources in K8s, it will use this service account itself. If you are not creating service account , K8s will create default service account, K8s will attach that default service account to your pod. If you are creating a service account , you can use your custom service account. But what happens after that ?

Whether you are logged in into K8s cluster as a user or whether your application is running on a K8s cluster as service account that is fine, after that how do you manage the rules ? or how do you manage the configuration ? So to define access after this part , K8s supports two important resources that is called Role and Role Binding. So you can also consider this as Cluster Role when it has cluster level permissions and you can consider it as Cluster Role Binding when it has Cluster level permissions. So this is not important at this point of time. Simply understand that K8s does all of these things using role and role binding


> Now what is this Role and Role Binding

So once your application is running as service account or you are logged in as user, the next part is how to grant access to it. So firstly you will create a role and you want to assign this role to the Developer, so what you are saying is they should have access to pod , they should have access to config maps , they should have access to secrets within the same name space, 

> <b><u>To have access within same namespace you will create a role, if they want to have access across the cluster then you will create cluster role thats the only difference but we will talk in detail as well</u></b>

> <b><u>So you have created this Role. Now you have to attach this role to the users, so to attach you will use Role Binding , thats the very simple concept.</u></b>

So what is a Role ? 
--> So a Role is YAML file where you will write all the things like they need to have access to config maps, they need to have access to secret etc. Even if it is a single user , you will create role and you will say whoever gets this role , attach to them . Lets say there is a user called "Samil", you are attaching this role to "Samil" , then Samil will get all these permissions defined in Role. If you are attaching this Role to "XYZ" person then "XYZ" person will get access to all of these resources that you defined in role.yaml

So you can consider or compare it with IAM Policies. So once you attach all of these things, once you say that anybody who gets access to these role would have access to all the things defined in role. But how do you actually, assign them the role ? So you have created a role and there is a user , how this user and how this service account and the role gets attached to each other ??

<b><u>To do this , you use something called as Role Binding</u></b>

So the simple Eco System will look like this

Service account /user ,  Role , Role Binding

--> So you will create Service Account or a user and you will create a Role , using this Role Binding , you will bind both Service Account / User with Role . So this is a very simple architecture

So if you don't have Role Binding, you will create a service account / user , you will create Role but both of them are not attached. So if you just have Role Binding and then you will create a service account but to bind it you need a Role. Without having a role and just having Role Binding you are not Binding your service account with Any permissions for which you define / need Role.

So simply Role will take care of Permissions , service account / user will be taking care of Users or creating user management , and Role Binding will take care of Binding the permissions to the users . So this is the concept of K8s RBAC.


![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/13.png)

So simply, if you are creating a Role within a specific namespace , it will be called as Role. If you are creating a Role in the scope of the Cluster , it is called as Cluster Role . Similarly role binding as well. Now what is the difference between Cluster Role and cluster role binding ?
I don't think its good to discuss this difference in this class because if there are any begineers who are trying to understand this concept, it would be difficult for them to understand

So whenever we are doing practicals, it you be easy to understand difference between cluster role , cluster role binding and role and role binding as well

So until here is theory part of K8s RBAC

--> Now we will see how to create 30 days free trial ,so you can make use of this openshift cluster for learning

20:00




















































































































































