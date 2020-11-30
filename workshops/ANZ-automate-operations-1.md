summary: Automate Operations Session 1
id: automate-operations-1
categories: automate-operations
tags: anz
status: Published 
authors: Nikhil Goenka
Feedback Link: mailto:d1-apac@dynatrace.com
Analytics Account: UA-175467274-1

# Automate Operations - Session 1
<!-- ------------------------ -->
## Introduction 
Duration: 1

This lab is the third session of the AIOps Enablement Series for ANZ Bank. This track focuses on the Automate Operations, which relates to how you could integate Dynatrace with orchestration tools to **Automate Operations**.

![overview](assets/ANZ-aiops/feedback1/overview-1.png)

### What You’ll Learn 
- Integrate Ansible with Dynatrace  
- Stimulate an issue in Dynatrace and let DAVIS AI pick up the issue automatically
- Auto-remediate using ansible-remediation scripts


<!-- ------------------------ -->
## Useful Commands ✅ 

Positive
: To start the docker with sample application:
   `docker run -d --name SampleBankApp -p 4000:3000 nikhilgoenka/sample-bank-app`
  * This would start the docker on port localhost:4000 with docker name as **SampleBankApp**
   
Positive
: To start the jenkins docker:
   `docker run -d --network mynetwork --name Jenkins-Dynatrace -p 8020:8080  -v /var/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock nikhilgoenka/jenkins-dynatrace-workshop`
   * -d runs the docker in daemon mode.
   * -p 8020:8080 - By default, jenkins docker would be running on 8080. Specifying **-p 8020:8080** binds the 8080 in docker to localhost on 8020. So, you can forward/listen requests from docker using `localhost:8020`.
   * -v Bind mounts a volume.
    By default, jenkins docker is maintaining the pipeline/data information in /var/jenkins_home. 
    Specifying **-v /var/jenkins:/var/jenkins_home** would mount the localhost:/var/jenkins directory so that the pipeline data is not lost once pipeline is re-started.
    Specifying **-v /var/run/docker.sock:/var/run/docker.sock** will allow the jekins docker to leverage the dockerd running on localhost. This would be required since we are starting the
    sample-app dockers while running the pipeline.

Positive
: To run the ansible-tower docker:
   `docker run -d --name ansible-tower -p 8090:443 ybalt/ansible-tower`
   This would start the docker on port **localhost:8090** with docker name as **ansible-tower**

**Other useful commands:**
* To **view all docker containers**: `docker ps -a`
* To **view the downloaded images** on localhost: `docker images`
* To **remove a particular image**: `docker rmi <IMAGE-NAME>`
* To **stop a docker**: `docker stop <CONTAINER-ID>`
* To **remove a docker**: `docker rm <CONTAINER-ID>`
* To **run a docker in interactive bash**: `docker run -it <CONTAINER> /bin/bash`
* To **delete all the unused images**: `docker system prune -a -f`
* To **pull a particular image**: `docker pull <docker-image>`
* Jenkins pipeline:
   Command: `https://github.com/nikhilgoenkatech/JenkinsBankApp`

<!-- ------------------------ -->

## Preconfiguration Setup
We will be using dockers to stimulate and remediate the issues, so ansible-tower docker will communicate with the other dockers using docker-network.
In order it works as expected, start with checking if you already have a network present by issuing `docker network ls`
If not, create a network by issuing the command `docker network create mynetwork`
   
Now, run **ansible-tower docker** as`docker create -v /var/lib/postgresql/9.6/main --name tower-data nikhilgoenka/ansibletower /bin/true`

The above would create a postgres data volume which can be used to retain and retrieve the ansible-tower data. Once, the volume is created, run the ansible-tower docker as `docker run -d --network mynetwork --name ansible-tower --volumes-from tower-data -p 8090:443 nikhilgoenka/ansibletower`
   
From within your terminal, let us install **python-docker** on the hosts so that ansible-tower can perform operations on the host by running the following command `pip install docker`
   
We will use d1pacmworkshop user to access/change/modify docker, so give it required permissions on the docker. To do so, run `sudo usermod -aG docker d1pacmworkshop` 
This would add d1pacmworkshop user to docker group and it will get the required permissions to access/change the docker.

Lastly, navigate to `/home/ubuntu/ACMD1Workshop/additional_resources/app_docker/scripts folder` and run `wget https://raw.githubusercontent.com/nikhilgoenkatech/AIOPSAnsibleBankPlaybooks/main/synthetic-monitor.sh` to download the synthetic scripts that would be run in order to validate the auto-remediation.
   
Connect to "https://<my-IP>:8090/" and click on "Advanced" followed by Proceed to <IP:8090>   ![Ansible-Docker](assets/ANZ-aiops/ansible1/Ansible-tower-browser-advanced-option.png)

Run the docker for **SampleBankApp** as below:
docker run --network mynetwork -d --name SampleBankApp -p 3000:3000 nikhilgoenka/sample-bank-app:1.0
Access the sample-bank app from your browser at http://<MACHINE-IP>:3000/login to populate the service "node-bank" under "Transactions & Services"

## Configure Ansible
Login into your ansible docker using credentials "admin" and password "dynatrace".
![Ansible-Docker](assets/ANZ-aiops/ansible1/Ansible-tower-login.png)

Import the license by clicking on "Browse" and uploading the license that was earlier shared with you in email.
![Ansible-Docker](assets/ANZ-aiops/ansible1/Ansible-tower-import-license.png)

### Setting up inventory
Now, create the inventory by clicking on **Inventory** in the left-side as seen below:
![Ansible-Docker](assets/ANZ-aiops/ansible1/Ansible-create-inventory.png)

That would open the page displaying the existing inventories. Delete the existing inventory for a clearer view before adding one as seen below:
![Ansible-Docker](assets/ANZ-aiops/ansible1/Ansible-create-inventory-2.png)
![Ansible-Docker](assets/ANZ-aiops/ansible1/Ansible-add-inventory.png)
   
Negative:
Inventories consists of all the hosts that you plan to run the playbook on. So, inventories act as the repository of the hosts.

Once you add the inventory, you will get an option to **add hosts** for that inventory. Add the host by navigating to the "Hosts" tab.
The host can be added with host-name=<AWS-IP> and description as "Application-Docker-Server". 
![Ansible-Docker](assets/ANZ-aiops/ansible1/Ansible-add-hosts.png)
![Ansible-Docker](assets/ANZ-aiops/ansible1/Ansible-add-hosts-2.png)
   
### Setting up Project
Similarly, let us add a **project**. Project is a collection of ansible playbooks. To add/edit projects, click on "Projects". Start by deleting the existing "Demo Project" for a cleaner view, followed by clicking on "+" and "Project" to add your projects.
   **Project-name**: `Application-playbooks`
   **SCM Type**: Git
   **SCM URL**: `https://github.com/nikhilgoenkatech/AIOPSAnsibleBankPlaybooks`

![Ansible-Docker](assets/ANZ-aiops/ansible1/Ansible-create-project.png)

### Setting up Credentials
With the hosts and playbooks added, now add the credentials that ansible will use in order to connect to the configured hosts and run the playbook from projects. Navigate to **Credentials** as shown below.
![Ansible-Docker](assets/ANZ-aiops/ansible1/Ansible-add-credentials.png)

Delete the existing `Demo Credentials` and further, add credentials for your AWS host.
   **Name**: Application-servers
   **Credential Type**: Machine
   **Username**: `d1pacmworkshop`
   **Password**: `dynatrace`
![Ansible-Docker](assets/ANZ-aiops/ansible1/Ansible-add-credentials-2.png)

### Setting up Templates
Add a template by navigating to **Templates** on the left-hand side and configure as below:
	
**Name**: Process-unavailable
**Inventory**: Application-DT-inventory
**Project**: Application-playbooks
**Playbook**: process-restart.yaml	

Add the extra variables as below:
**tenanturl**: <your-tenant-url>
**dttoken**: <your-token>
**dtcommentapiurl**: <your-dynatrace-url>/api/v1/problem/details/{{pid}}/comments?Api-Token={{dttoken}}
**dtdeploymentapiurl**: <your-dynatrace-url>/api/v1/events?Api-Token={{dttoken}}
**my_ip**: <your-aws-instance-ip>

Positive:
Replace tenant-url with `https://xxx.live.dynatrace.com/` for SaaS where for managed, change to `https://managed.server/e/{environment-id}`
![Ansible-Docker](assets/ANZ-aiops/ansible1/add-template-1.png)

## Integrate Ansible-tower and Dynatrace     	

### Availability alert
Enable the **availability alert** for the NodeJS process. Within dynatrace-tenant, navigate to **Technologies > NodeJS Process-Group > Settings > Availability monitoring ** and toggle the "Enable process group availability monitoring" as below:	
![Ansible-Docker](assets/ANZ-aiops/ansible1/availability-event.png)

### Configure Alerting Profile
Let us further set the Alerting profile so that the problems can be notified to 3rd party like email, slack-integration, etc. 
Within your tenant, navigate to "Settings > Alerting > Alerting profiles". Now, add a new alerting profile "Ansible-playbook" and set the rules as below for 
"Availability event".
![Ansible-Docker](assets/ANZ-aiops/ansible1/Alerting-profile-setup.png)

### Problem notification
Once alerting profile is setup, let us setup the problem notification so that the problem information is relayed to ansible-tower. To do so, within your tenant navigate to Settings > Integration > Problem Notifications and click on **+Set up notifications**
![Ansible-Docker](assets/ANZ-aiops/ansible1/Problem-notification-1.png)

Select **Ansible Tower** as the 3rd party integration tool
![Ansible-Docker](assets/ANZ-aiops/ansible1/Problem-notification-2.png)

Configure the notification as below:
**Ansible Tower job template URL**: URL of template added in step(11)
**Username**: admin
**Password**: dynatrace	
![Ansible-Docker](assets/ANZ-aiops/ansible1/Ansible-problem-notification.png)

## Stimulate the issue that triggers auto-remediation endpoint	
Now, within your browser go to http://<MACHINE-IP>:3000/login and log-in with incorrect credentials. This would crash the docker in the backend resulting in the service no longer available.
![Ansible-Docker](assets/ANZ-aiops/ansible1/docker-crashed.png)	
This would create a problem in Dynatrace and as per the problem-notification set, it would trigger a notification to template and trigger the playbook automatically. 

### Initial analysis
The remediation would perform initial analysis on the problem like the problem-type, severity, collecting logs and push these as comments on the problem.
![Ansible-Docker](assets/ANZ-aiops/ansible1/initial-analysis.png)

### Auto-remediation
Further, it would start the docker that has crashed due to incorrect logging and push a deployment event on the service:
![Ansible-Docker](assets/ANZ-aiops/ansible1/docker-start.png)
![Ansible-Docker](assets/ANZ-aiops/ansible1/custom-annotation.png)

### Validating the auto-remediation
Next, the playbook will create a synthetic monitor and run the user-actions on it to validate if the application is up entirely and there are no issues.![Ansible-Docker](assets/ANZ-aiops/ansible1/synthetic-monitor.png)

### Complete auto-remediation
Once completed, it would push the information in DT indicating that the remediation has been completed succesfully.
![Ansible-Docker](assets/ANZ-aiops/ansible1/problem-completion.png)

Positive: We won't close the problem as yet, so as DAVIS AI ingests the information and flag if the remediation had any issues.