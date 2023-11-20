本セッションでは、カスタムメトリックを作成し、それに対するSLOの設定をしていきます。本手順により標準では提供されていない特定のリクエスト毎のSLOを設定することが可能になります。

### カタログページのメトリック作成

カタログページへのリクエスト成功数のカスタムメトリックを作成します。s

* ナビゲーションメニューから**アプリケーション オブザーバビリティ > サービス**を開きます。
* `JourneyService`サービスをクリックします。
* **Key requests/endpoints**の右側にある**Top web requests**ボタンをクリックします。

![requests](../assets/cloud-observe/jp/requests.gif)


* **findJourneys**リクエストの右側にある**フィルターアイコン**をクリックします。
* **メトリックの作成**ボタンをクリックします。
* メトリック名に`findjourneyscount`を入力し、**メトリックの作成**ボタンをクリックします。

![custom-metric](../assets/cloud-observe/jp/custom-metric1.gif)

* メトリックの下のドロップダウンメニューから**成功した要求の数**を選択し、**メトリックの作成**ボタンをクリックします。
* メトリック名に`findjourneyssuccesscount`を入力し、**メトリックの作成**ボタンをクリックします。

![custom-metric](../assets/cloud-observe/jp/custom-metric2.gif)

### findJourneys用SLOの作成

カスタムメトリックを作成したので、その2つを使用したSLOを設定していきます。

1. メニューから**オートメーション > サービス レベル目標**を開きます。
1. 画面右上の**新しいSLOの追加**ボタンをクリックします。
1. **このSLOに名前付け**に任意の名前を入力します（例：`findJourneys Availability SLO`）
1. メトリックを定義を開きます。メトリック式には`(100)*(calc:service.findjourneyssuccesscount:splitBy():sum)/(calc:service.findjourneyscount:splitBy():sum)`と入力します。
1. フィルターの定義を開きます。時間枠のフィルターは`-5m`（過去5分間）を指定します。
1. エンティティ セレクターは空白のままにします。
1. 成功条件の追加はデフォルトのままにしておきます。
1. **評価**ボタンをクリックし、グラフが表示されることを確認します。
1. **作成**ボタンをクリックして、SLOを作成します。

![slo](../assets/cloud-observe/jp/advanced-slo.png)
