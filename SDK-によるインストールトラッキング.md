# SDK によるインストールトラッキング
iOS ネットワーク向けの Unity Ads を使って CPI（インストール 1 件あたり単価）のキャンペーンを実施するには、アプリがインストールされたことを Unity Ads に報告するため、インストールトラッキング機構を実装する必要があります。

Unity Ads はクライアントサイドとサーバサイド、どちらでもインストールトラッキングができるように既製のソリューションを提供しています。どちらが適しているかはご自分で判断していただく必要があります。App Store にアプリの更新をアップロードする必要がないという点では、サーバサイドのインストールトラッキングがお勧めといえます。

### 使用可能なトラッキングパラメータ

現在 iOS がサポートしているユーザー識別子は IDFA（広告識別子）のみです。これは iOS 6.0.1 以降のみでサポートされています。インストールトラッキングは iOS 5 以前を搭載したデバイスには対応していません。

### クライアントサイドのインストールトラッキング
トラッキングに必要なユーザー情報をサーバーサイドで収集しない場合、インストールトラッキングをゲーム自体に直接統合することができます。作業にかかる前に、ゲームの game ID を [Unity Ads 管理パネル][3] から取得しておいてください。game ID は "ゲーム" にある "登録済みゲーム" 内の "ゲームID" 欄に記載されています。

トラッキングを統合するには、[こちら][4]をクリックして Unity Ads iOS SDK をダウンロードします。

ダウンロードが終わったら、`UnityAds.framework` と `UnityAds.bundle` を Xcode プロジェクトにドラッグアンドドロップしてください。

> 注意: プロジェクトをコンパイルするには `StoreKit.framework` もプロジェクトに入れておく必要があります。iOS 6 をターゲットにしている場合は、`AdSupport.framework` もプロジェクトにドラッグアンドドロップしてください。

次のステップは `UnityAds` クラスを AppDelegate に追加することです。まず、必要なヘッダーを含める必要があります。

```objective-c
#import <UnityAds/UnityAds.h>
```
次に、`-application:didFinishLaunchingWithOptions: -method ` へ、Unity Ads に gameId とルートビューコントローラを渡して初期化するコードを追加します。

```objective-c
[[UnityAds sharedInstance] startWithGameId:YOUR_GAME_ID_HERE andViewController:myViewController];
```

> 注意: 上の YOUR_GAME_ID_HERE の部分は、自分の Unity Ads game ID で置き換えてください。game ID は、[Unity Ads 管理パネル][3] の "ゲーム" にある "登録済みゲーム" 内の "ゲームID" 欄に記載されています。

これで、あなたのゲームに Unity Ads インストールトラッキングが組み込まれました。

### サーバーサイドのインストールトラッキング
Unity Ads はサーバーサイドのインストールトラッキングも提供しています。詳しくは[こちらのドキュメント][5]を参照してください。

サードパーティのインストールトラッキングサービス（HasOffers など）を利用される場合は、support@applifier.zendesk.com までメールで対応状況をお問い合わせください。

[3]: https://unityads.unity3d.com/admin
[4]: https://github.com/Applifier/unity-ads-sdk
[5]: サーバー間のインストールトラッキング