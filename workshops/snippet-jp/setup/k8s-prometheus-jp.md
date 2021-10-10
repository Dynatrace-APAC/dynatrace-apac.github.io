<!-- Code for k8s Prometheus-->

アプリケーションには、Nginx、Redis、RabbitMQ、MySQL、MongoDBなどのサードパーティのテクノロジーが使用されていることが多く、これらについては、メトリクスの観点からさらなる洞察が必要となります。

Dynatraceでは、Prometheusと同じ形式のメトリクスを取り込むことができ、マイクロサービスやポッドのより大きな文脈の中にそれらを取り込み、これらのメトリクスの**自動適応ベースライン化**による**強化されたアラート**を可能にします。

さらに良いことに、Dynatraceはスクレイピングを実行してくれるので、**Prometheusサーバーは必要ありません**。

![Prometheus scraping](../assets/k8s/prometheus-scrap.png)

### Prometheusエクスポーターポッドのアノテーション

シェルターミナルに戻り、次のコマンドを実行して、ポッドに**Prometheus scraping**のためにannotationを付与します。

このコマンドは、**Production** namespace内のポッドにアノテートを付与します。

```bash
kubectl annotate po -n production --all --overwrite metrics.dynatrace.com/scrape=true
kubectl annotate po -n production --all --overwrite metrics.dynatrace.com/port=8080
```

Positive
: 上記手順に関する詳細は[公式サイト](https://www.dynatrace.com/support/help/how-to-use-dynatrace/infrastructure-monitoring/container-platform-monitoring/kubernetes-monitoring/monitor-prometheus-metrics/#annotate-prometheus-exporter-pods)を参照ください。

### メトリクスの探索

1～2分ほど待ってから、Dynatrace UIで
- **Metrics** ビューに移動します。
- **filter** テキストボックスに`process_`と入力し、**"Enter "**キーを押してください。
- いくつかの異なるメトリクスが表示されます。これらはPrometheusから抜粋したものです。何も表示されない場合は、もう少し待ってから、画面を更新して再度試してみてください。

![Metrics browser](../assets/k8s/prometheus-metrics1.png)

- いずれかの項目をクリックすると、収集されたメトリクスを **dimensions** も含めて調べることができます。Dynatraceは、Kubernetesのワークロード、ネームスペース、ノードなどを **自動的に関連付け**、これを原因究明エンジンに送り込んでいることに注目してください。

![Metrics browser](../assets/k8s/prometheus-metrics2.png)

これで、これらのメトリクスを簡単に **チャート化** したり、** ダッシュボード** を作成することができます。ダッシュボードのサンプルは後ほどハンズオンでご紹介しますので、参考にしてください。

さらに、メトリクスに基づいてアラートを作成することもできます。

### メトリクスに基づくカスタムアラートを取得する方法

Negative
: カスタムアラートはハンズオンの範囲ではありませんが、セッション終了後、ご自身で自由にお試しいただけます。

PrometheusのメトリクスをDynatraceで利用できるようになりました。サードパーティ製ではありますが、これらのメトリクスもDynatraceのメトリクスとして扱われます。

Dynatraceネイティブのメトリクスと同じように、チャートやダッシュボードでそれらのメトリクスを視覚化できることはすでに見てきました。

しかし、もちろん、ダッシュボードを見ることに毎日を費やしたくはないでしょう。欲しいのは、キャプチャしているメトリクスに関連する異常があった場合に通知されることです。

メトリクスに対して[custom Anomaly Detection rule](https://www.dynatrace.com/support/help/how-to-use-dynatrace/problem-detection-and-analysis/problem-detection/metric-events-for-alerting/)を定義することができます。

セッション終了後、お時間のある方はぜひお試しください。