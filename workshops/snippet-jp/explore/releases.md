<!-- Release monitoring -->

Dynatraceにはリリース監視機能が組み込まれており、導入したアプリケーションのバージョンやステージ（名前空間）、プロダクトを判断することが可能です。

### リリースの確認

メニューから**クラウドオートメーション > リリース**を開きます。

![releases](../assets/cloud-observe/jp/releases.png)

これは、**リリースインベントリ、リリースイベント、追跡された問題点**、の概要を示しています。

各コンポーネントの詳細を見ると、それぞれのコンポーネントの追加メタデータが表示されます。
これにより、監視対象のコンポーネントのコンテキスト情報や、コンポーネントの**ライフサイクル**および**問題の追跡**に関する情報を確認できます。

下の画像は**catalogue catalogue-\***を開いた例です。

![releases-detail](../assets/cloud-observe/jp/releases-detail.png)

リリースバージョン、ステージ、製品はKubernetesの以下と一致します。

* リリースバージョン：`app.kubernetes.io/version`
* ステージ：`Namespace*`
* 製品：`app.kubernetes.io/part-of`

![k8s-describe](../assets/cloud-observe/jp/k8s-describe.png)

Positive
: 詳しいドキュメントは[こちら](https://www.dynatrace.com/support/help/shortlink/release-hub)を参照ください。

