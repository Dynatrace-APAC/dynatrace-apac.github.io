<!-- Code for k8s Settings-->

### Configuring Kubernetes integration

In Dynatrace, go to **Settings > Cloud and virtualization > Kubernetes**

Follow the configuration steps below:

* Click on the **pencil icon** to edit the automatically created Kubernetes integration
* **Toggle ON** Monitor annotated Prometheus exporters
* **Toggle ON** Monitor events
* **Toggle ON** Include all events relevant for Davis
* Click on **Save**

![settings](../assets/k8s/k8s-configure.gif)

### Annotate Prometheus exporter pods

Back in the shell terminal, run the follow command to annotate the pods for **Prometheus scraping**

The command will annotate Prometheus metrics for pods within **Production** namespace

`kubectl annotate po -n production --all --overwrite metrics.dynatrace.com/scrape=true`

Positive
: The above steps are based on the official documentation [here](https://www.dynatrace.com/support/help/how-to-use-dynatrace/infrastructure-monitoring/container-platform-monitoring/kubernetes-monitoring/monitor-prometheus-metrics/#annotate-prometheus-exporter-pods) 