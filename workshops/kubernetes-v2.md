id: kubernetes-updated
summary: Full Stack Observability with vanilla Kubernetes while integating with k8s for additional context
categories: kubernetes, application-microservices-monitoring
tags: kubernetes, Beginner
status: Published 
authors: Brandon Neo
Feedback Link: mailto:APAC-SE-Central@dynatrace.com
Analytics Account: UA-175467274-1

# Dynatrace with Kubernetes
<!-- ------------------------ -->
## Introduction 
Duration: 1

This repository contains labs for the Hands-On Kubernetes Session. We will be using Kubernetes instance running in AWS for this hands-on but this will work on other platforms as well.

For the purposes of the Hands-On, we will automate and make the steps seamless for the participants

### Prerequisites
- Dynatrace SaaS/Managed Account. Get your free SaaS trial [here](https://www.dynatrace.com/trial/).
- SSH client such as [mobaxterm](https://mobaxterm.mobatek.net/).
- Chrome Browser

### What You’ll Learn 
- Deploying Dynatrace Operator
- Explore Automatic Kubernetes Dashboards
- Kubernetes Labels & Annotations
- Process Group Naming & Service Naming for Kubernetes
- Discover various Kubernetes View on Dynatrace
  - Kubernetes Namespace
  - Kubernetes Workloads
  - Kubernetes Containers / Pods

<!-- Step 1 -->
## Deploy Dynatrace Operator
Duration: 15
<<snippet/deploy/operator.md>>

<<snippet/restart/sockshop.md>>

<<snippet/setup/k8s-settings.md>>

<!-- Step 2 -->
## Import Prometheus Metrics
Duration: 10
<<snippet/setup/k8s-prometheus.md>>

<!-- Step 3 -->
## Kubernetes Labels and Annotations
Duration: 15
<<snippet/setup/k8s-labels.md>>

<!-- Step 4 -->
## Container Environment Variables
Duration: 15
<<snippet/setup/k8s-container-variables.md>>

<!-- Step 5 -->
## Process Group & Service Naming Rules for K8s
Duration: 15
<<snippet/setup/k8s-pg-svc-naming-rules.md>>

<!-- Step 6 -->
## Monitoring-as-Code
Duration: 10
<<snippet/deploy/monaco.md>>

<!-- Step 7 -->
## Kubernetes Dashboards
Duration: 10
<<snippet/explore/k8s-dashboards.md>>

<!-- Step 8 -->
## Exploring Kubernetes View
Duration: 20
<<snippet/explore/k8s-views.md>>

<!-- Step 9 -->
## Kubernetes workloads intelligent observability 
Duration: 10
<<snippet/explore/k8s-o11y.md>>


<!-- ------------------------ -->

## Feedback
Duration: 3

We hope you enjoyed this lab and found it useful. We would love your feedback!
<form>
  <name>How was your overall experience with this lab?</name>
  <input value="Excellent" />
  <input value="Good" />
  <input value="Average" />
  <input value="Fair" />
  <input value="Poor" />
</form>

<form>
  <name>What did you benefit most from this lab?</name>
  <input value="Using OneAgent Operator to deploy in Kubernetes" />
  <input value="Setting up Kubernetes integation" />
  <input value="Process Group Naming & Service Naming for Kubernetes" />
  <input value="Understanding Kubernetes within Dynatrace" />
</form>

<form>
  <name>How likely are you to recommend this lab to a friend or colleague?</name>
  <input value="Very Likely" />
  <input value="Moderately Likely" />
  <input value="Neither Likely nor unlikely" />
  <input value="Moderately Unlikely" />
  <input value="Very Unlikely" />
</form>

Positive
: 💡 For other ideas and suggestions, please **[reach out via email](mailto:APAC-SE-Central@dynatrace.com?subject=Kubernetes Workshop - Ideas and Suggestions)**.