### Managing your workload resource usage

Although Kubernetes platform is built on a number of virtualization and abstraction layers, you still need to be conscious of the resources that your workload is using.

This is because Kubernetes is usually designed as a platform running on shared infrastructure, so how much resources you use might affect the other applications running on the same cluster.

### Scenario
- Kubernetes platform administrators set a resource quota that your project (namespace) needs to comply with
- Application team's responsibility to assign given resources to different microservices
- Apply this deployment by typing in this command:

  ```bash
  cd sockshop
  ./deploy_newbuild.sh
  ```

Keep an eye on your Dynatrace console... you never know what could happen.

At some point, both the apps team and the k8s infra teams received an alert from Dynatrace. Let's take a look at it.

![oomkill-container](../assets/k8s/oomkill-problemcard1.png)

Negative
: What happened? Which **service** was impacted? In which **namespace**?

The container from the new cart service pod has been **OOMKilled**. This means its memory usage went above its configured limit. The thing is, before today, you never had any OOMKilled problems.

Click on the container to drill-down the the Containers view.

![oomkill-container](../assets/k8s/oomkill-problemcard2.png)

In the Containers view, you will have all the necessary metrics and data points to have a clear picture on how resources are consumed

![oomkill-memory](../assets/k8s/oomkill-memory.png)  

So it has something to do with the new build. Either:

1. The memory limit for the container was set wrongly, or
2. The new build has a memory leak, which over time makes it consume memory above the limit.
   - If that is the case, whatever the increase in memory limit, it will eventually be breached. You would just be buying time, the code itself needs to be fixed.
   - Test under different conditions and monitor the Java memory metrics (in the Process view). Dynatrace will provide you all the metrics you need. Make code corrections and test again. Wash-rinse-repeat.

   ![oomkill-memory](../assets/k8s/oomkill-memory-trends.png)

The dev team quickly found the issue and provided a fix. It is related to a configuration issue - The memory limit for the container was set wrongly.

In the terminal execute to apply the fix:

`kubectl apply -f ~/sockshop/manifests/sockshop-app/newbuilds/newbuild-quota-fix.yml`

### Lesson learned
- Importance of resource management in Kubernetes
- Resources will always be limited and associated to a cost, either in infrastructure or cloud services
- Understanding workload resource consumption is key to figure out what requests and limits you have to set for your different services

Also, the k8s infra ops team will always give you only the amount of resources you need. It is their responsiblity to make sure the platform stays healthy for everyone using it. So in the case you need more resources, you have to enter into a negotiation with the platform team. And as with any negotiation, it's always better if you come at the table with as much information you can get.

And you can count on Dynatrace for that!