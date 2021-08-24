<!-- Code for k8s Prometheus-->

### Annotate Prometheus exporter pods

Back in the shell terminal, run the follow command to annotate the pods for **Prometheus scraping**

The command will annotate Prometheus metrics for pods within **Production** namespace

```bash
kubectl annotate po -n production --all --overwrite metrics.dynatrace.com/scrape=true
kubectl annotate po -n production --all --overwrite metrics.dynatrace.com/port=8080
```

Positive
: The above steps are based on the official documentation [here](https://www.dynatrace.com/support/help/how-to-use-dynatrace/infrastructure-monitoring/container-platform-monitoring/kubernetes-monitoring/monitor-prometheus-metrics/#annotate-prometheus-exporter-pods) 