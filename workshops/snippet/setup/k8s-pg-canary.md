## (Optional) Process Detection for Canary Deployment 
Duration: 15

### Deploy the Canary Release 

Run the command below to trigger the canary release
```bash
kubectl apply -f https://raw.githubusercontent.com/Dynatrace-APAC/Workshop-Kubernetes/master/manifests/sockshop-app/canary/front-end-canary.yml
```

Execute `kubectl get pods -n production -o wide` and you will see you now have both **stable and canary releases running for the front-end service**

![JSON](../assets/k8s/Picture21.png)

Wait 1-2 minutes then look the Services in Dynatrace. You have **2 services in production**, one for **stable** and one for **canary** release.
For monitoring purposes, it should be the same service

![JSON](../assets/k8s/Picture22.png)

### Process Detection Rule Config

In the Dynatrace console, go in **Settings -> Processes and containers -> Process group detection**.

Expand the **Cloud application and workload detection** section. 

As per our [documentation](https://www.dynatrace.com/support/help/how-to-use-dynatrace/process-groups/configuration/adapt-the-composition-of-default-process-groups/#cloud-applications-and-workload-detection), you will need to use the **disable Cloud application and workload detection** while **enabling the rule based tag feature** with `DT-ContainerBoundariesAffected` to select the process groups. 

![Disable-cloud-apps](../assets/k8s/disable-cloud-workload.png)

Expand the **Process group detection rules** section. 

Click **Add detection** rule.

Select Use a **process property** to seperate processes

![JSON](../assets/k8s/Picture23.png)

We want to apply this rule for pods running in **production only (namespace=production)**

Also, extract the identifier after the **"."** in the pod name. 
Remember the pod names have **".stable "or ".canary"** in their name to distinguish them

![JSON](../assets/k8s/Picture24.png)

Run the below command to **recycle both stable and canary frontend pods**. The process detection rules are applied on process startup.

```bash
wget -O- https://raw.githubusercontent.com/Dynatrace-APAC/Workshop-Kubernetes/master/recycle-sockshop-frontend.sh | bash
```

Within Dynatrace, you can see that the Process Groups have been merged.

![Process-Group-Merged](../assets/k8s/Picture24.1.png)

The services are still detected as individual services and can be merged as well.

Go to **Settings -> Merge Service monitoring -> Create merged service**

![Service-Merged](../assets/k8s/Picture24.2.png)

### Validate

![JSON](../assets/k8s/Picture25.png)

With the services merged as one, you can now view monitor Stable vs Canary response

Create Multi-dimensional Analysis view by selecting **Create Chart**

![JSON](../assets/k8s/Picture26.png)

Choose **Response Time - Server** and select **Service Instance** as Dimension Splitting

![JSON](../assets/k8s/Picture27.png)
