id: kubernetes-jp
summary: kubernetes環境のフルスタック・オブザーバリティの実現方法
categories: kubernetes, application-microservices-monitoring
tags: kubernetes, beginner
status: Published 
authors: Brandon Neo
Feedback Link: mailto:DT-TYO-SE@dynatrace.com
Analytics Account: UA-175467274-1

# DynatraceとKubernetes
<!-- ------------------------ -->
## はじめに
Duration: 1

このリポジトリには、Hands-On Kubernetes Sessionのラボが含まれています。今回のハンズオンでは、AWSで動作するKubernetesインスタンスを使用しますが、他のプラットフォームでも動作します。

ハンズオンのために、参加者のためにステップを自動化し、シームレスにします。

### 事前準備
* DynatraceのAccount. フリートライアルの申し込み [here](https://www.dynatrace.com/trial/)
* Chrome ブラウザ
* SSH クライアント [mobaxterm](https://mobaxterm.mobatek.net/)

### 学習内容
- Helm Chartを介したDynatrace OperatorのKubernetesへの展開
- DynatraceにKubernetesを統合する
- Kubernetesの自動ダッシュボードを見る
- Kubernetesのラベルとアノテーション
- Kubernetesのプロセスグループネーミングとサービスネーミング
- DynatraceでKubernetesビューを見る

<!-- ------------------------ -->
##  Dynatrace ワンエージェントオペレーターのインストール
Duration: 15

メール内のリンクをクリックして、割り当てられたDynatraceテナントにログインします。

**Deploy Dynatrace -> Start installation -> Kubernetes**を開きます。

![k8s-setup](assets/k8s/k8s-setup.png)

### Helm 3のインストール

OneAgent Operatorは、様々なkubernetesのデプロイメント戦略によってインストールすることができます。UIからの指示に従って、まずHelm 3をセットアップします。 

以下のコマンドを使用して、Helm 3をインストールします。プロンプトが表示されたら、ウェルカムメールに記載されている**adminパスワード**を入力します。

`sudo snap install helm --classic`

### DynatraceでAPIとPaaSのトークンの取得
画面の指示に従って、APIトークンとPaaSトークンを作成してください。
APIトークンには必要なパーミッションが自動的に設定されています。
作成後、両方の**drop-down fields**から**newly created tokens**を選択してください。
その他のコマンドにも必要なステップがあらかじめ入力されています。

![k8s-setup](assets/k8s/api-paas-ui.gif)

### OneAgent Helm Repoの追加
セットアップの手順に従って、以下のコマンドを実行してください。

```bash
helm repo add dynatrace https://raw.githubusercontent.com/Dynatrace/helm-charts/master/repos/stable
kubectl create namespace dynatrace
```
### values.yamlの作成と適用

以下のコマンドを実行して、**values.yaml**という新しいファイルを作成します。

`nano values.yaml`

内容をコピーしてDyntraceのテナントに貼り付け、**Ctrl-X**の後に**Y**と**Enter**でファイルを保存します。

**EXAMPLE**

```bash
platform: "kubernetes"

oneagent:
  name: "oneagent"
  apiUrl: "https://mou612.managed-sprint.dynalabs.io/e/<ENVIRONMENT ID>/api"
  args:
    - --set-app-log-content-access=true
  skipCertCheck: false
  enableIstio: true

secret:
  apiToken: "<API KEY>"
  paasToken: "<PAAS KEY>"
```
次のコマンドをコピーして、**values.yaml**を適用し、**OneAgentをKubernetesにデプロイ**します。

```bash
helm install dynatrace-oneagent-operator \
dynatrace/dynatrace-oneagent-operator -n\
dynatrace --values values.yaml
```
成功すれば、以下のように**STATUS: deployed**というポジティブな出力が得られるはずです。

```bash
NAME: dynatrace-oneagent-operator
LAST DEPLOYED: Thu Aug 13 01:25:26 2020
NAMESPACE: dynatrace
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Thank you for installing dynatrace-oneagent-operator.

Your release is named dynatrace-oneagent-operator.
```

POSITIVE
: また、他のインストール手段については、[公式ドキュメントページ](https://www.dynatrace.com/support/help/technology-support/cloud-platforms/google-cloud-platform/google-kubernetes-engine/deploy-oneagent-on-google-kubernetes-engine-clusters/)で確認できます。

### ⚠️ トラブルシューティングの手順

Negative
: **check status of pods**, 次のコマンドを実行してください。戻り値として**Running**が得られるはずです。<br>
`kubectl get pods -n dynatrace`

Negative
: **check the logs**, 次のコマンドを実行してください。<br>
`kubectl logs -f deployment/dynatrace-oneagent-operator -n dynatrace`

Negative
: **delete secrets**, 次のコマンドを実行してください。以前、間違ったシークレットを入れてしまったかもしれません。<br>
`kubectl delete secret --all -n dynatrace`

Negative
: **delete all pods**, 次のコマンドを実行してください。これでポッドが循環して、新しいポッドインスタンスができあがります。 <br>
`kubectl delete --all pods -n dynatrace`

Negative
: 公式トラブルシューティング [here](https://www.dynatrace.com/support/help/technology-support/cloud-platforms/google-cloud-platform/google-kubernetes-engine/installation-and-operation/full-stack/troubleshoot-oneagent-on-google-kubernetes-engine/)

すべてがうまくいっていれば、**Show Deployment status**をクリックすると、ホストが表示されます。

### Sockshopサンプルアプリの再起動

様々なプロセスが自動的に検出されているのがわかりますが、Dynatraceはそれらを再起動するよう促します。これは、コードを変更せずに自動的にインストルメントを行うために必要です。

以下のコマンドを実行して、**Dev**と**Production**のサービスを循環させます。

```bash
kubectl delete pods --all -n dev
kubectl delete pods --all -n production
```

<!-- ------------------------ -->
## Kubernetesインテグレーションの設定
Duration: 15

Positive
: 公式の指示によると [here](https://www.dynatrace.com/support/help/technology-support/cloud-platforms/kubernetes/monitoring/connect-kubernetes-clusters-to-dynatrace/) Kubernetesとの統合のためには、まず、**Environment Activegate**を設定する必要があります。

### アクティブゲートの設定 
- Dynatraceの左メニューにある**Deploy Dynatrace**をクリックします。
- ページの下部にある**Install Activegate**をクリックします。
- **Linux**をクリックします。
- **ステップ2**をコピーして、シェルターミナルにペーストします。
- **ステップ4**をコピーし、"**sudo**"を追加します。(rootとしてインストール)をシェルターミナルに追加します。
プロンプトが表示されたら、ウェルカムメールに記載されている**adminパスワード**を入力します。

![Copy-AG-Commands](assets/k8s/activegate-2.png)

完了すると、**Deployment Status**にActivegateが表示されます。

![Activegate-connected](assets/k8s/Picture9.1.png)

### K8S概要ダッシュボードの設定

**Settings -> Process and Containers -> Process group detection -> Enable Cloud Application and workload detection**を開きます。

![Enable Cloud Workload](assets/k8s/enablecloud.png)

### サービス・アカウントとクラスタ・ロールの作成

Kubernetes APIにアクセスするためのサービスアカウントとクラスタロールを作成します。これにより、Kubernetes APIでの認証に必要なベアラートークンが作成されます。シェルターミナルで以下のスニペットを使用します。

```bash
kubectl apply -f https://www.dynatrace.com/support/help/codefiles/kubernetes/kubernetes-monitoring-service-account.yaml
```
### Kubernetesインテグレーションのセットアップ

**Settings -> Cloud and Virtualization -> Kubernetes -> Connect new cluster**を開きます。

### Kubernetes APIのURLの取得

以下のコマンドを入力し、それをコピーして**Kubernetes API URL**に設定します。

```bash
kubectl get ing k8-api-ingress | grep -oP 'api.kubernetes[^[:blank:]]*'
```

### Bearer Tokenの取得

Kubernetes Bearer Token**のために、以下のコマンドを入力し、コピーしてください。

```bash
kubectl get secret $(kubectl get sa dynatrace-monitoring -o jsonpath='{.secrets[0].name}' -n dynatrace) -o jsonpath='{.data.token}' -n dynatrace | base64 --decode &
```

![K8S-integration](assets/k8s/activegate-4.png)

- 接続の名前を入力します。例：**Kubernetes**。
- Kubernetes API URL Target**」を入力します。
   - API URLの前に **https://** を付けます。
- Kubernetes Bearer Token**を入力します。
- APIサーバーとの通信に有効な証明書を要求する」を無効にします。
- 別のイベントフィールドセレクターを追加します。
- フィールドセレクターの名前には `Non-node` を使用します。
- フィールドセレクターの式には `involvedObject.kind!=Node` を使用し、 **Save** します。
- イベントの監視**をオンにする
- Connect**」をクリックします。

接続に成功したら、左メニューの「Kubernetes」をクリックし、KubernetesのUIを見てみましょう。

![K8S-integration](assets/k8s/k8s.png)

Positive
: 上記の手順は、私たちの[offical documentation page](https://www.dynatrace.com/support/help/technology-support/cloud-platforms/kubernetes/installation-and-operation/further-integrations/connect-your-kubernetes-clusters-to-dynatrace/)から引用しています。APIコマンドを実験用にアレンジしています。

<!-- ------------------------ -->
## Kubernetesのラベルとアノテーション
Duration: 5

Sockshopアプリを再起動すると、Dynatraceにサービスが表示されるようになります。

`~/dtacmworkshop/manifests/sockshop-app/production/front-end.yml`を参考にして、Dynatraceが自動的にアノテーションとラベルをピックアップするように設定したいと思います。

```bash
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: front-end.stable
  namespace: production
spec:
  replicas: 1
  template:
    metadata:
      annotations:
        sidecar.istio.io/inject: "false"
        dynatrace/instrument: "true"
        pipeline.stage: prod-stable
        pipeline.build: 1.4.0.7424
        pipeline.project: sockshop
        support.contact: "jane.smith@sockshop.com"
        support.channel: "#support-sockshop-frontend"
      labels:
        app: front-end.stable
        stage: prod
        release: stable
        version: "1.4"
        tier: "frontend"
        product: "sockshop"
```
### サービスアカウントの視聴者の役割

OneAgentはポッドサービスアカウントを使用して、Kubernetes REST APIを介してそのメタデータを照会します。
サービスアカウントにアクセスするには、**viewer role**が付与されている必要があります。
CLIで、**production project**に対して以下のコマンドを実行します。

```bash
kubectl create rolebinding serviceaccounts-view --clusterrole=view --group=system:serviceaccounts:production --namespace=production
```

この手順を **dev プロジェクト** にも繰り返してください。

```bash
kubectl create rolebinding serviceaccounts-view --clusterrole=view --group=system:serviceaccounts:dev --namespace=dev
```

Dynatraceが変更をピックアップするのを待ちます。

Negative
: ⚠️ アプリを手動でリサイクルするには、以下のコマンドを実行してください。
`wget -O- https://raw.githubusercontent.com/Dynatrace-APAC/Workshop-Kubernetes/master/recycle-sockshop-frontend.sh | bash`

### 検証

作業が完了したら、Dynatraceで変更を検証することができます。

![JSON](assets/k8s/Picture12.png)

Positive
: 上記の手順は、[当社の公式ドキュメントページ](https://www.dynatrace.com/support/help/technology-support/cloud-platforms/kubernetes/other-deployments-and-configurations/leverage-tags-defined-in-kubernetes-deployments/)から引用しています。

<!-- ------------------------ -->
## コンテナの環境変数
Duration: 5

### 環境変数の追加

シェルターミナルで以下のコマンドを実行して、環境変数を追加します。

`nano ~/dtacmworkshop/manifests/sockshop-app/production/front-end.yml`

**インデントが正しく行われているか、エラープロンプトが表示されていないかを確認してください。**

```bash
        env:
        - name: DT_TAGS
          value: "product=sockshop"
        - name: DT_CUSTOM_PROP
          value: "SERVICE_TYPE=FRONTEND"
```
![JSON](assets/k8s/Picture13.png)

修正したファイルを**Ctrl-X**、**Y**、**Enter**で保存し、以下のコマンドを実行して変更を再適用します。

```bash
kubectl apply -f ~/dtacmworkshop/manifests/sockshop-app/production/front-end.yml
wget -O- https://raw.githubusercontent.com/Dynatrace-APAC/Workshop-Kubernetes/master/recycle-sockshop-frontend.sh | bash
```

### 検証

作業が完了したら、Dynatraceで変更を検証することができます。

![JSON](assets/k8s//Picture14.png)

<!-- ------------------------ -->
## K8sのプロセスグループとサービスのネーミングルール
Duration: 10

### プロセスグループのネーミングルール

**Settings -> Processes and containers -> Process group naming**を開き、**Add a new rule**をクリックします。

ルールに名前を付けてください。例: **Kubernetes Project.Namespace.Container**

フォーマットの入力: `k8s-{ProcessGroup:Kubernetes:pipeline.project}.{ProcessGroup:KubernetesNamespace}.{ProcessGroup:KubernetesContainerName}`

条件のドロップダウンで、プロパティの**Kubernetes namespace**と条件の**exists**を選択します。

![JSON](assets/k8s/Picture15.png)

**Preview**をクリックすると、マッチングしたエンティティが表示されます。

![JSON](assets/k8s/Picture16.png)

ポップアップの**Create Rule**と**Save Changes**をクリックします。

### 検証

作業が完了したら、Dynatraceで変更を検証することができます。

![JSON](assets/k8s/Picture17.png)

### サービス名の命名規則

**Settings -> Server-side service monitoring -> Service naming rules**を開き、**Add a new rule**をクリックします。

ルールに名前を付けてください。例: **Kubernetes Project.Namespace.Container**

フォーマットの入力: `{Service:DetectedName}.{ProcessGroup:KubernetesNamespace}`

条件のドロップダウンで、プロパティの**Kubernetes namespace**と条件の**exists**を選択します。

![JSON](assets/k8s/Picture18.png)

**Preview**をクリックすると、マッチングしたエンティティが表示されます。

![JSON](assets/k8s/Picture19.png)

ポップアップの**Create Rule**と**Save Changes**をクリックします。

### 検証

作業が完了したら、Dynatraceで変更を検証することができます。

![JSON](assets/k8s/Picture20.png)

<!-- ------------------------ -->

## (オプション) カナリア展開のためのプロセス検出 
Duration: 15

### カナリア・リリースのデプロイ

以下のコマンドを実行して、カナリアのリリースを開始します。
```bash
kubectl apply -f https://raw.githubusercontent.com/Dynatrace-APAC/Workshop-Kubernetes/master/manifests/sockshop-app/canary/front-end-canary.yml
```

`kubectl get pods -n production -o wide`を実行すると、フロントエンドサービスの**stableとcanaryの両方のリリースが実行されていることがわかります**。

![JSON](assets/k8s/Picture21.png)

1-2分待ってから、Dynatraceでサービスを確認します。本番環境では、**安定版**と**限定版**の2つのサービスがあります。
モニタリングのためには、同じサービスを使用する必要があります。

![JSON](assets/k8s/Picture22.png)

### プロセス検出ルール設定

Dynatraceコンソールで、**設定->プロセスとコンテナ->プロセスグループ検出**に入ります。

**Cloud Application and Workload Detection**セクションを展開します。

[ドキュメント](https://www.dynatrace.com/support/help/how-to-use-dynatrace/process-groups/configuration/adapt-the-composition-of-default-process-groups/#cloud-applications-and-workload-detection)によると、プロセスグループを選択するためには、`DT-ContainerBoundariesAffected`を使用して、**クラウドアプリケーションとワークロードの検出を無効にし**、**ルールベースのタグ機能を有効にする**必要があります。

![Disable-cloud-apps](assets/k8s/disable-cloud-workload.png)

プロセスグループ検出ルール**セクションを展開します。

検出ルールの追加」をクリックします。

**process property** を選択して、プロセスを分離します。

![JSON](assets/k8s/Picture23.png)

このルールは、**production only (namespace=production)**で動作するポッドに適用したいと思います。

また、ポッド名の **"."**の後の識別子を抽出します。
ポッド名には、区別するために **".stable "**または **".canary "**が付いていることを覚えておいてください。

![JSON](assets/k8s/Picture24.png)

以下のコマンドを実行して、安定版とカナリア版のフロントエンドポッドをリサイクルしてください。プロセス検出ルールは、プロセス起動時に適用されます。

```bash
wget -O- https://raw.githubusercontent.com/Dynatrace-APAC/Workshop-Kubernetes/master/recycle-sockshop-frontend.sh | bash
```

Dynatrace内では、プロセスグループが統合されていることがわかります。

![Process-Group-Merged](assets/k8s/Picture24.1.png)

サービスはまだ個々のサービスとして検出されており、同様にマージすることができます。

**設定** -> **サービスのマージ** -> **マージされたサービスの作成**を選択します。

![Service-Merged](assets/k8s/Picture24.2.png)

### 検証

![JSON](assets/k8s/Picture25.png)

サービスが1つに統合されたことで、モニターのStableとCanaryの応答を表示できるようになりました。

**Create Chart**を選択して、多次元解析ビューを作成します。

![JSON](assets/k8s/Picture26.png)

**Response Time - Server**を選択し、Dimension Splittingとして**Service Instance**を選択します。

![JSON](assets/k8s/Picture27.png)

<!-- ------------------------ -->
## Kubernetes ダッシュボード
Duration: 10

### BizOps Configuratorによるダッシュボードの設定

[BizOps Configurator](https://dynatrace.github.io/BizOpsConfigurator/)によるKubernetes Dashboardsの自動作成を利用する予定です。

トークンに以下のような**パーミッション**を設定します。

- **問題およびイベントフィード、メトリクス、トポロジーへのアクセス**
- **設定の読み込み**
- **設定の書き出し**
- **ユーザーセッション**

![K8S-Dashboards](assets/k8s/dashboard-token.png)

Follow the preloaded setup at <button>
[BizOps Configurator](https://dynatrace.github.io/BizOpsConfigurator/#deploy/persona/Ops/Platform%20Overview/K8s%20Overview)
</button>

- **tenantUrl**と **API-Token**の入力
- クリック **Next**. <em>(The persona, usecase and workflow are already selected for you.)</em>
- クリック **Done**.

![K8S-Dashboards](assets/k8s/k8s-dashboards.gif)

Positive
: ⚠️ エンティティを適切にフィルタリングするためには、サービス/ポッドに **[Kubernetes]namespace**というタグを追加する必要があります。これにより、パフォーマンスエンジニアリングダッシュボードで適切にフィルタリングできるようになります。これは後で製品のOOTBになる予定です。

Positive
: ⚠️ Dynatrace 196+で動作します。

### Kubernetes Overview
![#](assets/k8s/overview.png)


### クラスターの使用率
_____________________
Kubernetesのクラスタ使用率をご覧ください。全ノードのCPUとMemoryのRequestと制限を時間軸で、ネームスペースで分割しています。

![#](assets/k8s/cluster-utilization.png)


### リソースクオータ
_____________________
ネームスペースに割り当てられているKubernetesのリソースクォータ（MemoryとCPU）の概要と使用状況を把握できます。
![#](assets/k8s/quotas.png)

### コンテナの使用状況と健康状態
_____________________
クラスタ内のPodの健全性とフェーズを把握します。メモリやCPUの使用率、どのポッドがスロットルされているか、障害が発生しているか、スケジュールが保留されているかなどを確認します。また、Out-of-memory killed コンテナがあるかどうかも確認します。

![#](assets/k8s/containers.png)


### パフォーマンスエンジニアリング
_____________________
開発者や SRE エンジニアが、クラスター上の各アプリ、ポッド、各トランザクションのパフォーマンスを理解し、改善するために必要な情報を提供します。応答時間のパーセンタイル、低速トランザクション、マイクロサービスごとのデータベース実行、ネットワーク使用率などを表示します。アプリのラベルや名前空間などでトランザクションをフィルタリングできます。 

![#](assets/k8s/performanceeng.png)

### ユーザーエクスペリエンス
_____________________
エンドユーザーは満足していますか？アプリケーションのエンゲージメント、エクスペリエンス、ユーザー行動はどうですか？すべてのアプリケーションとユーザーのインサイトを1つのインスタンスで取得できます。

![#](assets/k8s/userexperience.png)

<!-- ------------------------ -->
## Exploring Kubernetes View
Duration: 5

Kubernetes Viewの様々な機能（Cluster Utilization、Cluster Workloads、K8S Eventsなど）について説明します。

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
   - 各アプリケーションをクリックすると、それを支える技術が見えてきます。
   
![KubernetesUI](assets/k8s/cloud-apps.png)


www.DeepL.com/Translator（無料版）で翻訳しました。
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
: 💡 その他のアイデアや提案については、[メールでのお問い合わせ](mailto:DT-TYO-SE@dynatrace.com?subject=DynatraceとKubernetes - アイデア、提案) をお願いします。