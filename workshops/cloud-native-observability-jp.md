id: cloud-native-observability-jp
summary: SREのためのクラウドネイティブなオブザーバビリティを実現するDynatraceの使い方
categories: cloud-obs, cloud-automation
tags: cloud-Obs, Intermediate
status: Published 
authors: Katsuyoshi Sumida
Feedback Link: mailto:DT-TYO-SE@dynatrace.com
new: 1
Analytics Account: UA-175467274-1

# Dynatraceによるクラウドネイティブなオブザーバビリティ
<!-- ------------------------ -->
## はじめに
Duration: 1

このリポジトリには、Cloud Native Observabilityで実施する内容が含まれています。

Dynatrace主催のハンズオンワークショップへ参加されている方には環境が自動で払い出されます。

### 事前準備
* DynatraceのAccount：[フリートライアルの申し込み](https://www.dynatrace.com/trial/)
* Kubernetes環境
* Chrome ブラウザ
* SSH クライアント [Tera Term](https://ja.osdn.net/projects/ttssh2/)

### 学習内容
- Dynatrace Operatorの導入
- リリース管理
- サービスレベル目標の設定
- Monitoring as Codeによる監視のコード化

<!-- Step 1 -->
## Dynatrace Operatorの導入
Duration: 15
<<snippet-jp/setup/install.md>>

<!-- Step 2 -->
## リリース管理
Duration: 10
<<snippet-jp/explore/releases.md>>

<!-- Step 3 -->
## SLO設定（基礎）
Duration: 10
<<snippet-jp/config/basic-slo.md>>

<!-- Step 4 -->
## SLO設定（応用）
Duration: 15
<<snippet-jp/config/advanced-slo.md>>

<!-- Step 5 -->
## ダッシュボード
Duration: 15
<<snippet-jp/config/dashboard.md>>

<!-- Step 6 -->
## Monitoring-as-Code
Duration: 10
<<snippet-jp/deploy/monaco.md>>

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
  <name>このラボから最も恩恵を受けたことは何ですか？</name>。
  <input value="KubernetesへのOneAgentの導入" />
  <input value="GitOps / コードとしてのモニタリング" />
  <input value="サービスレベル目標" />
  <input value="Releases" />
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
: 💡 その他のアイデアや提案については、[メールでのお問い合わせ](mailto:DT-TYO-SE@dynatrace.com?subject=Dynatraceによるクラウドネイティブなオブザーバビリティ - アイデア、提案) をお願いします。