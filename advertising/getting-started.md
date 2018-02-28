* [Getting started with advertising](#_self-service-user-acquisition_)
* [Campaign design guide](https://unityads.unity3d.com/help/advertising/campaign-design-guide)
* [Campaign optimization](https://unityads.unity3d.com/help/advertising/campaign-optimization)
* [Server-to-Server (S2S) Install Tracking](https://unityads.unity3d.com/help/advertising/s2s-install-tracking)
* [Advertising Stats API](https://unityads.unity3d.com/help/advertising/stats-api)

# _セルフサービスのユーザー獲得_

このドキュメントは、Unity Adsを使用してユーザー獲得キャンペーンを迅速に開始するのに役立ちます。

## 概要
* [アカウントの登録と確認](#sign-up-and-verify-account)
* [ゲームの追加](#add-a-game)
* [動画クリエイティブの追加](#add-video-creatives)
* [キャンペーンの設定](#set-up-a-campaign)
* [Submit for Moderation](#submit-for-moderation)

## アカウントの登録と確認
Unity Adsを使い始める最初のステップは、アカウントを作成することです。以下の手順を行なってください。

**Step 1:** [Unity Developer Networkアカウントの作成する](https://accounts.unity3d.com/sign-up)

**Step 2:** メールでUDNアカウントを確認する

**Step 3:** [UDNにサインインする](https://accounts.unity3d.com/sign-in)

**Step 4:** [Unity Ads signup page](https://unityads.unity3d.com/admin/#signup) に行き、Unity Adsサービスに登録する


[⇧ Back to top](#getting-started-with-unity-ads-self-service-user-acquisition)

## ゲームの追加
アカウントを作成して確認した後の次のステップは、ゲームを追加することです。すべてのキャンペーンとクリエイティブは、Unity Adsダッシュボードのゲームごとで保存されます。

![](https://cdn.applifier.com/files/add_game.png)

**Step 1:** ダッシュボードの左側から[Games]を選択します

**Step 2:** 追加したいゲームを選んで[Add New Game]を押します

**Step 3:** ゲームが実行されているプラットフォームを選択し、ゲームのストアURLを入力します。 次に、「Look Up Application」ボタンを使用してアプリストアを取得します

![](https://cdn.applifier.com/files/add_game_2.png)

**Step 4:** ゲームの情報を確認後、ゲームに適したCOPPAターゲティングを選択し、[Continue]を押します


[⇧ Back to top](#getting-started-with-unity-ads-self-service-user-acquisition)

## 動画クリエイティブの追加
ゲームを追加したら、UAキャンペーン（User Acquisitionキャンペーン？）を開始する次のステップは、クリエイティブアセットを追加することです。これは、キャンペーンが開始されたときに表示される動画とエンドカードです。

**Step 1:** あなたが今追加したゲームに向かいましょう。これを行うには、ダッシュボードの左側から[ゲーム]を選択し、ゲーム名をクリックしてゲームを選択し、[Ad Creative]に移動します

![](https://cdn.applifier.com/files/add_creative.png)

**Step 2:** デフォルトでは、空のクリエイティブパックが作成されます。[Video Creative Packs]をクリックしてこのクリエイティブパックを選択します

**Step 3:** デフォルトのクリエイティブパックの中に入ったら、動画とエンドカードをそれぞれのフィールドにドラッグアンドドロップすることができます。クリエイティブの仕様の詳細については、[Video Creative Documentation](https://unityads.unity3d.com/help/advertising/campaign-design-guide)をご覧ください。すべてが正しくアップロードされたらクリエイティブパックを保存してください（「アセットが正常にアップロードされました」が表示されます）


[⇧ Back to top](#getting-started-with-unity-ads-self-service-user-acquisition)

## キャンペーンの設定
ダッシュボードとクリエイティブをゲームに追加したら、新しいキャンペーンを設定していきます。

![](https://cdn.applifier.com/files/add_campaign.png)

**Step 1:** ゲームに戻って、[Ad Campaigns]を選択、次に「+ Create a Campaign Now」を選択し、キャンペーンの設定プロセスに進みます

**Campaign Name:** キャンペーンの名前（必須）

**Ad Type:** 私たちのネットワークで掲載する広告のタイプ（注 - 現時点で利用可能なのは動画広告のみ）

**Campaign Type:** このキャンペーンで使用する支払い方法

* CPI - 1インストールあたりのコスト。ユーザーがアプリをインストールしたときにのみ請求されます（推奨）

* CPV - 1Completed Viewあたりのコスト。ユーザーが動画広告を完全に視聴するたびに料金が請求されます

**Budget Type:** このキャンペーンに使用したい予算のタイプ

* Single budget - この予算入力はこのキャンペーンにのみ使用されます

* Shared budget - 複数のキャンペーンで1つの予算を共有したい場合は、このオプションを使用できます

**Scheduling:** キャンペーンを実行する開始日と終了日。デフォルトでは、終了日はありません

**Creatives Pack:** このキャンペーンで再生される動画です。[Change]を選択すると、別のクリエイティブパックに変更できます。複数のクリエイティブパックを追加していない限り、デフォルトでは1つのクリエイティブパックのみが表示されます。

**Countries and Prices:** このセクションでは、このキャンペーンのターゲットとする国を変更したり、各国の価格を設定したりすることができます。まず、[Change]をクリックします。これにより、展開されたターゲティングウィンドウが開きます。
![](https://cdn.applifier.com/files/countries_and_prices.png)

[Type country to search]ボックスを使用してターゲットにする国を検索し、[Enter]を押してその国をターゲティングリストに追加します。国コードボックスをクリックして入札単価を変更することができます。これにより、入札単価を変更できるポップアップウィンドウが表示されます。その国の入札単価を保存するには[OK]を押します。

**Targeting:**
![](https://cdn.applifier.com/files/targeting.png)

* オペレーティングシステムのターゲティング：オペレーティングシステムの最小および最大のターゲティングパラメータを使用すると、特定のオペレーティングシステムの範囲をターゲットに設定できます。

* デバイスのターゲティング：デバイスターゲティングのセクションでは、デバイスの種類（iPad、iPhone、iPad）または特定のデバイス（iPhone 6、iPad Mini 3）をターゲットに設定できます。この詳細なターゲティングはiOSでのみ利用できますが、近いうちにAndroidにも登場します。Androidターゲティングは画面サイズ別に分類されます。

* 接続のターゲティング：WiFiまたは電話回線に接続しているユーザーをターゲットに設定できます。ゲームがダウンロード制限（iOS - 100mb / Google Play - 50mb）を超えている場合は、電話回線のユーザーはゲームをダウンロードできないため、WiFiのみをターゲットに設定することをおすすめします（コンバージョン率が低下し、 私たちのネットワークの中での競争率も落ちます）。

**Third Party Tracking Url:** ここで、トラッキングURLを入力します。トラッキングリンクの設定方法の詳細については、[S2S Install Tracking]をご覧ください (https://unityads.unity3d.com/help/Documentation%20for%20Advertisers/Server-to-Server-Install-Tracking)。

[⇧ Back to top](#getting-started-with-unity-ads-self-service-user-acquisition)
## Submit For Moderation

あなたのキャンペーンが完全に設定されたら、それをよく確認を行い次第自由なタイミングで提出することができます。 キャンペーンを保存してから、キャンペーンのページの先頭に移動します。そこに「Submit for Moderation」ボタンが表示されます。 Unity Adsチームの誰かが24〜48時間以内にキャンペーンを管理します。
![](https://cdn.applifier.com/files/moderation.png)


[⇧ Back to top](#getting-started-with-unity-ads-self-service-user-acquisition)
