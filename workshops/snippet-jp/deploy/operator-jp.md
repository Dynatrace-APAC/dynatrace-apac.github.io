<!-- Code for deploying OneAgent via Operator -->

この演習では、Kubernetes(Microk8s)を実行しているLinuxインスタンスにOneAgentをデプロイし、そのインスタンスで何が実行されているかOneAgentが見つけたものを確認します。

### ブラウザ経由でのターミナルアクセス

ラボを円滑に進めるために、Linuxインスタンスに**Webブラウザを介したターミナル**でアクセスします。

メールに記載されている**URL**を使用して、SSHターミナルにアクセスします。URLが `Public IP Address:8080/wetty` のようになっていることを確認してください。

メールに記載されている**ログイン名**と**パスワード**を使用します。

![Deploy](../assets/dem/wetty.png)

### OneAgentのダウンロード

ブラウザを開き、DynatraceのGUIにアクセスしてください。

以下の手順で進めてください。

* ナビゲーションメニューから **Hub** を検索します。
* **Star**を付けてお気に入りに追加し、**クリック**します。
* **Kubernetes** を選択します。
* 右下の **Monitor Kubernetes** ボタンを選択します。 

![Deploy](../assets/cloud-observe/k8s-deploy.gif)

**Monitor Kubernetes / Openshift**ページ内で、以下の手順を行います。

* **名前** を入力してください 例：`k8s`
* **Create tokens**をクリックして、適切なパーミッションのPaaSおよびAPIトークンを作成してください。
* Skip SSL Certificate Checkを有効にします。
* **Copy** ボタンをクリックしてコマンドをコピーしてください。
* ターミナルウィンドウに貼り付けて実行してください。

![Deploy](../assets/cloud-observe/k8s-deploy-2.gif)

出力例

```bash
Connecting to github-releases.githubusercontent.com (github-releases.githubusercontent.com)|185.199.108.154|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 7310 (7.1K) [application/octet-stream]
Saving to: ‘install.sh’

install.sh                      100%[=====================================================>]   7.14K  --.-KB/s    in 0s      

2021-06-01 05:46:36 (40.7 MB/s) - ‘install.sh’ saved [7310/7310]


Check for token scopes...

Check if cluster already exists...

Creating Dynatrace namespace...

Applying Dynatrace Operator...
Warning: apiextensions.k8s.io/v1beta1 CustomResourceDefinition is deprecated in v1.16+, unavailable in v1.22+; use apiextensions.k8s.io/v1 CustomResourceDefinition
customresourcedefinition.apiextensions.k8s.io/dynakubes.dynatrace.com created
serviceaccount/dynatrace-dynakube-oneagent created
serviceaccount/dynatrace-dynakube-oneagent-unprivileged created
serviceaccount/dynatrace-kubernetes-monitoring created
serviceaccount/dynatrace-operator created
serviceaccount/dynatrace-routing created
podsecuritypolicy.policy/dynatrace-dynakube-oneagent created
podsecuritypolicy.policy/dynatrace-dynakube-oneagent-unprivileged created
podsecuritypolicy.policy/dynatrace-kubernetes-monitoring created
podsecuritypolicy.policy/dynatrace-operator created
podsecuritypolicy.policy/dynatrace-routing created
role.rbac.authorization.k8s.io/dynatrace-dynakube-oneagent created
role.rbac.authorization.k8s.io/dynatrace-dynakube-oneagent-unprivileged created
role.rbac.authorization.k8s.io/dynatrace-kubernetes-monitoring created
role.rbac.authorization.k8s.io/dynatrace-operator created
role.rbac.authorization.k8s.io/dynatrace-routing created
clusterrole.rbac.authorization.k8s.io/dynatrace-kubernetes-monitoring created
clusterrole.rbac.authorization.k8s.io/dynatrace-operator created
rolebinding.rbac.authorization.k8s.io/dynatrace-dynakube-oneagent created
rolebinding.rbac.authorization.k8s.io/dynatrace-dynakube-oneagent-unprivileged created
rolebinding.rbac.authorization.k8s.io/dynatrace-kubernetes-monitoring created
rolebinding.rbac.authorization.k8s.io/dynatrace-operator created
rolebinding.rbac.authorization.k8s.io/dynatrace-routing created
clusterrolebinding.rbac.authorization.k8s.io/dynatrace-kubernetes-monitoring created
clusterrolebinding.rbac.authorization.k8s.io/dynatrace-operator created
deployment.apps/dynatrace-operator created
W0601 05:46:39.025776   29593 helpers.go:553] --dry-run is deprecated and can be replaced with --dry-run=client.
secret/dynakube configured

Applying DynaKube CustomResource...
dynakube.dynatrace.com/dynakube created

Adding cluster to Dynatrace...
Kubernetes monitoring successfully setup.
$

```

Negative
: Dynatraceにデータが表示されるまで約5分かかります。

Positive
: Dynatraceは、OneAgentsの自動展開や、k8sの自動統合を行います。

### インストールの確認

**Show deployment status**をクリックすると、接続されているホストの状態を確認することができます。

下の画像のように、接続されたホストが表示されているはずです。

![Deploy](../assets/dem/download-deployment-status-1.png)

Positive
: 詳しいドキュメントは[こちら](https://www.dynatrace.com/support/help/technology-support/operating-systems/linux/)を参照ください。

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
: 公式サイトのトラブルシューティングページは[こちら](https://www.dynatrace.com/support/help/setup-and-configuration/setup-on-container-platforms/kubernetes/troubleshoot-k8s-deployment-and-connectivity/)を参照ください。

すべてがうまくいっていれば、**Show Deployment status**をクリックすると、ホストが表示されます。