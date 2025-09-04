> What is POD

--> We are moving from Docker from K8s . In K8s smallest unit of deployment is pod. In docker , you build a container and your deploy a container.In k8s also we will use these containers that is deployed in docker because end of the day if its K8s or docker, end goal is to deploy applications in containers. That is the concept of containerization but what K8s says is don't deploy your application/container as it is but deploy it as pod.
But the question is why in K8s we need to deploy container as pod and why can't we deploy it directly as container ?
So by definition pod is described as "how to run a container "

So in docker if you wanted to run a container, you would pass the command something like this as we saw in docker chapter

```
docker run -d <followed by name of image> -p ( for port mapping) -v (volume mount) --network (pass network details)
```

So in docker you pass all the above arguments in command line to run the container.

Where as in K8s , you will pass all these specifications in <b><u>pod.yaml file</u></b>

So in K8s you basically have a wrapper or a concept , that is similar to container but it abstracts the user defined commands in pod.yaml file

So in K8s instead of container , you deploy a pod. A pod can be a single container or multiple containers . ( We will see later why a pod can be multiple containers and what are its advantages but for now lets understand for single container)





























