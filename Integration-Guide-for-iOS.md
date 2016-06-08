#iOSのゲームに組み込む

Unity Ads は収益化ツールです。他のゲームのトレイラー動画をあなたのゲームのユーザーに表示することで、再生 1 回ごとにあなたは報酬を受け取り、ユーザーはゲーム内アイテムを受け取ります。Unity Ads でユーザー層の収益化を始めるには、まずこの製品をあなたのゲームに組み込む必要があります。本ドキュメントではその方法を手順を追って説明します。

>注意：このインストラクションは、Unity Ads を Unity 製でないゲームに統合するためのものです。 Unity をお使いの場合はこちらを参照してください。

iOS のクイックスタートガイドとサンプルが以下よりダウンロードできます。
>#####https://github.com/Applifier/unity-ads-quickstart-ios

##Unity Ads 管理パネル で設定する
Unity Ads をあなたの iOS ゲーム用に設定するには、Unity Ads 管理パネルにログインします。

###ゲームを追加する

1. Unity Ads 管理パネル から `+Add new project`をクリックし、ゲームを追加します。

2. 登録には iOS App ID か iTunes URL が必要です。

>例：
>
>https://itunes.apple.com/us/app/fatcat-rush/id457251993?mt=8

>or

>457251993

追加したゲームは一覧に表示されます。ゲームをクリックすると詳細オプションにアクセスできます。このページに記載されている`GAME ID`が、`startWithGameId`呼び出しに必要となります。

対象のOSをクリック > `Ad placements` にて、ゲーム内での広告配置を変更することも可能です。配置枠を追加作成して動画と画像の広告コンテナを別々に配置したり、広告にインセンティブを設定したり、様々な組み合わせを試行したりできます。各配置の設定は個別に変更可能です。

##Unity Ads を自分の iOS ゲームに統合する

1. 下記GitHub より、Unity Ads SDKをダウンロードします。
>#####https://github.com/Applifier/unity-ads-sdk

2. SDK フォルダのルートディレクトリから`UnityAds.framework`と `UnityAds.bundle`を Xcode プロジェクトへインポート（ドラッグアンドドロップ）

3. 以下の手順でフレームワークをプロジェクトに加えます。

① `Project Settings` > `Build Phases`

②`Link Binary with Libraries`セクションを開く

③ `+`をクリックしてそれぞれのフレームワークをプロジェクトに加えます

※下記フレームワークが含まれる必要があります。

`AdSupport.framework`, `AVFoundation.framework`, `CFNetwork.framework`, `CoreFoundation.framework`

`CoreMedia.framework`, `CoreTelephony.framework`, `StoreKit.framework`, `SystemConfiguration.framework`

##Unity Ads を初期化する

最初のステップは UnityAds クラスを AppDelegate に追加することです。

In AppDelegate.h:

```
#import <UnityAds/UnityAds.h>
```
この時、Unity Ads に gameId とルートビューコントローラを渡して初期化するコードを追加します

```
// Initialize Unity Ads
UIViewController* vc = self.window.rootViewController;
[[UnityAds sharedInstance] startWithGameId:@"YOUR_GAME_ID_HERE" andViewController:vc];
```

>※上の`YOUR_GAME_ID_HERE`の部分は、対象ゲームの Unity Ads gameId に置き換えてください。
>
>####gameIdの確認方法
>
>① Unity Ads 管理パネルの`PROJECT` > 対象のゲームをクリック
>
>② `GAME ID`の欄にて確認できます。

##ViewController をデリゲートとして設定する

ViewController は UnityAds のデリゲートとして働く必要があります。まず ViewController を以下のようにデリゲートとして登録します

```
[[UnityAds sharedInstance] setDelegate:self];
```

このデリゲートは最低限、以下のメソッドを実装しなければなりません。

```
- (void)unityAdsVideoCompleted:(NSString *)rewardItemKey skipped:(BOOL)skipped
```
このデリゲートメソッドはユーザーが Unity Ads で動画を見終わったときに呼び出されます。さらなる詳細は次の項目をご覧ください。

##Unity Ads で動画広告を表示する

Unity Ads SDK を初期化し、ViewController を Unity Ads のデリゲートとして追加したら、アプリケーション内に広告を表示できるようになります。ただし、広告を表示する前には必ず、現在のユーザーに表示できる広告があるかどうかを確認してください。

広告ユニットを表示できるかどうかを、`(bool)canShow`メソッド、placement IDが有効かどうかを`(bool)setZone`メソッドでチェックすることができます。

###Ad Placement (Zones)を使用する
この機能は、複数の広告セットアップを可能にします。

①Unity Ads 管理パネル上の対象のゲーム > 対象のOSをクリック

② `Ad placements` にて設定

配置枠を追加作成して動画と画像の広告コンテナを別々に配置したり、広告にインセンティブを設定したり、様々な組み合わせを試行したりできます。各配置の設定は個別に変更可能です。例えば、1 つのゲームにインセンティブ型広告（動画を最後まで見たら 100 コイン獲得）とインターステイシャル広告の両方を表示させることができます。通常、広告スキップはインセンティブ型広告では無効、そうでない広告では有効にします。

zoneId も`Ad placements`で確認することができます。

```
if ([[UnityAds sharedInstance] setZone:@"video"] && [[UnityAds sharedInstance]canShow]) {
  //placement is valid, and ads are available!
  [[UnityAds sharedInstance] show]; //Show an ad
}
```

広告ビューは、閉じられると自身を ViewController から自動的に削除します。コードから広告ビューを閉じるには、`hide` メソッドを呼び出します。

```
[[UnityAds sharedInstance] hide]
```

>注意：このAPIが動作する方法は、setZoneを呼び出さず、デフォルトの広告配置の設定を使用します。showを呼び出す前には常にsetZoneで場所を明示してください。

##ユーザーに報酬を与える

```
注意：アイテムキーの機能はサポートが終了予定となっています。近い将来にリリースされるSDKでは、この機能は除外される見込みです。
```
ユーザーが動画広告を見終わると、Unity Ads SDK は `-unityAdsVideoCompleted:rewardItemKey:skipped: delegate` メソッドを呼び出します。この時点で、提示した報酬アイテムをユーザーに渡すことになります。

>`unityAdsVideoCompleted:rewardItemKey:skipped:` これはユーザーが動画をスキップしたかどうかを示すブール値を持つようになりました。一般に、動画をスキップしたユーザーには報酬を渡さないものですが、最終的にはパブリッシャーの判断次第です。

>なお、`-unityAdsVideoCompleted:rewardItemKey:skipped:` メソッドの呼び出しは必ずしも Unity Ads adView が閉じられたことを保証しません。これをチェックするには `-unityAdsWillHide: delegate` メソッドを使います。

##統合をテストする
統合をテストするには、Unity Ads をテストモードで実行します。まず UnityAds クラスの `setTestMode:(BOOL)testModeEnabled`メソッドを呼び出してから、`startWithGameId`を呼び出します。

```
[[UnityAds sharedInstance] setTestMode:YES];
[[UnityAds sharedInstance] setDebugMode:YES];

[[UnityAds sharedInstance] startWithGameId:@"YOUR_GAME_ID_HERE" andWithViewController:myViewController];
```
