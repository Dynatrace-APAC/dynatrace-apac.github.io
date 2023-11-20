<!-- Code for deploying OneAgent via Operator -->

この演習では、Kubernetes(Microk8s)を実行しているLinuxインスタンスにDynatrace Operatorをデプロイします。

### Linuxターミナルへアクセス

Linuxインスタンスの**ターミナル**にアクセスします。

Positive
: ハンズオンワークショップへ参加されている方はメールに記載されている**EC2インスタンスの情報**を使用して、SSHでアクセスします。メールに記載されている**SSH Username**と**SSH Password**を使用します。
もしくは、ブラウザ上からcode-server経由でアクセスすることも可能です。メールに記載されているURLからアクアクセスをしてください。

### Dynatrace Operatorのインストール

ブラウザを開き、DynatraceのGUIにアクセスしてください。

以下の手順で進めてください。

* ナビゲーションメニューから **管理 > Dynatraceハブ** を開きます。
* **Kubernetes** を選択します。
* 右下の **Kubernetesのモニター** ボタンをクリックします。

![Deploy](../assets/k8s/jp/deploy-1.gif)

**Kubernetes / Openshiftのモニター**ページ内で、以下の手順を行います。

* **名前** を入力します 例：`k8s`
* Dynatrace Operator トークンおよびデータ取り込みトークンの**トークンの作成**をクリックして、適切なパーミッションのPaaSおよびAPIトークンを作成します。
* **SSL証明書チェックのスキップ**を有効にします。
* **ダウンロード: dynakube.yaml** ボタンをクリックしてdynakube.yamlファイルをダウンロードします。

![Deploy](../assets/k8s/jp/deploy-2.gif)

* メモ帳などでdynakube.yamlファイルを開き、dynakube.yamlファイルを全て選択し、コピーします。

![Deploy](../assets/k8s/jp/dynakube-1.gif)

* ターミナルで`vim dynakube.yaml`などを実行し、dynakube.yamlファイルを貼り付けます。

![Deploy](../assets/k8s/jp/dynakube-2.gif)

* Dynatrace UIに戻り、**コピー**ボタンをクリックして、ターミナルに貼り付け実行します。

![Deploy](../assets/k8s/jp/deploy-3.png)

出力例

![Deploy](../assets/k8s/jp/deploy-4.png)


Negative
: Dynatraceにデータが表示されるまで約5分かかります。

Positive
: Dynatraceは、OneAgentsの自動展開や、k8sの自動統合を行います。

### インストールの確認

**ディプロイメントステータスの表示**をクリックすると、接続されているホストの状態を確認することができます。

下の画像のように、接続されたホストが表示されているはずです。

![Deploy](../assets/k8s/jp/deployment-status.png)

Positive
: 詳しいドキュメントは[こちら](https://www.dynatrace.com/support/help/shortlink/full-stack-dto-k8)を参照ください。

### ⚠️ トラブルシューティングの手順

Negative
: **ポッドの状態を確認**するには、以下のコマンドを実行します。すると、**Running**という結果が返ってくるはずです。<br>
`kubectl get pods -n dynatrace`

Negative
: **ログの確認**は、以下のコマンドを実行します。<br>
`kubectl logs -f deployment/dynatrace-oneagent-operator -n dynatrace`

Negative
: **シークレットを削除**するには、以下のコマンドを実行してください。誤ったシークレットが含まれている可能性があります。 <br>
`kubectl delete secret --all -n dynatrace`

Negative
: **すべてのポッドを削除**するには、以下のコマンドを実行してください。これにより、すべてのポッドが削除され、新しいポッドインスタンスが作成されます。<br>
`kubectl delete --all pods -n dynatrace`

Negative
: 公式サイトのトラブルシューティングページは[こちら](https://www.dynatrace.com/support/help/shortlink/kubernetes-troubleshoot)を参照ください。

### easytravelアプリケーションの再起動

様々なプロセスが自動的に検出されているのがわかりますが、Dynatraceはそれらを再起動するよう促します。これは、コードを変更せずに自動的に監視を行うために必要です。

以下のコマンドを実行して、**easytravel**のNamespacesに含まれるPodsを作り直します。

```bash
kubectl delete pods --all -n easytravel
```
