id: biz-ops-jp
summary: Davis AIによるBizDevOpsの実現
categories: biz-ops, business-analytics
tags: biz-ops, Intermediate
status: Published 
Feedback Link: mailto:DT-TYO-SE@dynatrace.com
Analytics Account: UA-175467274-1

# DynatraceによるBizDevOps
<!-- ------------------------ -->
## はじめに
Duration: 15

このレポには、BizOps - Bridging the Gap to the Business Hands On Workshopの一環として実施されるラボが含まれています。

ハンズオンの目的は、参加者のためにステップを自動化し、シームレスにすることです。

### 事前準備
* DynatraceのAccount. フリートライアルの申し込み [here](https://www.dynatrace.com/trial/)
* Chrome ブラウザ

### セットアップ
このラボでは、以下のサンプルアプリケーションを使用します。
- サンプルアプリケーション 
    * [easyTravel](https://community.dynatrace.com/community/display/DL/easyTravel)
    * Easy Travelには2つのインターフェースがあります。
    - クラシック
	![Intro](assets/bizops2021/ETClassic.png)
	 - Angular
	![Intro](assets/bizops2021/ETAngular.png)

### 学習内容
- ITメトリクスにビジネスコンテキストをもたらす
  - セッション分析のために個々のユーザーを特定する
  - コンバージョン目標
  - セッションリプレイ
- データを可視化し、運用チームと基幹業務チームの連携を強化
- データの活用
- Dynatraceの以下の機能をご紹介します。
  * セッションリプレイ
  * ユーザーセッションプロパティ
  * User Session Query Language (USQL)
  * Dynatrace ダッシュボード

<!-- ------------------------ -->
## 1 - ITメトリクスにビジネスコンテキストをもたらす
Duration: 15

この演習の目的は、Dynatraceの基本的なReal User Monitoringの構成を強化し、ITメトリクスにビジネスコンテキストをもたらすことです。そのため、Dynatraceの基本的な設定は行わず、あらかじめ環境を設定しておきます。
* アプリケーションの整理
* JavaScriptフレームワークのサポートを有効にする
* ユーザーアクションの論理的なグループ化（ユーザーアクションネーミングの設定

基本的な設定に興味のある方は、以下のラボリファレンス[Digital Experience Management with Dynatrace](https://dynatrace-apac.github.io/workshops/dem/index.html?index=..%2F..index#0)を参考にしてください。

## 1a - セッション分析のために個々のユーザーを特定する

Dynatrace Real User Monitoringの重要な機能の一つに、異なるブラウザ、デバイス、ユーザーセッション間で個々のユーザーを一意に識別する機能があります。

デフォルトでは、Dynatraceは各新規ユーザーにユニークでランダムなIDを割り当てます。しかし、ユーザー名や電子メールアドレスなどで構成される、より意味のあるカスタムユーザータグを割り当てることができます。

この演習では、***ページソース***に基づいてユーザーをタグ付けするアプローチを使用します。このユーザータグ付けのアプローチは、アプリケーションのページソースで利用可能なデータを取り込むことで機能します。

Easy Travel Angularアプリでは、ユーザー名を右上に表示しています。これは、DOM要素のテキストやJavaScriptの変数を介して行われます。

Dynatraceで設定するには

1. 左のナビゲーションメニューから「Applications」を選択します。
2. **EasyTravel Angular** アプリケーションを選択します。
3. 参照」ボタン**(...)**をクリックし、「編集」**を選択する。
4. Capturing**ヘッダーの下で、**User tag**を探す。
   ![ユーザータグ](assets/bizops2021/task1a-usertag.gif)
5. Add user tag rule**をクリックする。
   - ソースの種類 **ソースタイプ: **CSSセレクタ
   - CSSセレクタのフィールド。

     ```
     a.greeting
     ```

   - トグル クリーンアップルールを適用 **オン** にする
   - Regexボックスに以下をコピーします。

     ```
     Hi, (.*+)
     ```

6. **Add user tag rule**ボタンをクリックします。
7. **Save changes**ボタンを忘れずにクリックしてください。

設定が完了すると、このように表示されます。
![User Tag](assets/bizops2021/task1a-usertag-complete.png)

Positive
: 参考資料: [Identify Individual Users for Session Analysis](https://www.dynatrace.com/support/help/how-to-use-dynatrace/real-user-monitoring/how-to-use-real-user-monitoring/cross-application-user-session-analytics/identify-individual-users-for-session-analysis/)


<!-- ------------------------ -->
## 1b - コンバージョンレートの作成
Duration: 15

コンバージョンゴールは、ユーザーがユーザージャーニーの重要なマイルストーンを満たしているかどうかを理解するのに役立ちます。例えば、チェックアウトの成功、ニュースレターの登録、デモの登録などです。

この演習では、Easy Travel Angular アプリケーションのコンバージョン目標を作成します。

1. 前のアクティビティと同じ画面で、**Session Replay and behavior**の見出しをスクロールダウンし、**Conversion Goals**を選択します。
   ![Conversion](assets/bizops2021/task1b-conversion.gif)
2. **Add goal**ボタンをクリックし、以下を入力します。
   - Name:

     ```
     Credit card validated
     ```

   - Type of goal: **User Action**  
   - Rule applies to: **XHR actions**  
   - Rule
     - **Page URL**
	  - **contains**

       ```
       easytravel/rest/validate-creditcard
       ```

   - 完成した構成は以下のようになります。
     ![Conversion](assets/bizops2021/task1b-conversion-complete.png)

### コンバージョン目標設定の検証

アプリケーションに照らし合わせて結果を確認します。**EasyTravel Angular** > **User behavior**について  

![Conversion](assets/bizops2021/tast1b-checkconversiongoal.png)

<!-- ------------------------ -->
## 1c - セッションおよびユーザーアクションのプロパティ
Duration: 15

この演習では、セッションプロパティとユーザーアクションプロパティを作成して、Dynatraceに追加データを公開します。これは、分析目的でユーザーに関する追加のコンテキスト（キャンペーンのソース、ユーザーが選択したさまざまなアイテムなど）をもたらすのに便利です。

Dynatraceは、タグを使ってセッションにコンテキストを与える一般的なソフトウェアのリストをあらかじめ定義しています。その一部をご紹介します。
- グーグルアナリティクス
- アドビ・アナリティクス
- ティーリーフ 
- などがあります。

これらは、**Property pack**タブにあります。

アプリケーションから抽出できる追加のデータソースについては、**custom defined properties**を使用して、監視するユーザーアクションやユーザーセッションの文字列、数値、および日付のプロパティを定義します。

プロパティの値は、各ユーザーの行動の一部として取得されます。プロパティ値を活用することで、ユーザーのアプリケーションとのインタラクションの詳細を把握することができます。

1. 前述のアクティビティと同じ画面で、**Capturing**の見出しに移動し、**Session and Action properties**を選択します。
   ![Session properties](assets/bizops2021/tast1c-sessionproperties.png)
2. ボタン **Add properties** をクリックします。

### Property packs - Google Analytics/Adobe/UTM codes など
これらはあらかじめ用意されているので、あとはリストから選択するだけです。ここでは、以下のものを選択します。
- ドロップダウンをクリックして、**ウェブ・プロパティ**を選択します。
- Configuration properties**では、以下の項目に対応する**Add**ボタンを選択します。
  - UTM Source
  - UTM campaign
  - UTM term
  ![Web property pack](assets/bizops2021/task1c-webprop-complete1.png)
- **Next**をクリックします。
- 各項目を展開して、**store as user action property**を切り替えます。
  ![Web property pack](assets/bizops2021/task1c-webprop-complete2.png)
- **Create properties**をクリックします。

### Custom properties - 各ユーザーの取引額
- カスタム定義プロパティ "タブを選択します。
- Expression Typeを**Server side request attribute**を選択します。
- Request attribute name: **Revenue**
- Display name:

  ```
  Booking
  ```

- (auto fill) Key: **booking**
- Storage type:
  - Store as **session** property
- Click **"Save property"**

完成した構成はこんな感じです。
![Custom property](assets/bizops2021/task1c-custprop-booking-complete.png)

### Custom properties - 閲覧した旅行パッケージの金額
上記の設定に続いて、引き続きCSSセレクタタイプのカスタムプロパティを追加します。
- Expression type: **CSS Selector**
- Data type: **Double**
- CSS Selector

  ```
  #summary > div:nth-child(5) > p
  ```

- Display name:

  ```
  Trip Cost
  ```

- (auto fill) Key: **tripcost**
- Storage type:
  - Store as **session** property
- **"Save property"**をクリック

完成した構成はこんな感じです。
![Custom property](assets/bizops2021/task1c-custprop-tripcost-complete.png)

### Custom properties - 閲覧・予約した旅行先
- Expression type: **Server side request attribute**
- Request attribute name: **Destination**
- Display name:

  ```
  Destination
  ```

- (auto fill) Key: **destination**
- Storage type:
  - Store as **session** property
  - Store as **user action** property
- **"Save property"**をクリック

完成した構成はこんな感じです。
![Custom property](assets/bizops2021/task1c-custprop-dest-complete.png)

### 設定完了画面
すべての設定が完了すると、次のようなセッション/ユーザーアクションプロパティのリストができます。
![Session properties](assets/bizops2021/task1c-properties-complete.png)

Positive 
: 参考資料: [Session Properties Help page](https://www.dynatrace.com/support/help/shortlink/user-session-properties) 、 [Request Attribute Help page](https://www.dynatrace.com/support/help/shortlink/request-attributes)

<!-- ------------------------ -->
## 1d - Session Replay
Duration: 15

この演習では、DynatraceのSession Replayの設定について説明します。

以下の手順で、セッションリプレイを有効にしてください。

1. 前のアクティビティと同じ画面で、**Session Replay and behavior**の見出しまでスクロールし、**Session Replay**を選択します。
2. **Enable Session Replay**トグルON
   ![SessionReplay](assets/bizops2021/task1d-sr1.png)
3. **Recoding mask settings** タブへスクロールダウン、**Mask user input**を選択
   ![SessionReplay](assets/bizops2021/task1d-sr2.png)
4. **Playback masking settings**タブをクリックし、**Mask user input**を選択
   ![SessionReplay](assets/bizops2021/task1d-sr3.png)
5. 下までスクロールし**Save**をクリック
   ![SessionReplay](assets/bizops2021/task1d-sr4.png)

Positive
: 参考資料: [Session Replay](https://www.dynatrace.com/support/help/how-to-use-dynatrace/real-user-monitoring/basic-concepts/session-replay/)

<!-- ------------------------ -->
## 2 - データの視覚化

Dynatraceで収集したデータを視覚化する準備が整いました。この演習では、**コンバージョン目標**、**session/action properties**が、分析のためのITメトリクスにはるかに多くの洞察力とコンテキストを提供する方法を見ることができます。

これが、これから作成を目指すダッシュボードです。

![Dashboard](assets/bizops2021/task2-Dashboard-complete.png)

<!-- ------------------------ -->
## 2a - ペイメントファネルの可視化

Positive
: 目標：ユーザーがサイト上で支払いを行う際のユーザー・ジャーニーを可視化すること

![Dashboard](assets/bizops2021/task2-PaymentFunnel-tile.png)

1. 左側のメニューから**Dashboards**を選択してください。
2. **Create Dashboard**のボタンをクリックし、名前を入力して**Create**をクリックします。
3. **User Sessions Query**タイルをドラッグして、**Configure tile**をクリックします。
   ![ダッシュボード](assets/bizops2021/task2-CreateDashboard.gif)
4. 以下のようなクエリを入力します。

   ```SQL
   SELECT FUNNEL(useraction.name like "*journeys*" AS "Journey Search", useraction.name = "click on book now (xhr: /easytravel/rest/journeys/)" AS "Click on Book Now", useraction.name = "click on sign in (xhr: /easytravel/rest/login)" AS "Login", useraction.name="click on book journey  (xhr: /easytravel/rest/validate-creditcard)" AS "Submit Payment") FROM usersession
   ```

5. **run query** ボタンをクリック。
6. 名前の変更

   ```
   Payment funnel
   ```

7. **save changes to dashboard** ボタンをクリック。

![Dashboard](assets/bizops2021/task2-PaymentFunnel-usql.png)

<!-- ------------------------ -->
## 2b - ビジネスKPI指標の可視化

Positive
: 目的 - ビジネスの健全性を示すいくつかの重要なメトリクスを可視化する。

### 時間軸での変換

Positive
: 目的 - 長期にわたるコンバージョンは、コンバージョン目標を達成するための傾向を追跡するのに役立ちます。コンバージョン率が非常に低い場合は、顧客がカスタマージャーニーを完了する際に摩擦に直面していることを示しています。

![Dashboard](assets/bizops2021/task2-Conversions-tile.png)

1. ダッシュボードを**Edit**モードにして、先に作成したタイルをクリックし、**clone**ボタンをクリックし、**configure tile**ボタンをクリックします。
   ![Dashboard](assets/bizops2021/task2-CloneDashboard.gif)
2. 以下のようなクエリを入力します。

   ```SQL
   select datetime(starttime, "E HH:mm", "10m"), count(*) as "Conversions" from usersession where useraction.matchingConversionGoals="Credit card validated" group by datetime(starttime,"E HH:mm","10m")
   ```

3. **run query**ボタンをクリックします。
4. **Line chart**を選びます。
5. 名前の変更

   ```
   Conversions over time
   ```

6. **save changes to dashboard**ボタンをクリックします。

![Dashboard](assets/bizops2021/task2-Conversions-usql.png)

### 予約収益

Positive
: 目的 - Easy Travelは旅行予約サイトなので、主なビジネスKPIは予約収入を安定的に確保することです。あなたの「ビジネス」の原動力によって、これらのKPIは幅広い範囲になります。

![Dashboard](assets/bizops2021/task2-Booking-tile.png)

1. 上のタイルをクローンして、**configure tile**をクリックします。
2. クエリを変更します。

   ```SQL
   select sum (doubleProperties.booking) as Revenue from usersession
   ```

3. **run query**ボタンをクリックします。
4. **Single value**を自動的に選択してくれます。
5. 名前の変更

   ```
   Booking revenue
   ```

6. **save changes to dashboard**ボタンをクリックします。

![Dashboard](assets/bizops2021/task2-Booking-usql.png)

### 放置されたカートの合計

Positive
: 目的 - 多くの場合、仮想の「ショッピングカート」の「バスケットの値」は、あなたのサイトへの訪問者が潜在的に取引をする金額です。しかし、支払いの失敗や無効なプロモーションコードなど、何らかの理由で訪問者が取引全体を放棄したり、「ショッピングカート」内のアイテムを変更したりする場合があります。この値と、影響を受けたユーザーのリストを追跡することで、訪問者が「チェックアウト」や「購入」の前にドロップアウトしているかどうかがわかり、ビジネスに影響を与える前に問題を解決することができます。

![Dashboard](assets/bizops2021/task2-Abandoned-tile.png)

1. 上のタイルをクローンして、**configure tile**をクリックします。
2. クエリを変更します。

   ```SQL
   SELECT sum (doubleProperties.tripcost) as "Revenue Lost" from usersession where useraction.matchingConversionGoals IS NULL AND doubleProperties.tripcost > 0
   ```

3. **run query**ボタンをクリックします。
4. **Single value**を自動的に選択してくれます。
5. 名前の変更

   ```
   Abandoned cart value
   ```

6. **save changes to dashboard**ボタンをクリックします。

![Dashboard](assets/bizops2021/task2-Abandoned-usql.png)

### カートの放棄によって影響を受けるユーザー

![Dashboard](assets/bizops2021/task2-AbandonedUsers-tile.png)

1. 上のタイルをクローンして、**configure tile**をクリックします。
2. クエリを変更します。

   ```SQL
   SELECT userid from usersession where useraction.name = "click on book journey  (xhr: /easytravel/rest/validate-creditcard)" and doubleProperties.booking is null
   ```

3. **run query**ボタンをクリックします。
4. **Single value**を自動的に選択してくれます。
5. 名前の変更

   ```
   Users affected by abandoned cart
   ```

6. **save changes to dashboard**ボタンをクリックします。

![Dashboard](assets/bizops2021/task2-AbandonedUsers-usql.png)

<!-- ------------------------ -->

## 3 - データに対するアクション
Duration: 15

DAVIS™️に挑戦しよう!

EasyTravel Angularの実際の予約アプリケーションにアクセスする
- ダッシュボードを開く
- **...Welcome to the Dynatrace Workshop 🔬** ダッシュボードをクリックします。
- **Booking Portals:✈ EasyTravel Angular**のリンクをクリックします。

![Action on the data](assets/bizops2021/task3-prework1.gif)

- 旅行パッケージの予約
- ユーザーIDは **alex** または **peter** です。
- お気づきですか？
- **(3) Payment**を複数回クリックしてみてください。
- ブラウザを閉じる前に、以下の作業を行ってください。
  - ブラウザの**開発者ツール**を開く

![Action on the data](assets/bizops2021/task3-prework2.png)

- **コンソール**タブに移動します。
- 次のように入力します。

  ```
  dtrum.endSession()
  ```

![Action on the data](assets/bizops2021/task3-prework3.png)

### シナリオ #1
**コンバージョン**が減少しています。そして、**Revenue**も減少しています。

![Scenario1](assets/bizops2021/task3-scenario1-1.png)

しかし、ITオペレーションチームの監視ダッシュボードでは、すべてのシステムが**グリーン**になっています。
![Scenario1](assets/bizops2021/task3-scenario1-2.png)

ITの問題ではないにしても、何かが原因でユーザーが不満を感じ、取引が完了しないのではないでしょうか。

Negative
: Dynatraceを使って、何が訪問者の摩擦の原因になっているかを調べます。

***ヒント:***

**Sample BizDevOps Dashboard**のいくつかのダッシュボードは、何が起こっているかを知る手がかりになるかもしれません。

![Scenario1](assets/bizops2021/task3-scenario1-3.png)

また、**Session Replay**を使用してユーザーセッションを調査すると、ログやアプリケーションコードの調査では検出できないアプリケーションの事実が判明することがあります。

### シナリオ #2
DAVISが異常を検出しました! また、影響を受けて予約プロセスを放棄したユーザーがいることも確認されました。

![Scenario2](assets/bizops2021/task3-scenario2-1.png)

Negative
: DAVISよりも早く原因を究明することができるのか？

***ヒント:***

先ほど作成したダッシュボードには、何がエラーの原因になっているかを理解するための情報がすべて含まれています。

**Problems**のタイルから始めて、そこからドリルダウンすることができます。

また、問題に直面しているユーザー名をリストアップしたタイルを使って、個々のユーザーセッションを調査することもできます。

<!-- ------------------------ -->
## 4 - BizDevOpsのユースケース

Dynatraceにビジネスアナリティクスを適用することで、以下の目標を容易に達成することができました。

![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase.png)

<!-- ------------------------ -->
## 4a - エラーを減らして "フリクションレス "なユーザーエクスペリエンスを実現する
Duration: 15

ドロップアウトやユーザーの不満の原因の第一は、エラーです。

エラーの原因はさまざまです。
- サーバーサイドアプリケーション - ビジネスロジック、コーディングエラーなど。
- クライアントサイドアプリケーション（ブラウザ/モバイルアプリ） - JavaScriptエラー、AJAXコール、ポップアップ画面など。
- サードパーティ - CDN、FacebookやTwitterなどの外部プロバイダなど。

さらに言えば、[コンテンツ・セキュリティ・ポリシー（CSP）違反](https://www.dynatrace.com/news/blog/automatically-detect-potential-frontend-attacks-that-cause-content-security-policy-csp-violations/) を引き起こすDDoS攻撃やクロスサイトスクリプティング攻撃もあります。
ウェブサイトの応答性を低下させたり、ユーザーのデータや個人情報を盗んだりします。

エラーには多くの種類があり、1つ1つ分析していては効率が悪いため、何を修正すべきか優先順位をつけるのは通常困難な作業です。

![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-errors1.png)

Positive
: この活動の目的は、次のようなことを行うことです。(1)コンバージョンとカスタマーエンゲージメントの向上、(2)最適化、(3)オペレーションクオリティーとレポーティング

このワークフローは、次のような人たちに有益です。
- UXデザイナー／デベロッパー
- カスタマー・エクスペリエンス・リード
- ウェブプログラマー／開発者

分析を始めるためのいくつかのエントリーポイント
   - 特定のアプリケーションで発生したすべてのエラーの概要 
   - 特定のユーザーアクションで発生したエラー
   - 特定のユーザーセッションのグループに発生したエラー
   - ダッシュボードのタイル
   - などがあります。

2つのワークフローを例に説明します。

### ワークフロー #1 - 特定のアプリケーションの概要からナビゲートする
1. Applications -> Easy Travel Angular -> Errorsへ進みます。
2. **Analyze errors**をクリックします。
3. いくつかのエラーが表示されるので、**HTTP request**タブで**HTTP 500**と表示されているものを選択します。
4. **dectection rules**を変更することができます。
5. ルールの1つは、**CSP errors**の検出をカバーしています。
   ![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-errors-CSP.png)

### ワークフロー #2 - Navigate from User Sessions
1. **User Sessions**へ進みます。
2. 上のフィルターを使う
3. データ設定をより詳細に操作するには、USQLを使用します。

### 参考
- [Automatically detect potential frontend attacks that cause Content Security Policy (CSP) violations](https://www.dynatrace.com/news/blog/automatically-detect-potential-frontend-attacks-that-cause-content-security-policy-csp-violations/)
- [Ensure unrivaled customer experience with Davis® AI-powered HTTP and custom error analytics](https://www.dynatrace.com/news/blog/extended-davis-awareness-of-http-and-custom-errors/)
- [Abandoned Shopping Cart – Minimizing errors, maximizing revenue](https://www.dynatrace.com/news/blog/abandoned-shopping-cart-minimizing-errors-maximizing-revenue/)
- [How to detect impacting 3rd Party API calls with Dynatrace Real User Monitoring (RUM)](https://www.dynatrace.com/news/blog/how-to-detect-impacting-3rd-party-api-calls-with-dynatrace-real-user-monitoring/)
- [Optimize your marketing campaign investment by leveraging BizDevOps](https://www.dynatrace.com/news/blog/optimize-your-marketing-campaign-investment-by-leveraging-bizdevops/)
- [Improve user experience with more visibility into CDN-related HTTP errors](https://www.dynatrace.com/news/blog/improve-user-experience-with-more-visibility-into-cdn-related-http-errors-part-1/)

<!-- ------------------------ -->
## 4b - 使いやすさを追求したパフォーマンスの最適化
Duration: 15

Googleは昨年、[Core Web Vitals](https://web.dev/vitals/)という取り組みを発表しました。

目標は、「ウェブ上で優れたユーザー体験を提供するために不可欠な品質シグナル」を提供するための測定値を**標準化**することです。

Googleは最近、これらのメトリクスを[ユーザーエクスペリエンスシグナル](https://developers.google.com/search/blog/2020/11/timing-for-page-experience)として検索結果ランキングに含めました。これは、これらのメトリクスが単にあると便利なものではなく、必要不可欠なものであることを示しています。

Dynatraceは、Google Core Web Vitalの測定結果を、アプリケーションのフロントエンドとバックエンド全体のインサイトで補完します。Dynatraceでは、3つのCore Web Vitalメトリクスをすべて活用することができ、数回のクリックで詳細を確認することができます。

### ページとページグループの解析ワークフロー
1. アプリケーションの概要ページからスタートします（**Applications**に進み、アプリケーション名を選択します）。
2. 2. アプリケーションの概要ページには、**Top 3 pages**が表示され、ページグループとページのタブがあります。必要な粒度を表示するタブを選択します。
   - グループ名を選択すると、選択したグループのページグループ概要が表示されます。
   - すべてのページグループを表示」を選択すると、すべてのページグループの多次元分析が表示されます。

Positive
: ***ポイント*** 多次元解析では、すべてのページまたはページグループの特に有用な概要を提供し、以下のような質問に対する答えを提供します。最も閲覧されているページ」「最も訪問されているページ」「多くのアクションが発生するビジーなページ」「最も多くのエラーを含むページ」「最初のページロードで遅いページ」「ルート変更時に遅いページ」などです。

### 分析ワークフロー
1. **View all page groups**を選択します。
   ![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-PerfOpti1.png)
2. **Performance metric**のドロップダウンで、**Largest contenful paint**を選択します。その他はデフォルトのままです。
3. **home**をクリックしてください。
   ![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-PerfOpti2.png)
4. **Performance**セクションまでスクロールダウンし、**Perform waterfall analysis**をクリックします。
   ![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-PerfOpti3.png)
5. ウォーターフォール分析では、これらのページのルート変更の根本的な動作を分析することができます。ここでは、詳細なパフォーマンスとエラー情報を文脈に沿って表示することができます。

![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-PerfOpti4.png)

### ダッシュボード・ワークフロー

1. 画面上部のパンくずを使って、1画面戻る。例：Applications > EasyTravel Angular > Page groups > home   ![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-PerfOpti5.png)
2.**Performance**セクションまでスクロールダウンし、**Analyze performance**をクリックします。
   ![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-PerfOpti6.png)
3. 下にスクロールして、**Detailed analysis...**のセクションに移動します。
4. "Analyze by" ドロップダウンから **UTM source**を選択します。
5. Performance metric "ドロップダウンから **Largest contenful paint**または **First contentful paint**のいずれかを選択する。
6. **Create metric**をクリックする。

![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-PerfOpti7.png)

7. **Split by UTM source**を有効にして、**5**を入力します。
   ![BizDevOps Use case](assets/bizops2021/BizDevOpsUseCase-PerfOpti8.png)
8. **Create metric**をクリックして、メトリックの作成を完了します。
9. ダッシュボードにチャート表示できるようになりました。

### 参考
- [Core Web Vitals: Practical metrics for optimal user experiences](https://www.dynatrace.com/news/blog/core-web-vitals-practical-metrics-for-optimal-user-experiences/)
- [Full support for Google’s Core Web Vitals improves your user experience and search rankings](https://www.dynatrace.com/news/blog/full-support-for-googles-core-web-vitals-improves-your-user-experience-and-search-rankings/)
- [Optimize modern web applications with automatic insights into pages and page groups](https://www.dynatrace.com/news/blog/optimize-modern-web-applications-with-automatic-insights-into-pages-and-page-groups/)
- [Business Insights extends support for optimizing Core Web Vitals](https://www.dynatrace.com/news/blog/business-insights-for-core-web-vitals/)


<!-- ------------------------ -->
## CSSセレクタを取得するには？

1. EasyTravel Angularホームページを評価する
   ![CSS](assets/bizops2021/ETAngular.png)
2. 任意の旅行パッケージを選択し、**book**をクリックし、ユーザー名**alex**とパスワード**alex** でログインします。
   ![CSS](assets/bizops2021/ETAngular-signin.png)
3. 名前の**Alex Elliot**を右クリックし、**Inspect**をクリックします。
4. ハイライトされた行の**...**をクリックし、**Copy** > **Copy Selector**を選択します。
   ![CSS](assets/bizops2021/ETAngular-css.gif)
5. これは、Webアプリケーションで定義されている任意のDOM要素に対して行うことができます。

<!-- ------------------------ -->
## リクエスト属性をどのように活用するか？
Duration: 15

先ほど示したように、2つのリクエスト属性 **revenue** と **destination** があらかじめ作成されています。

### 新しいリクエスト属性の定義
様々なデータソースからリクエスト属性を作成することができます。今回のハンズオンでは、旅行パッケージに申し込んだお客様の**忠誠度**を公開します。

![RequestAttribute](assets/bizops2021/RequestAttributeSource.png)

1. Settings > Server side service monitoring > Request Attributes を選択します。
2. **Define a new request attribute**ボタンをクリックします。
   ![RequestAttribute](assets/bizops2021/RequestAttributePreConfig.png)
3. **Request attribute name**を`loyalty status`として入力してください。
4. データタイプは **text** のままにしておきます。
5. **Add new data source**ボタンをクリックします。
   ![RequestAttribute](assets/bizops2021/RequestAttributeSetup1.png)
6. Configure 1st source:
   - Request attribute source: **Web request URL query parameter**
   - Parameter name: **loyalty**
   - **Save**ボタンを押してください。
   ![RequestAttribute](assets/bizops2021/RequestAttributeSetup2.png)
7. **Add new data source**ボタンをクリックします。
8. 2ndソースの設定
   - Request attribute source: **Java method parameter**
   - **Select method sources**ボタンをクリックします
     ![RequestAttribute](assets/bizops2021/RequestAttributeSetup3.png)
   - Select the process group: **com.dynatrace.easytravel.business.backend.jar easytravel**
     ![RequestAttribute](assets/bizops2021/RequestAttributeSetup4.png)
   - **Continue**ボタンを押してください。
   - 検索ボックスにこれを入力して、**Search**をクリックします。

     ```
     com.dynatrace.easytravel.business.webservice.BookingService
	 ```
   - クラスを選択し、**Continue**をクリックします。
     ![RequestAttribute](assets/bizops2021/RequestAttributeSetup5.png)
   - **Use the selected class**のラジオボタンをクリックし、**Continue**をクリックします。
     ![RequestAttribute](assets/bizops2021/RequestAttributeSetup6.png)
   - Select the method: **private void checkLoyaltyStatus**
   - **Finish**ボタンをクリックします。	
     ![RequestAttribute](assets/bizops2021/RequestAttributeSetup7.png)
   - キャプチャードロップダウンでは **2: java.lang.String**を選択します。
   - **Save**をクリックします。
     ![RequestAttribute](assets/bizops2021/RequestAttributeSetup8.png)
9. 最終的な設定は以下の画面のようになります。
10. 最後に**右上**の**Save**ボタンを忘れずにクリックしてください。

![RequestAttribute](assets/bizops2021/RequestAttributeFinal.png)
   
### セッションプロパティでのリクエスト属性の追加
1. 左のナビゲーションメニューから**Applications**を選択します。
2. **EasyTravel Angular**アプリケーションを選択します。
3. **(...)** ボタンをクリックし、**Edit**を選択します。
4. **Capturing**ヘッダーの下にある**Session and action properties**を探す。
5. **Add properties**ボタンをクリックする
6. **Custom defined property**タブを選択する
7. Expression type: に**Server side request attribute**を選択する
8. Request attribute name: **loyalty status**
- Display name:

  ```
  Member Status
  ```
- (auto fill) Key: **memberstatus**
- Storage type:
  - Store as **session** property
- **"Save property"**をクリックします。

### USQLによるデータの可視化
1. ステップ2の手順で、ダッシュボードに**USQLタイル**を追加します。
2. 以下のUSQLを使用し、**pie chart**を選びます。

   ```SQL
   select stringProperties.memberstatus, sum (doubleProperties.booking) as Revenue from usersession where stringProperties.memberstatus IS NOT NULL group by stringProperties.memberstatus order by sum(doubleProperties.booking) DESC
   ```
3. 次のようなタイルが表示されます。

![RequestAttribute](assets/bizops2021/RequestAttributeChart.png)


<!-- ------------------------ -->

## アンケート

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
  <input value="デジタルビジネスアナリティクスの学習" />
  <input value="User Session Propertiesの設定" />
  <input value="BizDevOpsのコロケーションを向上させるDynatraceの使い方" />
  <input value="USQLの使い方" />
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
: 💡 その他のアイデアや提案については、[メールでのお問い合わせ](mailto:DT-TYO-SE@dynatrace.com?subject=DynatraceによるBizDevOps - アイデア、提案)をお願いします。