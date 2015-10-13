# 統合ガイド（Android 向け）

Unity Ads は収益化ツールです。他のゲームのトレイラー動画をあなたのゲームのユーザーに表示することで、再生 1 回ごとにあなたは報酬を受け取り、ユーザーはゲーム内アイテムを受け取ります。Unity Ads でユーザー層の収益化を始めるには、まずこの製品をあなたのゲームに統合する必要があります。本ドキュメントではその方法を手順を追って説明します。

> 注意: このインストラクションは、Unity Ads を Unity 製でないゲームに統合するためのものです。 Unity をお使いの場合は[こちら](統合ガイド（Unity アセットストア パッケージ向け）)を参照してください。


Unity Ads の統合は以下の作業から成ります。

- Unity Ads を自分の Android ゲームに統合する
- [Unity Ads 管理パネル][1] で Unity Ads を設定する 

### Unity Ads を自分の Android ゲームに統合する
> 注意: 作業を始める前に、[Unity Ads SDK from GitHub][2] を GitHub からダウンロードしておいてください。

Unity Ads を統合する手順はごくシンプルで、煩雑な作業は不要です。概要をまとめると以下のようになります。

- `unity-ads` ライブラリプロジェクトを自分のアプリケーションにインポートする
- SDK を自分の game ID で初期化する
- ユーザーに対応するキャンペーンがあるか確認する
- Unity Ads をユーザーに表示する
- ユーザーが動画を見終わるのを待つ（オプションで報酬アイテムをユーザーに渡すことも可能）

コードレベルでの統合作業については後のセクションで説明します。統合の際に問題が起きた場合は、チケット登録のため [unityads-support@unity3d.com[en]](mailto:unityads-support@unity3d.com) にメールでご一報ください。

#### Add the Unity Ads SDK to Your Project (Android Studio)

Download unity-ads.aar from SDK repository. This package contains all binaries and configuration files for Unity Ads SDK.

You start importing Unity Ads by selecting 'File' > 'New' > 'New Module' and choosing 'Import .JAR/.AAR Package'. Select the downloaded aar file from your computer and import the file.

Now you need to add the imported Unity Ads module as a dependency to your own module. Right click your app module, select 'Open Module Settings' and open 'Dependencies' tab. Add a new dependency as 'Module Dependency' and select Unity Ads module.

#### Unity Ads SDK をプロジェクトに追加する (Eclipse)

SDK 内にある `unity-ads` ディレクトリが SDK プロジェクト本体です。

このプロジェクトは任意のワークスペースにインポートできますが、ワークスペース内にすでに `unity-ads` プロジェクトがないかどうか確認してください。存在する場合、それは古いバージョンの Unity Ads SDK を参照している可能性があります。既存の `unity-ads` プロジェクトを削除するか、ワークスペースを変更してください。

SDK ライブラリプロジェクトをインポートするには、Eclipse の File > Import メニューから General または Existing Projects into Workspace を選択します。

ブラウズして SDK フォルダのルートを選択します。SDK がリストに「unity-ads」という名前で表示されます。

「Copy projects into workspace」のチェックは外します。このオプションが有用になるのは、将来 SDK をインプレース更新し、それに合わせてワークスペースのライブラリプロジェクトも更新する場合です。

SDK ライブラリプロジェクトをインポートしたら、アプリケーションをそのライブラリプロジェクトにリンクする必要があります。プロジェクトのプロパティを表示し、「Android」タブを選択します。ダイアログの下部にある「Add」をクリックし、ワークスペースから「unity-ads」プロジェクトを選択します。

Unity Ads SDK をライブラリプロジェクトとして使う場合、`AndroidManifest.xml` の変更点や Proguard 設定を手動でマージする必要はありません。問題が発生する場合は、`project.properties` 内の `manifestmerger.enabled` が `true` に設定されているか確認してください。

#### Unity Ads をコードベースに統合する

##### UnityAds を初期化する
Unity Ads の機能は `UnityAds` クラスを通じて提供されます。`UnityAds` を初期化する際は、親 Activity（ほとんどの場合は現在のクラス）、あなたの game ID、`UnityAdsListener` を `UnityAds` に渡す必要があります。

```java
UnityAds.init((Activity)this, "11001", (UnityAdsListener)this);
```

##### Unity Ads をテストするためのデバックモードとテストモード
**デバックモード**に切り替えることで、UnityAds統合の際に何が起きているかをより詳細に見ることができます。何も出ない場合には、デバックモードを正常に動かすためにSDKをリコンパイルしてください。

**テストモード**では、統合のテストのために無制限に広告が提供されます。

>注意：コンパイルやストアへの提出の前に、デバックモードとテストモードを取り除き、最後の動作確認を行ってください。

```java
UnityAds.setDebugMode(true);
UnityAds.setTestMode(true);
```

##### Activity ライフサイクルとの統合
Unity Ads SDK が現在の Activity を把握できるよう、Activity ライフサイクルに変化があれば Unity Ads SDK に通知する必要があります。これは `onResume メソッド`内の `changeActivity(Activity)` メソッドを呼び出すことで簡単に実現できます。

例:

```java
@Override
public void onResume() {
  super.onResume();
  UnityAds.changeActivity(this);
}
```

ほとんどの場合はこれで充分ですが、Unity Ads を起動したときの、アプリケーションにおける散発的なビヘイビアを見るには、Activity の `onCreate()` メソッド内の `changeActivity(this)` も呼び出すとよいでしょう。

##### UnityAdsListener インターフェイス
Unity Ads SDK は UnityAdsListener インターフェイスを通じてゲームに重要なイベントを通知します。この `UnityAdsListener `も初期化の際に `UnityAds` に渡さねばなりません。または、`setListener(UnityAdsListener) `メソッド経由で代替リスナーを構成することもできます。インターフェイスの中で特に重要なメソッドは `onFetchCompleted` と `onFetchFailed` です。これらは現在のユーザーに配信できる広告があるかどうかをゲームに通知する働きをします。

`UnityAds` は、 `init` メソッド呼び出しで初期化されるとき、自動で在庫を非同期的にチェックします。チェックの結果、利用できるキャンペーン在庫があった場合は `onFetchCompleted` メソッドを呼び出し、在庫がなかった（または接続の問題で在庫を取得できなかった）場合には `onFetchFailed` を呼び出します。`onFetchCompleted` 情報を受け取るまでは、オファーを宣伝したり広告の表示を試みたりするべきではありません。

また、インターフェイスは以下のメソッドを通じて、Unity Ads に対するユーザー インタラクションの情報も提供します。

- `onShow()` – Unity Ads がユーザーに表示されたときに呼び出される
- `onHide()` – Unity Ads をユーザーが閉じたときに呼び出される
- `onVideoStarted()` – ユーザーが動画再生を開始したときに呼び出される
- `onVideoCompleted(String itemKey, boolean skipped)` – 動画再生が完了したときに呼び出される（このステップは、ユーザーに報酬を受け取る資格があるかどうかも通知します）

##### Unity Ads の表示
Unity Ads SDK の初期化が終われば、Unity Ads の広告をユーザーに表示できます。その前に、広告ユニットが表示できる状態かどうかを `canShow` で確認し、現在のユーザーに配信する在庫があるかどうかを確認する必要があります。それが終われば、`show` メソッドを呼び出すだけで広告がユーザーに表示されます。デフォルトでは、デフォルトのゾーン設定が使用されます。

```java
if(UnityAds.canShow()) {
  UnityAds.show();
}
```

###### 広告表示を使用する（ゾーン）
この機能は、複数の広告セットアップを可能にします。[Unity Ads 管理パネル][1]の "ゲーム" から設定するゲームを選択し、 "開発者用設定" にて設定します。例えば、1 つのゲームにインセンティブ型広告（動画を最後まで見たら 100 コイン獲得）とインターステイシャル広告の両方を表示させることができます。通常、広告スキップはインセンティブ型広告では無効、インターステイシャル広告では有効にします。

```java
  public boolean setZone(String zoneId) 
  public boolean setZone(String zoneId, String rewardItemKey) 
```

`setZone` は `show` の直前に呼び出すようにします。

例:

```java
UnityAds.setZone("12540-1392899280");

if(UnityAds.canShow()) {
  UnityAds.show();
}
```

`zoneId` は広告表示を定義するときに 、`rewardItemKey` は報酬アイテムを定義するときに [Unity Ads 管理パネル][1] で定義します。

You can get the currently active zone with `getZone` method that returns the currently active zoneId as a String.

`getZone` メソッドで現在のアクティブゾーンを zoneID の文字列として受け取ることができます。

>
>注意：このAPIが動作する方法は、`setZone`を呼び出さず、デフォルトの広告表示の設定を使用します。`show`を呼び出す前には常に`setZone` で場所を明示してください。


#### サーバーサイドのアイテム授受コールバックのためにユーザー ID を渡す

Unity Ads にオプション パラメータを渡すには、`show` メソッド に、要求されたプロパティを定義する `Map` オブジェクトを渡します。

- `UnityAds.UNITY_ADS_OPTION_GAMERSID_KEY` – このパラメータを通じて、オファーまたはユーザー ID を UnityAds に渡すことができます。 

例:

```java
HashMap<String,Object> properties = new HashMap<String,Object>();
properties.put(UnityAds.UNITY_ADS_OPTION_GAMERSID_KEY, "gamerSid");
UnityAds.show(properties);
```

#### ユーザーに報酬を与える

> 動画を見たユーザーに報酬を与える予定がない場合は、このセクションは無視してかまいません。

ユーザーが動画を最後まで見終わったら、約束の報酬を渡す必要があります。ユーザーが広告をスキップした場合にはその旨を示すブールフラグが立ちます。

動画が最後まで再生されたら、`onVideoCompleted(String itemKey, boolean skipped)` リスナーメソッドがコールされ、現在の報酬アイテムキーがパラメータとして渡されます。

> **アイテムキーについて**: アイテムキーは、ゲーム内で種類の異なる報酬を区別するために使います。アイテムの授受はゲームコード内で行われるため、ゲームをアップデートすることなく報酬アイテムを変更する方法が必要になります。そこで、Unity Ads SDK では、コードに渡すアイテムキーを [Unity Ads 管理パネル][1] 上で設定できるようにしています。できるだけ多くの報酬アイテムをコードに追加しておき、それぞれに異なるアイテムキーを付けて区別することをお勧めします。こうすれば、コードを変更することなくアイテムを変更できます。

現在デフォルトに設定されている報酬アイテムは、 `getCurrentRewardItemKey()` メソッド経由で取得できます。これを使って、どの報酬アイテムがユーザーに渡されるのかをチェックできます。`show` 呼び出しの前にアイテムを変更したい場合は、`setRewardItemKey(String)` メソッドを呼び出します。デフォルトアイテムのアイテムキーは、`getDefaultRewardItemKey()`を呼び出せばいつでも取得できます。

#### 統合サポート
統合の際に問題が起きた場合は、チケット登録のため [unityads-support@unity3d.com[en]](mailto:unityads-support@unity3d.com) にメールでご一報ください。

[1]: https://unityads.unity3d.com/admin
[2]: https://github.com/Applifier/unity-ads-sdk