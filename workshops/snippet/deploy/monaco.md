<!-- Code for Monaco -->

In this exercise, we will automate configuration of Dynatrace environment.

Using [Dynatrace Monitoring as Code (Monaco)](https://github.com/dynatrace-oss/dynatrace-monitoring-as-code), you can automate the configuration of all global Dynatrace environments without human intervention. Various use cases include:

* Having the ability to templatize our configuration for reusability across multiple environments
* Interdependencies between configurations should be handled without keeping track of unique identifiers
* Introducing the capability to easily apply – and update – the same configuration to hundreds of Dynatrace environments as well as being able to roll out to specific environments
* Identify an easy way to promote application specific configurations from one environment to another – following their deployments from development, to hardening to production.
* Support all the mechanisms and best-practices of git-based workflows such as pull requests, merging and approvals
* Configurations should be easily promoted from one environment to another following their deployment from development to hardening to production

To faciliate the session, you can run the monaco code with the below:

```bash
cd sockshop
./deploy-monaco.sh
```

After setting it up, configure the **DT_TENANT** and **DT_API_TOKEN** and **DT_DASHBOARD_OWNER** variables.
These can be found within the lab registration email.

```bash
export DT_TENANT= https://mou612.managed-sprint.dynalabs.io/e/<ENV>
export DT_API_TOKEN=dt0c01.IH6********************************************
export DT_DASHBOARD_OWNER=<your email address>
```

After setting up, run the following command to configure Dynatrace:

```bash
./push-monaco.sh 
```

Below are the configurations done:

* Synthetic monitoring
* Service naming rules
* Carts SLO
* Application definitions
* Dashboards
* Process naming rules
* Management zones