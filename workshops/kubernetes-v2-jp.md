id: kubernetes-jp
summary: KubernetesとPrometheusの統合によるフルスタックオブザーバビリティの実現
categories: kubernetes, prometheus, application-microservices-monitoring
tags: kubernetes, Beginner
status: Published 
authors: Brandon Neo
Feedback Link: mailto:DT-TYO-SE@dynatrace.com
new: 1
Analytics Account: UA-175467274-1

# DynatraceとKubernetes
<!-- ------------------------ -->
## はじめに
Duration: 1

このリポジトリには、Hands-On Kubernetes Sessionのラボが含まれています。今回のハンズオンでは、AWSで動作するKubernetesインスタンスを使用しますが、他のプラットフォームでも動作します。

Dynatrace主催のハンズオンワークショップへ参加されている方には環境が自動で払い出されます。

### 事前準備
* DynatraceのAccount. フリートライアルの申し込み [here](https://www.dynatrace.com/trial/)
* Chrome ブラウザ
* SSH クライアント [mobaxterm](https://mobaxterm.mobatek.net/)

### 学習内容
- Dynatrace Operatorの導入
- 自動作成されたKubernetesのダッシュボードの確認
- Kubernetesにおけるラベルとアノテーション
- Kubernetesのプロセスグループネーミングとサービスネーミング
- Dynatrace上でのKubernetes情報の確認
  - Namespace
  - ワークロード
  - コンテナ/ポッド

<!-- Step 1 -->
## Dynatrace Operatorの導入
Duration: 15
<<snippet-jp/deploy/operator-jp.md>>

<<snippet-jp/restart/sockshop-jp.md>>

<<snippet-jp/setup/k8s-settings-jp.md>>

<!-- Step 2 -->
## Prometheusメトリクスのインポート
Duration: 10
<<snippet-jp/setup/k8s-prometheus-jp.md>>

<!-- Step 3 -->
## Kubernetesにおけるラベルとアノテーション
Duration: 15
<<snippet-jp/setup/k8s-labels-jp.md>>

<!-- Step 4 -->
## コンテナ環境変数
Duration: 15
<<snippet-jp/setup/k8s-container-variables-jp.md>>

<!-- Step 5 -->
## Kubernetesのプロセスグループネーミングとサービスネーミング
Duration: 15
<<snippet-jp/setup/k8s-pg-svc-naming-rules-jp.md>>

<!-- Step 6 -->
## Monitoring-as-Code
Duration: 10
<<snippet-jp/deploy/monaco-jp.md>>

<!-- Step 7 -->
## Kubernetes Dashboards
Duration: 10
<<snippet-jp/explore/k8s-dashboards-jp.md>>

<!-- Step 8 -->
## Dynatrace上でのKubernetes情報の確認
Duration: 20
<<snippet-jp/explore/k8s-views-jp.md>>

<!-- Step 9 -->
## Kubernetesワークロードのインテリジェントオブザーバビリティ
Duration: 10
<<snippet-jp/explore/k8s-o11y-jp.md>>


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
  <input value="Dynatrace Operatorを使ってKubernetesにデプロイする方法" />
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