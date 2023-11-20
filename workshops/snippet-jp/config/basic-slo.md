基本的なサービスレベル目標(SLO)を設定していきます。Dynatraceが標準で監視しているメトリクスを用いることで簡単に、サービスレベル指標（SLI）を定義し、その閾値としてSLOを設定することができます。

### SLOの作成 (EasytravelService)

以下の手順で進めてください。

* メニューから**オートメーション > サービス レベル目標**を開きます。
* 画面右上の**新しいSLOの追加**ボタンをクリックします。

![basic-slo](../assets/cloud-observe/jp/basic-slo1.gif)

ここでは、easyTravelアプリケーションの`easytravel-angular-frontend`デプロイメント上で稼働している`EasytravelService`に対する可用性に関するSLOを作成します。

1. **サービスレベル可用性ボタン**をクリックします。
1. **この service-level availabilitySLO に名前付け**で、このSLOに名前を付けます：`EasytravelService Availability SLO`
1. メトリックを定義を開きます。メトリック名やメトリック式はデフォルトのままにしておきます。
1. フィルターの定義を開きます。時間枠のフィルターは`-1h`（過去1時間）を指定します。
1. エンティティ セレクターは`type("SERVICE"), tag("[KUBERNETES]app:easytravel-k8s"), tag("[KUBERNETES]type:angular-frontend"), webServiceName("EasytravelService")`を入力し、**プレビュー**をクリックします。エンティティが1つ表示されていることを確認します。
1. 成功条件の追加を開きます。ターゲットを`98`%、警告を`99`%に設定します。
1. **評価**ボタンをクリックし、グラフが表示されることを確認します。
1. **作成**ボタンをクリックして、SLOを作成します。

![basic-slo](../assets/cloud-observe/jp/basic-slo2.png)

サービスレベル目標が作成され、現在のステータスなどが確認できます。

![basic-slo](../assets/cloud-observe/jp/basic-slo3.png)

### SLOの作成 (JourneyService)

もう1つ`JourneyService`に対する可用性に関するSLOを作成します。

サービスレベル目標のページを開き、**新しいSLOの追加**ボタンをクリックします。

1. **サービスレベル可用性ボタン**をクリックします。
1. **この service-level availabilitySLO に名前付け**で、このSLOに名前を付けます：`JourneyService Availability SLO`
1. メトリックを定義を開きます。メトリック名やメトリック式はデフォルトのままにしておきます。
1. フィルターの定義を開きます。時間枠のフィルターは`-1h`（過去1時間）を指定します。
1. エンティティ セレクターは`type("SERVICE"), tag("[KUBERNETES]app:easytravel-k8s"), tag("[KUBERNETES]type:backend"), webServiceName("JourneyService")`を入力し、**プレビュー**をクリックします。エンティティが1つ表示されていることを確認します。
1. 成功条件の追加を開きます。ターゲットを`98`%、警告を`99`%に設定します。
1. **評価**ボタンをクリックし、グラフが表示されることを確認します。
1. **作成**ボタンをクリックして、SLOを作成します。

### SLO作成の確認

2つのサービス可用性に関するSLOがこれで定義できました。

![basic-slo](../assets/cloud-observe/jp/basic-slo4.png)

Dynatraceでは、エラーバジェットやバーンレートに関するメトリクスも自動で生成されますので、ダッシュボードで管理を簡単に行うことができるようになります。

また、本ハンズオンには含まれておりませんが**サービスのパフォーマンス**やフロントエンドの**ユーザーエクスペリエンス**などもSLOとして定義することが可能です。

Positive
: 詳しいドキュメントは[こちら](https://docs.dynatrace.com/docs/shortlink/objectives-hub)を参照ください。
