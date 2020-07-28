summary: Autonomous Cloud with Dynatrace
id: autonomous-cloud
categories: autonomous-cloud
tags: autonomous-cloud
status: Published 
authors: Brandon Neo
Feedback Link: mailto:APAC-SE-Central@dynatrace.com

# Autonomous Cloud with Dynatrace
<!-- ------------------------ -->
## Introduction 
Duration: 1

This repository contains labs for the Hands-On Autonomous Cloud Session. We will be providing the necessary details for your to access your environment.

For the purposes of the Hands-On, we will automate and make the steps seamless for the participants

You will be provided with the following:
- Dynatrace environment
- Kubernetes Server running
   - Sockshop Application (sample app) 
      - with Dev and Prod Environments
- Jenkins environment
- Ansible environment

![ACM-Setup](assets/ac/acmsetup.png)

### Prerequisites
- Chrome Browser
- Autonmous Workshop Email with credentials

![ACM-Setup](assets/ac/acm-1.jpg)

### What You’ll Learn
- Deploy a pipeline (Sockshop) in Jenkins 
- Installing Keptn 
- Integrating Jenkins to Keptn and Dynatrace
- Using Jenkins + Keptn with Dynatrace for
   - Automate Quality with Quality Gates evaluation
   - Automate Testing Load Testing and Performance Testing
   - Automate Operations with Self-healing with Ansible

<!-- ------------------------ -->
## Deploy SampleApp in Jenkins
Duration: 5

### Login to your Jenkins Instance

Access your Jenkins environment via a web browser with credentials from your email

![Jenkins-Setup](assets/ac/jenkins-login.png)

### Build a pipeline

Once you logged in, mouseover "**DeploySockShop**" in the list, click on the down arrow and select "**Build Now**"

![Jenkins-Setup](assets/ac/jenkins-1.png)

This process will take about 1-2 mins so we will let it run. 

Once finished, you can click on "**DeploySockshop**" again and see the various stages of the pipeline build. 

![Jenkins-Setup](assets/ac/jenkins-2.png)


This is just a demonstration of a working pipeline so there's no need to wait for it to complete.

<!-- ------------------------ -->
## Setting up Keptn
Duration: 5

### Login to your Kubernetes Instance

Login to the Kubernetes server via a SSH terminal (eg. Putty, Terminal, MobaXterm)

Access your Kubernetes environment with credentials from your email

Once you logged in, you can issue the commands below to install Keptn<br>
Part of the Keptn setup has been automated within the shell script below.

```bash
cd dtacmworkshop/keptn
sudo ./installKeptn.sh
```
![Keptn-Setup](assets/ac/keptn-1.png)

Keptn will install it's necessary components required for its setup.
Setup will take a 2-3 minutes.

![Keptn-Setup](assets/ac/keptn-2.png)

Once completed, take note of the **keptn API token** which is required for the next step.
You can also use the Keptn Bridge and API Endpoint

For example:<br>
KEPTN BRIDGE: http://bridge.keptn.<i>**YOUR-IP-ADDRESS**</i>.nip.io<br> 
KEPTN ENDPOINT: https://api.keptn.<i>**YOUR-IP-ADDRESS**</i>.nip.io/**swagger-ui**/

![Keptn-Setup](assets/ac/keptn-4.png)
Take note of to append "**/swagger-ui/**" to the end to view the Keptn API Swagger UI 

![Keptn-Setup](assets/ac/keptn-3.png)
There would be no project running at the moment on Keptn's bridge, but we will be creating at the next step.

<!-- ------------------------ -->
## Configuring Keptn within Jenkins
Duration: 5

### Setting up Keptn Plugin within Jenkins

Back within Jenkins, select **Jenkins (from the top left) > Manage Jenkins > Configure System**

![Plugin-Setup](assets/ac/jenkins-3.png)

Scroll down and under "**Global Properties**" enter the KEPTN API Token from the Terminal into the field<br>
Click "**Save**" to save your setting.

![Plugin-Setup](assets/ac/jenkins-4.png)

Steps have been pre-configured prior to the labs. To find out more about the plugin, refer to the [Keptn Jenkins Shared Library](https://github.com/keptn-sandbox/keptn-jenkins-library) on Github. 

<!-- ------------------------ -->
## Keptn's SLI/SLO-based Quality Gates
Duration: 20

We will be using Jenkins Pipeline to triggers an SLI/SLO-based Quality Gate Evaluation in Keptn. 
This can also be done through the Keptn CLI or the API. 

![SLI-quality-gate](assets/ac/evalpipeline_animated.gif)

Within Jenkins, mouseover "**01-qualitygate-evaluation**" in the list, click on the down arrow and select "**Build Now**"

![Quality-Gate](assets/ac/quality-gate-1.png)

The initial build will fail as Jenkins by default doesn't scan the pipeline for parameters and there are several that have to be specified. Click on "**01-qualitygate-evaluation**" to drill into the pipeline.

![Quality-Gate](assets/ac/quality-gate-2.png)

Click on **"01-qualitygate-evaluation"** and select **"Build with Parameters"** on the left menu.

![Quality-Gate](assets/ac/quality-gate-3.png)

As with the preconfigured form, we will be using the **"evalservice"** tag within Dynatrace to identify the appropriate service which we will validate the SLI and SLO. The name of the tag can be passed to our Jenkins Pipeline as a parameter. So we will first need to tag a service within Dynatrace.

### Login to your Dynatrace Environment

Access your Jenkins environment via a web browser with credentials from your email. <br>
When you access your Dynatrace tenant for the first time you’ll need to set a password.<br>

![Quality-Gate](assets/ac/quality-gate-8.png)

Once you logged into Dynatrace, you will find a preconfigured Dashboard. 

### Setup Tag within Dynatrace

![Quality-Gate](assets/ac/quality-gate-4.png)

This prebuild dashboard will also contain links providing quick access to the various portals. 

Select "**Transactions and Services**" on the left navigation bar and select "**front-end**" service. <br>
This can be either for Dev or Prod service. 

![Quality-Gate](assets/ac/quality-gate-5.gif)

Drop-down "**Properties and tags**", Click on "**Add-tag**" and enter "**evalservice**" as tag

### Build the pipeline in Jenkins

Back in Jenkins, click on "**Build**" button to run the pipeline

![Quality-Gate](assets/ac/quality-gate-6.png)

Once the pipeline completes, you can see the changes reflected in Keptn's bridge and Dynatrace.<br>
Within Dynatrace, you will also discover a new custom info event.<br>
The new event will contain details of the quality gate results with details such as JobURL, JobName from Jenkins as well as Keptn source and Keptn's bridge.<br>
The links will also bring you to Jenkin's and Keptn's portals with more detailed information on each side.

![Quality-Gate](assets/ac/quality-gate-7.gif)

To find out more about the setup of this hands-on, the full tutorial could be found [here](https://github.com/keptn-sandbox/jenkins-tutorial/blob/master/README.md#11-integrate-keptns-slislo-based-quality-gates).

<!-- ------------------------ -->
## Jenkins Simple Load Test
Duration: 20

### Load Testing in Pipeline

This is an extended version of the previous build where the pipeline also has a very simple load-testing capability built-into one of the stages.

![load-test](assets/ac/simpletestpipeline_animated.gif)

Go to "**Transactions and Services**", filter with "**[Kubernetes]stage:dev**" tag and tag "**testservice**" to identify the service. 

![load-test](assets/ac/load-test-2.gif)

Back In Jenkins, click on "**Back to Dashboard**" to return to the list of Jenkin's pipelines.<br>
Mouseover "**02-simpletest-qualitygate**" in the list, click on the down arrow and select "**Build Now**"<br>
As per the previous lab, the initial build will fail but you can drill into the "**Build with parameters**" on the 2nd attempt.

![load-test](assets/ac/load-test-1.png)

Change the form with the following details:<br>
Service: **testservice**<br>
DeploymentURI: **Website Dev URL** (as provided in email)<br>
URL Paths: **/:homepage;/category.html:Category**<br>

Once completed, you can click on "**Build**" button to start the pipeline.

![load-test](assets/ac/load-test-3.png)

You can also get the URL of your Carts Dev URL from your dashboard.<br>
Simply right-click and copy link address. 

![load-test](assets/ac/load-test-4.png)

Once completed, you can look at Keptn's bridge / Dynatrace's Frontend Service / Jenkins for the changes.<br>
You will find the additional SLIs based on the calculated service metrics.

![load-test](assets/ac/load-test-5.png)

To find out more about the setup of this hands-on, the full tutorial could be found [here](https://github.com/keptn-sandbox/jenkins-tutorial/blob/master/README.md#12-integrate-keptns-slislo-based-quality-gates---with-simple-testing-tool-in-pipeline).

<!-- ------------------------ -->
## Keptn's Performance Test Quality Gate
Duration: 20

### Performance Testing in Pipeline

This lab extends the previous one where we had Jenkins execute a simple test as part of the Jenkins Pipeline. Keptn was then used for SLI/SLO based quality-gate evaluation. In this tutorial we use Keptn to also take care of the actual test execution using JMeter.

![perf-test](assets/ac/perfaaspipeline_animated.gif)

We will also be using calculated service metrics as defined SLIs for the test execution.<br>
The creation of these service metrics was created as part of the lab setup. <br>
These metrics could be found in **Settings > Server-side service monitoring > Calculated Service metrics**<br>

![perf-test](assets/ac/perf-test-4.png)

These metrics as well as their dimensions will be used during the execution of the SLIs. <br>
As we already added the "**testservice**" tagged, we can simply run the 3rd pipeline within Jenkins. <br>
Back in Jenkins, click on "**03-performancetest-qualitygate**". <br><br>
Change the form with the following details:<br>
Service: **testservice**<br>
DeploymentURI: **Website Dev URL** (as provided in email)

![perf-test](assets/ac/perf-test-1.png)

Once completed, you can click on "**Build**" button to start the pipeline.

You can also get the URL of your Carts Dev URL from your dashboard.<br>
Simply right-click and copy link address. 

![perf-test](assets/ac/perf-test-2.png)

Once completed, you can look at Keptn's bridge / Dynatrace's Frontend Service / Jenkins for the changes.<br>
Within Dynatrace, you can see the automated performance tests that generated a pass result for the quality gate.<br>
You can also drill in context into Keptn's bridge as well as Jenkins.

![perf-test](assets/ac/perf-test-3.gif)

<!-- ------------------------ -->
## Automate SampleApp Deployment
Duration: 20

We will now modify the SampleApp Sockshop by automating it with Keptn's SLI/SLO-based Quality Gates.

![deploy-app](assets/ac/deploy-app-1.png)

Within Jenkins, mouseover "**DeploySockShop**" in the list, click on the down arrow and select "**Configure**"<br>
Open another browser tab, go to [Dynatrace APAC's Autonomous Cloud's Git Repo](https://github.com/Dynatrace-APAC/Workshop-Autonomous-Cloud/blob/master/jenkins/DeploySockShop_Keptn_LoadTest_and_QualityGate.Jenkinsfile).<br>
Copy the **amended SockShop Jenkinsfile raw file** and replace it within the Pipeline section in Jenkins.  

![deploy-app](assets/ac/deploy-app-2.gif)

Click on "**Save**" and at the "**Build with Parameters**" page, select "**Build Two**" and start the build. 

![deploy-app](assets/ac/deploy-app-3.png)

<!-- ------------------------ -->
## Self Healing as a Service
Duration: 20

Dynatrace integrates with many runbook automation tools such as Ansible. We'll be using Ansible to showcase self-healing problems and automate operations. 

### Generate Load for Carts Service

Within the SSH terminal, run the following commands to trigger load for the carts service.

```bash
cd ~acm_student/dtacmworkshop/utils/
sudo ./cartsLoadTest.sh
```

### Login to Ansible Tower

Access your Ansible environment via a web browser with credentials from your email.

![Ansible](assets/ac/ansible-2.png)

### Configure Dynatrace Problem Notification

Within Ansible, click on "**Templates**" on the left navigation and copy the URL for **Remediation** Job template. <br>
In the Dynatrace UI, navigate to **Settings > Integration > Problem Notification > Setup Notification > Ansible Tower **<br>
Enter the **Ansible Tower job template URL** you copied<br>
Enter the credentials to access Ansible (as provided in email)<br>
Click the Sent test notification button to validate your configuration<br>
On success, a green confirmation message will be displayed<br>
Save your configuration

![Ansible](assets/ac/ansible-3.gif)

### Adjusting Anomaly Detection

Both problem and anomaly detection in Dynatrace leverage our DAVIS AI technology. This means that DAVIS learns how each and every microservice behaves and baselines them. Therefore, in a demo scenario like we have right now, we have to override the AI engine with user-defined values to allow the creation of problems due to an artificial increase of a failure rate. (Please note: if we would have the application running and simulate end-user traffic for a couple of hours/days there would be no need for this step.)

In your Dynatrace tenant, navigate to “**Transaction & services**” and filter by tag "**[Kubernetes]stage:prod**" and select ItemsController Service<br>
Within the ItemsController Service page, click on on the **three dots ( … )** next to the service name. Click on **Edit**<br>
On the next screen, edit the **anomaly detection settings** as per below<br>
- Disable **Global anomaly detection**
- Detect increases in failure rate using **fixed thresholds** 
- Alert if **0 %** custom failure rate threshold is exceed during any 5-minute period. 
- Set Sensitivity to **High** and change less than the value to **1 request/min**.

![Ansible](assets/ac/ansible-5.gif)

### Launch remediation playbook

Navigate back to the Ansible Tower UI. From the side menu, navigate to **Resources -> Templates**.<br>
Click on the **rocket icon** to launch the start-campaign playbook. <br>
Hit **Next** on the prompt popup window and then **Launch**. <br>
As the playbook runs, validate that status is successful.

![Ansible](assets/ac/ansible-6.gif)

### Observe remediation

Back in ItemsController, look at the service events. The playbook have notified Dynatrace when the promotional rate was changed to 50%.

![Ansible](assets/ac/ansible-7.png)

You should see the Failure Rate increasing, eventually leading to Dynatrace detecting a Problem
You might need to refresh your browser a few times

![Ansible](assets/ac/ansible-8.png)

Observe the new problem appearing. The comments section will show the remediation actions taken by Ansible Tower

![Ansible](assets/ac/ansible-9.png)

Drill-down in the Problem.
You will see a new configuration change event reported by Ansible Tower
The promotional rate has been set back to 0% to remediate to the transaction failures

![Ansible](assets/ac/ansible-10.png)

Jobs executed in Ansible Tower
 - start-campaign (set rate to 50%)
 - remediation
   - push comment to Dynatrace Problem
   - retrieve problem details
   - launch remediation action related to problem context
   - update Dynatrace Problem
stop-campaign (set rate to 0%)

![Ansible](assets/ac/ansible-11.png)

** Problem Resolution - Problem was resolved automatically

![Ansible](assets/ac/ansible-12.png)
