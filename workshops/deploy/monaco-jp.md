<!-- Code for Monaco -->

[Dynatrace Monitoring as Code (Monaco)](https://github.com/dynatrace-oss/dynatrace-monitoring-as-code)を使用して、Dynatraceの設定をコートとして管理することが可能です。

これには以下のようなユースケースがあります。

* 複数の環境で再利用できるように、構成をテンプレート化する機能を持つこと
* コンフィグレーション間の相互依存性は、一意の識別子を追跡することなく処理する必要があります。
* 同じコンフィグレーションを何百ものDynatrace環境に簡単に適用・更新できる機能を導入し、特定の環境にロールアウトすることができます。
* アプリケーション固有の構成をある環境から別の環境へと促進するための簡単な方法を特定し、開発からハードニング、本番までのデプロイメントを追跡します。
* プルリクエスト、マージ、承認など、gitベースのワークフローのすべてのメカニズムとベストプラクティスをサポートする。
* コンフィグレーションは、開発からハードニング、本番へのデプロイメント後、1つの環境から別の環境へと簡単に昇格できなければなりません。

### monacoのインストール

monacoはGithubからダウンロードすることができます。

```bash
cd sockshop/
curl -L https://github.com/dynatrace-oss/dynatrace-monitoring-as-code/releases/download/v1.7.0/monaco-linux-amd64 -o monaco-cli
chmod +x monaco-cli
```

`monaco-cli help`と入力することでコマンドの詳細を確認することができます。

**DT_TENANT**と**DT_API_TOKEN**と**DT_DASHBOARD_OWNER**の変数を設定します。
これらは、ラボの登録メール内に記載されています。

```bash
export DT_TENANT= https://mou612.managed-sprint.dynalabs.io/e/<ENV>
export DT_API_TOKEN=dt0c01.IH6********************************************
export DT_DASHBOARD_OWNER=<your email address>
```

コマンドの実行後に、以下のコマンドを実行してDynatraceの設定を行います。本演習ではダッシュボードの作成のみ実施するため不要な設定は削除しておきます。

```bash
cp -r monaco/sockshop/dashboard/ dashboard
rm -rf monaco/sockshop/*
cp -r dashboard/ ~/sockshop/monaco/sockshop/
export SKIP_PROMETHEUS=false
./monaco-cli -e=monaco/sockshop-environment.yaml -p=sockshop monaco
```

`Deployment finished without errors`が出力されていれば成功です。

### ダッシュボードの確認

**お気に入り > ダッシュボード**もしくは**観察と探索 > ダッシュボード**からダッシュボードを開きます。
`Environment Overview Dashboard`と`Prometheus - Environment Overview Dashboard`の2つのダッシュボードが作られていることを確認します。

![Dashboard](../assets/k8s/dashboard.png)

**Prometheus - Environment Overview Dashboard**ではPrometheusが収集したメトリクスの概要を確認することができます。

![Prometheus](../assets/k8s/prometheus-dashboard.png)
