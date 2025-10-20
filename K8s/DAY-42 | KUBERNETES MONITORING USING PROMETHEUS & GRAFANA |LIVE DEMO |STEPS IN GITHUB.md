In this class we will learn about why aspect like we need monitoring ?
What is advantage of monitoring ? What is prometheus ? What is Grafana ?

After that we will understand how to install all these tools .So the only pre-requisite about this class is you have to have a K8s cluster that can be anything like a real K8s cluster on prod or your Dev K8s like minikube , k3s , k3d anything is fine. We are going to learn about installation and then finally we will monitor and see how to monitor the minikube or any other dev k8s cluster using prometheus and visualizing using Grafana. We will prepare grafana dashboard that will show metrics of API server and the deployments that we have on our K8s cluster, what is the status , what are the replicas . We are going to fetch a lot of information from K8s cluster

First question is why monitoring is required ?

--> So lets say in your Organization , you have one K8s cluster. So when you have single K8s cluster , thats not a problem becuase for just one single K8 cluster probably because you are single Devops Engineer, what you can do is monitor your own K8s Cluster. But what happens if this one K8s cluster is used by multiple teams. Lets say you have multiple teams who are accessing this cluster, and one of the teams says that in K8s cluster something is going wrong. They will say that the deployment is not receiving the request or service is not accessible for short while . So how you will you solve this problem? Or atleast how would you understand the issue as Devops Engineer ?

Now this is just one K8s cluster , as the number of K8s cluster increases , probable you have Dev env, you have staging env , production env , so your K8s clusters keeps growing. So as the K8s clusters keeps increasing, then you would definitely need observability or monitoring platform . So that is where K8s comes into picture. So Prometheus was initially developed by Sound Cloud , then it is open sourced right , so right now Prometheus is completely open sourced platform and anybody can use this prometheus , on their clusters like even if you are running K8s Cluster behind or you are running K8s cluster in your Enterprise , you can use Prometheus because its open sourced one.

So if you have prometheus then what is the requirement of Grafana?

So Grafana is basically for visualization. So promtheus can give you lot of information using the query , like you can use the queries and you can get all the information regarding your K8s cluster but for better visualization, you would understand through live demo or through practical regarding why you need grafana

Grafana can use any data sources and prometheus can be used as one of the data source

So what is architecture of prometheus . So sometimes an interviewer can ask you this question. You can explain as per below diagram


![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/prometheus-architecture.gif)


When you install prometheus , there is a component in prometheus , called prometheus server.<b><u> Prometheus server has HTTP Server, and prometheus collects all the information like from your K8s cluster</u></b>. By default K8s has API server and this API server exposes a lot of metrics about your K8s cluster. So maybe 5 years or 6 years ago you might had to do a lot of configuration, but right now these tools are very matured and even they have contributed back to K8s so a lot of metrics are exposed out of your box in your K8s cluster. Previously, you may had to do a lot of configuration for your K8s but right now the number of configurations have reduced significantly. So K8s have an inbuild API server, this inbuilt API server exposed a lot of matrix. So it says if you access me at                   <API server IP>/matrix , you can get all the information about what are all the resources in your K8s cluster. Now <b><u>prometheus will try to fetch this information and it stores this entire information in a time series data base</u></b>. So what is time series data base ? It is just like with respect to timestamp it stores the information of the matrix of your K8s cluster. So this is about default K8s resources . But what if you want to do more resource or you want to get beyond the default or out of the box matrix which K8s API server is using? That also we are going to learn today.
    

Then, <b><u>it stores all of these things in node disk that is HDD/SSD whatever you are using. Since it is time series database , any database has to store information somewhere . So it stores this information on node disk</u></b>. Then <b><u>it has a monitoring system like you know you can configure Prometheus with Alert Manager and you can send notifications to different platforms like slack, pagerduty , send it via email, send to various things</u></b>. So what happens under the hood is if you create the alert manager, So prometheus can push alert to the alert manager and you can configure this alert manager to send out notifications to different places. Probably, in my Organization lets say I have decided to use slack for alerting so what happens is whenever prometheus identifies like you can say what kind of metrics or what kind of alerts have to be pushed . Lets say I configure API server is not responding according to the limit I set then what you can say is Prometheus , send an alert to alertmanager. So prometheus will send an alert to alert manager saying that K8s API server is showing unstable behavior. So this alertmanager depending upon the number of things you have configured like slack, emails etc the alert manager will send notification to selected setting configured. So thats what alert manager does. And apart from that , like this is about the default configuration but somebody can also go to prometheus server and prometheus provide very good UI so you can also go to the prometheus UI and you can execute some PromQl queries. PromQl queries is nothing but Prometheus queries. So you can execute some prometheus queries , to get the information from prometheus whatever it has recorded or you can use dashboard such as grafana or like any other tool like AWS supports API ,so prometheus also supports API and you can use some curl command or postman and you can get that information from prometheus as well.
    
So this is high level architecture of Prometheus. So as we keep learning prometheus this architecture will look even more simple
    

Now like I told you why Grafana??
    
    
So grafana is just for data visualization.
    
   So when you do query to prometheus ,<b><u>it gives output in a visualized format </b></u> for ex: lets say it is giving output in json format and if you want to setup Dashboards in your organizations somewhere so everybody can monitor, json or any kind of sucn templating languages are difficult to read , so if you have lot of information , it is very easy to represent information in charts or any kind of diagrams so thats what Grafana does for you. So it provides very good visualization. It retrieves information from prometheus , you can configure prometheus as data source and you can get information into Grafana and inside Grafana, what you can do is create some nice diagrams like charts for visualization.

We will now look at the demo 
    
--> In demo , the good thing which is explained is "prometheus-kube-state-metrics"
    
K8s API server, it exposes few metrics of your K8s. So it gives you information about K8s API server , it gives you information about the default intallations on your K8s cluster ( like kubeAPIserver, coredns, kube-controller etc) but as your monitoring your k8s clusters , you might need more information , probable like you might need information about all the deployments , all the pods , all the services on your K8s cluster , you might want to know if replica count is matching the expected replica count of all the K8s cluster. So what the K8s community or wht people at kube-state-metrics have done is they have created a kube-state-metrics controller , so you can create service for this kube-state-metrics and you can use this kube-state-metrics to call the kube-state-metrics on the metrics endpoint ,so it will give you a lot of information about your existing K8s cluster. So this information is beyond the information your K8s cluster API server is providing. So this is the importance of "kube-state-metrics" K8s controller.
    
    
![](https://raw.githubusercontent.com/SamilVanzar/Devops/main/images/Day30/16.png)    
    

    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    























