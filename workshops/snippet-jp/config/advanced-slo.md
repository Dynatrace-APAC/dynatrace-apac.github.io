カスタムメトリックを作成し、それに対するSLOの設定をしていきます。本手順により標準では提供されていない特定のリクエスト毎のSLOを設定することが可能になります。

### カタログページのメトリック作成

カタログページへのリクエスト成功数のカスタムメトリックを作成します。

* ナビゲーションメニューから**アプリケーションとマイクロサービス > サービス**を開きます。
* フィルタリング基準で**タグ > \[Kubernetes\]stage >prod**を入力し、`front-end`サービスをクリックします。

![service](../assets/cloud-observe/jp/service1.gif)

* **ダイナミック リクエストの表示**をクリックします。
* スクロールダウンして、**/catalogue**リクエストで、**graphアイコン（分析ビューの作成）**を選択します。
* メトリックの下のドロップダウンメニューから**成功した要求の数**を選択し、**メトリックの作成**ボタンをクリックします。
* メトリック名に`cataloguesuccesscount`を入力し、**メトリックの作成**ボタンをクリックします。

![custom-metric](../assets/cloud-observe/jp/custom-metric1.gif)

* メトリックの下のドロップダウンメニューから**リクエスト数**に変更し、**メトリックの作成**ボタンをクリックします。
* メトリック名に`cataloguecount`を入力し、**メトリックの作成**ボタンをクリックします。

* **Metric** の下のドロップダウンメニューで、**request count** を変更して選択します。
* **Create metric** をクリックしてください。
* Metric nameのところで、`cataloguecount`とします。
* **Create metric** をクリックしてください。

![custom-metric](../assets/cloud-observe/jp/custom-metric2.gif)

### カタログ用SLOの作成

カスタムメトリックを作成したので、その2つを使用したSLOを設定していきます。

1. メニューから**クラウドオートメーション > サービス レベル目標**を開きます。
1. 画面右上の**新しいSLOの追加**ボタンをクリックします。
1. **このSLOに名前付け**に任意の名前を入力します（例：`Catalogue Availability SLO`）
1. メトリック式には`(100)*(calc:service.cataloguesuccesscount:splitBy():sum)/(calc:service.cataloguecount:splitBy():sum)`と入力します。
1. フィルターの定義では時間枠のフィルターは`-5m`（過去5分間）を指定します。
1. エンティティ セレクターはデフォルトで入力されているものを削除します。
1. 成功条件の追加はデフォルトのままとします。
1. **評価**ボタンをクリックし、グラフが表示されることを確認します。
1. **作成**ボタンをクリックして、SLOを作成します。

![slo](../assets/cloud-observe/jp/advanced-slo.png)
