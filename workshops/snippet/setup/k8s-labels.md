<!-- Step to setup labels and annotations -->
Duration: 5

With the Sockshop app restarted, you should be able to see services in Dynatrace.

Referring to `~/sockshop/manifests/sockshop-app/production/front-end.yml`, we will want to setup Dynatrace to automatically pick up the annotations and labels.

```bash
---
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: front-end.stable
    product: sockshop
    release: stable
    stage: prod
    tier: frontend
    version: "1.4"
  name: front-end.stable
  namespace: production
spec:
  replicas: 1
  selector:
    matchLabels:
      app: front-end.stable
      product: sockshop
      release: stable
      stage: prod
      tier: frontend
      version: "1.4"
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      annotations:
        pipeline.build: 1.4.0.7424
        pipeline.project: sockshop
        pipeline.stage: prod-stable
        sidecar.istio.io/inject: "false"
        support.channel: '#support-sockshop-frontend'
        support.contact: jane.smith@sockshop.com
      labels:
        app.kubernetes.io/name: front-end
        app.kubernetes.io/version: "1.4"
        app.kubernetes.io/part-of: sockshop
        app: front-end.stable
        product: sockshop
        release: stable
        stage: prod
        tier: frontend
        version: "1.4"
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
