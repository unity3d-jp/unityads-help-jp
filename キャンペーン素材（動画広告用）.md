/* 
Title: Campaign Creatives for Video Ads
Sort: 2
*/

# キャンペーン素材（動画広告用）

Unity Ads は、自分のゲームにユーザーを集めるツールです。このドキュメントでは、Unity Ads でキャンペーンを作成するのに必要なアセットとリソースについて説明します。

##### キャンペーンの作成に必要なアセットは以下のとおりです。

- 15～30 秒であなたのゲームを宣伝する動画 1 本
- 800×600 の広告画像 1 枚: 動画終了時画面（横長）用。左右各 100 ピクセルはトリミング余白
- 600×800 の広告画像 1 枚: 動画終了時画面（縦長）用。上下各 100 ピクセルはトリミング余白

## 動画アセット

>注意: ゲームの動画広告素材をお持ちでない場合は、Unity Ads にご相談ください。制作代行を承ります。まずは unityads-support@unity3d.com までメールでご連絡ください。

ゲームの宣伝に使用する動画は、長さ 15～30 秒、サイズ 640×360（16:9 ワイドスクリーン アスペクト比）にしてください。Unity Ads はきわめて幅広い種類の動画形式に対応しています。お使いの動画形式に対応しているかどうかは、こちらのサポート対象リストで確認いただけます。

###### アセット要件
- *サイズ*: 640×360 ピクセル
- *長さ*: 15 to 30 秒
- *入力形式*: さまざまな形式に対応しますが、H.264 でエンコードした MOV、MP4、AVI ファイルを推奨
- *ファイルサイズ*: 推奨サイズ 10MB。最大サイズ 30MB。動画は必要に応じて様々なビットレートに再エンコードされます。最終的には、ユーザーが現在利用しているネットサークの帯域幅やキャッシュ設定に最適化された状態で表示されます。
- *For iOS Only*: Videos that are to be run on iOS can *not* have a Google Play badge. They must only have an Apple App Store badge, or none at all
- *iOSのみの注意事項*: iOS用に流される動画広告では、Google playのロゴを表示することが禁止されています。Apple App Storeのロゴのみを表示する、もしくは全てのロゴの表示を無くして下さい。

## 画像アセット（エンドスクリーン）
When the video ends, the user is presented with an end screen that shows a large graphic of your game, with an call-to-action button prompting the user to download the game. 

動画広告の終わりにはエンドスクリーンが表示されます。これによってダウンロードを促しましょう。

###### End Card Required Specifications エンドスクリーン要件
- *Resolution*: There are two required end cards. A portrait and landscape version:
- エンドスクリーンには、縦向きと横向きの画像がそれぞれ必要となります。
 * 800×600px: This is the landscape version of the end card. Please include a 100px crop zone on the left and right margins (no important information on left or right side, 100px deep)
 * 800×600 の広告画像 1 枚: 動画終了時画面（横長）用。左右各 100 ピクセルはトリミング余白
 * 600×800px: This is the portrait version of the end card. Please include a 100px crop zone on the top and bottom margins (no important information on top or bottom, 100px deep)
 * 600×800 の広告画像 1 枚: 動画終了時画面（縦長）用。上下各 100 ピクセルはトリミング余白
- *ファイルフォーマット*: JPG or PNG
- *For iOS Only*: End cards that are to be run on iOS can *not* have a Google Play badge. They must only have an Apple App Store badge, or none at all
- *iOSのみの注意事項*: iOS用に流される動画広告では、Google playのロゴを表示することが禁止されています。Apple App Storeのロゴのみ、もしくは全てのロゴの表示を無くして下さい。