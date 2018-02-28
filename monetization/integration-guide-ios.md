このガイドでは、Unity AdsをObjective-CまたはSwiftのネイティブプログラムのiOSプロジェクトに組み込む方法について説明します。

Unity Adsのフレームワークを [こちら][31] でダウンロードして下さい

動画チュートリアルはこちらで入手できます
  - ObjectC: [YouTube][62] (5:41)
  - Swift: [YouTube][63] (6:34)

<!---
動画にリンクされたスクリーンショットを使用して、埋め込み動画を「偽造」することは可能です：
[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg)](https://www.youtube.com/watch?v=YOUTUBE_VIDEO_ID_HERE)
-->

プロジェクトの例は [GitHub][70] にあります。

### コンテンツ:  
- [クイックスタートガイド](#quickstart-guide)
  - [ダッシュボードを使用する](#create-a-game-project-in-the-unity-ads-dashboard)
  - [組み込みガイド](#integrate-unity-ads-in-swift)
    - [Swift](#integrate-unity-ads-in-swift)
    - [Objective-C](#integrate-unity-ads-in-objective-c)
- [アドバンスドガイド](#unity-ads-ios-advanced-guides)

[//]: (comments)

----

# クイックスタートガイド
### Unity Ads Dashboardでゲームプロジェクトを作成する

UDNアカウントを使用して[Unity Ads Dashboard][50]にログインします。
  - まだUDNアカウントをお持ちでない場合は、[こちら][52]で登録して下さい

ダッシュボードから、新しいゲームプロジェクトを作成します。
 > 注:あなたのゲームがアメリカの子供に向けて特別に設計されていない限り、_[13歳未満の子供を対象にする]_ オプションをオフのままにしておきます。全年齢対象のゲームでは、COPPAコンプライアンスに準拠する必要はありません。

ゲームプロジェクトを選択すると、（7桁の）**Game ID** が表示されます。この番号を使用してUnity Adsを有効にします。

ゲームID（iOSまたはAndroid）を選択し、**[Placements]** タブをクリックすると、*placement ID* が表示されます。
  最初、それぞれのゲームIDには2つの場所取りがあります:
  - `動画` (デフォルト/ 5秒後に動画をスキップする)
  - `報酬タイプの動画` (スキップオプションなし)

[settings]タブで *各ゲームIDに対して* テストモードを有効にします。
  > Apple/Play Store Game ID > 設定 > テストモード: ONにする

## SwiftへのUnity Adsの組み込み
> Objective-Cは下へスクロール移動
#### Unity Adsをインポートする

最新のUnity Ads Frameworkを [こちら][31] でダウンロードしてください。

フレームワークをプロジェクトにドラッグアンドドロップしてコピーします。

<!--- プロジェクト設定の画像を追加する -->

ViewControllerにUnity Adsをインポートし、Unity Adsデリゲートを設定します：

```Swift
import UnityAds

class GameViewController: UIViewController, UnityAdsDelegate {
```

UnityAdsコールバックをUnityAdsデリゲートに追加します。
  - _unityAdsReady_
  - _unityAdsDidStart_
  - _unityAdsDidError_
  - _unityAdsDidFinish_

```Swift
func unityAdsReady(_ placementId: String) { }

func unityAdsDidStart(_ placementId: String) { }

func unityAdsDidError(_ error: UnityAdsError, withMessage message: String) { }

func unityAdsDidFinish(_ placementId: String, with state: UnityAdsFinishState) { }
```
この時点で、プロジェクトをコンパイルする必要があります。

#### Unity Adsを初期化し、動画を表示する

`UnityAds.initialize（）`でSDKを初期化します。

ダッシュボードから **7桁のGame ID** を入力し、UnityAdsデリゲートに **self** と入力します。例：

```Swift
override func viewDidLoad() {
  super.viewDidLoad()
  UnityAds.initialize("1003843", delegate: self)
}
```

動画の準備ができていることを確認し、有効なplacement IDを使用して動画を表示します：

```swift
let placement = "rewardedVideo"
if (UnityAds.isReady(placement)) {
  //a video is ready & placement is valid
  UnityAds.show(self, placementId: placement)
}
```
この時点で、あなたのゲームで動画広告を視聴できるはずです。

#### 広告の視聴者に報酬を与える

Unity Adsで収益を最大化するには、ゲーム中のプレーヤーに広告を見てもらうことが重要です。一般的な報酬は以下のとおりです。

- 無料のゲーム内通貨
- 数分のパフォーマンスの向上
- ゲームオーバー後にコンティニューすることができる特典
- その他ゲーム内アイテムまたはロック解除

`unityAdsDidFinish（）`コールバックメソッドを使用して動画がスキップされなかったことを確認し、プレーヤーに報酬を与えます：

```swift
func unityAdsDidFinish(_ placementId: String, with state: UnityAdsFinishState) {
  if state != .skipped{
    //Add code to reward the player here
  }
}
```

----

## Objective-CへのUnity Adsの組み込み
#### Unity Adsをインポートする

最新のUnity Ads Frameworkを [こちら][31] でダウンロードしてください。

フレームワークをプロジェクトにドラッグアンドドロップしてコピーします。

ViewControllerのheader（.h）にUnity Adsをインポートし、Unity Adsデリゲートを設定します。

```
#import "UnityAds/UnityAds.h"

@interface ViewController : UIViewController<UnityAdsDelegate>
```

ViewControllerのimplementation（.m）に、次の _@required_ メソッドを追加します。

```
- (void)unityAdsReady:(NSString *)placementId{
}

- (void)unityAdsDidError:(UnityAdsError)error withMessage:(NSString *)message{
}

- (void)unityAdsDidStart:(NSString *)placementId{
}

- (void)unityAdsDidFinish:(NSString *)placementId withFinishState:(UnityAdsFinishState)state{
}
```

:white_check_mark: この時点で、プロジェクトをコンパイルする必要があります。

#### Unity Adsを初期化し、動画を表示する

`UnityAds.initialize()`でSDKを初期化します:

 > 注: _7桁のGame ID_ を文字列で入力してください。

```Swift
- (void)viewDidLoad {
  [super viewDidLoad];
  [UnityAds initialize:@"1088169" delegate:self];
}
```

> 注：初期化には10秒以上かかりますので、ゲームのライフサイクルの早い段階でUnityAdsを初期化してください

動画の準備ができていることを確認し、ダッシュボードの既存のplacement IDを使用して動画を表示します。

```
if ([UnityAds isReady:@"rewardedVideo"]) {
  [UnityAds show:self placementId:@"rewardedVideo"];
}
```
:white_check_mark: この時点で、あなたのゲームで動画広告を視聴できるはずです。

#### 広告の視聴者に報酬を与える

Unity Adsで収益を最大化するには、広告を視聴するプレーヤーに報酬を与えることが重要です。 一般的な報酬は以下のとおりです。

- 無料のゲーム内通貨
- 数分のパフォーマンスの向上
- ゲームオーバー後にコンティニューできるチャンス
- ゲーム内アイテムまたはレベルのロック解除

プレーヤーに報酬を与えるには、 `unityAdsDidFinish（）`コールバックメソッドを使い、ビデオがスキップされていないことを確認してください：

```
func unityAdsDidFinish(_ placementId: String, with state: UnityAdsFinishState) {
  if state != .skipped{
    //reward the player!
  }
}
```

この時点で、動画を表示したり、プレーヤーに報酬を与えることが可能です。

----
## Unity AdsのiOSアドバンスドガイド

- [API Reference][API ref]
  - [Errors][Errors]
  - [Finish States][Finish States]
  - [Placement States][Placement States]
- [Mediation network guide][Mediation]
- [Server-to-Server callbacks][s2s]

その他のご質問は、**unityads-support@unity3d.com** までご連絡ください。

<!--- Internal links -->

[10]: optimize-ad-revenue
[12]: placements


<!--- SDK 2.0 download links -->
[31]: https://github.com/Unity-Technologies/unity-ads-ios

<!--- Dashboards -->
[50]: https://dashboard.unityads.unity3d.com
[52]: https://id.unity.com

<!--- Internal Docs -->
[62]: https://www.youtube.com/watch?v=T6VIC5E_Wt0&p=OBJECTIVEC
[63]: https://www.youtube.com/watch?v=Dhxivc9wWZ8&p=SWIFT

[70]: https://github.com/Unity-Technologies/unity-ads-ios/tree/master/UnityAdsExample

[API ref]: https://github.com/Unity-Technologies/unity-ads-ios/wiki/sdk_ios_api_reference
[Errors]: https://github.com/Unity-Technologies/unity-ads-ios/wiki/sdk_ios_api_errors
[Finish States]: https://github.com/Unity-Technologies/unity-ads-ios/wiki/sdk_ios_api_finishstates
[Placement States]: https://github.com/Unity-Technologies/unity-ads-ios/wiki/sdk_ios_api_placementstates
[Mediation]: https://github.com/Unity-Technologies/unity-ads-ios/wiki/sdk_metadata_mediation
[s2s]: https://unityads.unity3d.com/help/monetization/s2s-redeem-callbacks
