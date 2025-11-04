## Python Application Workflow

-- As Devops Engineer you should have the knowledge of how python application workflow happens.

-- Whenever you work with Django application, the first thing you need to know is how to install Django application. To install Django, you first need to install python and then you can install Django using pip command.

-- Once you install Django using pip, you can simply run "install Django" to verify if your Django is imported or not. Once you install Django, important thing to understand is if Django application has Django-admin or not. So once you install Django application, you can also install "Django-admin" using pip

--django-admin is very similar to ansible-galaxy. So what "ansible-galaxy" does is ansible-galaxy will give you a skeleton of your ansible code. Similarly whenever you try to write micoservice using Django, so Django-admin can be used to create skeleton

- In our case name of the project is "Devops" . So we execute the command 

django-admin startproject Devops

So above command creates skeleton, and inside "devops" it creates skeleton except the "demo" folder . Now lets see what is inside the three folders which got created by skeleton using startproject. So once you executed the skeleton using startproject ,it created project for you and inside the project there is nothing related to your applications. So it creates your overall project configuration. So for example, if you go inside the devops folder, all the files are related to project configuration. Like "settings.py" has the entire information about what are all the IPs which can be whitelisted , what is the database you are going to connect. If you have any secret keys , any secure information , if you want to use any django middlewares or if you want to support any templates, any webserver gateway interface etc , all of these things are present as part of your "settings.py".

-So you can understand "settings.py" as a settings python file which will set the entire configurations for your django project.

- After that you have "urls.py" which is responsible for serving the content . So here if you see url IP address "100.25.154.10:8000/demo" .So what is this "/demo" ? So, "/demo" is the context root of your application .So the "urls.py" , will understand that anybody who tries to hit the "/admin" or anybody who tries to hit "/demo" , it will serve the application of "demo" application. 

- So now what is this demo application ?
    - So overall using the django-admin start project , you just created a project . There is no application which you created. So you can understand that you have created base for your application. After that you have to create your applications. So to create your application, there is again one simple command called as 
     
    - python manage.py startapp demo

- Again executing above command , creates bunch of files for you. So this is how easy to write web application in python . So every Devops Engineer should have some basic knowledge of web development. So executing above command created a bunch of files except "templates" which we will understand in sometime why we created it. But apart from it created a bunch of files . Inside "views.py", you will write your actual code. In this example , we wrote very simple python code inside  "views.py" which is called as index. What this is doing is , it is rendering html file. This html file you have to place in the folder called "templates" and from templates your content gets served.

So this is the overall workflow of Django application.

So if somebody wants to do something on their own, before the existance of docker ,they used to download the corresponding github repo or download the artifact you created, and then what they used to do was that they would create all the dependencies that you have in "requirements.txt" and install all of them. After that the actual problem starts. So the QA Engineer might be on windows, the QA Engineer might be on MAC OS , QA Engineer can be on specific distribution of linux where these commands won't work. So that was one of the classic problems, system engineers or devops engineers use to face . The Developer says that the application is working fine but the QA says no the application doesn't work. So no one is sure if the mistake is on CI/CD pipeline or how Developer created application or if its an issue with QA Engineer , you won't be able to figure out. So this problem is again solved by Dcoker itself because in container we are bundling and packaging everything for your application to run. So anybody who wants to run this application in our case Django application, they don't have to execute even one single command other than "docker build " and "docker run" . so those are the only two commands they have to run and only need docker installed on their machines. If they have docker installed, it is irrelavant to you whether they are using Windows machine, linux , mac because docker itself runs on a virtual machine or on a bare metal ( i.e Physical server) and it consumes the resources from the host operating system other than the minimalistic dependencies that it has inside the container. So it can be windows or it can be any operating system because it has the system dependencies in it which are required to run your application .So lets say your application requires python , your application needs specific pip module , so everything will be part of your container itself along with source code or the binary of your application . Only if it requires some information like it has to consume something from kernel , or it has to do some system calls , only that thing it will do from host operating system , remaining every other thing it will do from container itself . So those are some of the classic problems that the container fixes.

So lets go and see roles and responsibilities of Devops Engineer

Lets say as Devops Engineer you get task to containerize django application. If you are not familiar with programming , you can sit with Devlopers and understand the workflow of application.

Now as Devops engineer I know that the first thing I need to do containerize this application , I need to start writing docker file.

In the docker file, first thing to do is select base image . So in this example application: 
https://github.com/iam-veeramalla/Docker-Zero-to-Hero/blob/main/examples/python-web-app/Dockerfile

We have chosen base image as ubuntu so first line is 

```
FROM ubuntu
```

--> Then we selected work directory. It is basically an identification on where your source code is going to save or you have multiple projects in your organization or you have multiple people in your team, and whenever you are writing this docker file , you can create as a standard saying we will always put the source code whenever we are containerizing the application in the folder called "/app" . So this is kind of a standard that you follow. Hence we use below command

```
WORKDIR
```


For the next line of code is where your devops Engineer experience comes into place ie if you have programing or developing experience, the first thing to copy inside your work directory is your "requirements.txt" file because this is the file where you have your python dependencies. So the dependencies that are required to run your application. In this example, it is "Django" and "tzdata" . So we will use below command
    
    `COPY requirements.txt /app`




After that , we will copy source code itself to working directory. So we will execute below command

```
COPY devops /app
```

So using dependency and source code you will form a bundle or binary of your application.

So in our code we have ubuntu base image , created working directory and copied source code and requirements in working directory. Now if we run this program, it will fail .This is because we have ubuntu base image and we haven't installed python. Probably we could have choose base image as python by typing "FROM python" , then in such cases you don't have to install python . However, for understanding we have chosen base image as ubuntu in this project. So we will use below code to install

```
RUN apt-get update && \
apt-get install -y python3 python3-pip && \
pip install --no-cache-dir -r requirements.txt && \
cd devops
```

Using above command w ehave installed python , pip as well as installed requirements.txt using pip

So all configurations is done now and my application is good to use. Now the final command I need to know as devops engineer is what should I put in ENTRYPOINT and what should I put in CMD. So lets understand the difference between ENTRYPOINT and CMD. So in docker , both ENTRYPOINT and CMD can be used to execute as your START command. so whenever someone runs the command "docker run ..." both ENTRYPOINT and CMD can serve as your starting command but the only difference is 
<b><u> ENTRYPOINT is something you cannot change. So they cannot override the valud of ENTRYPOINT in your docker image. Where as CMD is something which is configurable.</u></b>

```
ENTRYPOINT ["python3"]
CMD ["manage.py", "runserver", "0.0.0.0:8000"]
```
So manage.py , runserver , 0.0.0.0:8000 are all configurable fields which are passed as part of CMD. This can be changed as per your requirement.

However, the parameter passed in ENTRYPOINT cannot be changed. So user cannot change the executable itself of python3 and change it to GO language . This is not allowed.So anything defined in ENTRYPOINT is non-overridable values.

So since CMD is configurable if port 8000 is used by some other application in your org , you can change it to 8001.

However, if you want your user not to change anything , you can define all the parameters in ENTRYPOINT.

-- Now for application to run , you would need to first build the application. In order to do that, you have to run below command

```
docker build .
```

This will create docker image . You can see this image being created using below command

```
docker images
```

Now we will run this image and spin it up as container using below command

```
docker run -it <image ID obtained from docker images command>
```

[ Here -it means run container interactively]

Running this container and trying to access Django application on port 8000 won't work because you have only opened port 8000 on your container. In order to expose it on EC2 instance, you would also need to allow access /expose port 8000 on your EC2 instance for external IPs on corresponding security groups. 

Once you open port 8000 on your EC2 instance, you would also need to map container port 8000 with external EC2 instance port 8000 using below command

```
docker run -p 8000:8000 -it <docker image ID>
```








































































































































