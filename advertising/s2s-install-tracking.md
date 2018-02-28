Unity AdsでCPIキャンペーンを開始するために、開発者はサーバー間のデータを介してインストールトラッキング情報を送信できます。宣伝するアプリケーションはUnity Ads専用のインストールトラッキングを含めるためのアップデートは必要ありません。このドキュメントでは、サードパーティのインストールトラッキングサービスを利用するか独自のアプリケーションサーバーを用意することで、iOSおよびAndroidゲームの新しいインストールをUnity Adsに通知するよう設定することが可能です。

### Winner-only Transmissionインストールトラッキング
[AppsFlyer][10]、[Adjust][11]、[Tuneによるモバイルアプリトラッキング][5]、[Kochava][4]などの外部モバイルインストールトラッキングサービスを既に使用している場合は、インストールトラッキングサービスにUnity Adsの視聴（またはクリック）を通知するトラッキングURLを簡単に設定できます。インストールトラッキングサービスでUnity AdsのポストバックURLを設定して、Unity Adsにインストールコンバージョンを通知します。

### トラッキングURL
[Unity Ads Admin Panel][2]では、キャンペーンのカスタムトラッキングURLを設定することができます。このURLは、キャンペーンに対するインプレッション（またはクリック）でユーザーの身元をインストールトラッキングサービスへ報告します。この情報はサービスによって使用され、カスタムしたURLへのアクセスを正しい広告ネットワークに帰属させることで適切なユーザーの詳細と情報とともにそのネットワークにコールバックします。

#### ダイナミックカスタムトラッキングURLトークン
トラッキングURLには、動的に置き換えられるトークンがあります。次の一覧が例です。

|Token|Description|
|-----|-----------|
|`{ifa}` (on iOS)| オリジナル（upper case）形式のプレーンテキストのiOS [広告識別子 (IDFA)][6]: "33EB7AE0-2950-4A69-B290-88038EB4324D".|
|`{ifa}` (on Android)| オリジナル（lower case）形式の[Google Advertising ID][7]: "b7cd3f27-8064-44b6-b9f6-a7657a5f399d4". Androidの主要な識別方法としてAndroid IDに置き換えます。移行段階では、両方の識別子を並行して実行する必要があります。|
|`{ifa_md5}` (on iOS)| md5ハッシュ形式のiOS [広告識別子 (IDFA)][6],　大文字形式からハッシュ化: "DE7A778EA2B8D2BCA909A860EC705090".|
|`{ifa_md5}` (on Android)| md5ハッシュ形式の[Google Advertising ID][7], 小文字形式からハッシュ化: "53E2A8B25482634D1EE14245330AF063".|
|`{android_id_md5}`| Androidデバイスのmd5ハッシュ化した[Android ID][8]。これは、Google Advertising ID {ifa}が利用できない場合（Google Play ServicesがAndroidデバイスにインストールされていない場合）にのみ使用してください。MD5でのみハッシュされます。 例: MD5 ("dc57ba135d7521d8") = "5e0572516f9657268ae0a7ef4f7a5bd1"|
|`{ip}`| 「123.123.123.123」という形式でのユーザのIP番号。これは情報提供の目的でのみ提供されており、インストールトラッキングのユーザー識別には適していません。|
|`{country_code}`| 小文字のユーザーのISO 3166-1 alpha-2国コード(注意: 英国および北アイルランドの英国の場合はgbです。)|
|`{campaign_id}`| Unity AdsのキャンペーンID, 例: "546b9257365339e0031572bd"|
|`{campaign_name}`| Unity Adsのキャンペーン名|
|`{game_id}`| 宣伝してるゲームのUnity AdsでのゲームID, 例: "11004"|
|`{source_game_id}`| ユーザーが広告を見ているゲームに対するパブリッシャのUnity AdsのゲームID, 例: "11017"|
|`{os}`|デバイスのオペレーティングシステム, 例: "9.2.1" (iOS), "4.4.0" (Android)|
|`{device_type}`| デバイスの種類, 例: "iPad4,1", "motorola XT1254", "samsung SM-G900F"|
|`{creative_pack}`| クリエイティブパックの名前, 例: "Video Creatives Pack - EN - 15s"|
|`{language}`| デバイスの言語, 例: "en-GB"|
|`{user_agent}`| デバイスのuser agent, 例: "Mozilla/5.0 (iPhone; CPU iPhone OS 9_3_5 like Mac OS X) AppleWebKit/601.1.46 (KHTML, like Gecko) Mobile/13G36" (iOS), "Mozilla/5.0 (Linux; Android 6.0.1; SM-G920V Build/MMB29K) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.98 Mobile Safari/537.36" (Android) |
|`{device_make}`| デバイスのメーカー, 例: "Apple" (iOS), "samsung" (Android)|
|`{device_model}`| デバイスのモデル, 例: "iPhone7,2" (iOS), "SM-G900F" (Android) |
|`{cpi}`| $USでの1インストールあたりのコスト 例: 2.85  |
|`{video_orientation}`| 表示されたクリエイティブの方向（「ポートレート」または「ランドスケープ」）を返します。注：Unity Ads SDK 2.0以降でのみ使用可能 |




> **重要**
>
> **すべての** トラッキングURLが次の要件を満たしていることを確認してください。**正しく設定がされていない場合、アトリビューションが正しく機能しません**
>
> * URLと任意のリダイレクトは **HTTPS** を使用します
> * URLは **{ifa}** 動的カスタムトークンを含みます
> * HTTPリダイレクトは、HTML/Javascriptではなく, <a href="https://www.w3.org/Protocols/HTTP/HTRESP.html" target="_blank">HTTP 3XX codes</a> で実行する必要があります
> * URLがApp StoreまたはGoogle Playにリダイレクトされない
>
> HTTPSをサポートしていないトラッキングプロバイダを使用している場合は、adops@unity3d.comまでご連絡ください。

例えば、AppsFlyer Tracking URLの場合:

```
https://app.appsflyer.com/id1063631875?idfa={ifa}&c={campaign_name}&af_sub2={source_game_id}&redirect=false
```
は次のように表示されます:
```
https://app.appsflyer.com/id1063631875?idfa=9C528B70-E96D-4590-8766-2A40ED2B3573&c=Unity_Android_USA_Target&af_sub2=33333&redirect=false
```

#### Server responses to Impressions or Clicks
トラッキングURLは、インプレッション（ユーザーがキャンペーンを視聴したとき）またはクリック（動画が終了した後に終了画面でユーザーがダウンロードリンクをクリックしたとき）のどちらからでも実行されます。**どちらの場合も、URLはApple App StoreまたはGoogle Playにリダイレクトしないでください。サーバーはHTTP 200 OKで応答する必要があります。** UnityAds SDKは、アプリケーションシートのそれぞれのストアページを読み込んで、現在プレーヤになっているゲームの外にプレーヤーを誘導しないようにします。

### Postback URL Request
コンバージョンしたユーザーを報告します。つまり、キャンペーンの新しいインストールはポストバックURLを介して行われます。インストールはHTTP GETリクエストで報告できます。iOSの場合のURLは:

```
https://postback.unityads.unity3d.com/games/[GAME_ID]/install?advertisingTrackingId=[YOUR_MACRO_FOR_IDFA]
```
そして、Androidの場合は:
```
https://postback.unityads.unity3d.com/games/[GAME_ID]/install?advertisingTrackingId=[YOUR_MACRO_FOR_GOOGLE_AD_ID]
```
ここで「GAME_ID」はあなたのUnity AdsのゲームIDです。このIDは、[Unity Ads Admin Panel][2]にログインし、[ACQUIRE]メニューの[Campaigns]を選択して、ページのゲームリストで見ることができます。**現在、ゲームのiTunes IDまたはGoogle Play IDは、Unity AdsのゲームIDの代わりに使用しないでください。**

> 注：既存のユーザーの場合、従来のimpact.applifier.comドメインは新しいpostback.unityads.unity3d.comドメインと並行して動作しますが、新しいドメインに移行することをお勧めします。

#### Postback URL Parameters
ポストバックURLリクエストでは、次の識別パラメータを中継する必要があります。サポートされているパラメータは次のとおりです。

|Key|Description|
|---|-----------|
|`advertisingTrackingId` (on iOS)|大文字形式の[広告識別子 (IDFA)][6]: "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX". すべてのインストールで必須(raw形式またはmd5を使用する必要があります).|
|`advertisingTrackingIdMD5` (on iOS)|md5ハッシュ形式の[Identifier for Advertising (IDFA)][6], 小文字形式でハッシュ化。すべてのインストールで必須(raw形式またはmd5を使用する必要があります).|
|`advertisingTrackingId` (on Android)|小文字形式の[Google Advertising ID][7]: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx". 使用可能な場合は常にすべてのインストールで必須(raw形式またはmd5を使用する必要があります).|
|`advertisingTrackingIdMD5` (on Android)|md5ハッシュ形式の[Google Advertising ID][7], 小文字形式でハッシュ化。使用可能な場合は常に、すべてのインストールで必須(raw形式またはmd5を使用する必要があります).|
|`rawAndroidId`| [Android ID][8] (オリジナルの小文字形式から). **非推奨**. AndroidデバイスがGoogle Play Servicesを正しく統合し、Google Playをインストールしている場合、このパラメータは不要です。Google Advertising IDを持たないすべてのAndroidインストール（Google Playサービスを持たず、関連する統合されたデバイス）で必須です。|
|`androidId`| [Android ID][8] (オリジナル形式からmd5ハッシュ). **非推奨**. AndroidデバイスがGoogle Play Servicesを正しく統合し、Google Playをインストールしている場合、このパラメータは不要です。 Google Advertising IDを持たないすべてのAndroidインストール（Google Playサービスを持たず、関連する統合されたデバイス）では必須です。|
|`attributed`|このインストールがUnity Adsに起因するかどうかを示すフラグ（1または0）。デフォルトは attributed = 1です。 attributed = 0の場合、インストールはプレイヤーにのみマークされ、請求されません。 注：このパラメータは、Blanket Transmissionを使用する場合にのみ使用する必要があります。つまり、すべてのインストールが帰属する代わりに送信されます。|



### Postback URL Response Format
Unity Adsのインストールトラッキングサーバからの応答はJSONとして出力されます。メッセージが正常に受信された場合、サーバーは * status * フィールドに* ok *の値で応答します。サーバーは常にHTTP応答コードも出力します。

ポストバックが、成功したコンバージョンを開始した場合、つまり有料のインストールの場合、応答にはパラメータ "install"：trueが含まれます。例えば、ユーザーが既にゲームをインストールしていた場合、またはルックバックウィンドウが経過した場合など、有効なコンバージョンでなかった場合、応答は "install"：falseを読みます。

さらに、コンバージョンが成功した場合は、ソースゲームIDがsourceGameパラメータに表示されます。広告主は、このパラメータを使用して、発生する取引の品質に基づいて、特定のソースゲームまたはパブリッシャーをブロックすることができます。コンバージョンが成功しなかった場合、sourceGameパラメータは省略されます。

例:
```
{"install":true,"sourceGame":"11007","responseCode":200,"status":"ok"}
```
* status *フィールドが存在しない場合は、フィールド "error" で任意のエラーを利用できます。

> 注：ステータスコードは、このポストバックコールがUnity Adsの有料インストールとして実際に記録されたかどうかを示していません。メッセージが正常に受信され、処理された状態です。有償インストールの場合は、代わりに "install":true を使用します。

### サードパーティのインストールトラッキングサービスの手順


#### AppsFlyer
[AppsFlyer][10] documentation:
- <a href="https://support.appsflyer.com" target="_blank">AppsFlyer support</a>
- <a href="https://support.appsflyer.com/hc/en-us/sections/201695093-AppsFlyer-Ad-Network-Integration" target="_blank">UnityAds (former Applifier) integration</a>


#### Adjust
[Adjust][11] documentation:
- <a href="https://docs.adjust.com/en/tracker-generation/" target="_blank">Adjust tracker generation</a>
- <a href="https://docs.adjust.com/en/special-partners/unity-ads/" target="_blank">UnityAds integration</a>


#### MobileAppTracking by Tune (previously HasOffers)
[MAT][5] documentation:
- <a href="https://help.tune.com/marketing-console/setting-up-an-integrated-advertising-partner/" target="_blank">Setting up an integrated advertising partner</a>
- <a href="https://help.tune.com/marketing-console/attribution-analytics-getting-started-guide/" target="_blank">TUNE starting guide</a>


#### Kochava
[Kochava][12] documentation:
- <a href="https://support.kochava.com" target="_blank">Kochava support</a>
- <a href="https://support.kochava.com/campaign-management/create-an-install-campaign" target="_blank">Create an install campaign</a>


[1]: https://zencoder.com/en/formats
[2]: https://unityads.unity3d.com/admin
[3]: https://www.hasoffers.com/
[4]: https://www.kochava.com/
[5]: https://www.tune.com/
[6]: https://developer.apple.com/library/ios/#documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html
[7]: https://developer.android.com/google/play-services/id.html
[8]: https://developer.android.com/reference/android/provider/Settings.Secure.html#ANDROID_ID
[9]: https://unityads.unity3d.com/help/advertising/s2s-install-tracking#postback-url-request
[10]: https://www.appsflyer.com/
[11]: https://www.adjust.com/
[12]: https://www.kochava.com/
[13]: https://support.kochava.com/
