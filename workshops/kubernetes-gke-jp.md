id: kubernetes-gke-jp
summary: Dynatraceを用いたGKEにおけるフルスタック・オブザーバリティの実現方法
categories: kubernetes, gke, application-microservices-monitoring
tags: kubernetes, Beginner
status: Published 
authors: Brandon Neo
Feedback Link: mailto:DT-TYO-SE@dynatrace.com
Analytics Account: UA-175467274-1

# GKEにおけるKubernetesのフルスタック・オブザーバビリティ
<!-- ------------------------ -->
## はじめに
Duration: 1

このリポジトリには、Hands-On Kubernetes Sessionのラボが含まれています。このハンズオンでは、Google Kubernetes Engine (GKE)を使用しますが、他のPaaSプラットフォームでも使用可能です。

ハンズオンの目的は、参加者のためにステップを自動化し、シームレスにすることです。

### 事前準備
- DynatraceのAccount. フリートライアルの申し込み [here](https://www.dynatrace.com/trial/)
- Google Cloud アカウント
- Chrome ブラウザ

### 学習内容
- Helm Chartを介したDynatrace OperatorのKubernetesへの展開
- DynatraceでのKubernetes統合のセットアップ
- Dynatraceでのアーリーアクセス機能フラグの有効化
- DynatraceでのKubernetesビューの発見

<!-- ------------------------ -->
## Google Kubernetes Engine (GKE)の設定
Duration: 5

### Google Cloud Platform アカウントの登録

https://cloud.google.com/free/ にアクセスして、既存のGoogleアカウントで無料のGCPアカウントを登録してください。

Googleアカウントをお持ちでない方は、新規にご登録いただくことも可能です。

サインアップすると、GCPアカウントに関連付けられた無料のクレジットが得られます。12ヶ月分＋30,000円

GCPコンソールへのログインは[こちら](https://console.cloud.google.com/home/)から行えます。

![GCP-Homepage](assets/k8s/Picture1.png)

### Kubernetes Engine APIの有効化 

また、Kubernetes Engine APIで**API Billing**を有効にする必要があります。

![k8s-Engine](assets/k8s/Picture3.png)

GKEインスタンスをセットアップする際に、請求書作成ページが表示されるはずです。

表示されない場合は、[こちら](https://support.google.com/googleapi/answer/6158867?hl=en)の手順に従ってください。

### クラウドシェルの起動

![GKE-Menu](assets/k8s/Picture4.png)

右上の「ターミナル」アイコンをクリックします。

クラウドベースのターミナルのようなものがページの下に表示されます。

GKEクラスタの設定を開始します。

### 3. GKE クラスタの作成

![GKE-CLI](assets/k8s/Picture5.png)

以下のコマンドで、Ubuntuが動作する**k8sworkshop**という名前のGKEクラスターをGKEで作成します。
また、Dynatrace Activegate用のコンピュートVMも作成します。Dynatrace ActivegateはKubernetesとの連携に使用します。

```bash
gcloud container clusters create k8sworkshop --image-type=ubuntu --zone australia-southeast1-a
gcloud compute instances create dynatrace-activegate --image-family ubuntu-1604-lts --image-project ubuntu-os-cloud --zone australia-southeast1-a
```

これで、GKEクラスタが動作するようになります。

![GKE-CLI](assets/k8s/gcp.png)
**kubectl get nodes**を実行すると、ノードの数が表示されます。

<!-- ------------------------ -->
## Kubernetesインテグレーションの設定
Duration: 5

Kubernetesインテグレーションの公式説明書[こちら](https://www.dynatrace.com/support/help/technology-support/cloud-platforms/kubernetes/monitoring/connect-kubernetes-clusters-to-dynatrace/)の通り、まずEnvironment Activegateをセットアップする必要があります。

### Dynatrace-Activegate ターミナルに SSH で入り、Activegate をインストールする。


1. Google Cloud の左のナビゲーションバーで、**Compute Engine** -> **VM instances** に移動します。
![Activegate-connected](assets/k8s/activegate-0.png)

2. dynatrace-activegate**の行にあるSSHボタンをクリックして、インスタンスにSSH接続します。
![Activate-connected](assets/k8s/activegate.png)

2. Dynatrace内で、左メニューの「Deploy Dynatrace」をクリックします。
3. ページ下部の「Install Activegate」をクリックします。
4. "Linux "をクリックします。
5. 手順2をコピーし、ターミナルに貼り付ける。
6. 6. 手順4をコピーして、"sudo"(rootとしてインストール)をターミナルに追加する
![Copy-AG-Commands](assets/k8s/activegate-2.png)

インストールが完了すると、「Deployment Status」にActivegateが表示されます。

![Activegate-connected](assets/k8s/Picture9.1.png)

### K8S概要ダッシュボードの設定

[設定] -> [プロセスとコンテナ] -> [プロセスグループの検出] -> [クラウドアプリケーションとワークロードの検出の有効化]に進みます。

![Enable Cloud Workload](assets/k8s/enablecloud.png)

公式の[documentation page](https://www.dynatrace.com/support/help/technology-support/cloud-platforms/kubernetes/installation-and-operation/further-integrations/connect-your-kubernetes-clusters-to-dynatrace/)に記載されている手順を自動化し、API URLとベアラートークンをAPI経由で自動的に提供しました。メインのCloud Shellターミナルに戻り、以下を入力します。

``` bash
wget -O- https://raw.githubusercontent.com/Dynatrace-APAC/Workshop-Kubernetes/master/setup-k8s-ui.sh | bash
```
上記の結果を受けて、**Settings** -> **Cloud and Virtualization** -> **Kubernetes**に値を入力します。

![K8S-integration](assets/k8s/activegate-4.png)
1. GKE K8S」など、接続の名前を付けます。
2. Kubernetes APIのURLを入力してください。
   - SSHターミナルからKubernetes APIのURLをコピーします。
3. 3.Kubernetes Bearer Tokenの入力
   - SSHターミナルからBearer Tokenをコピーします。
4. "Require valid certificates for communication with API server "を無効にする。
5. 別のイベントフィールドセレクタを追加
6. フィールドセレクターの名前を以下のようにしてください。
`Hipster shop`
7. フィールドセレクタの式を以下のようにしてください。
`metadata.namespace=hipster-shop`
8. Save and Click on Connect

接続に成功したら、左メニューの「Kubernetes」をクリックし、KubernetesのUIを見てみましょう。

![K8S-integration](assets/k8s/k8s.png)
<!-- ------------------------ -->
## Dynatrace OneAgent Operator for Kubernetesのインストール
Duration: 5

1. Google Cloud Consoleの左のナビゲーションバーにあるKubernetes Engine -> Applicationsに進みます。
2. "Deploy From Marketplace "をクリックします。
3. 上記の検索欄で「Dynatrace」と検索します。
![Activegate-connected](assets/k8s/operator.png)
4. Dynatrace OneAgent Operator」をクリックし、「Configure」をクリックします。
5. 以下のフィールドに入力します。
- API URL
DynatraceのURLをコピーして、最後に **"/api "**を追加します。
![API-URL](assets/k8s/operator-1-withURL.png)

- API Token
Settings -> Integration -> Dynatrace APIから作成します。
  - Access problem and event feed, metrics, and topology toggle を有効にする。
  - 設定情報の書き込みを有効にするのトグル（次のステップのActivegateの設定に必要）。
![API-Token](assets/k8s/api-token.png)

- PaaS token
Settings -> Integration -> Platform as a Service から作成します。
![PaaS-Token](assets/k8s/paas-token.png)

GCPコンソールに値をコピーする
![Activegate-connected](assets/k8s/operator-1.png)

6. Deployをクリックします。
![Activegate-connected](assets/k8s/operator-2.png)

完了したら、左パネルの「ホスト」をクリックすると、接続されているK8Sノード（3ノード）が表示されます。 

![GKE-Hosts](assets/k8s/Picture7.1.png)
<!-- ------------------------ -->
# # Hipster Shopの立ち上げ
Duration: 5

今回のハンズオンでは、Googleのサンプルアプリケーションである<a href="https://github.com/GoogleCloudPlatform/microservices-demo">Hipster Shop</a>を起動する必要があります。

### Hipster Shopの実行

```bash
wget -O- https://raw.githubusercontent.com/Dynatrace-APAC/Workshop-Kubernetes/master/deploy.sh | bash
```

デプロイされたら、GCPからフロントエンドのエンドポイントを見つけることができます（**Kubernetes Engine -> Services & Ingress**）。

![JSON](assets/k8s/Picture10.png)

実行すると、露出したfrontend-external IPにアクセスしてHipster Shopに行くことができます。

![JSON](assets/k8s/hipstershop.png)

<!-- ------------------------ -->
## Dynatraceを知る
Duration: 5

### サービスの自動検出

Dynatraceの「トランザクションとサービス」で、自動検出された5つのサービスを見ることができます。
![Discovered Services](assets/k8s/lab5-autodiscoveredservices.png)

いくつかのサービスは発見されていますが、<a href="https://github.com/GoogleCloudPlatform/microservices-demo#service-architecture">Hipster Shop's Service architecture</a>と一致しないものもあることがわかります。
Hipster Shopは最先端のテクノロジー（GPRCなど）を使用しており、Dynatraceはクラウドの絶え間ない進化をサポートしています。

![Architecture](assets/k8s/architecture-diagram.png)

### OneAgentでの追加機能の利用

OneAgentは急速に変化しているため、アーリーアクセスの機能はデフォルトでは自動的に有効になっていません。
これは、本番環境に影響を与えるような不測の事態を避けるためです。今回のワークショップでは、これらの機能を有効にすることができます。**Settings -> Service-side service monitoring -> Deep Monitoring -> New Oneagent Features**にアクセスします。

Global Settingsで、以下の機能フラグを有効にします。これらは異なるページにあるので、ページを切り替える必要があります。

検索フィルターバーを使用して、**"GRPC "**を検索することができます。
![GRPC-Features](assets/k8s/lab5-b4EnableGRPC-settings.png )

![Features](assets/k8s/features.png)

2つの追加NodeJS機能フラグを含め、以下の機能がすべて有効になっていることを確認してください。
![All-Features](assets/k8s/all-features.png)

OneAgents の機能を有効にするには、ポッドの再起動が必要です。以下のコマンドを実行して、ポッドを再起動します。

```bash
kubectl delete pods --all -n hipster-shop
```
![Restart](assets/k8s/restart.png)

Dynatraceに戻り、「トランザクションとサービス」にアクセスして、更新されたサービスのリストを確認します。
![Discovered Services](assets/k8s/lab5-AfterEnableGRPC-settings.png)

":8080 "をクリックし、続いてService Flowをクリックすると、サービスが自動的に検出され、上のアーキテクチャ図と一致することがわかります。

![Service Flow](assets/k8s/serviceflow.png)

<!-- ------------------------ -->
## Kubernetes Viewを探る
Duration: 5

Kubernetes View内の様々な機能（Cluster Utilization、Cluster Workloads、K8S Eventsなど）を確認できます。

![KubernetesUI](assets/k8s/k8s-ui.png)

### Kubernetes Clusterの利用状況を分析する
   - マウスオーバーして、CPUとMemoryの使用率をMin / Maxで表示します。
   - Analyze Nodesをクリックすると、各ノードの詳細が表示されます。
   
![KubernetesUI](assets/k8s/cluster-util.png)

### Kubernetes Cluster Workloadsの分析 
   - Kubernetesコントローラに分散して実行されているワークロードとポッドに注目してください

![KubernetesUI](assets/k8s/cluster-workload.png)

### Kubernetesのイベントを分析する
   - イベントの種類に注目 BackOff, Unhealthy

![KubernetesUI](assets/k8s/events.png)

### Kubernetesの名前空間を分析する
   - hipster-shop**をクリックして、各種kubernetesサービス（クラウドアプリケーション）にドリルダウンします。

![KubernetesUI](assets/k8s/namespace.png)

### クラウドアプリケーションをクリックして探索
   - それぞれのサービスをクリックすると、そのサービスを支える技術が見えてきます。
   
![KubernetesUI](assets/k8s/cloud-apps.png)

<!-- ------------------------ -->

## アンケート
Duration: 3

このラボを楽しんでいただき、お役に立てれば幸いです。ご意見、ご感想をお待ちしております。
<form>
  <name>このラボでの全体的な経験はどうでしたか？</name>
  <input value="とても良い" />
  <input value="良い" />
  <input value="普通" />
  <input value="悪い" />
  <input value="とても悪い" />
</form>

<form>
  <name>このラボで最も役立ったことは何ですか？</name>
  <input value="OneAgent Operatorを使ってKubernetesにデプロイする方法" />
  <input value="Kubernetesの統合の設定" />
  <input value="Kubernetesのプロセスグループネーミングとサービスネーミング" />
  <input value="DynatraceにおけるKubernetesの理解" />
</form>

<form>
  <name>このラボを友人や同僚に勧める可能性はどの程度ありますか？</name>
  <input value="強く勧めたい" />
  <input value="勧めたい" />
  <input value="わからない" />
  <input value="勧めない" />
  <input value="全く勧めない" />
</form>

Positive
: 💡 その他のアイデアや提案については、[メールでのお問い合わせ](mailto:DT-TYO-SE@dynatrace.com?subject=GKEにおけるKubernetesのフルスタック・オブザーバビリティ - アイデア、提案) をお願いします。