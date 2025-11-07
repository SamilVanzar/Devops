# Multi-Stage Docker Builds and Distroless Images

Before understanding Multi-Stage Docker builds lets try to understand what would you need to create a simple calculator application using python. Now what will be your steps lets understand . So you will write docker file as we checked in last class

```
FROM Ubuntu
WORKDIR /app
RUN apt-get update && \
apt-get install -y python3 python3-pip && \
pip install --no-cache-dir -r requirements.txt && \

CMD [ ( configurations you need to run calculator app)]
```
So this will be very good docker file that you would have written. But there is a problem with this docker file which you need to understand. You only need Python run time to execute this application. Since this is python application, you only need python run time to execute this application. If it was java application, you would only need java runtime. But if you look at the docker image that you have created, it has 100 other things . It has ubuntu base image. This ubuntu base image comes with a lot of ubuntu system dependencies ,it will come with apt packages , it will come with apt repositories. It has a lot of overload on your simple docker image that you created . End of the day , you want to run python application and this python application only needs python runtime and not even the application that you have installed as part of "apt-get install or apt-get update". <b><u>The "apt-get and apt-install" are only required to build your application</u></b>.

So these are two different stages, build is different stage and running your application is different . To build your application, lets say you are building a java application, so to build your java application you might need 100 of java libraries  or you might need jar files for building java applications . But to execute your java application, what you require is "jre runtime" + "java binary" .So those are the only things you need.


So the question that comes is why you need a base image which in our case is ubuntu which comes with so much overhead .So to justify it wou would say that I need ubuntu base image because I want to install all of the python dependencies and its very easy to install them using ubuntu .

So to solve this problem what docker said is , let me introduce new concept and this concept is called as "multi-stage builds". What you will do as part of multi-stage build is you will split your docker file into two parts ( it can be multiple parts but we will first understand two parts).

Out of two parts, in the first part what we will do is the same thing as we did while writing normal docker image i.e.

```
FROM ubuntu as build
WORKDIR /app
RUN apt-get update && \
apt-get install -y python3 python3-pip && \
pip install --no-cache-dir -r requirements.txt && \
```

So we will not add CMD as well as ENTRYPOINT in the first part. Instead what we will do is <b><u>we will take the binary from the execution of first part</u></b> . Also we will name first stage with alias name as "build" as written in first line.

So we will take the binary from the first part of docker file or dockerimage and use it in the second part of docker file . 

So both first and second image will be written as one single docker file <u><b>BUT IT WILL HAVE TWO FROM STATEMENTS</b></u>


So in the second stage , the FROM statement would be very minimal image. So it can be an image which can just have python runtime or it can be an image which can just have java runtime. So you can find this run time images easily . Also in this step 2 , you can choose distroless images. (We will learn distroless image soon)

But here you need to understand is in last lecture you used 

```
FROM Ubuntu
```
Because its very easy to install packages or its very easy to get all the dependencies that are required for building your application. For ex : Ubuntu image by default has curl . it by default has wget and a lot of other things that are needed to build your application. But the python runtime or java runtime which you installed in second stage are specifically python images and doesn't necessarily have curl. So you don't install curl because curl was only required during your build process in stage 1. During your application execution stage, you don't require curl because you only have to run. So this specific stage 2 will only have FROM python runtime environment plus you will copy artifact binary generated in stage 1 to stage 2 i.e. you will use command COPY --from build . And then execute CMD or ENTRYPOINT. So second stage commands would be

```
FROM python
copy --From build
CMD [/app]
```

So the first stage will not have ENTRYPOINT or CMD but second stage will have it.

![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/28.png)

So the advantage of this is you have reduced your image size significantly. All the content which is in part 1 will not be in your final image. so every command mentioned in part 1 will be part of your build image and second part will be your final image. So if you look at your final image, it will have only python runtime , it will only have binary you have built in stage 1 and just the executable.


Lets take one more example that would explain bit more about the advantage of multistage build image. Lets take some complicated example which has front end , back end and some DB related stuff. In such cases what you will do is you will effectively choose one base image first. Then install all of the packages which are required for your 3 tier application . Lets say front end is React , backend is Java Springboot, DB is MySQl . So in this docker image you have to install all of these things. Firstly you install Ubuntu base image . Now question might be why don't you directly install Java base image ?
The answer is if you install java as your base image, you would need to install a lot of things or lot of dependencies and docker image will become very complicated. This were the steps Devops Engineer used to do years ago . But now with Multistage docker image , they choose very rich base image like FROM Ubuntu. With rich base image, we meant is image which has all the dependencies. You are not worried if the base image is 1GB or 2GB. The reason being base image would be removed and final image that you are creating will only have the last stage that is the final stage

-- Lets say if you are not using multistage docker. Then firstly you would start with "FROM Ubuntu" then you will install the dependencies regarding Java, then you will install all the dependencies related to react. You will install all dependencies regarding MYSql. Then you will build your Java Application . Then you will build your frontend . Then as part of your entrypoint, you will execute a combined application like for example
"/app.ear". So this will be the output which you will execute. The problem here would be that this docker image size will go crazy like 1.5 to 2 GB. That huge size would be because the base image "ubuntu" it self would be around 400MB. On top of that you have installed java which could be around 50MB. On top of it you installed react , which would be around 100MB . On top of it you installed MySql which could be around 100 MB. So all of this images piled up on base image which caused it to reach 1 GB.

![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/31.png)

Using this multi-stage docker concept, you can avoid this problem because in final stage what we will only do is because final stage is only java runtime environment , we will say

```
FROM openjdk:11 (or any latest version)
COPY --from build (in stage 1 you create an alias like FROM Ubuntu as build)
ENTRYPOINT [app.ear]
```
So what this final images says is it has openjdk java distroless is 100 MB and then the COPY --from .... is just a binay getting copied so it will be just around 50MB . So your final image size would only be 150 MB. Thus using multi-stage docker you reduced image size from 1GB to 150MB. This is the advantage of multistage docker build


In above example, we saw two staged in multistage docker file but there can be multiple stages as well. For ex: In first stage , you might only be focused on building your frontend , in second stage you might only be focused on building your backend and in the final stage you will only get binaries of dependencies from frontend stage , backend stage and as part of your cmd you can just execute it. So if someone asks how many stages can be in Multistage docker build , the answer would be countless. So you can create N number of stages but there will be only one final stage which will be a minimalistic image. This minimalistic image will be lower in size which is advantage of multistage docker build.


Now we will learn about distroless images. Distroless image is basically a very minimalistic image that will hardly have any packages. For ex : If you choose a python distroless image (You can easily find distroless images), this python distroless image will only have python run-time. Like in the previous example, we chose a openjdk distroless image which was 100 MB because it only has openjdk and nothing else. Now there can be another kind of distroless images that will not even have python runtime , for example if you take example of golang application. Golang application is a statically typed application. So this statically typed applications, they don't even require runtimes . So you don't require Go runtime for executing Go application because of which you will reduce your docker image to as small as like 10-15 MB . So we will see practical example as well which will be based on Go Lang language because we will see advantage of distroless images + Multistage Docker builds.

So what is Distroless image ?
-- A distroless image is very minimalistic or lightweight docker image that will only have the run time environments. 

So if you are choosing distroless image like python distroless image, so even if you try to perform some basic shell commands like for example "find", "curl" , "wget" , "ls" all these commands will fail telling executable not found because the main idea is just to have final build or environment is to only have python runtime and the only purpose of this environment is to execute "/app" or python application . So one biggest advantage you get from distroless image is <b><u>SECURITY</u></b> along with size


Other then reducing container size, you shall have highest security with distroless images . So this can be answered in interview as well . So in normal or multistage docker images where you use normal ubuntu base image or in the final stage were using python runtime images which had vulnerabilities which could be exploited by hackers. So moving to distroless images like python distroless image which only had python runtime which wouldn't have even have basic packages like "find,"wget" , "ls" ,"curl" and since packages are not present, there wouldn't be related vulnerabilities thus providing highest level of security. Also if you use static images like Go Lang, you have more security then distroless images because it doesn't even require runtime. So this distroless images are like 99% of time not exposed to vulnerabilities.

Example of Multistage Docker : 
https://github.com/iam-veeramalla/Docker-Zero-to-Hero/tree/main/examples/golang-multi-stage-docker-build

Example of Distroless image:
https://github.com/GoogleContainerTools/distroless/tree/main/java










































































