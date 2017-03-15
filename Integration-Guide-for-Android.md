# Androidのゲームに組み込む

このガイドでは、Unity Ads を Android Studio のプロジェクトに組み込む方法を説明します。

最新版の Unity Ads は[こちら](https://github.com/Unity-Technologies/unity-ads-android/releases)からダウンロードして下さい。

ビデオチュートリアルが以下でご覧いただけます。（英語）

- Androdid: [YouTube](https://www.youtube.com/watch?v=MNdJ0KWlYPw) (6:57)

サンプルプロジェクトは [GitHub](https://github.com/Unity-Technologies/unity-ads-android/tree/master/app) を参照して下さい。

## 目次
- Unity Ads ダッシュボードを設定する
- Android Studio で Unity Ads を組み込む
- Android Studio を使わずに組み込む

Unity Ads 1.5.x からのアップグレードの場合、Android transition guide を参照して下さい。

## Unity Ads ダッシュボードを設定する

### Unity Ads ダッシュボードに Game Project を作成する

1. UDN アカウントをつかって Unity Ads ダッシュボードにログイン（アカウントをお持ちでない場合、[こちら](https://id.unity.com/)からサインアップして下さい。）

2. ダッシュボードから、新しい game project を作成

>注意: あなたのゲームがアメリカの子供のために特別に設計されていない限り、「13歳未満の子供を対象にする」オプションはオフのままにしてください。全年齢向けのゲームでは、COPPAへの準拠をオンにする必要はありません。

### アプリIDと広告枠IDを確認する

3. 作成したゲームプロジェクトをクリック > アプリID の欄に番号が表示されます。Unity Ads を有効にする際、この番号を使用します。

4. アプリID (iOS もしくは Android）をクリック > 広告枠タブをクリック > 広告枠IDが表示されます。設定開始時には、以下二つの広告枠が設定されています。

- video (デフォルト / 5秒後にスキップ可能)
- rewardedVideo (スキップ不可)

### テストモードを利用する

以下の手順でテストモードが利用可能です。

アプリIDをクリック > 設定タブをクリック > テストモードのスイッチをオンにする

## Android Studio で Unity Ads を組み込む

1. 最新版のバイナリを [GitHub](https://github.com/Unity-Technologies/unity-ads-android/releases) よりダウンロードして下さい。

2. 対象の Android project を Android Studio で開きます。

3. Add new module より、`unity-ads.aar.` をインポートします。※今回は "unity-ads" というモジュール名にします。

4. モジュール設定を開き、"unity-ads" モジュールを dependency として加えます。

5. 以下を java Activity file に加えます。

`import com.unity3d.ads.IUnityAdsListener;`
`import com.unity3d.ads.UnityAds;`

6.  `implements IUnityAdsListener` をクラスに加えるとともに、Android Studio に callback methods を加えます。

7. Within the activity that implements `IUnityAdsListener`, initialize Unity Ads by calling `UnityAds.initialize(this, gameId, this)` where `gameId`  is a String value set to the game ID of the Android platform, found under your project in the Unity Ads dashboard.

`IUnityAdsListener` のインプリメントの間に、`UnityAds.initialize(this, gameId, this)` の call によるイニシャライズを行います。`gameId` は各プロジェクトに振り分けられる数字の列で、Unity Ads ダッシュボードで確認できます。

>注意: Unity Ads は一度しかイニシャライズを行いません。SDK 2.0 では、より強力なネットワークリトライの仕組みが組み込まれています。これによりネットワーク接続無しでの安全なイニシャライズが可能です。また、広告のリクエストをネットワークが復帰したタイミングで行います。

8. 次のコードは、デフォルトの広告枠が表示されます
`if (UnityAds.isReady()) { UnityAds.show(this); }`

Unity Ads SDK 2.0 では、各 show メソッドの呼び出しでアクティビティ引数が使用されます。（上の例がこれにあたります）

API の詳細については、Unity Ads Android API reference を参照してください。また、さまざまな広告枠が表示されるような統合方法の例については、[サンプルアプリケーション](https://github.com/Unity-Technologies/unity-ads-android/blob/master/app/src/main/java/com/unity3d/ads/example/UnityAdsExample.java)を参照してください。

## Android Studio を使わずに組み込む

お使いのビルドシステムにて、AAR パッケージをお使いいただけ無い場合、同様のリソースを unity-ads.zip として GitHub にて ZIP ファイルでも提供しています。以下の3点に注意して下さい。

- ビルド時に、`classes.jar`を含める

- `AndroidManifest.xml` から手動でマニフェストをマージします。 `AdUnitActivity` アクティビティと `AdUnitSoftwareActivity` アクティビティの両方を含めるようにしてください。また、`INTERNET` と `ACCESS_NETWORK_STATE` 権限を追加する必要があります。
- ProGuardを使用している場合、proguard.txt のすべての行を ProGuard 構成に追加します。

## Unity Ads Android Advanced Guides

- [API Reference](https://github.com/Unity-Technologies/unity-ads-android/wiki/sdk_android_api_reference)
- [Errors](https://github.com/Unity-Technologies/unity-ads-android/wiki/sdk_android_api_errors)
- [Finish States](https://github.com/Unity-Technologies/unity-ads-android/wiki/sdk_android_api_finishstates)
- [Placement States](https://github.com/Unity-Technologies/unity-ads-android/wiki/sdk_android_api_placementstates)
- [Mediation network guide](https://github.com/Unity-Technologies/unity-ads-android/wiki/sdk_metadata_mediation)
- [Server-to-Server callbacks](https://unityads.unity3d.com/help/monetization/s2s-redeem-callbacks)