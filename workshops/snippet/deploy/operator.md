<!-- Code for deploying OneAgent via Operator -->

In this exercise, we will deploy the OneAgent to a Linux instance running Kubernetes(Microk8s) and let the OneAgent discover what is running in that instance.

### Using Terminal via Web Browser

To faciliate the labs, we will access the Linux instance **via terminal through a web browser**.

Use the **URL** was provided in your email to access the browser-based terminal. Please follow the instructions in your email.

If you are still not able to use the web browser, you can download any SSH client like PuTTy or MobaXterm and access the session via the SSH client.

Use the **login name** and **password** as provided in your email. It works for any method of access.

### Download the OneAgent

Open your browser and access the Dynatrace URL.

Follow these steps below:

* Search for **Hub** from the navigation menu.
* **Star** it to add to favourites and **click** on it.
* Select **Kubernetes**
* Select **Monitor Kubernetes** button from the bottom right.  

![Deploy](../assets/cloud-observe/k8s-deploy.gif)

Within the **Monitor Kubernetes / Openshift** page, follow these steps below:

* Enter a **Name** for the connection Example: `k8s` or `workshop`
* Click on **Create token** to create the **Dynatrace operator token** (all other tokens are optional)
* **Toggle ON** Skip SSL Certificate Check
* **Download** the dynakube.yaml file
* **Either upload** it to the browser-based terminal **or create a yaml file and copy** the contents to that file in the terminal session.
* Click **Copy** button to copy the commands. 
* **Paste** the command into your terminal window and execute it.

![Deploy](../assets/cloud-observe/k8s-deploy-2.png)

Negative
: Note that it will take about 5 mins for data to appear within Dynatrace

Positive
: Dynatrace handles automatic deployment of OneAgents as well as automatic k8s integration. 

### Validate the installation in Deployment status

Click on **Show deployment status** to check the status of the connected host. 

You should be able to see a connected host as per the image below.

![Deploy](../assets/dem/download-deployment-status-1.png)

Positive
: Dynatrace Documentation is referenced [here](https://www.dynatrace.com/support/help/technology-support/operating-systems/linux/)

### ⚠️ Troubleshooting steps

Negative
: To **check status of pods**, run command below. You should get a **Running** as a return.<br>
`kubectl get pods -n dynatrace`

Negative
: To **check the logs**, run command below.<br>
`kubectl logs -f deployment/dynatrace-oneagent-operator -n dynatrace`

Negative
: To **delete secrets**, run command below. You might have included a wrong secret previously. <br>
`kubectl delete secret --all -n dynatrace`

Negative
: To **delete all pods**, run command below. This will cycle through the pods and you will have new pod instances.<br>
`kubectl delete --all pods -n dynatrace`

Negative
: Official troubleshooting page could be found [here](https://www.dynatrace.com/support/help/setup-and-configuration/setup-on-container-platforms/kubernetes/troubleshoot-k8s-deployment-and-connectivity/)

If everything is working, you will see the host appearing when you click on **Show Deployment status**