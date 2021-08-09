### Restart Sockshop Sample App

You can see the various processes automatically detected but Dynatrace prompts you to restart them. This is required for us to automatically instrument them without code changes.

Run the below command to cycle through your services for **Dev** and **Production**.

```bash
kubectl delete pods --all -n dev
kubectl delete pods --all -n production
```
