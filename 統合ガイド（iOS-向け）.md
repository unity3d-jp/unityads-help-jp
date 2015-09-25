# 統合ガイド（iOS 向け）

Unity Ads は収益化ツールです。他のゲームのトレイラー動画をあなたのゲームのユーザーに表示することで、再生 1 回ごとにあなたは報酬を受け取り、ユーザーはゲーム内アイテムを受け取ります。Unity Ads でユーザー層の収益化を始めるには、まずこの製品をあなたのゲームに組み込む必要があります。本ドキュメントではその方法を手順を追って説明します。

> 注意: このインストラクションは、Unity Ads を Unity 製でないゲームに統合するためのものです。 Unity をお使いの場合は[こちら](統合ガイド（Unity アセットストア パッケージ向け）)を参照してください。


Unity Ads の統合は以下の作業から成ります。

- Unity Ads を自分の iOS ゲームに統合する
- [Unity Ads 管理パネル][2] で Unity Ads を設定する

### Unity Ads を自分の iOS ゲームに統合する
> 注意：作業を始める前に、Unity Ads SDK を GitHub からダウンロードしておいてください。 [Unity Ads SDK from GitHub][1].

Unity Ads を統合する手順はごくシンプルで、煩雑な作業は不要です。概要をまとめると以下のようになります。

- SDK フォルダのルートディレクトリから UnityAds.framework と UnityAds.bundle を Xcode プロジェクトへインポート（ドラッグアンドドロップ）する
- SDK を自分の Unity Ads gameId で初期化する
- ユーザーに対応するキャンペーンがあるか確認する
- Unity Ads をユーザーに表示する
- ユーザーが動画を見終わるのを待ち、報酬アイテムをユーザーに渡す

> 注意: プロジェクトをコンパイルするには ``StoreKit.framework`` , ``AdSupport.framework`` , ``CoreTelephony.framework``  の３つをプロジェクトに入れておく必要があります。

コードレベルでの統合作業については後のセクションで説明します。統合の際に問題が起きた場合は、チケット登録のため support@applifier.zendesk.com にメールでご一報ください。

#### AppDelegate 内で UnityAds を初期化する
最初のステップは UnityAds クラスを AppDelegate に追加することです。まず、必要なヘッダーを含める必要があります。

```objective-c
#import <UnityAds/UnityAds.h>
```

次に、`-application:didFinishLaunchingWithOptions: ` メソッドへ、Unity Ads に gameId とルートビューコントローラを渡して初期化するコードを追加します。

```objective-c
// Initialize Unity Ads
[[UnityAds sharedInstance] startWithGameId:@"YOUR_GAME_ID_HERE" andViewController:myViewController];
```

>注意: 上の `YOUR_GAME_ID_HERE` の部分は、自分の Unity Ads gameId に置き換えてください。gameId は、[Unity Ads 管理パネル][2] の "ゲーム" > "登録済みゲーム" にて確認できます。

何らかの理由でビューコントローラを Unity Ads に初期化時点で渡せない場合は、``startWithGameId``を呼び出すだけでビューコントローラなしに Unity Ads を初期化できます。

例:

```objective-c
[[UnityAds sharedInstance] startWithGameId:@"YOUR_GAME_ID_HERE"];
```

さらに、Unity Ads を使用する前に、ビューコントローラを ``setViewController`` メソッド経由で渡します。

```objective-c
- (void)setViewController:(UIViewController *)viewController;
```
例:

```objective-c
[[UnityAds sharedInstance] startWithGameId:16];

// Other initialisation code
// ...

// Add the view controller
[[UnityAds sharedInstance] setViewController:myViewController];
```

#### ViewController をデリゲートとして設定する
ViewController は UnityAds のデリゲートとして働く必要があります。まず ViewController を以下のようにデリゲートとして登録します。

```objective-c
[[UnityAds sharedInstance] setDelegate:self];
```

このデリゲートは最低限、以下のメソッドを実装しなければなりません。

```objective-c
- (void)unityAdsVideoCompleted:(NSString *)rewardItemKey skipped:(BOOL)skipped
```

このデリゲートメソッドはユーザーが Unity Ads で動画を見終わったときに呼び出され、`skipped` が `false` であれば、ユーザーは報酬として提示されたゲーム内アイテムを受け取ります。

#### Unity Ads で動画広告を表示する
Unity Ads SDK を初期化し、ViewController を Unity Ads のデリゲートとして追加したら、アプリケーション内に広告を表示できるようになります。ただし、広告を表示する前には必ず、現在のユーザーに表示できる広告があるかどうかを確認してください。広告ユニットを表示できるかどうかを UnityAds クラスの `canShow` メソッドでチェックし、配信できる在庫の有無を `canShowAds` メソッドでチェックします。チェックが済んだら、``show`` メソッドを使ってユーザーに広告を表示します。

##### 広告配置を使用する（ゾーン）
この機能は、複数の広告セットアップを可能にします。[Unity Ads 管理パネル][2]の "ゲーム" から設定するゲームを選択し、 "開発者用設定" にて設定します。例えば、1 つのゲームにインセンティブ型広告（動画を最後まで見たら 100 コイン獲得）とインターステイシャル広告の両方を表示させることができます。通常、広告スキップはインセンティブ型広告では無効、インタースティシャル広告では有効にします。

`setZone` は `show` の直前に呼び出すようにします。

`zoneId` は広告配置を定義するときに 、`rewardItemKey` は報酬アイテムを定義するときに [Unity Ads 管理パネル][2] で定義します。


```objective-c
// Set the zone before checking readiness or attempting to show.
[[UnityAds sharedInstance] setZone:@"rewardedVideoZone"];
 
// Use the canShow method to check for zone readiness,
//  then use the canShowAds method to check for ad readiness.
if ([[UnityAds sharedInstance] canShow])
{
    // If both are ready, show the ad.
    [[UnityAds sharedInstance] show];
}
```

広告ビューは、閉じられると自身を ViewController から自動的に削除します。コードから広告ビューを閉じるには、`hide` メソッドを呼び出します。

```objective-c
[[UnityAds sharedInstance] hide]
```

>**Note:** The way this API works is that you either don't call `setZone` at all and use the default ad placement settings everywhere, or you explicitly call `setZone` always before calling `show`.

>注意：このAPIが動作する方法は、`setZone`を呼び出さず、デフォルトの広告配置の設定を使用します。`show`を呼び出す前には常に`setZone` で場所を明示してください。

#### サーバーサイドのアイテム授受コールバックのためにユーザー ID を渡す

Unity Ads にオプション パラメータを渡すには、show メソッド に、要求されたプロパティを定義する dictionary を渡します。

- `kUnityAdsOptionGamerSIDKey:@"playerName"` – このパラメータは、オファーまたはユーザー ID を Unity Ads に渡すのに使います。

例:
```objective-c
[[UnityAds sharedInstance] show:@{
    kUnityAdsOptionGamerSIDKey:@"gamerSid"}]
```

#### ユーザーに報酬を与える

>動画を見たユーザーに報酬を与える予定がない場合は、このセクションは無視してかまいません。

ユーザーが動画広告を見終わると、Unity Ads SDK は `-unityAdsVideoCompleted:rewardItemKey:skipped: delegate method` を呼び出します。この時点で、提示した報酬アイテムをユーザーに渡すことになります。このデリゲート メソッドは報酬アイテムのアイテムキーをパラメータとしてあなたに渡します。詳細は下の「アイテムキーについて」セクションを参照してください。
 
> `unityAdsVideoCompleted:rewardItemKey:skipped:` これはユーザーが動画をスキップしたかどうかを示すブール値を持つようになりました。一般に、動画をスキップしたユーザーには報酬を渡さないものですが、最終的にはパブリッシャーの判断次第です。
 
> なお、`-unityAdsVideoCompleted:rewardItemKey:skipped: method` 呼び出しは必ずしも Unity Ads adView が閉じられたことを保証しません。これをチェックするには `-unityAdsWillHide: delegate method` を使います。



#### アイテムキーについて
> 注意: 現在、アイテムキーは非推奨の機能となっており、管理パネルからは既に削除されております。

アイテムキーは、ゲーム内で種類の異なる報酬を区別するために使います。アイテムの授受はゲームコード内で行われるため、ゲームをアップデートすることなく報酬アイテムを変更する方法が必要になります。そこで、Unity Ads SDK では、コードに渡すアイテムキーを [Unity Ads 管理パネル][2] 上で設定できるようにしています。できるだけ多くの報酬アイテムをコードに追加しておき、それぞれに異なるアイテムキーを付けて区別することをお勧めします。こうすれば、コードを変更することなくアイテムを変更できます。

複数アイテムの例:

```objective-c

- (void)unityAdsVideoCompleted:(NSString *)rewardItemKey skipped:(BOOL)skipped
{
   NSLog(@"unityAdsVideoCompleted:rewardItemKey:skipped: -- key: %@", rewardItemKey);
   if([rewardItemKey isEqualToString:@"item_armor"]) {
      [self awardUserWithArmor]
   } else if([rewardItemKey isEqualToString:@"gold_50k"]) {
      [self awardUserWithGold50]
   } else if([rewardItemKey isEqualToString:@"gold_100k"]) {
      [self awardUserWithGold100]
   }
}
```

#### 複数の報酬アイテムを使用する
> 注意: 複数アイテムは SDK バージョン 1.0.3 以降でサポートされています。

ユーザーへのインセンティブに柔軟性を持たせるために、Unity Ads SDK では [Unity Ads 管理パネル][2] で報酬アイテムを複数タイプ設定し、どの報酬アイテムを渡すかを実行時に決めることができます。例えば、無料の強化アイテムを提供する際に、複数の強化アイテムの中からユーザーが欲しいものを選べるようにしたいという場合に有用です。

各ゲームにはデフォルト報酬アイテムを Unity Ads 管理パネル で定義する必要があります。これはクライアントサイドコードで報酬アイテムを明示的に選択しなかった場合に使用されます。

ユーザーに表示される報酬アイテムを変更するには、UnityAds インスタンスの - `(BOOL)setRewardItemKey:(NSString *)rewardItemKey;` メソッドを呼び出します。”

例:

```objective-c
-(void) showToUser
{

   // Get the reward item key the user selected in our game's offer menu
   NSString* reward = [myGameUI getRewardItemElement];
   // Tell Unity Ads to use this reward
   /* Note: The Unity Ads UI is already inited in the background, and this call will cause it to change
      to show the new reward item and if you show the Unity Ads view _immediately_ after calling this,
      there may be some flickering, so make sure to call setRewardItemKey as soon as possible in your code */
   [[UnityAds sharedInstance] setRewardItemKey:reward];
   // Show Unity Ads
   [[UnityAds sharedInstance] show];

   /* Now when the user completes the video, the itemKey passed
      to the unityAdsVideoCompleted:rewardItemKey:skipped: delegate method
      will be the one set in this method using the
      setRewardItemKey method
    */

}
```

現在の報酬アイテムセットをクエリするには `- (NSString *)getCurrentRewardItemKey;` メソッド を使います。使用可能な報酬アイテムすべてをクエリするには、`- (NSArray *)getRewardItemKeys; メソッドを使います。これはアイテムキーの NSArray*` を返します。アイテムの詳細 `(name, picture)` は、`- (NSDictionary *)getRewardItemDetailsWithKey:(NSString *)rewardItemKey;` メソッドを使ってクエリできます。このメソッドが返す `NSDictionary*` オブジェクトは 2 つのキーを含み、それぞれのキーは以下の値を持ちます。

```objective-c
Key kUnityAdsRewardItemPictureKey // contains the URL to the picture as a value
Key kUnityAdsRewardItemNameKey // contains the human readable name (e.g. “100 Coins!”) as a value
```

例:

```objective-c
NSArray* items = [[UnityAds sharedInstance] getRewardItemKeys];
for (id itemKey in items) {
  NSDictionary* itemDetails = [[UnityAds] sharedInstance] getRewardItemDetailsWithKey:itemKey];
  NSLog(@"Item %@: Picture: %@, Name: %@",
         itemKey,
         [itemDetails objectForKey:kUnityAdsRewardItemPictureKey]
         [itemDetails objectForKey:kUnityAdsRewardItemNameKey]);
}
```

#### 統合をテストする
統合をテストするには、Unity Ads をテストモードで実行します。まず UnityAds クラスの setTestMode:(BOOL)testModeEnabled メソッドを呼び出してから、`startWithGameId` を呼び出します。

例:

```objective-c
// TEST MODE: Do not use in production apps
[[UnityAds sharedInstance] setTestMode:YES];
[[UnityAds sharedInstance] setDebugMode:YES];

[[UnityAds sharedInstance] startWithGameId:@"YOUR_GAME_ID_HERE" andWithViewController:myViewController];
```
### Unity Ads を Unity Ads 管理パネル で設定する
Unity Ads をあなたの iOS ゲーム用に設定するには、[Unity Ads 管理パネル][2]（https://unityads.unity3d.com/admin/）にログインします。

#### ゲームを追加する
まず、ゲームを [Unity Ads 管理パネル][2] に登録します。iOS App ID か iTunes URL が必要です。

例:

> #### https://itunes.apple.com/us/app/fatcat-rush/id457251993?mt=8
または
> #### 457251993

ゲームを登録するには、[Unity Ads 管理パネル][2] のメニューから "ゲーム" ページを開き、"新しいゲームを追加" をクリックします。

追加したゲームは一覧に表示されます。ゲームをクリックすると詳細オプションにアクセスできます。"開発者用設定" をクリックしてゲームの統合の詳細を開きます。このページに記載されている Unity Ads gameId が、`startWithGameId` 呼び出しに必要となります。

ここで "高度な設定" から "広告の表示手法" にて、ゲーム内での広告配置を変更することも可能です。配置枠を追加作成して動画と画像の広告コンテナを別々に配置したり、広告にインセンティブを設定したり、様々な組み合わせを試行したりできます。各配置の設定は個別に変更可能です。

[1]: https://github.com/Applifier/unity-ads-sdk
[2]: https://unityads.unity3d.com/admin/