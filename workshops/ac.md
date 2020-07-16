summary: Autonomous Cloud with Dynatrace
id: autonomous-cloud
categories: Workshops
tags: dynatrace
status: Published 
authors: Brandon Neo
Feedback Link: https://thebrandneo.com

# Autonomous Cloud with Dynatrace
<!-- ------------------------ -->
## Introduction 
Duration: 1

This repository contains labs for the Hands-On Kubernetes Session. We will be using Google Kubernetes Engine (GKE) for this hands-on but this will work on other platforms as well.

For the purposes of the Hands-On, we will automate and make the steps seamless for the participants

### Prerequisites
- Dynatrace SaaS/Managed Account. Get your free SaaS trial here.
- Google Cloud account with access to GKE. Get your free trial access here.
- Chrome Browser

### What You’ll Learn 
- Deploying Dynatrace Operator via Helm Chart on Kubernetes
- Setup Kubernetes integration on Dynatrace
- Enabling early access feature flags in Dynatrace
- Discover Kubernetes View on Dynatace

<!-- ------------------------ -->
## Setting up your Google Kubernetes Environment (GKE)
Duration: 5

### Sign up for a Google Cloud Platform Account

Head over to https://cloud.google.com/free/ and sign up for a free GCP account with your existing Google account. 

You can also signup for a new Google Account if you don't have one

Upon signup, you will have free credits tied to your GCP account. (12 months + 400AUD)

You can login to your GCP console [here](https://console.cloud.google.com/home/).

![GCP-Homepage](assets/Picture1.png)

### Enable Kubernetes Engine API 

You will also need to <b>Enable your API Billing</b> with Kubernetes Engine API. 

![k8s-Engine](assets/Picture3.png)

You should be prompted to the billing page while setting up your GKE instance. 

If not, you can follow the steps [here](https://support.google.com/googleapi/answer/6158867?hl=en)

### Activate Cloud Shell

![GKE-Menu](assets/Picture4.png)

Click on the Terminal Icon on the top right

A Cloud based Terminal lookalike will appear at the bottom of the page

We will start setting up our GKE Cluster 

### 3. Create your GKE Cluster

![GKE-CLI](assets/Picture5.png)

Create your GKE cluster named <b>k8sworkshop</b> running Ubuntu in GKE with the following command.
We will also be creating a compute VM for a Dynatrace Activegate. We will use the Dynatrace Activegate for Kubernetes integration.

```bash
gcloud container clusters create k8sworkshop --image-type=ubuntu --zone australia-southeast1-a
gcloud compute instances create dynatrace-activegate --image-family ubuntu-1604-lts --image-project ubuntu-os-cloud --zone australia-southeast1-a
```

Once completed, you will have a running GKE Cluster!

![GKE-CLI](assets/gcp.png)
Running <b>kubectl get nodes</b> will reveal number of nodes 

<!-- ------------------------ -->
## Setting up your Kubernetes Integration
Duration: 5

As per the official instructions [here](https://www.dynatrace.com/support/help/technology-support/cloud-platforms/kubernetes/monitoring/connect-kubernetes-clusters-to-dynatrace/) for the Kubernetes integration, you will need to setup an Environment Activegate first.

### SSH into Dynatrace-Activegate terminal and install Activegate


1. On the left navigation bar in Google Cloud, go to <b>Compute Engine</b> -> <b>VM instances</b>
![Activegate-connected](assets/activegate-0.png)

2. Click on the SSH button on the <b>dynatrace-activegate</b> row and SSH into the instance
![Activegate-connected](assets/activegate.png)

2. Within Dynatrace, click on Deploy Dynatrace on the left menu
3. Click on "Install Activegate" at the bottom of the page
4. Click on "Linux"
5. Copy Step 2 and paste into your terminal.
6. Copy Step 4 and append "sudo" (installing as root) onto terminal
![Copy-AG-Commands](assets/activegate-2.png)

Once completed, you should see Activegate under Deployment Status.

![Activegate-connected](assets/Picture9.1.png)

### Setup the K8S Overview Dashboard

Go to Settings -> Process and Containers -> Process group detection -> Enable Cloud Application and workload detection

![Enable Cloud Workload](assets/enablecloud.png)

Automating the steps from our offical [documentation page](https://www.dynatrace.com/support/help/technology-support/cloud-platforms/kubernetes/installation-and-operation/further-integrations/connect-your-kubernetes-clusters-to-dynatrace/), we provided the API URL and bearer token automatically via API. Back in your main Cloud Shell terminal, enter the below

``` bash
wget -O- https://raw.githubusercontent.com/Dynatrace-APAC/Workshop-Kubernetes/master/setup-k8s-ui.sh | bash
```
With the above results, enter the values to <b>Settings</b> -> <b>Cloud and Virtualization</b> -> <b>Kubernetes</b>

![K8S-integration](assets/activegate-4.png)
1. Give a name for the connection eg. GKE K8S
2. Enter in your Kubernetes API URL Target 
   - Copy the Kubernetes API URL from the SSH terminal
3. Enter in the Kubernetes Bearer Token
   - Copy the Bearer Token from the SSH terminal
4. Disable "Require valid certificates for communication with API server"
5. Add another event field selector
6. User the below for the field selector name
``` bash
Hipster shop 
```
7. User the below for the Field selector expression
``` bash
metadata.namespace=hipster-shop
```
8. Save and Click on Connect

Once successfully connected, click on Kubernetes on the left menu and explore the Kubernetes UI. 

![K8S-integration](assets/k8s.png)
<!-- ------------------------ -->
## Install Dynatrace OneAgent Operator for Kubernetes
Duration: 5

1. On your Google Cloud Console, on the left navigational bar, go to Kubernetes Engine -> Applications
2. Click on "Deploy From Marketplace"
3. Search for Dynatrace in the search field above
![Activegate-connected](assets/operator.png)
4. Click on Dynatrace OneAgent Operator and click on Configure
5. Fill in the following fields<br>
- API URL <br>
Copy your Dynatrace URL and append <b>"/api"</b> to the end<br>
![API-URL](assets/operator-1-withURL.png)

- API Token <br>
Create one from Settings -> Integration -> Dynatrace API
  - Enable Access problem and event feed, metrics, and topology toggle
  - Enable Write Configuration toggle (needed for Activegate setup for the next step)<br>
![API-Token](assets/api-token.png)

- PaaS token <br>
Create one from Settings -> Integration -> Platform as a Service
![PaaS-Token](assets/paas-token.png)

Copy the values into your GCP console
![Activegate-connected](assets/operator-1.png)

6. Click on Deploy<br>
![Activegate-connected](assets/operator-2.png)<br>

Once completed, you can click on Hosts on the left panel to see your connected K8S nodes (3 nodes)  

![GKE-Hosts](assets/Picture7.1.png)
<!-- ------------------------ -->
## Setting up Hipster Shop
Duration: 5

For our Hands-On, you will need to run <a href="https://github.com/GoogleCloudPlatform/microservices-demo">Hipster Shop</a> which is a Google sample application.

### Run the Hipster Shop

```bash
wget -O- https://raw.githubusercontent.com/Dynatrace-APAC/Workshop-Kubernetes/master/deploy.sh | bash
```

Once deployed, you can locate the front-end endpoint from GCP (<b>Kubernetes Engine -> Services & Ingress</b>)

![JSON](assets/Picture10.png)

Once running, you can go to the exposed frontend-external IP to go to Hipster Shop.

![JSON](assets/hipstershop.png)

<!-- ------------------------ -->
## Exploring Dynatrace
Duration: 5

### Automatic Discovery of services

In Dynatrace, go to Transactions and Services to see the automatic 5 discovered services.
![Discovered Services](assets/lab5-autodiscoveredservices.png)

You will realized that some services are discovered but some might not match <a href="https://github.com/GoogleCloudPlatform/microservices-demo#service-architecture">Hipster Shop's Service architecture</a>.
Hipster Shop uses cutting edge technologies (such as GPRC) which Dynatrace supports with the constant evolution in the cloud.

![Architecture](assets/architecture-diagram.png)

### Enabling additional features within OneAgent

Because of the rapid rate of change coming to OneAgent, features that are in Early Access aren't automatically enabled by default. 
This is to prevent unforseen circumstances which might impact your production environments. For the purposes of workshop, we can enable these features. Go to <b>Settings -> Service-side service monitoring -> Deep Monitoring -> New Oneagent Features</b>

Under Global Settings, enable the following feature flags. They are on different pages so you would need to toggle through the pages.

You can use the search filter bar to search for <b>"GRPC"</b>
![GRPC-Features](assets/lab5-b4EnableGRPC-settings.png )

![Features](assets/features.png)

Make sure all the below features all enabled, including the 2 additional NodeJS feature flags.
![All-Features](assets/all-features.png)

Enabling OneAgents features requires a restart of the pods. Run the following command to restart the pods.

```bash
kubectl delete pods --all -n hipster-shop
```
![Restart](assets/restart.png)

Back in Dynatrace, go to and Transactions and Services to see the updated list of services.
![Discovered Services](assets/lab5-AfterEnableGRPC-settings.png)

Clicking on Go Service ":8080" followed by Service Flow, you can see that the service are automatically detected and matches the architecture diagram above.

![Service Flow](assets/serviceflow.png)

<!-- ------------------------ -->
## Exploring Kubernetes View
Duration: 5

Explore the various functionalities within the Kubernetes View such as Cluster Utilization, Cluster Workloads, K8S Events

![KubernetesUI](assets/k8s-ui.png)

### Analyze the Kubernetes Cluster utilization
   -  Mouseover and note the CPU and Memory usage with the Min / Max
   -  Click on Analyze Nodes to drill deeper into each node
   
![KubernetesUI](assets/cluster-util.png)

### Analyze the Kubernetes Cluster Workloads 
   -  Notice the Workloads and Pods running spilt between Kubernetes controllers

![KubernetesUI](assets/cluster-workload.png)

### Analyze the Kubernetes Events
   -  Notice the different types of events BackOff, Unhealthy

![KubernetesUI](assets/events.png)

### Analyze the Kubernetes Namespace
   -  Click on <b>hipster-shop</b> and drill down into various kubernetes services (Cloud applications)

![KubernetesUI](assets/namespace.png)

### Explore Cloud Applications by clicking onto them
   - Click onto each of them and discover their supporting technologies
   
![KubernetesUI](assets/cloud-apps.png)

