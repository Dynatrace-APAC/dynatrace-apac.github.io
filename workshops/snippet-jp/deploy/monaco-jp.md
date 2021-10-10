<!-- Code for Monaco -->

この演習では、Dynatrace環境の設定を自動化します。

[Dynatrace Monitoring as Code (Monaco)](https://github.com/dynatrace-oss/dynatrace-monitoring-as-code)を使用して、すべてのグローバルなDynatrace環境の設定を人手を介さずに自動化することができます。
以下の様々なユースケースがあります。

* 複数の環境で再利用できるように、構成をテンプレート化する機能を持つこと
* コンフィグレーション間の相互依存性は、一意の識別子を追跡することなく処理する必要があります。
* 同じコンフィグレーションを何百ものDynatrace環境に簡単に適用・更新できる機能を導入し、特定の環境にロールアウトすることができます。
* アプリケーション固有の構成をある環境から別の環境へと促進するための簡単な方法を特定し、開発からハードニング、本番までのデプロイメントを追跡します。
* プルリクエスト、マージ、承認など、gitベースのワークフローのすべてのメカニズムとベストプラクティスをサポートする。
* コンフィグレーションは、開発からハードニング、本番へのデプロイメント後、1つの環境から別の環境へと簡単に昇格できなければなりません。

セッションを円滑に進めるために、以下のコードを実行します。

```bash
cd sockshop
./deploy-monaco.sh
```

コマンドの実行後に、**DT_TENANT**と**DT_API_TOKEN**と**DT_DASHBOARD_OWNER**の変数を設定します。
これらは、ラボの登録メール内に記載されています。

```bash
export DT_TENANT= https://mou612.managed-sprint.dynalabs.io/e/<ENV>
export DT_API_TOKEN=dt0c01.IH6********************************************
export DT_DASHBOARD_OWNER=<your email address>
```

コマンドの実行後に、以下のコマンドを実行してDynatraceの設定を行います。

```bash
./push-monaco.sh 
```

以下の設定が自動的に行われました。

* シンセティック・モニタリング
* サービスネーミングルール
* カートSLO
* アプリケーション定義
* ダッシュボード
* プロセスのネーミングルール
* 管理ゾーン