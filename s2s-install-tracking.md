# サーバー間のインストールトラッキング


> 【トラッキングURLのセキュア接続対応について】

> 2016年1月1日よりUnity AdsではトラッキングURLのセキュア接続対応による変更によって、iOS/Android共にHTTPによるトラッキングURLでの情報通信サービスがご利用いただけません。

> お手数をおかけいたしますが、ご発注の際はHTTPSを用いているトラッキングツールをご利用いただくか
> キャンペーンに対してセキュアを伴った情報通信が可能であるかをトラッキングツール側へお問い合わせください。


Unity Ads では、CPI キャンペーンの立ち上げを支援するため、インストールトラッキング情報をサーバー間の統合を通じて送れるようにしています。こうすれば、宣伝対象のアプリケーションをアップデートして Unity Ads 専用のインストールトラッキングを組み込む必要もありません。このドキュメントでは、サードパーティのインストールトラッキングサービス、または自前のアプリケーションサーバーに、あなたの iOS/Android ゲームが新たにインストールされたら Unity Ads に通知する機能を組み込む方法を説明します。

Unity Ads で外部インストールトラッキングを行う方法は、大別して 2 通りあります。

- **ウィナーオンリー トランスミッション** - Unity Ads から生じたインストールのみが通知されます。帰属の特定は通常、サードパーティのインストールトラッキングサービスが実施します。

- **ブランケット トランスミッション** – 新規インストールすべてを Unity Ads に通知し、Unity Ads 側でどのインストールが広告主の Unity Ads キャンペーンに帰属するかを判断します。ユーザーがインストールからさかのぼって一定期間内にそのキャンペーンを表示/クリックしたかどうかが基準となります。


### ウィナーオンリー トランスミッションのインストールトラッキング

すでに外部のモバイル インストールトラッキング サービス（例: *Tune*、*Kochava*、*Adjust* などによる Mobile App Tracking）を利用している場合、インストールトラッキングの手法としては**ウィナーオンリー トランスミッション**が適しています。Unity Ads からインストールトラッキングサービスにビュー（またはクリック）を通知するトラッキング URL と、インストールトラッキングサービスから Unity Ads にインストールコンバージョンを通知する ポストバック URL は、容易に構成できます。

### トラッキング URL
[Unity Ads 管理パネル][2] を使ってキャンペーン用のカスタムトラッキング URL を定義することができます。この URL はキャンペーンのインプレッション（ started ）またはクリック時に、そのユーザーの識別情報をインストールトラッキングサービスにレポートします。サービスはこの情報を使って、その後のインストールとそれに寄与した広告ネットワークを関連付け、そのネットワークに当該ユーザーの詳細をコールバックします。

#### 動的 カスタム トラッキング URL トークン
トラッキング URL では以下の動的置換トークンが使用できます。

|トークン|内容|
|-----|-----------|
|`{ifa}` (iOSの場合)|iOS の広告識別子 [Identifier for Advertising (IDFA)][6] です。平文で、次のようにオリジナルの（大文字の）形のまま表されます。「XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX」|
|`{ifa}` (Androidの場合)|[Google Advertising ID][7] です。次のようにオリジナルの（小文字の）形のまま表されます。「xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx」これは将来、Android ID に換わり Android における主要な識別手段となります。移行期間中は両方の識別子を併用する必要があります。|
|`{ifa_md5}` (iOSの場合)|iOSの広告識別子 [Identifier for Advertising (IDFA)][6] の mb5 ハッシュ、全て大文字です。"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"|
|`{ifa_md5}` (Androidの場合)|[Google Advertising ID][7]の mb5 ハッシュ、全て小文字です。"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"|
|`{android_id_md5}`|MD5 ハッシュされた、Android デバイスの [Android ID][8]。これは Google Advertising ID {ifa} が使えない（Google Play サービスのインストールされていない Android 端末）にのみ使用して下さい。MD5 ハッシュされた状態でのみ渡すことができます。例: MD5 ("dc57ba135d7521d8") = "5e0572516f9657268ae0a7ef4f7a5bd1"|
|`{ip}`|ユーザーのIP アドレスです。例えば "123.123.123.123" といった形式で表されます。これはあくまで参考として提供されるものです。インストールトラッキングにおけるユーザー識別には適切ではありません。|
|`{country_code}`|ユーザーの ISO 3166-1 alpha-2 国名コードです。小文字で表します（注: 英国（グレートブリテン及び北アイルランド連合王国）は gb で表します）。|
|`{campaign_id}`|Unity Ads キャンペーン ID。例: "546b9257365339e0031572bd"|
|`{campaign_name}`|Unity Ads キャンペーン名。|
|`{game_id}`|宣伝されるゲームの Unity Ads gameId。例："11004"|
|`{source_game_id}`|パブリッシャーがユーザーに広告を見せるゲームの Unity Ads gameId。例: "11017"|
|`{device_type}`|端末情報。例えば "iPad4", "motorola XT1254" など。|
|`{device_make}`|端末のメーカー。例えば "iPad4", "motorola XT1254" など。|
|`{device_model}`|端末のモデル。例えば "iPhone7,2" (iOS), "SM-G900F" (Android) など。|
|`{creative_pack}`|クリエイティブパックの名称。例えば "Apple" (iOS), "samsung" (Android) など|
|`{os}`| 端末のオペレーティングシステム。 例えば "9.2.1" (iOS), "4.4.0" (Android)|
|`{user_agent}`| 端末ユーザーのエージェント。 例えば "Mozilla/5.0 (iPhone; CPU iPhone OS 9_3_5 like Mac OS X) AppleWebKit/601.1.46 (KHTML, like Gecko) Mobile/13G36" (iOS), "Mozilla/5.0 (Linux; Android 6.0.1; SM-G920V Build/MMB29K) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.98 Mobile Safari/537.36" (Android)| 
|`{cpi}`| インストールの単価（ $US ）。 例えば "2.85"|

##Unity Ads トラッキングURLのルール

- HTTPS以外のURLは使用できません
- URLには少なくとも{ifa}のダイナミックカスタムトークンが必要です
- HTTPSのリダイレクトはHTTP 3XX codesを通してのみ可能で、HTML/Javascriptを介してリダイレクトすることはできません
- App Store, Google Playにリダイレクトすることはできません


例えば、AppsFlyer の Tracking URL は

```
https://app.appsflyer.com/id1063631875?idfa={ifa}&c={campaign_name}&af_sub2={source_game_id}&redirect=false
```

以下のように表されます。

```
https://app.appsflyer.com/id1063631875?idfa=9C528B70-E96D-4590-8766-2A40ED2B3573&c=Unity_Android_USA_Target&af_sub2=33333&redirect=false
```


#### インプレッション （ started ）またはクリックに反応する
トラッキング URL は、インプレッション（ユーザーがキャンペーンを見た）またはクリック（ユーザーが動画再生終了時に表示されるダウンロードリンクをクリックした）の、どちらの時点でも起動させることができます。いずれの場合でも、URL は App Store や Google Play へのリダイレクトは不可能です。この URL は HTTP 200 OK を返します。Unity Ads SDK は、それぞれのストアページをアプリケーションシートにロードして、ゲームプレイの妨げにならないようにします。

### ポストバック URL リクエスト
ユーザーのコンバージョン、つまりキャンペーンで新しいインストールがあったことのレポートは、ポストバック URL を通して行います。インストールは HTTP GET リクエストで報告されます。URL は iOS の場合、以下のようになります。

```
https://postback.unityads.unity3d.com/games/[GAME_ID]/install?advertisingTrackingId=[YOUR_MACRO_FOR_IDFA]
```

Android の場合は以下のようになります。

```
https://postback.unityads.unity3d.com/games/[GAME_ID]/install?advertisingTrackingId=[YOUR_MACRO_FOR_GOOGLE_AD_ID]
```

`GAME_ID` のところにあなたの Unity Ads Game ID が入ります。ID は[Unity Ads 管理パネル][2] にログインし、 "ゲーム" にある "登録済みゲーム" 内の "ゲームID" 欄で確認することが出来ます。この ID は現在、1000～20000 の範囲の整数となっています。なお現時点では、Unity Ads Game ID の代わりにあなたのゲームの iTunes ID/Google Play ID を使うべきではありません。

>注意: 旧ポストバックの形式である `impact.applifier.com` のドメインもご利用いただけますが、新しい形式である `postback.unityads.unity3d.com`ドメインの使用をお勧めします。

#### ポストバック URL パラメータ
以下の識別パラメータは、ポストバック URL リクエストで渡す必要があるものです。サポートされているパラメータは以下のとおりです。

|Key|Description|
|---|-----------|
|`advertisingTrackingId` (in iOS)|広告識別子 [Identifier for Advertising (IDFA)][6] 。大文字で表される。「XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX」すべてのインストールに必須。（必ず raw もしくは mb5 形式を使用して下さい）|
|`advertisingTrackingIdMD5` (in iOS)|広告識別子 [Identifier for Advertising (IDFA)][6] の mb5 ハッシュ形式。大文字で表される。「XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX」すべてのインストールに必須。（必ず raw もしくは mb5 形式を使用して下さい）|
|`advertisingTrackingId` (in Android)|[Google Advertising ID][7] 。小文字で表される。「xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx」該当するすべてのインストールに必須。（必ず raw もしくは mb5 形式を使用して下さい）|
|`advertisingTrackingIdMD5` (in Android)|[Google Advertising ID][7] の mb5 ハッシュ形式。小文字で表される。「xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx」該当するすべてのインストールに必須。（必ず raw もしくは mb5 形式を使用して下さい）|
|`androidId`| [Android ID][8]（オリジナルの小文字の形、またはオリジナルから生成した MD5 ハッシュ）。Android インストールのうち Google Advertising ID を持たないものには必須。|
|`attributed`|このインストールが UnityAds に帰属していて課金対象になるかどうかを示すフラグ（1 または 0）。デフォルトは attributed=1。attributed=0 の場合、インストールはプレイヤーに関連付けられるが、課金対象にはならない。注: このパラメータはブランケット トランスミッション方式（帰属するものだけでなく、すべてのインストールを通知する）の場合のみ使う。|
|`rawAndroidId`| [Android ID][8]（オリジナルの小文字の形）。*非推奨です。* このパラメータは、Google Play サービスのインストールされた Android 端末には不要。Google Play サービスがインストールされておらず、Google Advertising ID を持たない端末の場合に Android IDを取得するためのもの。|

### ポストバック URL 応答フォーマット
Unity Ads インストールトラッキングサーバーからの応答は JSON として出力されます。メッセージを正常に受信した場合、このサーバーは*ステータスフィールド*に「*ok*」の値を入れて返します。また、このサーバーは常に HTTP レスポンスコードを出力します。

ポストバックを呼び出したのが、コンバージョンの成功（つまり課金対象となるインストール）だった場合、レスポンスはパラメータ `“install”:true` を含みます。有効なコンバージョンでなかった（例: ユーザーはそのゲームをインストール済みだった、または遡及期間を過ぎてからコンバージョンが発生した）場合、レスポンスは `“install”:false` となります。

さらに、コンバージョンが成功した場合、sourceGame パラメータにソースゲームの game id が表示されます。広告主はこのパラメータを利用して、一部のソースゲームやパブリッシャーを、生成するトラフィックの質によってブロックする場合があります。コンバージョンが成功しなかった場合は、sourceGame パラメータは省略されます。

例:

```
{"install":true,"sourceGame":"11007","responseCode":200,"status":"ok"}
```

*ステータスフィールド*がない場合、エラーが発生するとエラーフィールドに表示されます。

> 注意: このステータスコードは、当該ポストバック呼び出しが課金対象のインストールとして Unity Ads へ実際に登録されたかどうかを示すものではありません。あくまでメッセージの受信と処理に成功したことを示すだけです。課金対象のインストールについては、代わりに `“install”:true` を使ってください。

### サードパーティのインストールトラッキングサービス向けガイド

####AppsFlyer

詳細は AppsFlyer 公式サイトをご参照下さい。

- [AppsFlyer サポートページ](https://support.appsflyer.com/hc/ja)

####Adjust

詳細は Adjust 公式サイトをご参照下さい。

- [Adjust Unity Ads](https://docs.adjust.com/ja/special-partners/unity-ads/)
- [Adjust トラッカーの生成](https://docs.adjust.com/ja/tracker-generation/)

####MobileAppTracking by Tune (previously HasOffers)

詳細は Tune 公式サイトをご参照下さい。

- [統合広告パートナーを設定する](https://help.tune.com/marketing-console/%E7%B5%B1%E5%90%88%E5%BA%83%E5%91%8A%E3%83%91%E3%83%BC%E3%83%88%E3%83%8A%E3%83%BC%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B/?lang=ja)
- [TUNE starting guide](https://help.tune.com/marketing-console/attribution-analytics-getting-started-guide/)

#### Kochava
[Kochava][4] を使用して Unity Ads のキャンペーントラッキングを設定する場合の手順は以下のとおりです。

トラッキング URL:

1. iOS の場合は、トラッキング URL の末尾に、デバイス ID なしで、以下を追記します。
```
&device_id={ifa_md5}&site_id=1&device_hash_method=MD5&device_id_is_hashed=true&device_id_type=idfa
```
アンドロイドの場合は、代わりに以下を追記します。
```
&device_id={android_id_md5}&site_id=1&device_hash_method=md5&device_id_is_hashed=true&device_id_type=android_id&adid={ifa}&pbr=1
```
結果は例えば以下のようになります。	
```
http://control.kochava.com/v1/cpi/click?campaign_id=YOUR_KOCHAVA_CAMPAIGN_ID&network_id=KOCHAVA_NETWORK_ID&creative_id=1&device_id={ifa_md5}&site_id=1&device_id_hash_method=MD5&device_id_is_hashed=true&device_id_type=idfa&mac_md5={mac_address_md5}
```
これをカスタムトラッキング URL フィールドに入力します。

2. Kochava に依頼して、あなたのトラッキング URL からのリダイレクトを無効にしてもらいます。
3. ボックス「This URL redirects to AppStore / Google Play Store（この URL を App Store/Google Play Store にリダイレクトする）」のチェックを外します。

##### ポストバック URL:
1. Kochava に依頼して、あなたの Unity Ads Game ID を Kochava のシステムに登録してもらいます。
1. Postback Conversion レポートで、統合パートナーのリストから Applifier を選択します。

### ブランケット トランスミッション インストールトラッキングの説明
今までにサードパーティのインストールトラッキングサービスを利用したことがない場合、 "ウィナーオンリー" トランスミッション方式の代わりに "ブランケット" トランスミッション方式を使うのも良い方法です。これは、新しいインストールをすべて Unity Ads のポストバック URL にレポートし、どれが Unity Ads に起因するインストールかの判別は Unity Ads に任せるというものです。具体的に言えば、今まであなたのアプリケーションをインストールしたことがないユーザーが、Unity Ads のキャンペーン動画を視聴した後、72 時間以内にあなたのゲームをインストールし起動した場合、これはあなたのキャンペーンに課金対象のインストールとしてカウントされます。

サーバー間のインストールトラッキングを利用するには、ゲームがすでに広告識別子（ [Identifier for Advertising][6] ）をあなたのサーバーに送信している状態でなければいけません。新しいユーザーがアプリをインストールすると、あなたのサーバーはそのユーザーの広告識別子を Unity Ads サーバーに送信します。Unity Ads はその識別子を、あなたのアプリの広告を見たユーザーの広告識別子と照合します。もし一致すれば、そのユーザーは Unity Ads 経由でのインストール数に計上されます。

なお、広告識別子というのは iOS の場合で、Android 8 では [Google Advertising ID][7] または [Android ID][8] となります。
 
> 注意: インストールトラッキングは iOS 6 以降のデバイスに対してのみ有効です。iOS 5 以前のデバイスはサポート対象外となりました。

インストールしたユーザーをレポートする方法は***ウィナーオンリー トランスミッション***と同じです。詳細については前述の[ポストバック URL リクエスト](https://github.com/unity3d-jp/unityads-help-jp/wiki/%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E9%96%93%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB%E3%83%88%E3%83%A9%E3%83%83%E3%82%AD%E3%83%B3%E3%82%B0#%E3%83%9D%E3%82%B9%E3%83%88%E3%83%90%E3%83%83%E3%82%AF-url-%E3%83%AA%E3%82%AF%E3%82%A8%E3%82%B9%E3%83%88)セクションを参照してください。

[1]: http://zencoder.com/en/formats
[2]: https://unityads.unity3d.com/admin
[3]: http://www.hasoffers.com/
[4]: http://www.kochava.com/
[5]: https://www.adjust.com/
[6]: http://developer.apple.com/library/ios/#documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html
[7]: https://developer.android.com/google/play-services/id.html
[8]: https://developer.android.com/reference/android/provider/Settings.Secure.html#ANDROID_ID