/*
Sort: 1
*/

# Getting Started with Unity Ads: Self-Service User Acquisition ユーザー獲得スタートガイド
This document serves to help marketers quickly get started with user acquisition campaigns through Unity Ads
このドキュメントは、広告主の皆様が Unity Ads を通してすばやくユーザーを獲得することを手助けするものです。

## Outline 概要
* [Sign Up and Verify Account](#sign-up-and-verify-account) サインアップとアカウント認証
* [Add a Game](#add-a-game) ゲームを追加する
* [Add Video Creatives](#add-video-creatives) クリエイティブを追加する
* [Set Up a Campaign](#set-up-a-campaign) キャンペーンのセットアップ
* [Submit for Moderation](#submit-for-moderation) 入稿申請

## Sign Up and Verify Account サインアップとアカウント認証
The first step in getting started with Unity Ads is to create an account. You can do this by following the steps below:

Unity Ads を始める最初のステップは、アカウントの作成です。以下に従って作成しましょう。

**Step 1:** [Unity Developer Network(UDN) アカウントを作成する](https://accounts.unity3d.com/sign-up)

**Step 2:** メールにて UDN アカウントを認証する

**Step 3:** [UDN アカウントでサインインする](https://accounts.unity3d.com/sign-in)

**Step 4:** Head over to the [Unity Ads signup page](https://unityads.unity3d.com/admin/#signup) and register for the Unity Ads service [Unity Ads サインアップ](https://unityads.unity3d.com/admin/#signup)に進み、 Unity Ads サービスに登録する


[⇧ トップに戻る](#getting-started-with-unity-ads-self-service-user-acquisition)

## Add a Game ゲームの追加
The next step after creating and verifying your account is to add a game. All campaigns and creatives will be stored at the game level in the Unity Ads dashboard. 

アカウントを作成したら、次はゲームを追加しましょう。全てのキャンペーンとクリエイティブは、Unity Ads ダッシュボードのゲームの項目に保存されます。

![](http://cdn.applifier.com/files/add_game.png)

**Step 1:** Select "Games" from the left-hand side of the dashboard 

**Step 1:** ダッシュボードの左側から "ゲーム" を選択する

**Step 2:** Hit "Add New Game" to begin the process of adding your game 

**Step 2:** "新しいゲームを追加" をクリック

**Step 3:** Select which platform your game is live on and input your game's store URL. Then, scrape the app store using the "Look Up Application" button

**Step 3:** 追加するゲームのプラットフォームを選択し、そのゲームが登録されているストアURLを入力します。 "ゲーム（アプリケーション）を探す" ボタンをクリックして、アプリケーションストアからゲームを検索します。

![](http://cdn.applifier.com/files/add_game_2.png)

**Step 4:** Review your game's information, then select the appropriate COPPA targeting for your game and hit "Continue" 

**Step 4:** ゲームの情報を確認したら、あなたのゲームのターゲットに適切な​​対象年齢を選択し、次へをクリックします。

[⇧ トップへ戻る](#getting-started-with-unity-ads-self-service-user-acquisition)

## Add Video Creatives 動画クリエイティブの追加
After you've added your game, the next step in starting a UA campaign will be to add the creative assets. These are the videos and end cards that will be shown when the campaigns have begun. 

ゲームの後には、クリエイティブを追加しましょう。キャンペーンが始まると、ここで追加した動画とエンドスクリーンがユーザーに表示されます。

**Step 1:** Head into the game you just added. To do this, select "Games" from the left-hand side of the dashboard, select the game by clicking on the game name, and then head over to the "Ad Creatives" 

**Step 1:** ダッシュボードの左側にある項目、"ゲーム"  から、先程追加したゲームの名前をクリックします。その後、 "広告クリエイティブ" をクリックします。

![](http://cdn.applifier.com/files/add_creative.png)

**Step 2:** By default, we create an empty creative pack for you. Select this creative pack by clicking on "Video Creative Packs"

**Step 2:** デフォルトでは、空のクリエイティブパックとなっています。この "Video Creative Packs" をクリックします。

**Step 3:** Once you're inside of the default creative pack, you can drag and drop your video and end cards into their respective field. For more information on our creative specs, check out the [Video Creative Documentation](https://github.com/Applifier/unity-ads/wiki/Campaign-Creatives-for-video-ads). Make sure to save your creative pack after everything has uploaded properly (you'll see "Asset uploaded successfully")

**Step 3:** デフォルトのクリエイティブパックに入ったら、ドラッグ・アンド・ドロップで動画やエンドスクリーンの画像を追加することができます。クリエイティブの詳細な要件は[こちら](https://github.com/Applifier/unity-ads/wiki/Campaign-Creatives-for-video-ads)を参照してください。アップロードの後には、"保存" をクリックして変更を保存してください。

[⇧ トップへ戻る](#getting-started-with-unity-ads-self-service-user-acquisition)

## Set Up a Campaign キャンペーンのセットアップ
Now that you've added your game to the dashboard and creatives to your game, its time to set up a new campaign. 

ダッシュボードにクリエイティブを入れたら、キャンペーンをセットアップしましょう。

![](http://cdn.applifier.com/files/add_campaign.png)

**Step 1:** Head back into your game, but this time select "Ad Campaigns", then select "+ Create a Campaign Now" and go through the campaign set up process:

**Step 1:** "広告キャンペーン" 内の "新しいキャンペーンの作成をクリックし、以下の項目を入力します。

**Campaign Name:** The name of your campaign (required)

**キャンペーン名:** キャンペーンの名前

**Ad Type:** The type of ad you'd like to run on our network (required - only video ads available at this time)

**広告の種類:** 追加する広告の種類を選択します（現時点では動画広告のみとなっています） 

**Campaign Type:** The payment method you'd like to use for this campaign

**キャンペーンの種類:** このキャンペーンで使用する支払い方法を選択します

* CPI - Cost per Install. You will only be charged when a user installs your app (recommended)

* CPI - Cost per Install - ユーザーがアプリをインストールした時に支払いが発生します（推奨） 

* CPV - Cost per Completed View. You will be charged every time a user completely watches one of your video ads

* CPV - Cost per Completed View - ユーザーが動画広告を完全視聴した際に支払いが発生します
 
**Budget Type:** The type of budget you'd like to use for this campaign

**バジェットタイプ:** このキャンペーンで使用するバジェットタイプを選択します

* Single budget - this budget input will only be used for this campaign

* 単一キャンペーン - 予算をこのキャンペーンのためだけに使います

* Shared budget - you can use this option if you wanted more than one campaign to share a single budget

* 共有バジェット -  予算を他のキャンペーンと共有します

**Scheduling:** The start and end date that you want to run your campaign for. By default, there won't be an end date

**スケジュール:** キャンペーンの開始日を設定します。デフォルトでは終了日は設定されませんが、"終了日の設定" をクリックして決めることができます。

**Creatives Pack:** This is the video that will run on this specific campaign. You can select "Change" to change to a different creative pack. Keep in mind that you'll only see one creative pack by default, unless you've added more than one creative pack.

**クリエイティブ・パック:** ここでは今回のキャンペーンで再生する動画広告を選択します。 "変更" をクリックすると別のクリエイティブ・パックが選択できます。複数のクリエイティブ・パックを追加した場合を除き、デフォルトのクリエイティブ・パックは一つです。

**Countries and Prices:** In this section, you can change the countries that you want this campaign to target, as well as set the prices for each of those countries. First, click on "Change", which will open up the expanded targeting window. 

**国及び価格:** キャンペーンの対象となる国を設定します。国ごとに単価の設定を変更することができます。 "変更" をクリックすると詳細が開きます。

![](http://cdn.applifier.com/files/countries_and_prices.png)

Search the country you want to target by using the "Type country to search" box and hit enter to add that country to the targeting list. You can then change the bid by clicking on the country code box. This will bring up a pop-up window where you can change the bid. Hit "OK" to save the bid for that country. 

"国の検索" を使ってターゲットにしたい国を検索し、追加できます。追加された国名をクリックすると、ポップアップウィンドウにその国での単価が表示されます。こちらは変更可能です。 "OK"を押すと変更が保存されます。

**Targeting:** 

**ターゲティング:**

![](http://cdn.applifier.com/files/targeting.png)

* Operating System Targeting: The minimum and maximum Operating System targeting parameter allows you to target a specific range of operating systems

* OSによるターゲティング: 対象とするOSのバージョンの最小値と最大値を設定します

* Device Targeting: The device targeting section allows you to target a series of devices (iPad, iPhone, iPad) or a specific device (iPhone 6, iPad Mini 3). This granular targeting is only available on iOS, but will be coming to Android in the near future. Android targeting is broken down by screen size. 

* デバイスによるターゲティング: 対象とするデバイスを選択できます。 iPad, iPhone, iPod touch のように端末の種類を指定する、もしくは iPhone 6, iPad Mini 3 のような詳細な指定も可能です。この端末指定は iOS のみで利用可能です。（Androidについては今後対応予定です。）

* Connection Targeting: This allows you to target users that are connected to WiFi or a cellular connection. If your game is above the download limits (iOS - 100mb/ Google Play - 50mb), then we suggest targeting WiFi only, as users on cellular wouldn't be able to download your game (which lowers conversion rates and makes you less competitive in our network).

* 接続形態によるターゲティング: WiFi、もしくは携帯ネットワークから、対象とする接続形態を指定できます。今回のキャンペーンのゲームが、ダウンロード制限 (iOS - 100mb/ Google Play - 50mb) を超える場合には WiFi のみを対象とすることを推奨します。（こういったケースの際に携帯ネットワークも対象とすると、ゲームをダウンロード出来ずにコンバージョンレートが下がり、結果として Unity Ads での表示回数も下がってしまう可能性があります。）
 

**Third Party Tracking Url:** This is where you will input your tracking URL. This is required for all campaigns that do not have the Unity Ads SDK integrated. For more information on how to set up tracking links, please check out our [S2S Install Tracking](http://unityads.unity3d.com/help/Documentation%20for%20Advertisers/Server-to-Server-Install-Tracking).

**サードパーティトラッキングツール:** ここにはトラッキングツールの URL を入力します。 これは Unity Ads SDK の入っていない全てのキャンペーンに必要となります。トラッキングツールのより詳しい情報は[こちら](http://unityads.unity3d.com/help/Documentation%20for%20Advertisers/Server-to-Server-Install-Tracking)をご参照下さい。

[⇧ Back to top](#getting-started-with-unity-ads-self-service-user-acquisition)

## Submit For Moderation 入稿申請

Once your campaign has been completely set up, you'll be able to submit it for moderation. Save your campaign, then head to the top of the campaign's page. There you will see a "Submit for Moderation" button. Someone from the Unity Ads team will moderate your campaign within 24-48 hours.

キャンペーンのセットアップが完了したら入稿申請をお願いします。キャンペーンを保存したら、トップページの "Submit for Moderation" ボタンをクリックして下さい。Unity Ads チームが、1~2営業日の間にキャンペーンを開始します。

![](http://cdn.applifier.com/files/moderation.png)

[⇧ トップへ戻る](#getting-started-with-unity-ads-self-service-user-acquisition)