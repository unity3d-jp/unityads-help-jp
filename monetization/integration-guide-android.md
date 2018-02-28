このガイドでは、Unity AdsをAndroid Studioで作成したプロジェクト、つまりネイティブAndroidアプリに組み込む方法について説明します。

最新のUnity Adsを [こちら][2] でダウンロードしてください。

Androidの動画チュートリアルは [YouTube][5] (6:57) で入手できます。

<!---
動画にリンクされたスクリーンショットを使用して、埋め込み動画を「偽造」することは可能です：
[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg)](https://www.youtube.com/watch?v=YOUTUBE_VIDEO_ID_HERE)
-->

プロジェクトの例は [GitHub][3] にあります。

### コンテンツ:  
- [クイックスタートガイド](#quickstart-guide)
  - [ダッシュボードを使用する](#create-a-new-game-project-in-the-unity-ads-dashboard)
  - [組み込みガイド](#integrate-unity-ads-in-android-studio)
- [アドバンスドガイド](#unity-ads-android-advanced-guides)

# クイックスタートガイド

[![組み込みの動画 (6m:57s)](https://img.youtube.com/vi/MNdJ0KWlYPw/0.jpg)](https://www.youtube.com/watch?v=MNdJ0KWlYPw)

### Unity Adsのダッシュボードでゲームプロジェクトを作成する

UDNアカウントを使用して[Unity Ads Dashboard][50]にログインします。
  - まだUDNアカウントをお持ちでない場合は、[こちら][52]で登録して下さい

ダッシュボードから、新しいゲームプロジェクトを作成します。
 > 注:あなたのゲームがアメリカの子供に向けて特別に設計されていない限り、_[13歳未満の子供を対象にする]_ オプションをオフのままにしておきます。全年齢対象のゲームでは、COPPAコンプライアンスに準拠する必要はありません。

ゲームプロジェクトを選択すると、（7桁の）**Game ID** が表示されます。この番号を使用してUnity Adsを有効にします。

ゲームID（iOSまたはAndroid）を選択し、**[Placements]** タブをクリックすると、*placement ID* が表示されます。
  最初、それぞれのゲームIDには2つの場所取りがあります:
  - `動画` (デフォルト/ 5秒後に動画をスキップする)
  - `報酬型動画` (スキップオプションなし)

[settings]タブで *各ゲームIDに対して* テストモードを有効にします。
  > Apple/Play Store Game ID > 設定 > テストモード: ONにする

### Android StudioへのUnity Adsの組み込み:

1. Githubから[latest release][2]バイナリをダウンロードします。具体的には **unity-ads.aar** です.
*  Android Studioで既存のAndroidプロジェクトを開く、もしくは新たなプロジェクトを作成する。
*  新しいモジュールを追加し、**unity-ads.aar** をインポートします。モジュールには「unity-ads」などの名前を付けます。
*  アプリのモジュール設定を開き、 "unity-ads"モジュールを依存関係として追加します（「依存関係」はうまく訳せていません。dependencyという設定か何かがある？）。
*  Javaアクティビティファイルに次のインポートを追加します。
  * `import com.unity3d.ads.IUnityAdsListener;`
  * `import com.unity3d.ads.UnityAds;`
*  あなたのクラスに `implements IUnityAdsListener`を追加し、Android Studioに必要なコールバックメソッドを生成させます
*  `IUnityAdsListener`を実装するアクティビティ内でUnityAds.initialize（this、gameId、this）を呼び出してUnity Adsを初期化します。`gameId`はAndroidプラットフォームのゲームIDに設定されたString値です。[Unity Adsダッシュボード][1]のプロジェクトで確認することができます。
   _**注:** Unity Adsは一度だけ初期化されます。SDK 2.0は、より堅牢なネットワークリトライロジックを備えています。 したがって、ネットワーク接続なしでも安全に初期化することができます。ネットワークが利用可能になると、SDKは広告を要求します。_
*  次のコードは _default_ プレースメントの広告を表示します:
   `if (UnityAds.isReady()) { UnityAds.show(this); }`

Unity Ads SDK 2.0は、各showメソッド呼び出し（上記の例では `this`）でアクティビティ引数をとります。

APIの詳細については、[Unity Ads Android APIリファレンス][APIリファレンス]をご覧ください。さまざまなプレースメントの広告を表示など、プロジェクトにUnity Ads SDKを実装する方法の例については[サンプルアプリケーション][4]を参照してください。

### Android Studioを使用しない組み込み

ビルドシステムでAARパッケージを使用できない場合は、GitHubリリースのZIPファイル **unity-ads.zip** でも同じリソースを提供しています。Unity Adsを正常に使用するには、3つのことを行う必要があります。

* **classes.jar** をビルドに含めます。
* **AndroidManifest.xml** からマニフェストを手動でマージします。「AdUnitActivity」アクティビティと「AdUnitSoftwareActivity」アクティビティの両方を必ず含めてください。 また、`INTERNET` と `ACCESS_NETWORK_STATE` パーミッションを追加する必要があります。
* ProGuardを使用している場合は、**proguard.txt** のすべての行をProGuard環境設定に追加してください。

## Unity AdsのAndroidアドバンスドガイド

- [API Reference][API ref]
  - [Errors][Errors]
  - [Finish States][Finish States]
  - [Placement States][Placement States]
- [Mediation network guide][Mediation]
- [Server-to-Server callbacks][s2s]

その他のご質問は、**unityads-support@unity3d.com** までご連絡ください。

[1]:https://dashboard.unityads.unity3d.com
[2]:https://github.com/Unity-Technologies/unity-ads-android/releases
[3]:https://github.com/Unity-Technologies/unity-ads-android/tree/master/app
[4]:https://github.com/Unity-Technologies/unity-ads-android/blob/master/app/src/main/java/com/unity3d/ads/example/UnityAdsExample.java
[5]:https://www.youtube.com/watch?v=MNdJ0KWlYPw

[API ref]: https://github.com/Unity-Technologies/unity-ads-android/wiki/sdk_android_api_reference
[Errors]: https://github.com/Unity-Technologies/unity-ads-android/wiki/sdk_android_api_errors
[Finish States]: https://github.com/Unity-Technologies/unity-ads-android/wiki/sdk_android_api_finishstates
[Placement States]: https://github.com/Unity-Technologies/unity-ads-android/wiki/sdk_android_api_placementstates
[Mediation]: https://github.com/Unity-Technologies/unity-ads-android/wiki/sdk_metadata_mediation
[s2s]: https://unityads.unity3d.com/help/monetization/s2s-redeem-callbacks

<!--- Dashboards -->
[50]: https://dashboard.unityads.unity3d.com
[52]: https://id.unity.com
