

--> On high level we will learn about what is config map in K8s.The why aspect of what is a secret , why a secret does exist in K8s , difference between config map and secret , live demo on how to create config map , how to create secret , different types of secret , then we will see how to use or how to reference this inside a pod or deployment of your K8s

### <b><u>Config Map</u></b>


What is config map in K8s ?

Just for couple of minutes , if you forget about K8s. Lets say you are app developer and have an understanding of how application works . So lets say there is an application called "Backend Application". So this backend application, what it does , is it talks to a DB, retrieves some information from DB and gives this information back to user. So this is very simple application.

This backend is trying to talk to DB and it is trying to give information to user when user has requested the information. 

Now what kind of information is required by application from DB ? Like it requires some information like what is the DB port ? What is the DB username ? what is DB password ? what is connection type or what are the number of connectors required ? And few more information that this application require from DB.

Now how this information is retrieved ?
This can be retrieved using environment variable inside the application. You know that hard rule or thumb rule about application is that , application should not have these details hardcoded . If these details are hardcoded , what would happen in future if these details gets changed , then the user will get Null or some vague information. If the username got changed or password got changed or port number got changed , then in such cases user will get wrong information or he might not get any info at all regarding the user. To solve this problem , you won't hardcord this information inside the application , but a general practice is you would save this as part of Env variable or you would try to save this as specific file in a specific path inside your application or inside your file system and you will try to retrieve this information from file system using OS modules , let say using python , you can use OS modules or you use java , you can get that from the operating system libraries that the java supports. So this is how you read this information.

So how do you do this inside the world of K8s?

Inside K8s there are 2 things

1) First is about the same problem we discussed , Lets say we want to solve the problem with respect to DB port , the DB connection type  and all of such information . Here we are not talking about DB username and DB password. For some time lets put DB username and DB password aside and lets talk about the DB port and DB connection type and these kind of information. So what K8s says is because K8s deals with containers , how a user can get this information as part of your container env variable or as part of your container or as part of specific file inside your container ? So to achieve this , what k8s says is we support something called as config map. So what you can do is as a Devops Engineer or as a configuration manager Engineer, you can create a config map inside K8s cluster and put the information like DB port or any kind of information inside the config map and you can mount this config map or you can use the details of the configmap inside your K8s pod. So the information can be stored inside your pod as env variable , or stored as file inside your pod on your container file system but this information needs to be retrieved from configmap because as a user you cannot login into pod and you cannot go to the container or once inside container you cannot create this env variable . So this is not a right practice. So you can do it , but the problem is that sometimes you might not have access to the container login itself or other problem is that what if this fields gets changed continously or whenever you are creating a docker file itself you don't know these values because probably these values are fed to your application at a later point of time. So this is not possible. So what K8s suggests is go with the **"Config map"**

So as a devops Engineer ,what you need to do is you can collect this information that a user requires , you can talk to the DB admin and as a devops Engineer , you can create a config map and inside the config map , you can store this values. Once you store these values, you can mount this configmap or you can use data of this configmap as Env variables inside your K8s pod. How you will do it ? and what are the different ways like I told you is
1) You can use them as env variables
2) Use them as volume mounts


So we will discuss about both the use cases when we see live demo but for now you understood the purpose of configmap. So what problem config map is solving ? 

> <b><u>So config map is solving the problem of storing the information , that can be used by your application later. So config is used to store data and this data can be at later point of time used by your application or your pod or your deployment.</b></u>


Now, if configmap is solving this problem, why you need a secret in K8s ??


--> Secrets  solves same problem , but secret deals with <b><u> SENSITIVE DATA</u></b> that means to say like you know if you are just providing all of the information , if you go back to the previous slide , like I told you , you have parameters like DBpassword , DB username , like if you put this information along with the DB port and all the details in the configmap, there is a major problem in the K8s that is in K8s whenever you create a resource , what happens is this information gets saved in etcd . So if this information is getting saved in etcd , in etcd usually the data is usually saved as objects and any hacker who tries to get access to etcd can retrieve the information. So if they are retrieving the information of your DB username and DB password , that means your entire application or your entire platform is compromised. So they can get the details of your DB. So if they are getting details of your DB, then your K8s cluster does not have proper security. So to solve this problem , what you will do is K8s says that if you have non-sensitive data then store it in config-map , if you have sensitive data then store it in secrets.

Now what happens if you store in secret ? What difference does it make ?

So what K8s says is , if put any kind of data in secret what we will do is we will encrypt the data at the REST. What this means to say is before the object is saved in "etcd" , K8s will encrypt the data . So by default , K8s uses basic encryption, but what K8s says is we will also allow you to use your own encryption mechanism like custom encryption. So you can pass these values to the API server and say that whenever API server is feeding some information to the etcd, you can use this custom encryption and even if hacker is trying to access this etcd because he does not know the decryption key ,so he can read all the information from etcd , lets say he read the information about configmaps , deployment, Pod but when it comes to secrets , he will just retrieve an encrypted information that is of no use for him.

<b><u> So whenever you have sensitive information , go with storing the objects or values in the secrets and whenever you have non-sentive information then go with the config maps . This is the difference between secrets and config-maps</u></b>


Now lets go a step back and see step by step approach on what is happening?

--> Lets say you are a user , as a user you are creating a config-map. So what you will do , is you will write a yaml file for config map and inside the yaml file, like I told you , you will add all the details , so you can get the yaml syntax from K8s documentation as well. Once you do this , you will use "kubectl apply" . Once you do "kubectl apply", you create this config map on your cluster. So your configmap is created. So what is happening here ? 
You config map is created and at the same time API server is saving this information inside "etcd" as well . So this is the entire process with respect to configmap with respect to config map. Now, for a hacker if you are storing this sensitive information , he has 2 points to retrieve the information 

1) If the hacker has access to your K8s cluster, he can come to the configmap and he can just say "kuebctl describe configmap" or he can just say "kubectl edit configmap" and he can get the information from the configmap to get DB information and your DB password is compromised
2) Or he can go to the "etcd" , and because the data is not encrypted , he can get the information from etcd as well and DB password can get compromised.

So these are the 2 major problems that secret is solving

1) What if hacker comes to secrets and he can again use the "kubectl describe" and he can read the information. So for this what K8s recommends is , apart from K8s doing its part of encrypting keys, what K8s suggests is whenever you create secrets , use a strong RBAC . Say that no user can access to RBACs who are not required. For ex : There is very popular concept in Devops which is called as "Least Priviledge" . Least priviledge is a concept where you will only grant least access . That is very less number of people should have access to secrets. Like the same concept as IAM in AWS. So if you are restricting the access to lets say developer who is trying to get access to K8s cluster , he can have read access to config maps , he can have read access to pods, can have read access to deployments but he has no requirement to have access to secrets. You can prevent that in the user RBAC , you can just say that he should have access to all resources but not secrets.
2) When secret is used , in etcd data at rest is encrypted and hacker does not have decryption key so thats why your information is secure.

So this is how you prevent both the things.

So this is the difference between config map and secrets. 

Incase this question gets asked in interview, you can say that both configmap and secrets both of them are used to store the information or pass the information ( like to you want to save some json inforation like key:value pair inside your K8s cluster which is then fed to application running in pod in K8s ) you can use both of them for same purpose but secrets is used for sensitive information where as config maps is used for non-sensitive information . How does secrets solves this sensitive information problem ? like with secrets , data is encrypted at rest and with secrets you can also enforce strong RBAC so that for the entire secret resources in K8s you can say only Devops Engineer can have access to it. You can do it using the K8s RBAC.


Now we will quickly move to the demo




























































































































