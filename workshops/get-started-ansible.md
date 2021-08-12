id: get-started-ansible
summary: Get started with Redhat Ansible and archive automation with Dynatrace
categories: ansible, get-started
tags: microlab, Introduction
status: Published 
authors: Brandon Neo
Feedback Link: mailto:APAC-SE-Central@dynatrace.com
Analytics Account: UA-175467274-1

# Get Started with Ansible
<!-- ------------------------ -->
## Introduction

This section contains the various approaches to get started with Redhat Ansible. This includes the various flavors of Ansible including Redhat Ansible Tower.


Positive
: If you need to use a **demo application**, you can use Dynatrace's sample app [easyTravel](https://community.dynatrace.com/community/display/DL/easyTravel)

### What You’ll Learn
- Installing Ansible
- Install OneAgent with Ansible
- Auto-Remediation use case
- Discover and Explore Dynatrace

<!-- ------------------------ -->
## Automated Deployment with Ansible
Duration: 15

In this exercise, we will deploy the OneAgent to a Linux instance with Ansible.

Based off our [Dynatrace Ansible Github](https://github.com/Dynatrace/Dynatrace-OneAgent-Ansible), you can rollout Dynatrace Oneagent easily across on Linux and Windows Operating Systems with different available configurations and ensures the OneAgent service maintains a running state. It also provides the tasks to interact with the various OneAgent configuration files.

Following the steps from our [documentation](https://www.dynatrace.com/support/help/setup-and-configuration/dynatrace-oneagent/deployment-orchestration/ansible/), we will use an Ansible collection to orchestrate OneAgent deployment.

You can also use other Ansible playbooks examples can be seen [here](https://github.com/popecruzdt/dt-ansible/blob/popecruzdt/dt-oneagent-install-linux.yml)

<<snippet/install/ansible.md>>

<<snippet/deploy/oneagent-ansible.md>>

<!-- ------------------------ -->
## Self Healing with Ansible
Duration: 15

Deploying applications continously requires an **automated delivery pipeline**. Dynatrace tracks all key metrics through the various CI/CD stages into production. Through understanding the feedback loop, you can stop faulty builds before they reach production and start deploying software faster at higher quality.

Identify bad code changes before executing GIT COMMIT in your IDE
Stop broken builds earlier in your delivery pipeline
Detect architectural regressions with automated tests
Deploy with confidence

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