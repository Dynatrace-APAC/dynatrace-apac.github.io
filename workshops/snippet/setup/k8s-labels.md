<!-- Step to setup labels and annotations -->
Duration: 5

With the Sockshop app restarted, you should be able to see services in Dynatrace.

Referring to `~/easyTravel/manifests/frontend/angularfrontend-deployment.yaml`, we will want to setup Dynatrace to automatically pick up the annotations and labels.

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: easytravel-angularfrontend
  namespace: easytravel
spec:
  selector:
    matchLabels:
      app: easytravel-angularfrontend
      product: easytravel
  replicas: 1
  template:
    metadata:
      labels:
        app: easytravel-angularfrontend
        product: easytravel
      annotations:
        support.contact: "demoability@dynatrace.com"
        support.channel: "#team-demoability"
```
### Viewership role for service accounts

The OneAgent will use a pod service account to query for its metadata via the Kubernetes REST API.
The service accounts must be granted **viewer role** in order to have access
In the CLI, execute the following command for the **production project**

```bash
kubectl create rolebinding serviceaccounts-view --clusterrole=view --group=system:serviceaccounts:production --namespace=production
```

You can repeat the procedure for the **dev project**

```bash
kubectl create rolebinding serviceaccounts-view --clusterrole=view --group=system:serviceaccounts:dev --namespace=dev
```

Wait for the Dynatrace to pickup the change. 

Negative
: ⚠️ To manually recycle the apps, run command below.
`wget -O- https://raw.githubusercontent.com/Dynatrace-APAC/Workshop-Kubernetes/master/recycle-sockshop-frontend.sh | bash`

### Validate

Once working, you can validate the change in Dynatrace

![JSON](../assets/k8s/Picture12.png)

Positive
: The above steps are taken from [our official documentation page](https://www.dynatrace.com/support/help/technology-support/container-platforms/kubernetes/other-deployments-and-configurations/leverage-tags-defined-in-kubernetes-deployments/)
