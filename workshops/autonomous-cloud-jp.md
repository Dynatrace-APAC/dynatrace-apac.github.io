id: autonomous-cloud-jp
summary: アプリケーションデプロイにおけるJenkinsとKeptnによるテストの自動化と品質の安定化を実現する方法
categories: keptn, jenkins, ansible, cloud-automation, application-microservices-monitoring
tags: autonomous-cloud, Advanced
status: Published 
authors: Brandon Neo
Feedback Link: mailto:DT-TYO-SE@dynatrace.com
Analytics Account: UA-175467274-1

# アプリデプロイ時の品質の安定化と自動テスト
<!-- ------------------------ -->
## はじめに
Duration: 1

このリポジトリには、Hands-On Autonomous Cloud Sessionのラボが含まれています。あなたの環境にアクセスするために必要な情報を提供します。

ハンズオンのために、参加者のために手順を自動化し、シームレスにします。

以下のものが提供されます。
- Dynatrace環境
- 稼働中のKubernetesサーバー
   - Sockshopアプリケーション(サンプルアプリ) 
      - Dev環境とProd環境と
- Jenkins環境
- Ansible環境

![ACM-Setup](assets/ac/acmsetup.png)

### 事前準備
- Chromeブラウザ
- 認証情報を含むAutonmous Workshopメール

![ACM-Setup](assets/ac/acm-1.jpg)

### 学習内容
- パイプライン（Sockshop）をJenkinsでデプロイする 
- Keptnのインストール 
- JenkinsとKeptnおよびDynatraceの統合
- Jenkins＋KeptnとDynatraceを使って
   - Quality Gatesの評価による品質の自動化
   - テストの自動化 負荷テストとパフォーマンステスト
   - Ansibleによるセルフヒーリングで運用を自動化する
- サービスとしてのセルフヒーリング

<!-- ------------------------ -->
## JenkinsでSampleAppをデプロイ
Duration: 5

### Jenkinsへのログイン

メールに記載されている認証情報を使って、WebブラウザでJenkins環境にアクセスします。

![Jenkins-Setup](assets/ac/jenkins-login.png)

### パイプラインの構築

ログインしたら、リストの中の "**DeploySockShop**"にマウスオーバーし、下矢印をクリックして "**Build Now**"を選択します。

![Jenkins-Setup](assets/ac/jenkins-1.png)

このプロセスは1～2分程度かかりますので、そのまま実行させます。

終了後、再び「**DeploySockshop**」をクリックすると、パイプラインビルドの様々な段階を確認することができます。

![Jenkins-Setup](assets/ac/jenkins-2.png)


これはあくまでもパイプラインが動作しているデモなので、完了するまで待つ必要はありません。

<!-- ------------------------ -->
## Keptnの設定
Duration: 5

### Kubernetes インスタンスへのログイン

SSHターミナル（Putty、Terminal、MobaXtermなど）を使ってKubernetesサーバーにログインします。

メールに記載されている認証情報を使って、Kubernetes環境にアクセスします。

ログイン後、以下のコマンドを実行してKeptnをインストールします<br>。
Keptnのセットアップの一部は、以下のシェルスクリプトで自動化されています。

```bash
cd dtacmworkshop/keptn
sudo ./installKeptn.sh
```
![Keptn-Setup](assets/ac/keptn-1.png)

Keptnは、セットアップに必要なコンポーネントをインストールします。
セットアップには2～3分かかります。

![Keptn-Setup](assets/ac/keptn-2.png)

セットアップが完了したら、次のステップで必要となる**keptn API token**をメモしてください。
また、Keptn BridgeとAPI Endpointを使用することもできます。

例e:<br>
KEPTN BRIDGE: http://bridge.keptn.<i>**YOUR-IP-ADDRESS**</i>.nip.io<br> 
KEPTN ENDPOINT: https://api.keptn.<i>**YOUR-IP-ADDRESS**</i>.nip.io/**swagger-ui**/

![Keptn-Setup](assets/ac/keptn-4.png)
Keptn APIのSwagger UIを表示するには、最後に「**/swagger-ui/**」を付けることに注意してください。

![Keptn-Setup](assets/ac/keptn-3.png)
現在、Keptnのブリッジではプロジェクトは動いていませんが、次のステップで作成する予定です。

<!-- ------------------------ -->
## Jenkins内でのKeptnの設定
Duration: 5

### Jenkins内でのKeptnプラグインの設定

Jenkinsに戻って、**Jenkins（左上から）> Manage Jenkins > Configure System**を選択します。

![Plugin-Setup](assets/ac/jenkins-3.png)

スクロールダウンして、「**Global Properties**」の下にあるフィールドに、ターミナルからのKEPTN API Tokenを入力します<br>。
Save**"をクリックして設定を保存します。

![Plugin-Setup](assets/ac/jenkins-4.png)

ステップはラボの前に事前に設定されています。プラグインの詳細については、Githubの[Keptn Jenkins Shared Library](https://github.com/keptn-sandbox/keptn-jenkins-library)を参照してください。

<!-- ------------------------ -->
## KeptnのSLI/SLOベースのクオリティーゲート
Duration: 20

今回はJenkins Pipelineを使って、KeptnでSLI/SLOベースのQuality Gate Evaluationを誘発します。
これは、KeptnのCLIやAPIを使って行うこともできます。

![SLI-quality-gate](assets/ac/evalpipeline_animated.gif)

Jenkins内で、リスト内の "**01-qualitygate-evaluation**"にマウスオーバーし、下矢印をクリックして "**Build Now**"を選択します。

![Quality-Gate](assets/ac/quality-gate-1.png)

Jenkinsのデフォルトでは、パイプラインのパラメータをスキャンしていないため、最初のビルドは失敗します。01-qualitygate-evaluation**"をクリックして、パイプラインを掘り下げます。

![Quality-Gate](assets/ac/quality-gate-2.png)

01-qualitygate-evaluation "**をクリックし、左メニューの**"Build with Parameters "**を選択します。

![Quality-Gate](assets/ac/quality-gate-3.png)

事前に設定したフォームと同様に、Dynatrace内の**"evalservice "**タグを使用して、SLIとSLOを検証する適切なサービスを特定します。このタグの名前は、Jenkinsパイプラインにパラメータとして渡すことができます。そこで、まずDynatrace内でサービスにタグを付ける必要があります。

### Dynatrace環境にログイン

メールに記載されている認証情報を使って、WebブラウザでJenkins環境にアクセスします。<br>。
初めてDynatraceテナントにアクセスする際には、パスワードを設定する必要があります。<br>。

![Quality-Gate](assets/ac/quality-gate-8.png)

Dynatraceにログインすると、あらかじめ設定されたダッシュボードが表示されます。

### Dynatrace内でのタグの設定

![Quality-Gate](assets/ac/quality-gate-4.png)

このプレビルドダッシュボードには、様々なポータルに素早くアクセスできるリンクも含まれています。

左側のナビゲーションバーで「**Transactions and Services**」を選択し、「**front-end**」サービスを選択します。<br>。
これは開発または生産サービスのいずれかになります。

![Quality-Gate](assets/ac/quality-gate-5.gif)

ドロップダウンメニューの "**プロパティとタグ**"から "**タグの追加**"をクリックし、タグとして "**evalservice**"を入力します。

### Jenkinsでパイプラインを構築する

Jenkinsで "**Build**"ボタンをクリックしてパイプラインを実行します。

![Quality-Gate](assets/ac/quality-gate-6.png)

パイプラインが完了すると、KeptnのブリッジとDynatraceに変更が反映されているのがわかります。
Dynatrace内では、新しいカスタム情報イベントも発見できます。<br>。
この新しいイベントには、JenkinsからのJobURLやJobName、KeptnのソースやKeptnのブリッジなど、品質ゲートの結果の詳細が含まれています。<br>。
また、リンクからはJenkinのポータルとKeptnのポータルに移動し、それぞれの詳細情報が表示されます。

![Quality-Gate](assets/ac/quality-gate-7.gif)

このハンズオンのセットアップについては、チュートリアルの全文を[こちら](https://github.com/keptn-sandbox/jenkins-tutorial/blob/master/README.md#11-integrate-keptns-slislo-based-quality-gates)でご覧いただけます。

<!-- ------------------------ -->
## Jenkinsのシンプルな負荷テスト
Duration: 20

### パイプラインでの負荷テスト

これは、前回のビルドの拡張バージョンで、パイプラインにはステージの1つに非常にシンプルなロードテスト機能が組み込まれています。

![load-test](assets/ac/simpletestpipeline_animated.gif)

"**Transactions and Services**"にアクセスし、"**[Kubernetes]stage:dev**"タグと "**testservice**"タグでフィルタリングしてサービスを識別します。

![load-test](assets/ac/load-test-2.gif)

Jenkinsに戻り、「**Back to Dashboard**」をクリックすると、Jenkinのパイプラインの一覧に戻ります。<br>。
リストの中の「**02-simpletest-qualitygate**」にマウスオーバーし、下矢印をクリックして「**Build Now**」を選択します。
先ほどの実験のように、最初のビルドは失敗しますが、2回目の試行では「**Build with parameters**」にドリルインすることができます。

![load-test](assets/ac/load-test-1.png)

フォームを次の詳細で変更します:<br>。
サービスです。**テストサービス**<br>
DeploymentURI: **WebサイトのデプロイメントURL**（メールに記載されているもの）<br>。
URLパスを **/:homepage;/category.html:Category**<br> <br>

完了したら、"**Build**"ボタンをクリックして、パイプラインを開始します。

![load-test](assets/ac/load-test-3.png)

また、ダッシュボードからCarts DevのURLを取得することもできます。<br>。
単に右クリックして、リンクアドレスをコピーするだけです。

![load-test](assets/ac/load-test-4.png)

完成したら、Keptnのブリッジ/DynatraceのFrontend Service/Jenkinsで変更点を見てみましょう。<br>
計算されたサービスメトリクスに基づいて、追加のSLIを見つけることができます。

![load-test](assets/ac/load-test-5.png)

このハンズオンのセットアップの詳細については、完全なチュートリアルを[こちら](https://github.com/keptn-sandbox/jenkins-tutorial/blob/master/README.md#12-integrate-keptns-slislo-based-quality-gates---with-simple-testing-tool-in-pipeline)でご覧いただけます。

<!-- ------------------------ -->
## Keptnのパフォーマンステストクオリティーゲート
Duration: 20

### パイプラインでのパフォーマンステスト

このラボは前回のラボを発展させたもので、Jenkinsのパイプラインの一部としてJenkinsに簡単なテストを実行させました。KeptnはSLI/SLOベースの品質ゲート評価に使用されました。このチュートリアルでは、Keptn を使用して、JMeter を使用した実際のテストの実行も行います。

![perf-test](assets/ac/perfaaspipeline_animated.gif)

また、テストの実行には、定義されたSLIとして計算されたサービスメトリクスを使用します。
これらのサービスメトリクスの作成は、ラボのセットアップの一部として作成しました。<br>。
これらのメトリクスは、**Settings > Server-side service monitoring > Calculated Service metrics**<br>にあります。

![perf-test](assets/ac/perf-test-4.png)

これらのメトリクスとその寸法は、SLI の実行中に使用されます。<br>。
すでに「**testservice**」タグを追加しているので、Jenkins内で3つ目のパイプラインを実行するだけです。<br>。
Jenkinsに戻り、「**03-performancetest-qualitygate**」をクリックします。<br><br>以下のようになります。
以下の詳細でフォームを変更します:<br>。
サービス。**テストサービス**<br>
DeploymentURI: **WebサイトのデプロイメントURL**（メールに記載されているもの

![perf-test](assets/ac/perf-test-1.png)

完了したら、"**Build**"ボタンをクリックしてパイプラインを開始します。

ダッシュボードからCarts DevのURLを取得することもできます。<br>。
右クリックしてリンクアドレスをコピーするだけです。

![perf-test](assets/ac/perf-test-2.png)

完了したら、Keptnのブリッジ/DynatraceのFrontend Service/Jenkinsで変更点を確認できます。<br>。
Dynatrace内では、品質ゲートのパス結果を生成した自動化されたパフォーマンステストを見ることができます。<br>。
また、Jenkinsだけでなく、Keptn's bridgeにもコンテキストを掘り下げることができます。

![perf-test](assets/ac/perf-test-3.gif)

<!-- ------------------------ -->
## SampleAppのデプロイの自動化
Duration: 20

ここでは、KeptnのSLI/SLOベースのQuality Gatesを使って自動化することで、SampleApp Sockshopを修正します。

![deploy-app](assets/ac/deploy-app-1.png)

Jenkinsの中で、リストの中の「**DeploySockShop**」にマウスオーバーし、下矢印をクリックして「**Configure**」を選択します<br>。
別のブラウザタブを開き、[Dynatrace APAC's Autonomous Cloud's Git Repo](https://github.com/Dynatrace-APAC/Workshop-Autonomous-Cloud/blob/master/jenkins/DeploySockShop_Keptn_LoadTest_and_QualityGate.Jenkinsfile)にアクセスします。<br>。
修正したSockShop Jenkinsfile raw file**をコピーして、JenkinsのPipelineセクション内に置き換えます。 

![deploy-app](assets/ac/deploy-app-2.gif)

「**保存**」をクリックし、「**Build with Parameters**」のページで「**Build Two**」を選択してビルドを開始します。

![deploy-app](assets/ac/deploy-app-3.png)

<!-- ------------------------ -->
## サービスとしてのセルフヒーリング
Duration: 20

Dynatraceは、Ansibleなどの多くのランブック自動化ツールと統合されています。今回は、Ansibleを使って自己修復問題を紹介し、運用を自動化します。

### カートサービスのための負荷の生成

SSHターミナル内で以下のコマンドを実行し、カートサービスのロードを開始します。

```bash
cd ~acm_student/dtacmworkshop/utils/
sudo ./cartsLoadTest.sh
```

### Ansible Towerへのログイン

メールに記載されている認証情報を使って、WebブラウザでAnsible環境にアクセスします。

![Ansible](assets/ac/ansible-2.png)

### Dynatrace問題通知の設定

Ansible内で、左ナビゲーションの「**Templates**」をクリックし、**Remediation** Job templateのURLをコピーします。<br>
DynatraceのUIで、**設定 > 統合 > 問題通知 > 設定通知 > Ansible Tower**に移動します<br>。
コピーした**Ansible TowerジョブテンプレートのURL**を入力します<br>。
Ansibleにアクセスするための認証情報を入力する（メールで提供されたもの）<br>。
Sent test notification（テスト通知の送信）」ボタンをクリックして、設定を検証します<br>。
成功すると、緑色の確認メッセージが表示されます<br>。
設定の保存

![Ansible](assets/ac/ansible-3.gif)

### 異常検知の調整

Dynatraceの問題検知と異常検知には、DynatraceのAI技術「DAVIS」が活用されています。つまり、DAVISは一つ一つのマイクロサービスがどのように振る舞うかを学習し、それらをベースラインとしています。そのため、現在のようなデモシナリオでは、AIエンジンをユーザー定義の値でオーバーライドして、故障率を人為的に増加させることで問題を発生させる必要があります。(注意：アプリケーションを実行し、エンドユーザートラフィックを数時間/数日シミュレートする場合は、このステップは必要ありません)

Dynatraceのテナントで、「**Transaction & services**」に移動し、タグ「**[Kubernetes]stage:prod**」でフィルタリングし、ItemsController Serviceを選択します<br>。
ItemsController Serviceのページで、サービス名の横にある**3つのドット（ ... ）**をクリックします。Edit**<br>をクリックします。
次の画面で、**anomaly detection settings**を以下のように編集します<br>。
- Global anomaly detection**を無効にする
- 固定しきい値**を使用して故障率の増加を検出する 
- 任意の5分間に**0 %**のカスタム故障率のしきい値を超えた場合に警告する。
- 感度を**High**に設定し、*1リクエスト/分**未満の値を変更する。

![Ansible](assets/ac/ansible-5.gif)

### リメディエーション・プレイブックの起動

Ansible Tower の UI に戻ります。サイドメニューから、**Resources -> Templates**に移動します。<br>。
ロケットアイコン**をクリックして、start-campaign playbookを起動します。<br>。
プロンプトのポップアップウィンドウで **Next** をクリックし、**Launch** をクリックします。<br>。
プレイブックが実行されると、ステータスが成功したことを確認します。

![Ansible](assets/ac/ansible-6.gif)

### リメディエーションの観察

ItemsControllerに戻って、サービスイベントを見てみましょう。プロモーションレートが50％に変更されたときに、プレイブックがDynatraceに通知しています。

![Ansible](assets/ac/ansible-7.png)

失敗率が増加し、最終的には Dynatrace が問題を検出していることがわかります。
何度かブラウザを更新する必要があるかもしれません。

![Ansible](assets/ac/ansible-8.png)

新たな問題が発生したことを確認してください。コメント欄には、Ansible Towerによって行われた修復アクションが表示されます

![Ansible](assets/ac/ansible-9.png)

問題をドリルダウンします。
Ansible Tower によって報告された新しい構成変更イベントが表示されます。
プロモーションレートが0%に戻され、トランザクションの失敗に修正されました

![Ansible](assets/ac/ansible-10.png)

Ansible Tower で実行されたジョブ
 - start-campaign (レートを50%に設定)
 - 修復
   - Dynatrace Problemにコメントをプッシュ
   - 問題の詳細を取得
   - 問題のコンテクストに関連する修復アクションの開始
   - Dynatrace Problemの更新
キャンペーンの停止（レートを0％に設定

![Ansible](assets/ac/ansible-11.png)

** 問題解決 - 問題は自動的に解決されました

![Ansible](assets/ac/ansible-12.png)

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
  <input value="Keptnの設定" />
  <input value="JenkinsとDynatrace間のKeptnの統合" />
  <input value="自律型クラウドの原理を理解する" />
  <input value="サービスとしての自己治癒力" />
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
: 💡 その他のアイデアや提案については、[メールでのお問い合わせ](mailto:DT-TYO-SE@dynatrace.com?subject=アプリデプロイ時の品質の安定化と自動テスト - アイデア、提案) をお願いします。