<!-- Code for k8s dashboards -->

### Dashboard Setup via BizOps Configurator

We will be using automatically creating Kubernetes Dashboards via [BizOps Configurator](https://dynatrace.github.io/BizOpsConfigurator/)

Setup the token with the **permissions** as per below

- **Access problem and event feed, metrics and topology**
- **Read configuration**
- **Write configuration**
- **User sessions**

![K8S-Dashboards](../assets/k8s/dashboard-token.png)

Follow the preloaded setup at <button>
[BizOps Configurator](https://dynatrace.github.io/BizOpsConfigurator/#deploy/persona/Ops/Platform%20Overview/K8s%20Overview)
</button>

- Enter in your **tenantUrl** and **API-Token**
- Click **Next**. <em>(The persona, usecase and workflow are already selected for you.)</em>
- Click **Done**.

![K8S-Dashboards](../assets/k8s/k8s-dashboards.gif)

Positive
: ⚠️ In order for the entities to be filtered properly, you'll need to add a tag **[Kubernetes]namespace** to your services/pods, so this could be filtered properly in the Performance Engineering Dashboard. This will be later OOTB in the product.

Positive
: ⚠️ Works with Dynatrace 196+

### Kubernetes Overview
![#](../assets/k8s/overview.png)


### Cluster utilization
_____________________
See the Kubernetes cluster utilization. CPU and Memory Request and limits over time for all nodes and splitted by namespaces.

![#](../assets/k8s/cluster-utilization.png)


### Resource Quotas
_____________________
Get an overview and understanding of the Kubernetes resource quotas (Memory and CPU) assigned to your namespaces and its usage. 
![#](../assets/k8s/quotas.png)

### Container usage & health
_____________________
Understand the health and phases of your Pods in your clusters. Their memory and cpu usage, which pods are throttled, have failed or are pending to be scheduled. Also check if you have Out-of-memory killed containers.

![#](../assets/k8s/containers.png)


### Performance Engineering
_____________________
Give your developers and SRE engineers all they need to understand and improve the performance of each app, pod and each transaction on your clusters. View the response time percentiles, slow transactions, database executions per microservice, its network usage and more. Filter the transactions by App label, namespace and much more.  

![#](../assets/k8s/performanceeng.png)

### User Experience
_____________________
Are your endusers satisfied? how is the engagement, experience and user behaviour of your applications? Get the insights of all your applications and users in an instance.

![#](../assets/k8s/userexperience.png)
