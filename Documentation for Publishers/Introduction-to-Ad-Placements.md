## What are ad placements 

## アドプレイスメントとは

Ad placements allows you to have better control on where and how your ads are shown in the game. 

アドプレイスメントを使えば、広告をゲーム中のどこに、そしてどのように見せるかをよりよくコントロールすることができます。

With ad placements you can have different configuration for your ads depending on where in the game you are showing the ads.

ゲーム中の広告を配置するシチュエーションに応じて、異なった広告構成にすることも可能です。

The default placements that are created for your game are:
デフォルトのプレイスメントは以下の通りです。

* default placement: Video ads, skipping enabled after 5 seconds 
* default placement: 動画広告, 再生して5秒後からスキップ可能
* Rewarded placement: Only videos, no-skipping allowed 
* Rewarded placement: 動画のみ, スキップは不可能
* Picture only placement: For places where full video ad-experience would not fit. 
* Picture only placement: 動画広告が収まらない場所のための、画像のみのプレイスメント

## When should I use the different placements? 

## 他のプレイスメントはどんなときに使えますか？

You can normally just simply show advertisements with just the default placements. And if you have no reason to do otherwise this is perfectly OK.

単純に広告を見せたい場合には、デフォルトのプレイスメントを使えば問題ありません。

However if you wish to for example show rewarded videos, where a user gets a coin/gem/extra life for watching a video you can disable skipping for for this placement.

しかしながら、例えばリワード動画広告を見せたい場合、つまりユーザーがスキップできない動画広告を見ることで、ゲーム上のコインやライフを得られるというような場合には `報酬型動画` を使いましょう。

## How to select which placement to show? 

## プレイスメントはどのように決めれば良いですか？

You can use the placements that are created in the admin-interface inside your games code. The actual placements have to be designed by you, but to get an idea what possible placements to use, take a look at our example game (Space Ads):

あなたのゲームコード内における `admin-interface` で作られたプレイスメントを使用することができます。これは自分でデザインしなければなりません。参考として、有用なプレイスメントの例を[こちら](https://github.com/Applifier/unity-ads/wiki/Downloads)からご覧いただけます。




## Code examples for Unity Ads package 

## Unity Ads package のコード例

Using different zones in Unity Ads inside Unity C# code is simple. Just add the zoneId in the call parameter

Unity C＃ のコードで、 Unity Ads で異なるゾーンを使用することは簡単で、call parameterにzoneIDを加えるだけです。

```
    Advertisement.Show("PLACEMENT ID");
```

If you want to use the default placement, you can omit (or pass null) the parameter

デフォルトのプレイスメントが使いたいときには、パラメーターを省く、もしくは `null` と入力します。

```
    Advertisement.Show();
```


## Code examples for native iOS 

## ネイティブ iOS のコード例

With native SDK integration you must either A) always use the default placement or B) remember to call setZone(""); before each show call

ネイティブ SDK 統合では、`show` をコールする前に以下のどちらかをしなければなりません。
 
- 常にデフォルトのプレイスメントを使う
- 毎回 `setZone("")` をコールする



```
if ([[UnityAds sharedInstance] canShow]) {
    [[UnityAds sharedInstance] setZone:@"PLACEMENT ID"];
    [[UnityAds sharedInstance] show:....];
}
```

## Code examples for Android 

## Android のコード例

With native SDK integration you must either A) always use the default placement or B) remember to call setZone(""); before each show call

iOSと同じく、ネイティブ SDK 統合では、`show` をコールする前に以下のどちらかをしなければなりません。
 
- 常にデフォルトのプレイスメントを使う
- 毎回 `setZone("")` をコールする

```
if(UnityAds.canShow()){
    UnityAds.setZone("PLACEMENT ID");
    UnityAds.show(options);
}
```


