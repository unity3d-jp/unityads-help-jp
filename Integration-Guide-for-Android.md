#Androidのゲームに組み込む

Unity Ads は収益化ツールです。他のゲームのトレイラー動画をあなたのゲームのユーザーに表示することで、再生 1 回ごとにあなたは報酬を受け取り、ユーザーはゲーム内アイテムを受け取ります。Unity Ads でユーザー層の収益化を始めるには、まずこの製品をあなたのゲームに統合する必要があります。本ドキュメントではその方法を手順を追って説明します。

>注意: このインストラクションは、Unity Ads を Unity 製でないゲームに統合するためのものです。Unity をお使いの場合はこちらを参照してください。

Unity Ads の統合は以下の作業から成ります。

- Unity Ads 管理パネル で Unity Ads を設定する
- Unity Ads を自分の Android ゲームに統合する

##Unity Ads 管理パネル で設定する

Unity Ads をあなたの Android ゲーム用に設定するには、Unity Ads 管理パネルにログインします。

###ゲームを追加する

1. Unity Ads 管理パネル から +Add new projectをクリックし、ゲームを追加します。

2. 登録には Google play store URL が必要です。


追加したゲームは一覧に表示されます。ゲームをクリックすると詳細オプションにアクセスできます。このページに記載されている`GAME ID`が、`startWithGameId`呼び出しに必要となります。

対象のOSをクリック > `Ad placements` にて、ゲーム内での広告配置を変更することも可能です。配置枠を追加作成して動画と画像の広告コンテナを別々に配置したり、広告にインセンティブを設定したり、様々な組み合わせを試行したりできます。各配置の設定は個別に変更可能です。

##Unity Ads を自分の Android ゲームに統合する

1. 下記GitHub より、Unity Ads SDKをダウンロードします。
 
 >#####https://github.com/Applifier/unity-ads-sdk

Unity Ads を統合する手順はごくシンプルで、煩雑な作業は不要です。概要をまとめると以下のようになります。

- Unity Ads SDKを対象ゲームのgameIdで初期化
- Unity Ads ライブラリプロジェクトを、対象ゲームにインポートする
- ユーザーに対応するキャンペーンがあるか確認する
- Unity Ads をユーザーに表示する
- ユーザーが動画を見終わるのを待つ（オプションで報酬アイテムをユーザーに渡すことも可能）

コードレベルでの統合作業については後のセクションで説明します。統合の際に問題が起きた場合は、チケット登録のため unityads-support@unity3d.com[en] にメールでご一報ください。

##Android Studio を使って Unity Ads SDK をプロジェクトに追加する

SDK レポジトリから unity-ads.aar をダウンロードします。このパッケージには、Unity Ads SDK の全てのバイナリとファイルが含まれています。

SDK ライブラリプロジェクトをインポートするには、File > New > New Module から Import .JAR/.AAR Package を選択します

Unity Ads モジュールは、お使いになっているモジュールに依存します。お使いになっている app モジュールを右クリックし、Open Module Settings から Dependencies タブを開いて、 Module Dependency から Unity Ads module を選択してください。

##Eclipse を使って Unity Ads SDK をプロジェクトに追加する

SDK 内にある unity-ads ディレクトリが SDK プロジェクト本体です。

このプロジェクトは任意のワークスペースにインポートできますが、ワークスペース内にすでに unity-ads プロジェクトがないかどうか確認してください。存在する場合、それは古いバージョンの Unity Ads SDK を参照している可能性があります。既存の unity-ads プロジェクトを削除するか、ワークスペースを変更してください。

SDK ライブラリプロジェクトをインポートするには、Eclipse の File > Import メニューから General または Existing Projects into Workspace を選択します。

ブラウズして SDK フォルダのルートを選択します。SDK がリストに「unity-ads」という名前で表示されます。

「Copy projects into workspace」のチェックは外します。このオプションが有用になるのは、将来 SDK をインプレース更新し、それに合わせてワークスペースのライブラリプロジェクトも更新する場合です。

SDK ライブラリプロジェクトをインポートしたら、アプリケーションをそのライブラリプロジェクトにリンクする必要があります。プロジェクトのプロパティを表示し、「Android」タブを選択します。ダイアログの下部にある「Add」をクリックし、ワークスペースから「unity-ads」プロジェクトを選択します。

Unity Ads SDK をライブラリプロジェクトとして使う場合、AndroidManifest.xml の変更点や Proguard 設定を手動でマージする必要はありません。問題が発生する場合は、project.properties 内の manifestmerger.enabled が true に設定されているか確認してください。

##Unity Ads をコードベースに統合する

###UnityAds を初期化する

Unity Ads の機能は UnityAds クラスを通じて提供されます。UnityAds を初期化する際は、親 Activity（ほとんどの場合は現在のクラス）、あなたの game ID、UnityAdsListener を UnityAds に渡す必要があります。

```
UnityAds.init((Activity)this, "11001", (UnityAdsListener)this);
```

###Unity Ads をテストするためのデバックモードとテストモード
デバックモードに切り替えることで、UnityAds統合の際に何が起きているかをより詳細に見ることができます。何も出ない場合には、デバックモードを正常に動かすためにSDKをリコンパイルしてください。テストモードでは、テスト用の広告を無制限に配信します。

```
UnityAds.setDebugMode(true);
UnityAds.setTestMode(true);
```

###Activity ライフサイクルとの統合

Unity Ads SDK が現在の Activity を把握できるよう、Activity ライフサイクルに変化があれば Unity Ads SDK に通知する必要があります。これは `onResume` メソッド内の `changeActivity(Activity)` メソッドを呼び出すことで簡単に実現できます。

例：

```
@Override
public void onResume()
{
    super.onResume();
    UnityAds.changeActivity(this);
}
```

ほとんどの場合はこれで充分ですが、Unity Ads を起動したときの、アプリケーションにおける散発的なビヘイビアを見るには、Activity の `onCreate()` メソッド内の `changeActivity(this)` も呼び出すとよいでしょう。

###UnityAdsListener インターフェイス

Unity Ads SDK は `UnityAdsListener` インターフェイスを通じてゲームに重要なイベントを通知します。この UnityAdsListenerも初期化の際に UnityAds に渡さねばなりません。または、`setListener(UnityAdsListener)`メソッド経由で代替リスナーを構成することもできます。インターフェイスの中で特に重要なメソッドは `onFetchCompleted` と `onFetchFailed` です。これらは現在のユーザーに配信できる広告があるかどうかをゲームに通知する働きをします。

UnityAds は、 `init` メソッド呼び出しで初期化されるとき、自動で在庫を非同期的にチェックします。チェックの結果、利用できるキャンペーン在庫があった場合は `onFetchCompleted` メソッドを呼び出し、在庫がなかった（または接続の問題で在庫を取得できなかった）場合には onFetchFailed を呼び出します。onFetchCompleted 情報を受け取るまでは、オファーを宣伝したり広告の表示を試みたりするべきではありません。

また、インターフェイスは以下のメソッドを通じて、Unity Ads に対するユーザー インタラクションの情報も提供します。

- `onShow()` – Unity Ads がユーザーに表示されたときに呼び出される
- `onHide()` – Unity Ads をユーザーが閉じたときに呼び出される
- `onVideoStarted()` – ユーザーが動画再生を開始したときに呼び出される
- `onVideoCompleted(String itemKey, boolean skipped)` – 動画再生が完了したときに呼び出される（このステップは、ユーザーに報酬を受け取る資格があるかどうかも通知します）

###Unity Ads の表示

Unity Ads SDK の初期化が終われば、Unity Ads の広告をユーザーに表示できます。その前に、広告ユニットが表示できる状態かどうかを canShow で確認し、現在のユーザーに配信する在庫があるかどうかを確認する必要があります。それが終われば、show メソッドを呼び出すだけで広告がユーザーに表示されます。デフォルトでは、デフォルトのゾーン設定が使用されます。

```
if ( UnityAds.canShow() )
{
    UnityAds.show();
}
```

###Ad Placement (Zones)を使用する

この機能は、複数の広告セットアップを可能にします。Unity Ads 管理パネル上の対象のゲーム > 対象のOSをクリックし "Ad placements" にて設定します。配置枠を追加作成して動画と画像の広告コンテナを別々に配置したり、広告にインセンティブを設定したり、様々な組み合わせを試行したりできます。各配置の設定は個別に変更可能です。例えば、1 つのゲームにインセンティブ型広告（動画を最後まで見たら 100 コイン獲得）とインターステイシャル広告の両方を表示させることができます。通常、広告スキップはインセンティブ型広告では無効、インターステイシャル広告では有効にします。

```
public boolean setZone(String zoneId)
```

zone ID が有効でない場合、false が返されます。

広告が表示される前に、setZone と canShow は true を返します。今のところ、`canShow` メソッドは zone ID のセットには適当ではありません。

例：

```
if ( UnityAds.setZone("rewardedVideo") && UnityAds.canShow() )
{
    UnityAds.show();
}
```

zoneId は広告表示を定義するときに 、rewardItemKey は報酬アイテムを定義するときに Unity Ads 管理パネルの`Ad placement` で定義します。

`getZone` メソッドで現在のアクティブゾーンを zoneID の文字列として受け取ることができます。

Note: Current functionality for the SDK requires you to implement the API using one of two use cases:

1. Never call the setZone method, and use the default zone instead. During initialization, the default zone is assigned as the current zone.

2. Call the setZone method each time you call canShow, or before you call show in order to set the current zone.

Note: The way this API works is that you either don't call setZone at all and use the default ad placement settings everywhere, or you explicitly call setZone always before calling show.

>注意：この API が動作する方法は、setZone を呼び出さず、デフォルトの広告表示の設定を使用します。show を呼び出す前には常に setZone で場所を明示してください。

##アイテムキーの機能は SDK 2.0 以降削除される予定です

###サーバーサイドのアイテム授受コールバックのためにユーザー ID を渡す

Unity Ads にオプション パラメータを渡すには、`show` メソッド に、要求されたプロパティを定義する `Map` オブジェクトを渡します。

`UnityAds.UNITY_ADS_OPTION_GAMERSID_KEY –`

```
HashMap<String,Object> properties = new HashMap<String,Object>();
properties.put(UnityAds.UNITY_ADS_OPTION_GAMERSID_KEY, "gamerSid");
UnityAds.show(properties);
```

##ユーザーに報酬を与える

>動画を見たユーザーに報酬を与える予定がない場合は、このセクションは無視してかまいません。

ユーザーが動画を最後まで見終わったら、約束の報酬を渡す必要があります。ユーザーが広告をスキップした場合にはその旨を示すブールフラグが立ちます。

```
アイテムキーの機能は SDK 2.0 以降削除される予定です
```

動画が最後まで再生されたら、onVideoCompleted(String itemKey, boolean skipped) リスナーメソッドがコールされ、現在の報酬アイテムキーがパラメータとして渡されます。

>もし skipped が false の場合、ユーザーは広告を見たとみなされ、リワードが与えられます。

##統合サポート

統合の際に問題が起きた場合は、チケット登録のため ads-support@unity3d.co.jp にメールでご一報ください。