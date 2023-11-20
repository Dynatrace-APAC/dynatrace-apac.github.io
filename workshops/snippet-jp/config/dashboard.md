ここではダッシュボードにSLOウィジェットを作成し、監視する方法について説明します。

 * ナビゲーションメニューから**観察と探索 > ダッシュボード**もしくは**お気に入り > ダッシュボード**を開きます。

Dynatraceには、**Kubernetes workload overview**や**Real User Monitoring**など複数のダッシュボードがあらかじめ用意されています。

### 新規ダッシュボードの作成

ここでは、既存のダッシュボードではなく

* ダッシュボードのリスト画面から**ダッシュボードの作成**ボタンをクリックします。
* ダッシュボード名に任意の名前（例：`SRE向けダッシュボード`）を入力し、**作成**をクリックします。

![dashboard](../assets/cloud-observe/jp/dashboard1.gif)

### SLOタイルの追加

* ダッシュボードの編集でタイルのフィルタリングに`slo`と入力することでサービスレベル目標タイルだけが表示されます。
* **サービスレベル目標**をダッシュボードへドラッグアンドドロップします。
* SLOの選択では`EasytravelService Availability SLO`を選択します。
* ダッシュボード上の何もタイルが置かれていないところをクリックし、同様の手順で`JourneyService Availability SLO`と`findJourneys Availability SLO`も追加します。

![dashboard](../assets/cloud-observe/jp/dashboard2.gif)

### グラフ・ウィジェットの追加 - SLOステータスのグラフ表示

* ダッシュボード上の何もタイルが置かれていないところをクリックし、**グラフ**タイルを任意の場所にドラッグアンドドロップします。
* **データ エクスプローラーでタイルを設定**をクリックします。
* **メトリックを選択してください**と記載されているところに`func:slo`と入力し、`EasytravelService Availability SLO`を選びます。
* **クエリの実行**ボタンをクリックします。

* 右側の設定項目から**形式**を**0.00**を選択し、**軸 > Y axis 1**の最小、最大に`90, 100`と入力し、**しきい値**に以下の値を入力します。
    - 🟩 ≥ `99`
    - 🟨 ≥ `98`
    - 🟥 ≥ `0`

![data-explorer](../assets/cloud-observe/jp/data-explorer.png)

* **変更をダッシュボードに保存します**ボタンをクリックします。
* **タイトル**を`EasytravelService Availability SLO`に変更します。
* 同様に**JourneyService**と**findJourneys**に関してもグラフを追加します。
* **findJourneys**に関しては**形式**を**0.00**を選択し、**軸 > Y axis 1**の最小、最大に`99, 100`と入力し、**しきい値**に以下の値を入力します。
    - 🟩 ≥ `99.99`
    - 🟨 ≥ `99.98`
    - 🟥 ≥ `0`


### グラフ・ウィジェットの追加 - バーンレートのグラフ表示

* **グラフ**タイルをダッシュボードの任意の場所にドラッグアンドドロップします。
* **データ エクスプローラーでタイルを設定**をクリックします。
* **メトリックを選択してください**と記載されているところに`func:slo.errorBudgetBurnRate`と入力し、`Error budget burn rate - EasytravelService Availability SLO`を選びます。
* **クエリの実行**ボタンをクリックします。

![dashboard](../assets/cloud-observe/jp/burn-rate.png)

* **変更をダッシュボードに保存します**ボタンをクリックします。
* **タイトル**を`EasytravelService Error budget burn rate`などに変更し、**完了**をクリックします。
* 同様に**JourneyService**と**findJourneys**に関してもグラフを追加します。

編集が完了したら、**完了**ボタンをクリックします。

![dashboard](../assets/cloud-observe/jp/dashboard3.png)
