<!-- Code for k8s Process Group & Service Naming Rules -->
Duration: 10

### Process Group Naming Rules

Go to **Settings -> Processes and containers -> Process group naming** and click **Add a new rule**

Provide a name to the rule, for example : **Kubernetes Project.Namespace.Container**

Enter this format : `k8s-{ProcessGroup:Kubernetes:pipeline.project}.{ProcessGroup:KubernetesNamespace}.{ProcessGroup:KubernetesContainerName}`

In the conditions drop-down, select the property **Kubernetes namespace** and the condition **exists**

![JSON](../assets/k8s/Picture15.png)

Click on **Preview** to view the matching entities 

![JSON](../assets/k8s/Picture16.png)

Click on **Create Rule** and **Save Changes** in the pop-up.

### Validate

Once working, you can validate the change in Dynatrace

![JSON](../assets/k8s/Picture17.png)

### Service Naming Rules

Go in **Settings -> Server-side service monitoring -> Service naming rules** and click **Add a new rule**

Provide a name to the rule, for example : **Kubernetes Project.Namespace.Container**

Enter this format : `{Service:DetectedName}.{ProcessGroup:KubernetesNamespace}`

In the conditions drop-down, select the property **Kubernetes namespace** and the condition **exists**

![JSON](../assets/k8s/Picture18.png)

Click on **Preview** to view the matching entities 

![JSON](../assets/k8s/Picture19.png)

Click on **Create Rule** and **Save Changes** in the pop-up.

### Validate

Once working, you can validate the change in Dynatrace

![JSON](../assets/k8s/Picture20.png)