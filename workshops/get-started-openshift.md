id: get-started-openshift
summary: Get started with Redhat Openshift and getting full observability with Dynatrace
categories: get-started, openshift
tags: microlab 
status: Published 
authors: Brandon Neo
Feedback Link: mailto:APAC-SE-Central@dynatrace.com
Analytics Account: UA-175467274-1

# Get Started with Openshift
<!-- ------------------------ -->
## Introduction

This section contains the simple steps required to get started with Redhat Openshift. 

Positive
: If you need to use a **demo application**, you can use Dynatrace's sample app [easyTravel](https://community.dynatrace.com/community/display/DL/easyTravel)

### What You’ll Learn
- Setting up Openshift
- Deploy Dynatrace Operator
- Discover and Explore Dynatrace

<!-- ------------------------ -->
## Setting up Openshift 
Duration: 15

### Choosing your deployment approach

There are various ways to setting up a [free Red Hat Openshift trial](https://www.openshift.com/try). You can also find other DIY apporaches on **Github via Terraform / Ansible**. The various approaches to creating the Openshift clusters are provided as **one-click options** with the [Redhat Hybrid Cloud Console](https://cloud.redhat.com/)

* Go to the [Redhat Hybrid Cloud Console](https://cloud.redhat.com/).
    * You can login with an existing account or create a new account

![Deploy](assets/get-started-openshift/redhat-login.png)

* Redhat Openshift could be created on the Cloud as a **Managed service (eg. AWS, Azure, IBM Cloud)** but also as **run-it-yourself** options

![Deploy](assets/get-started-openshift/redhat-options.png)

* For the purposes on this lab, we will be running the setup with **Red Hat Dedicated Trial**

<!-- ------------------------ -->
## Deploy Dynatrace Operator
Duration: 15

We will now log into the Cluster and deploy Dynatrace OneAgent. Dynatrace supports various Openshift deployment approaches with the various rollouts. There are different ways to activate Dynatrace on OpenShift and each way has its own advantages. 
We recommend these deployment strategies in terms of feature completeness and lack of constraints.
Our [documentation link](https://www.dynatrace.com/support/help/technology-support/container-platforms/openshift/openshift-deployment-overview/) contains the various approaches.

### Download the OneAgent

Open your browser and access the Dynatrace URL.

Follow these steps below:

* Select **Dynatrace Hub** from the navigation menu.
* Select **Openshift**
* Select **Monitor Openshift** button from the bottom right.  

![Deploy](assets/get-started-openshift/hub-openshift.gif)

Within the **Monitor Kubernetes / Openshift** page, follow these steps below:

* Enter a **Name** for the connection Eg. `openshift`
* Click on **Create tokens** to create PaaS and API tokens with appropriate permissions
* **Toggle ON** Skip SSL Certificate Check
* Click **Copy** button to copy the commands. 
* **Paste** the command into your terminal window and execute it.

![Deploy](assets/get-started-openshift/deploy-openshift.gif)

Example:

```bash
Connecting to github-releases.githubusercontent.com (github-releases.githubusercontent.com)|185.199.108.154|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 7310 (7.1K) [application/octet-stream]
Saving to: ‘install.sh’

install.sh                      100%[=====================================================>]   7.14K  --.-KB/s    in 0s      

2021-06-01 05:46:36 (40.7 MB/s) - ‘install.sh’ saved [7310/7310]


Check for token scopes...

Check if cluster already exists...

Creating Dynatrace namespace...

Applying Dynatrace Operator...
Warning: apiextensions.k8s.io/v1beta1 CustomResourceDefinition is deprecated in v1.16+, unavailable in v1.22+; use apiextensions.k8s.io/v1 CustomResourceDefinition
customresourcedefinition.apiextensions.k8s.io/dynakubes.dynatrace.com created
serviceaccount/dynatrace-dynakube-oneagent created
serviceaccount/dynatrace-dynakube-oneagent-unprivileged created
serviceaccount/dynatrace-kubernetes-monitoring created
serviceaccount/dynatrace-operator created
serviceaccount/dynatrace-routing created
podsecuritypolicy.policy/dynatrace-dynakube-oneagent created
podsecuritypolicy.policy/dynatrace-dynakube-oneagent-unprivileged created
podsecuritypolicy.policy/dynatrace-kubernetes-monitoring created
podsecuritypolicy.policy/dynatrace-operator created
podsecuritypolicy.policy/dynatrace-routing created
role.rbac.authorization.k8s.io/dynatrace-dynakube-oneagent created
role.rbac.authorization.k8s.io/dynatrace-dynakube-oneagent-unprivileged created
role.rbac.authorization.k8s.io/dynatrace-kubernetes-monitoring created
role.rbac.authorization.k8s.io/dynatrace-operator created
role.rbac.authorization.k8s.io/dynatrace-routing created
clusterrole.rbac.authorization.k8s.io/dynatrace-kubernetes-monitoring created
clusterrole.rbac.authorization.k8s.io/dynatrace-operator created
rolebinding.rbac.authorization.k8s.io/dynatrace-dynakube-oneagent created
rolebinding.rbac.authorization.k8s.io/dynatrace-dynakube-oneagent-unprivileged created
rolebinding.rbac.authorization.k8s.io/dynatrace-kubernetes-monitoring created
rolebinding.rbac.authorization.k8s.io/dynatrace-operator created
rolebinding.rbac.authorization.k8s.io/dynatrace-routing created
clusterrolebinding.rbac.authorization.k8s.io/dynatrace-kubernetes-monitoring created
clusterrolebinding.rbac.authorization.k8s.io/dynatrace-operator created
deployment.apps/dynatrace-operator created
W0601 05:46:39.025776   29593 helpers.go:553] --dry-run is deprecated and can be replaced with --dry-run=client.
secret/dynakube configured

Applying DynaKube CustomResource...
dynakube.dynatrace.com/dynakube created

Adding cluster to Dynatrace...
Kubernetes monitoring successfully setup.
$

```

Negative
: Note that it will take about 5 mins for data to appear within Dynatrace

Positive
: Dynatrace handles automatic deployment of OneAgents as well as automatic k8s integration. 

### Validate the installation in Deployment status

Click on **Show deployment status** to check the status of the connected host. 

You should be able to see a connected host as per the image below.

![Deploy](assets/dem/download-deployment-status-1.png)

<!-- ------------------------ -->
## Extending Observability

Dynatrace allows multiple ways to extend the observability of your environment.

### Setup etcd for Openshift

To get deeper insights into your self-managed OpenShift control plane, you can also use an extension to **extract the etcd metrics** exposed on your cluster.

* Select **Dynatrace Hub** from the navigation menu.
* Type **etcd** within the filter
* Select **etcd for Openshift**
* Select **Add to environment** button from the bottom right. 

![Deploy](assets/get-started-openshift/dynatrace-hub.png)

### Prometheus

You can also collect metrics from **Prometheus exporters in Kubernetes** to analyze and alert on them with Dynatrace.

* Select **Dynatrace Hub** from the navigation menu.
* Type **Prometheus** within the filter
* Select **Prometheus**
* Select **Activate Prometheus** button from the bottom right. 

<!-- ------------------------ -->
## Advanced Use Cases

Dynatrace is an all-in-one platform that's purpose-built for a wide range of use cases.

![Deploy](assets/get-started/dynatrace/all-in-one-platform.png)

**Infrastructure Monitoring** 
- Dynatrace delivers simplified, automated infrastructure monitoring that provides broad visibility across your hosts, VMs, containers, network, events, and logs. Dynatrace continuously auto-discovers your dynamic environment and pulls infrastructure metrics into our Davis® AI engine, so you can consolidate tools and cut MTTI.

**Applications and Microservices** 
- Dynatrace provides automated, code-level visibility and root-cause answers for applications that span complex enterprise cloud environments. Dynatrace automatically captures timing and code-level context for transactions across every tier. It also detects and monitors microservices automatically across the entire hybrid cloud, from mobile to mainframe.

**Digital Experience Monitoring (DEM)** 
- Dynatrace DEM provides Real User Monitoring (RUM) for every one of your customer's journeys, synthetic monitoring across a global network, and 4K movie-like Session Replay. This powerful combination helps you optimize your applications, improve user experience, and provide superior support across all digital channels.

**Digital Business Analytics**
- By tying business metrics and KPIs to data that's already flowing through our application performance and digital experience modules, you get real-time, AI-powered answers to your critical business questions.

**Cloud Automation**
- Dynatrace AIOps gives you precise answers automatically. Dynatrace collects high-fidelity data and maps dependencies in real-time so that the Dynatrace explainable AI engine, Davis®, can show you the precise root causes of problems or anomalies, enabling quick auto-remediation and intelligent cloud orchestration.

The above use cases are setup as labs which you can run through:
* [Digital Experience Management with Dynatrace](/workshops/dem)
* [Advanced Observability with Dynatrace](/workshops/advanced-observability)
* [BizOps with Dynatrace](/workshops/bizops)
* [Autonomous Cloud with Dynatrace](/workshops/autonomous-cloud)

These are also conducted virtually as [Hands-On Workshops](/schedule). 

### Addtional Resources

- To learn more about what Dynatrace can do for you, watch the videos on [Dynatrace Free Trial Resources](https://www.dynatrace.com/news/free-trial-resources/).

- To learn about our on-premises alternative, read [Get started with Dynatrace Managed](https://www.dynatrace.com/support/help/get-started/get-started-with-dynatrace-managed/).