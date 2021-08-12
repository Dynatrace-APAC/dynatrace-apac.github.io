### Install OneAgent with Ansible

Following the steps in Dynatrace:

* Filter or find **Deploy Dynatrace** from the navigation menu
* Select **Start installation**
* Download **Ansible collection** to your Linux instance
* Use the command below to **install the collection** 

`ansible-galaxy collection install dynatrace.oneagent`

This will install **dynatrace.oneagent** collection that consist of a single role that deploys OneAgent using dedicated configuration and ensuring the OneAgent service maintains a running state. For more information, see [Using collections](https://docs.ansible.com/ansible/latest/user_guide/collections_using.html) in Ansible documentation. You can also find the latest version at [Ansible Galaxy](https://galaxy.ansible.com/dynatrace/oneagent)

### Creating the PaaS Token

Following the steps in Dynatrace:

* Go to **Settings > Integration > Platform as a Service** 
* Click on **Generate token** and give a token name eg. **Ansible**
* Click on **Generate** and **copy** the token by selecting **Reveal token** to use for the next step.

![Deploy](assets/adv-observe/paas-token.png)

###  Running the playbook

Using the example playbook below, replace the **dynatrace_environment_url** and **dynatrace_paas_token** variables to install OneAgent.
You can tweak the command with a text editor eg. Notepad++

**EXAMPLE**

```yaml
---
- hosts: all
  become: true
  roles:
    - role: Dynatrace.OneAgent
  vars:
    dynatrace_environment_url: {your-environment-id}.live.dynatrace.com
    dynatrace_paas_token: {your-paas-token}
```

* Save the above yaml as `install.yml`
* To install the playbook, run the following command:

`ansible-playbook dt-oneagent-install-linux.yml`

![Deploy](assets/adv-observe/dynatrace-env.png)

### Validate the installation

Go to **Deployment status** on the left navigation. You should see OneAgent installed on the current host.

![Deploy](assets/adv-observe/deployment-status.png)
