## Docker Networking

Question: Why is docker networking required ?

--> We need docker networking for containers to communicate with each other as well as host operating system.

--> Consider there is host on top of which you have installed Docker for running containers. Lets say you have two containers C1 and C2 where C1 container is related to Development and C2 container is related to Finance . There can be scenario where one container needs to talk to another container and there can be scenario where one container needs to be completed isolated from another container.

==> Lets consider scenario 1 , where container C1 needs to communicate with container C2. So there has to be networking way where container C1 can talk to container C2 using IP address. Also all containers should definetely talk to host . Since containers do no have complete operating system and are lightweight in nature, so containers need to talk to host operating system as well.

- Before understanding scenario 1, lets understand how container C1 communicates with host operating system. Now every operating system has default ethernet interface "eth0" .Lets say host operating system eth0 interface IP address is 192.168.3.4
- Also , each container has default ethernet interface called eth0. Lets say that the container eth0 interface of container has IP address of 172.16.0.2.
- Now based on the IP address of container and host we can see that both of them are in separare subnets. If you try to ping from container to host , ping will fail
- So to solve this issue what docker has done is , <b><u>docker creates a virtual eth ("veth") which basically is docker0</u></b>, Without this virtual eth , container cannot talk to host. So by default when you create container ,  virtual eth "veth" i.e. "docker0" gets created. And this is called <b><u> BRIDGE NETWORKING</u></b> .This is the default networking in docker . The reason its calles as bridge networking is because a container has different subnet and the host has different subnet and using bridge , container would be able to communicate with host . This bridge is virtual ethernet. Hence its called veth or docker0 . If you try to delete this veth or bridge network, then container would never be able to talk to host . This creates problem because container doesn't have its own operating system and is dependent on host . Lets say you have an application which is running as container . Now customer has access of host but since there is no communication between host and container , customer wouldn't be able to reach container and access application.

Thus if bridge networking / veth / docker0 is not available by default out of the box then there is no way container can communicate with host which would cause problems for users on internet to access the application hosted on container. Hence Bridge Networking is always available by default "out-of-box" . Hence by default docker network is bridge network.

![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/32.png)

One thing to understand here is Bridge networking is not the only way a container can talk to host , there are multiple ways . Like the other networking way is 

<b><u>"Host Networking"</u></b> which means the container would directly access the network of your host. In this case what docker does is whenever a container is created, docker will bind the container IP address to the "eth0" of host. So what happens is lets say your host has an IP address of 192.16.3.4 and you created container , what docker does is in "Host Networking" is it  will give container IP address in the range of "192.168.3.x" for ex: container will get an ip address of 192.168.3.6 . So both container and host are in same subnet and can communicate with each other. So any user who has access to host will also be able to access application running inside container. However, the "Host networking" is very problematic approach because you want containers to be "SECURE". However, with "Host Networking" , if new containers would start getting IP addresses in the scope of network as host , anyone who has access to host would also get access to containers regardless of they are required to have access or not which makes the setup insecure.

There is one more type of setup called "Overlay Network" but that is applicable in K8s and we will not discuss in details here . But in general overview, an overlay network is used where applications are running on multiple hosts or hosts running in clusters. Like in K8s , there are multiple hosts setup in HA and you have same application running on multiple hosts then overlay network helps create a network which his common across multiple hosts. But since docker we don't need cluster of hosts , we will not discuss in details here. Also there are other networking as well but these three which we discussed are very popular

Now going back to the communication between container 1 and container 2

-- In Scenario 1, we needed container 1 to communicate with container 2. Now with default or out of box networking  there is "Bridge Networking" or "veth" or "docker0" . Since there is only one "veth" ,both container 1 and container 2 would same  veth network  network and would get IP address from same "veth" network and through this docker0 which is virtual ethernet they would be able to communicate to host . Hence, by default when you deploy C1 and C2 , they would be able to communicate with each other through veth/docker and also communicate with host.

![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/33.png)

-- In Scenario 2, lets say you want to keep container 1 and container 2 completely isolated. However, the default docker0 virtual ethernet would create security issues since through docker0 virtual ethernet network by default these two containers would be able to communicate with each other. So this is security problem because "docker0" becomes common path for hacker. Even not for hacker , if there is an internal person who wants to mess up with system can easily do it because "docker0" is common path to host and all the containers because through veth all the containers can talk to each other as well with host. So this Out of the box nature is not secure. So how we acheieve scenario 2 for isolating two containers ? So question is how do we achieve this logical isolation.

-- So logical isolation can be achived using bridge networking itself. You cannot achive logical isolation using host networking because all the containers are on same host so there is no way to have hosts running separate/difference IP address. Third networking type of overlay networking is not applicable because its complicated and you don't want to implement . So lets see how do we achive this using bridge networking


--> For Scenario 2 , where C1 and C2 should be isolated is that docker allows you to create <b><u>CUSTOM BRIDGE</u></b> network. So by default out of box there is one default bridge network available. However, you can always create your own custom bridge network. So what happens here is for Container C1 there is veth interface and using docker0 default virtual ethernet it will communicate with host eth0. However, for C2 container we will create custom bridge network named "CBN" instead of default "docker0" virtual network . So C1 will communicate to host using "bridge0" default network where as C2 will communicate with host using "CBN" . Thus, we removed the common link "docker0" which was causing containers to talk to each other by default. You can create custom bridge network using "docker network" command.


Next there is also example demo which demonstrates the concept we learned.



























































