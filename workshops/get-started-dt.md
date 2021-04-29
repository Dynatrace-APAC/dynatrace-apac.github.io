summary: Get Started with Dynatrace
id: get-started-dt
categories: get-started
tags: microlab
status: Published 
authors: Brandon Neo
Feedback Link: mailto:APAC-SE-Central@dynatrace.com
Analytics Account: UA-175467274-1

# Get Started with Dynatrace
<!-- ------------------------ -->
## Introduction

This microlab contains the simple steps required to get started with Dynatrace. 

Positive
: If you need to use a **demo application**, you can use Dynatrace's sample app [easyTravel](https://community.dynatrace.com/community/display/DL/easyTravel)

### What You’ll Learn
- Create your Dynatrace account
- Deploy Dynatrace OneAgent
- Discover and Explore Dynatrace

<!-- ------------------------ -->
## Create your Dynatrace Account
Duration: 5

### To create an account

* Go to the [Dynatrace free trial page](https://www.dynatrace.com/trial/).
* Enter your **business email address** and select **Start free trial**.
* Provide a few account details (nothing too technical) and agree to the terms and conditions.
* Select Create account.
* Once created, you'll be shown the page below and you can deploy Dynatrace. 

![Deploy](assets/get-started/dynatrace/welcome-user.png)

THAT'S IT! You're done! You should be directed to your own Dynatrace SaaS tenant page (eg. **abc12345.live.dyntrace.com**)

<!-- ------------------------ -->
## Deploy Dynatrace OneAgent
Duration: 5

### Download the OneAgent

Follow these steps below to install Dynatrace OneAgent on a host.

* Select **Dynatrace Hub** on the left navigation bar
* Select **OneAgent** 
* Select **Download OneAgent** button from the bottom right.  
* Choose your **Operating system** or **platform** eg. Windows, Linux or Kubernetes, Openshift
* Follow the on-screen instructions for getting and installing OneAgent.

Positive
: For Windows, there are no additional parameters to provide. Just select Download OneAgent installer to download the Windows installer. 

Once done, the installer will tell you that Dynatrace OneAgent has successfully connected to a Dynatrace cluster node and display a congratulations message.

Negative
: For troubleshooting, use our [help documentation page](https://www.dynatrace.com/support/help/setup-and-configuration/dynatrace-oneagent/troubleshooting/troubleshoot-oneagent-installation/)

### Validate the installation in Deployment status

Click on **Deployment status** in the left navigation to check the status of the connected host. 

You should be able to see a connected host as per the image below.

![Deploy](assets/dem/download-deployment-status-1.png)

## Discover Dynatrace

Dynatrace will automatically discover the processes and dependencies of your application. You will be able to explore various functionalities within Dynatrace like:

* [Smartscape](https://www.dynatrace.com/support/help/how-to-use-dynatrace/smartscape/)
* [Host view](https://www.dynatrace.com/support/help/how-to-use-dynatrace/hosts/) 
* [Processes](https://www.dynatrace.com/support/help/how-to-use-dynatrace/process-groups/)
* [Network](https://www.dynatrace.com/support/help/how-to-use-dynatrace/networks/) 
* [Logs](https://www.dynatrace.com/support/help/how-to-use-dynatrace/log-monitoring/)
* [Transactions and services](https://www.dynatrace.com/support/help/how-to-use-dynatrace/transactions-and-services/)

Negative
: Note that for Dynatrace to detect **service level calls**, you'll need to restart the **application service**

**Example of a populated Smartscape**

![Smartscape](assets/dem/smartscape.png)

All the analyzed through **Dynatrace DAVIS**, our AI Engine that uses deterministic AI to performs an automatic fault-tree analysis. This is causation not correlation.

To learn more, use the [Interactive Demos](/interactive-demo) to find out more about why [Dynatrace](https://www.dynatrace.com/platform/) is unique

<!-- ------------------------ -->
## Fine tune your environment
Duration: 5

You can now access your [home dashboard](https://www.dynatrace.com/support/help/how-to-use-dynatrace/dashboards-and-charts/), by logging in to your Dynatrace monitoring environment. All monitoring data should be there already. However, some additional post-deployment steps are required. These steps could be found in our [documentation page](https://www.dynatrace.com/support/help/shortlink/section-get-started#step-4-fine-tune-your-environment)

### Deploy to other hosts 

You can see how easy it is to deploy Dynatrace to one host and immediately start monitoring it. But that was just a taste of what Dynatrace can do for you.

### Advanced Use Cases

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

The above use cases are also labs which you can run through:
* [Digital Experience Management with Dynatrace](/workshops/dem)
* [Advanced Observability with Dynatrace](/workshops/advanced-observability)
* [BizOps with Dynatrace](/workshops/bizops)
* [Autonomous Cloud with Dynatrace](/workshops/autonomous-cloud)

These are also conducted virtually as [Hands-On Workshops](/schedule). 

### Addtional Resources

- To learn more about what Dynatrace can do for you, watch the videos on [Dynatrace Free Trial Resources](https://www.dynatrace.com/news/free-trial-resources/).

- To learn about our on-premises alternative, read [Get started with Dynatrace Managed](https://www.dynatrace.com/support/help/get-started/get-started-with-dynatrace-managed/).