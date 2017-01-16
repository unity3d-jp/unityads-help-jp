# iOSのゲームに組み込む

このガイドでは、Unity Ads を iOS のプロジェクト（Objective-C, Swift）に組み込む方法を説明します。

Unity Ads Framework は[こちら](https://github.com/Unity-Technologies/unity-ads-ios)からダウンロードして下さい。

Video tutorials available here
ビデオチュートリアルが以下でご覧いただけます。（英語）

- Objective-C: [YouTube](https://www.youtube.com/watch?v=T6VIC5E_Wt0&p=OBJECTIVEC) (5:41)
- Swift: [YouTube](https://www.youtube.com/watch?v=Dhxivc9wWZ8&p=SWIFT) (6:34)

サンプルプロジェクトは [GitHub](https://github.com/Unity-Technologies/unity-ads-ios/tree/master/UnityAdsExample) を参照して下さい。 

##目次
- [Unity Ads ダッシュボードを設定する](https://github.com/unity3d-jp/unityads-help-jp/wiki/Integration-Guide-for-iOS#unity-ads-%E3%83%80%E3%83%83%E3%82%B7%E3%83%A5%E3%83%9C%E3%83%BC%E3%83%89%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B)
- [Swift を使用して Unity Ads を組み込む](https://github.com/unity3d-jp/unityads-help-jp/wiki/Integration-Guide-for-iOS#swift-%E3%82%92%E4%BD%BF%E7%94%A8%E3%81%97%E3%81%A6-unity-ads-%E3%82%92%E7%B5%84%E3%81%BF%E8%BE%BC%E3%82%80)
- [Objective-C を使って Unity Ads を組み込む](https://github.com/unity3d-jp/unityads-help-jp/wiki/Integration-Guide-for-iOS#objective-c-%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6-unity-ads-%E3%82%92%E7%B5%84%E3%81%BF%E8%BE%BC%E3%82%80)
- [Advanced Guides](https://github.com/unity3d-jp/unityads-help-jp/wiki/Integration-Guide-for-iOS#unity-ads-ios-advanced-guides)

##Unity Ads ダッシュボードを設定する

###Unity Ads ダッシュボードに Game Project を作成する

1. UDN アカウントをつかって Unity Ads ダッシュボードにログイン（アカウントをお持ちでない場合、[こちら](https://id.unity.com/)からサインアップして下さい。）

2. ダッシュボードから、新しい game project を作成

>注意: あなたのゲームがアメリカの子供のために特別に設計されていない限り、「13歳未満の子供を対象にする」オプションはオフのままにしてください。全年齢向けのゲームでは、COPPAへの準拠をオンにする必要はありません。

###アプリIDと広告枠IDを確認する

3. 作成したゲームプロジェクトをクリック > アプリID の欄に番号が表示されます。Unity Ads を有効にする際、この番号を使用します。

4. アプリID (iOS もしくは Android）をクリック > 広告枠タブをクリック > 広告枠IDが表示されます。設定開始時には、以下二つの広告枠が設定されています。

- video (デフォルト / 5秒後にスキップ可能)
- rewardedVideo (スキップ不可)

### テストモードを利用する

以下の手順でテストモードが利用可能です。

アプリIDをクリック > 設定タブをクリック > テストモードのスイッチをオンにする

##Swift を使用して Unity Ads を組み込む

###Unity Ads のインポート

1. [こちら](https://github.com/Unity-Technologies/unity-ads-ios)から最新の Unity Ads Framework をダウンロードします。

2. Framework をプロジェクトにドラッグアンドドロップし、コピーします。

3. ViewController に Unity Ads をインポートし、Unity Ads delegate をセットします。

```
import UnityAds

class GameViewController: UIViewController, UnityAdsDelegate {
```

※以下の UnityAds callbacks を UnityAdsDelegate に加えます。

- unityAdsReady
- unityAdsDidStart
- unityAdsDidError
- unityAdsDidFinish

```
func unityAdsReady(_ placementId: String) { }

func unityAdsDidStart(_ placementId: String) { }

func unityAdsDidError(_ error: UnityAdsError, withMessage message: String) { }

func unityAdsDidFinish(_ placementId: String, with state: UnityAdsFinishState) { }
```

ここでプロジェクトをコンパイルします。

###Unity Ads のイニシャライズと動画の表示

1. `UnityAds.initialize()`を使ってSDKをイニシャライズします。

2. アプリIDを管理画面からコピーし、UnityAdsDelegate に入力します。

```
override func viewDidLoad() {
  super.viewDidLoad()
  UnityAds.initialize("1003843", delegate: self)
}
```

※動画の準備ができていることを確認し、有効な広告枠IDを使用して動画を表示してください

```
let placement = "rewardedVideo"
if (UnityAds.isReady(placement)) {
  //a video is ready & placement is valid
  UnityAds.show(self, placementId: placement)
}
```

この時点で、あなたのゲームで動画広告を見ることができるはずです。

### 広告を視聴したプレイヤーにリワードを与える

プレイヤーにゲーム内リワードを与えることは、Unity Ads の広告収入を最大化するためにとても重要です。以下のようなリワードがよくある事例です。

- ゲーム内通貨
- 一時的な能力ブースト
- ゲームオーバーの後のコンティニュー（広告を観たらコンティニューできる）
- ゲーム内アイテムの付与、またはアンロック

`unityAdsDidFinish() ` callback method うとともにプレイヤーにリワードを与える時、動画がスキップできないようになっているか確認できます。

```
func unityAdsDidFinish(_ placementId: String, with state: UnityAdsFinishState) {
  if state != .skipped{
    //Add code to reward the player here
  }
}
```

## Objective-C を使って Unity Ads を組み込む

###Unity Ads のインポート

1. [こちら](https://github.com/Unity-Technologies/unity-ads-ios)から最新版の Unity Ads Framework をダウンロードします。

2. Framework をプロジェクトにドラッグアンドドロップし、コピーします。

3. ViewController header (.h) に Unity Ads をインポートし、Unity Ads delegate をセットします。

```
#import "UnityAds/UnityAds.h"

@interface ViewController : UIViewController<UnityAdsDelegate>
```

Viewcontroller implementation (.m) に以下の *@required* methods を加えます。

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

ここでプロジェクトをコンパイルします。

###Unity Ads のイニシャライズと動画の表示

1. `UnityAds.initialize():` を使ってSDKをイニシャライズします。

2. アプリIDを入力。

```
- (void)viewDidLoad {
  [super viewDidLoad];
  [UnityAds initialize:@"1088169" delegate:self];
}
```

初期化には10秒以上かかりますので、ゲームサイクルの早い段階でUnityAdsを初期化するようにしてください。

※動画の準備ができていることを確認し、有効な広告枠IDを使用して動画を表示してください

```
if ([UnityAds isReady:@"rewardedVideo"]) {
  [UnityAds show:self placementId:@"rewardedVideo"];
}
```

この時点で、あなたのゲームで動画広告を見ることができるはずです。

### 広告を視聴したプレイヤーにリワードを与える

プレイヤーにゲーム内リワードを与えることは、Unity Ads の広告収入を最大化するためにとても重要です。以下のようなリワードがよくある事例です。

- ゲーム内通貨
- 一時的な能力ブースト
- ゲームオーバーの後のコンティニュー（広告を観たらコンティニューできる）
- ゲーム内アイテムの付与、またはアンロック

`unityAdsDidFinish()` callback method を使うとともに、動画がスキップできないようになっているか確認して下さい。


```
func unityAdsDidFinish(_ placementId: String, with state: UnityAdsFinishState) {
  if state != .skipped{
    //reward the player!
  }
}
```

##Unity Ads iOS Advanced Guides
- [API Reference](https://github.com/Unity-Technologies/unity-ads-ios/wiki/sdk_ios_api_reference)
- [Errors](https://github.com/Unity-Technologies/unity-ads-ios/wiki/sdk_ios_api_errors)
- [Finish States](https://github.com/Unity-Technologies/unity-ads-ios/wiki/sdk_ios_api_finishstates)
- [Placement States](https://github.com/Unity-Technologies/unity-ads-ios/wiki/sdk_ios_api_placementstates)
- [Mediation network guide](https://github.com/Unity-Technologies/unity-ads-ios/wiki/sdk_metadata_mediation)
- [Server-to-Server callbacks](https://github.com/Unity-Technologies/unity-ads-ios/wiki/sdk_metadata_s2s_callbacks)