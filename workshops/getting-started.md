summary: Getting Started with Dynatrace
id: getting-started
categories: getting-started
tags: microlab
status: Published 
authors: Brandon Neo
Feedback Link: mailto:APAC-SE-Central@dynatrace.com
Analytics Account: UA-175467274-1

# Digital Experience Management with Dynatrace
<!-- ------------------------ -->
## Introduction
Duration: 1

This microlab contains the simple steps required to get started with Dynatrace. 

Negative
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

![Deploy](assets/getting-started/welcome-user.png)

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

![Deploy](assets/dem/download-deployment-status.png)

You should be able to see a connected host as per the image below.

![Deploy](assets/dem/download-deployment-status-1.png)

Negative
: Note that for Dynatrace to detect **service level calls**, you'll need to restart the **application service**

## Discover Dynatrace

Dynatrace will automatically discover the processes and dependencies of your application. You will be able to explore various functionalities within Dynatrace like:

* Smartscape
* Host view 
   * CPU, memory, NICs, and disks 
   * Availability 
   * Processes
   * Events 
   * Logs
* Transactions and services

**Example of a populated Smartscape**

![Smartscape](assets/dem/smartscape.png)

You can also explore our Interactive Demos to find out more about why [Dynatrace] is unique(/interactive-demo) 

<!-- ------------------------ -->
## Fine tune your environment
Duration: 15


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
  <input value="Understanding Real User Monitoring setup" />
  <input value="Learning Synthetic" />
  <input value="Learning Session Replay" />
  <input value="Learning User Session Query Language" />
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
: 💡 For other ideas and suggestions, please **[reach out via email](mailto:APAC-SE-Central@dynatrace.com?subject=DEM Workshop - Ideas and Suggestions")**.