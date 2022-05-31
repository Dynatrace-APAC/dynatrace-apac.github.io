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
export DT_TENANT=https://mou612.managed-sprint.dynalabs.io/e/<ENV>
export DT_API_TOKEN=dt0c01.********************************************
export DT_DASHBOARD_OWNER=<your email address>
```

本演習ではエラーの発生を防ぐため、一部の設定については移動させておきます。

```bash
mv monaco/sockshop/azure-credentials /tmp
mv monaco/sockshop/kubernetes-credentials /tmp
mv monaco/sockshop/synthetic-monitor /tmp
```

以下のコマンドを実行してDynatraceの設定を行います。

```bash
export dev_frontend_ip=$(kubectl -n dev get ingresses.networking.k8s.io front-end -o jsonpath='{.spec.rules[0].host}')
export production_frontend_ip=$(kubectl -n production get ingresses.networking.k8s.io front-end -o jsonpath='{.spec.rules[0].host}')
export SKIP_PROMETHEUS=true
./monaco-cli -e=monaco/sockshop-environment.yaml -p=sockshop monaco
```

`Deployment finished without errors`が出力されていれば成功です。

![monaco](../assets/cloud-observe/jp/monaco.png)

### 実行結果の確認

monacoにより、以下の設定が行われたことを確認します。

* ダッシュボード
* SLO
* アプリケーション定義
* サービスネーミングルール
* プロセスのネーミングルール
* 管理ゾーン

![monaco](../assets/cloud-observe/jp/monaco-slo.png)
