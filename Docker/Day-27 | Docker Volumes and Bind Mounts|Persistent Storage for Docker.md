## Bind Mounts and Containers

--> Lets understand the problem regarding why we need bind mount and containers.

- Lets say you have container and have installed nginx application inside this container. This nginx application continously places user information and users IP address from where user has accessed the application. All such information is stored by nginx in logfile. This log file is very important because lets say you are doing security audit of your company or performing any kind of details about user information , the log file has to have all the information for past 10 days or 7 days as per the requirement of your Organization. For instance this container has gone down so what happens in this case is because container has gone down , the log file gets deleted. This log file gets deleted because containers are ephemeral (shorliv) in nature.

- Also as we have discussed in our previous classes containers are very lightweight in nature and they don't have their own storage. They share CPU , memory , storage from host operating system and don't have their own resources. Now since container uses all of these resources from host operating system and don't have their own resources , when container goes down all these resources from host operating system gets freed up and killed. So now the log files or user details present in nginx gets deleted. If the log file is deleted , organization does not have information about users who authenticated in nginx or users IP addresses or any kind of user information which needs to be audited or tracked since it gets deleted once container goes down. 

- So the root case of the problem is that there is no persistent volume which can save container data storage.

So K8s offers two solutions for this problem
1) Bind Mount
2) Persistent Volume

Lets understand both

1) <u><b> Bind Mounts</b></u> : 

Allows to bind a directory from host to your container. As below example, consider C1 as your container and Host. Now what bind mounts does is it will bind your folder on your container C1 with a folder on host. So lets say you have folder called "/app" on your container and also you have folder "/app" on your host. So basically using bind mounts, you can bind them. That means any file which you are writing on "/app" folder will also be present on "/app" folder on host. So even if the container goes down , the "/app" folder is already present on the host. So what happens is next time when new container comes up , lets say container came up with name C2 , what it can do is again you can bind that specific container C2 with the same "/app" folder on the host so that the information is not lost. So whenever the container has come up , it already has the information. Even if lets say container is now coming up, since you have all the data in "/app" folder on host , you can access container data anytime. So this is the concept of bind mounts. You are binding a specific directory from host to the specific directory on the container.


2. <u><b>Persistent Volumes</b></u>:


What volumes does is , it does the same thing as bind mounts but it provides better lifecycle. By lifecycle, it basically means volume is using docker CLI. Using docker command you can create volume . So volume again is a <u>logical partition which you are creating on your host</u>. So you create something called volume and you can manage the same things like "create volume" , "destroy volume", "edit and move volume attached from container C1 to container C2 " , "attach same volume to both containers C1 and C2". So technically both bind mounts and volumes are technically solving same problem of container temporary storage. The difference is you are not attaching host folder to container folder but instead you are asking host to create logical volume . So when you go to your host and perform "df" , it will provide all the disks info . Also when you create volume, this volume is also mounted to the container.

Also there are few advantages of using volume as below

2.1) You are managing the entire volume directly using docker CLI . You can use commands like "docker create volume" , "docker volume ls". So it becomes easy for the users to manage volume using docker cli itself.

2.2) This volume has a lifecycle. By lifecycle we mean is you are getting advantage of managing it like creating , destroying , editing and all such things.

2.3)  With bind mounts , you are binding folder from your host with folder on your container which limits you to specific host. What if that host is lost/corrupted/destroyed ? So, volumes also give you an option where you can create volume on any place . You can create volume on same host , or you can create volume on any external EC2 instance, or you can create any external storage devices like S3 or NFS . So you are not restricted to one single host option. You can use volume with bunch of external sources. This is huge advantage.

Consider your host doesn't have big disk or your container need huge volume, to do all of these things your host will be very little in resources. So what volume offers you is you can also create volume on some external devices , so you can also easily take their backups or move volume from one cloud provider to another. So you can play around with the volumes.

2.4) These external volumes can also be of high performance as well. You personal host like laptop / desktop storage on which you have containers might not have high disk throughput like read/write. So if your container needs storage with high IO , you can create very high performance external storage and mount it on your container as well.

so these are some of the advantages of using persistent volume.

--> Now there are some common misunderstanding in using docker volume command. Some places you will see people using

"docker -v" and in some documentation you will see people using "docker --mount" . So simple explanation to both these commands is they are doing the same thing.


Whenever you are trying to mount a volume to a container, you can either use "-v" or you can either use "--mount" . So both are same things with only syntax difference. The reason for syntax difference is if you are using
"-v" you are attaching volume and you are passing all the other arguments in same command . Like for example

docker -v <source:directory:destination:temporary storage>

For ex: docker -v /etc:/tmp:/root:/home

So all the options you provide are simply separated using semicolon

Whereas if you are using "--mount" , it is more verbose option. What verbose means is this case you elaborate in detail . Like instead of defining values in docker -v command, you define all the details like for example

docker --mount src "/tmp" 
               dest "/home"
               permission "777"

So basically using docker --mount you allow users to understand what you are running . So when you are passing comment , you provide all details so that if some user is trying to understand this command they can simply read all key value pairs and understand what is happening like you are trying to mount specific directory "/tmp" from source operating system to destination container "/home" director with all read , write , execute permission . So if in your organization you want to mount volume better to use option of "--mount" instead of "-v" because its very verbose option and it can allow users to understand very easily.
























































