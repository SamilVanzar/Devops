## Virtual Machines

Consider you have one acre land where you are living with your family and entirely using this one acre land.

One day you suddenly decide that I don't need this entire 1 acre land. Even though I have park , garden ,water and everything but still I am not using this entire 1 acre of land. You have realized that you only half acre of this land happily. 

So you realized that I am only using half acre of land and wasting other half acre of land. Nobody is using other half acre and its going unused.

So in the remaining half acre,  you created another property . And you gave this another property for rent. So your comfort or convinience is not changed . So you are still enyoying your space and you also got rent by renting new property you built in free land . The other guy staying in remaning 1/2 property is also not diturbing you hence you are enjoying your comform plus getting rent

So keeping 1 acre of land for yourself and not using half acre was inefficient way. Instead renting out half acre of land is more efficient way.

So Devops is all about efficiency . If you do automation , you do tasks, but your end goal as Devops Engineer is efficiency.

![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/21.png)

Now lets convert this into real world software industry problem

Lets say you have a server. Inside your server, lets say that your organization , lets say "example.com" . What example.com or system admin at "example.com" has done is they have bought 5 servers . Lets say they deployed an application , 
Lets say they deployed APP1 in server 1 . Lets say this APP1 require 4 GB RAM and using 4 CPU processors. Now what happen is , Server 1 is of size 100GB RAM and 100 cores. However, you deployed an application of 4 GB RAM and 4 CPU cores. So you are wasting remaining resources of 96 CPU cores and 96 GB RAM.

Similarly, there is an app installed on Server 5 which utilizes 5 CPU cores and 5 GB RAM . Server 5 total capacity is of 50 GB RAM and 50 CPU cores, so you are wasting 45 GB RAM and 45 CPU cores. Which means you are using 10% of resources and wasting 90% of resources

So this created a big problem of <b><u>INEFFICIENCY</u></b>

> <u><b>Hence the concept of Virtualization came into picture</b></u>

So the concept of virtualization is lets say you bought physical server again of 100 CPU cores and 100 GB RAM. You gave this server to team 1, but with the concept virtualization what you did was on this one physical server you installed a <u><b>HYPERVISOR</b></u>

Now what is what hypervisor ?

It is software that can install virtual machines on your physical servers. And what you have done is , instead of 1 server you have created 5 servers inside the same server 1. So what you are actually doing is you are doing a "logical partition" and not physical partition. Like you are not physically breaking the server nto 5 parts but instead you are doing a logical separation.

Hence you are doing "Logical Isolation" of Physical server 1 and calling it as Virtual Machine 1, VM2 , VM3 , VM4 , VM5

We are calling them virtual machines because they logical / virtual and don't exist physically.

So as Devops engineer you added efficiency to process by using Hypervisor and creating virtual machines inside physical server.

Some of the popular Hypevisors are 
1) VMWare 2) Xen

Using these popular Hypervisors, what you do is do logical separation inside a physical server . You break the physical server logically and create logical machines or virtual machines. And now instead of just team 1 which was using server 1 , what can happen is

Team 1 can use Virtual Machine 1 on server 1
Team 2 can use Virtual Machine 2 on same server 1
Team 3 can use Virtual Machine 3 on same server 1
Team 4 can use Virtual Machine 4 on same server 1
Team 5 can use Virtual Machine 5 on same server 1

This is the advantage of using a Virtual Machine . 

Just to repeat or make you understand , concept of virtual machine is nothing but you are basically creating Virtual Evironments, that function as Virtual Computer systems on same Server. And these virtual machines have their own CPU, own memory and their own hardware. So VM1 is not dependent on VM2 , VM3 , VM4 , VM5 either for memory , CPU or hardware. So they are called logical computer systems. They have their own memory , their own CPU , their own hardware but the only difference is instead of physical servers they are logical servers.

And who is doing this entire process ? Your Hypervisor is doing this entire process . So hypervisor is a key that is dealing with your virtual machines or Hypervisor is something that is creating your virtual machines

![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/22.png)

Same concept is followed by cloud platforms like Amazon , Azure , GCP or any other cloud platform. 

Lets say for example Amazon  build their datacenters in Mumbai, India. What this means is they take a huge land and they install their physical servers here. They install hundreds or thousands of Physical servers. So just in previous example, there were 5 servers, similarly Amazon bought hundreds and millions of servers and hosted it in Mumbai

whenever a user makes a request to Amazon and select region as Mumbai and I say I want one virtual machine ( in AWS terms its called as EC2), your request would be sent to one of these datacenters and on one of these Physcal server. On this Physical server, there is Hypervisor installed. This Hypervisor will create a VM for you and will share required details. This is how the world of VMs operate.


> So the world of VM operated on Hypervisor whether its Amazon , google , Microsoft they all have hypervisors installed on their Physical servers and whenver someone is requesting for virtual machine/EC2 instance ( in different Cloud platform , people call it in different ways) but if you take general terms of Virtual machine , what happens is this Hypervisor will give you a virtual machine from one of the Physical server depending upon the region you have selected.



![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/23.png)

So user request certain resource lets say 10GB RAM and 12 core processor from amazon choosing region which is near user lets say Mumbai DC ,so what AwS will do is it will receive the request, will look for the Physical server which ideal for my requirement  . So lets say there is one Server which has free resources of 1000 GB RAM and 1000 CPU cores and no one is using it. So Amazon will ask Hypervior on this server, to create virtual machine with user specification of 10GB RAM and 12 core processor and once created Amazon sends the user with Virtual Machine. So Amazon sends back IP address and all the information to user which is required by user to access Virtual machine. So user gets virtaul access to the machine and doesn't own the physical server of amazon. User only pays for the access of the virtual machine. So lets say a user pays 100$ for accessing virtual machine, it cannot go to amazon and ask the address of the physical server present in datacenter of Mumbai requesting he wants to make some change to the Physical machine. User only has CLI or virtual access of the machine using IP address and Key Value pair requested.

So if Amazon would not have come up with this concept, this 100 Physical servers would have been atmost used by 100 different users. So because there are only 100 Physical servers, it could be atmost used by 100 different users. But instead using concept of virtual machine and using concept Hypervisor, what amazon has done is instead of 100 people or 100 users, this virtual machines can be used by millions of users. That is where Hyvervisor and virtual machines have improved the efficiency and concept of servers. So this is new leap that came few years back and was not present 10-20 years back.  

































































































