<!-- Code for k8s Container Environment Variables-->
Duration: 5

### Adding Environment variables

In shell terminal, add some Environment Variables with the following command

`nano ~/easyTravel/manifests/frontend/angularfrontend-deployment.yaml`

**Make sure that the indentation is correct and that they aren't any error promptings**

```bash
        env:
        - name: DT_TAGS
          value: "product=sockshop"
        - name: DT_CUSTOM_PROP
          value: "SERVICE_TYPE=FRONTEND"
```
![JSON](../assets/k8s/Picture13.png)

Save the amended file with **Ctrl-X**, followed by **Y** and **Enter** and run the below command to re-apply the change.

```bash
kubectl apply -f ~/easyTravel/manifests/frontend/angularfrontend-deployment.yaml
wget -O- https://raw.githubusercontent.com/Dynatrace-APAC/Workshop-Kubernetes/master/recycle-sockshop-frontend.sh | bash
```

### Validate

Once working, you can validate the change in Dynatrace

![JSON](../assets/k8s//Picture14.png)

