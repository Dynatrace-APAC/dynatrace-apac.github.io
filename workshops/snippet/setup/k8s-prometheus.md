<!-- Code for k8s Prometheus-->

### Import Prometheus Metrics
Applications often use 3rd-party technologies, such as Nginx, Redis, RabbitMQ, MySQL and MongoDB, for which additional insights in terms of metrics are required.

Dynatrace provides you the ability to ingest those same Prometheus format metrics, bringing them in the larger context of the microservices and pods, and allowing for **enhanced alerting** with **auto-adaptive baselining** of these metrics.

And even better, Dynatrace will scrape the metrics at the source, so you **don't even need a Prometheus server**.

![Prometheus scraping](../assets/k8s/prometheus-scrap.png)

### Annotate Prometheus exporter pods

Back in the shell terminal, run the follow command to annotate the pods for **Prometheus scraping**

The command will annotate Prometheus metrics for pods within **Production** namespace

```bash
kubectl annotate po -n production --all --overwrite metrics.dynatrace.com/scrape=true
kubectl annotate po -n production --all --overwrite metrics.dynatrace.com/port=8080
```

Positive
: The above steps are based on the official documentation [here](https://www.dynatrace.com/support/help/how-to-use-dynatrace/infrastructure-monitoring/container-platform-monitoring/kubernetes-monitoring/monitor-prometheus-metrics/#annotate-prometheus-exporter-pods)

### Explore the metrics

You will have to wait one or two minutes and then, in the Dynatrace UI:
- Go to the **Metrics** view
- In the **filter** text box, type `process_`, and hit **"Enter"** key
- A few different metrics should be displayed. These are scrapped from Prometheus. If you don't get anything, wait a bit more, refresh the screen and try again

![Metrics browser](../assets/k8s/prometheus-metrics1.png)

- Go ahead to click on any one of them to explore the metrics collected, including the **dimensions**. Notice that Dynatrace **automatically associates** the Kubernetes workloads, namespace, nodes etc., and feeds this into the causation engine!

![Metrics browser](../assets/k8s/prometheus-metrics1.png)

You can now easily **chart** these metrics and also **create dashboards**. We will provide a sample dashboard later in the hands on for your reference.

In addition, you can also create an alert based on the metrics.

### How to get custom alerts based on the metrics?

***Note: Custom alerts is not part of the scope of the hands-on, but you are free to try this on your own, after the session.

Now you have access to Prometheus metrics in Dynatrace, even though they come from a 3rd party, those metrics are now first class citizens in Dynatrace.

We have already seen that you can visualize those metrics in charts and dashboards all the same as Dynatrace-native metrics.

But, of course, you don't want to spend your days looking at dashboards. What you want is to be notified if there's an anomaly related to the metrics you are capturing.

You can define a [custom Anomaly Detection rule](https://www.dynatrace.com/support/help/how-to-use-dynatrace/problem-detection-and-analysis/problem-detection/metric-events-for-alerting/) for your metric.

Please give this a try if you have time after the session.