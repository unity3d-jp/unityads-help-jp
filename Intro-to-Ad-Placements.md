## 広告表示ついて

広告表示の設定によって、広告をゲーム中のどこに、そしてどのように見せるかをよりよくコントロールすることができます。
ゲーム中の広告を配置するシチュエーションに応じて、異なった広告構成にすることも可能です。

デフォルトの広告表示は以下の通りです。

* default placement: 動画広告, 再生して5秒後からスキップ可能
* Rewarded placement: 動画のみ, スキップは不可能

## 他の広告表示はどんなときに使えますか？

単純に広告を見せたい場合には、デフォルトの広告表示を使えば問題ありません。

しかしながら、例えばリワード動画広告を見せたい場合、つまりユーザーがスキップできない動画広告を見ることで、ゲーム上のコインやライフを得られるというような場合には `報酬型動画` を使いましょう。

もし上記２種以外の広告表示（例えばトラッキングなど）を使用したい場合、新ダッシュボードでは "対象のプロジェクト" > "プラットフォームの選択" > "Ad placements" から作成することができます。

## 広告表示の設定はどこから変更できますか？

広告表示は自由に変更することができます。[Unity Ads 管理パネル](https://unityads.unity3d.com/admin/)の "ゲーム" から設定したいゲームを選択し、 "開発者用設定" の "高度な設定" から設定します。

新ダッシュボードでは "対象のプロジェクト" > "プラットフォームの選択" > "Ad placements" から設定することができます。

参考として、有用な広告表示の例を[こちら](https://github.com/Applifier/unity-ads/wiki/Downloads)でご覧いただけます。

## Unity Ads package のコード例

Unity C＃ のコードで、 Unity Ads で異なるゾーンを使用することは簡単で、call parameterにzoneIDを加えるだけです。

```
if(Advertisement.IsReady("PLACEMENT ID")) {
        Advertisement.Show("PLACEMENT ID");
    }
```

デフォルトの広告表示が使いたいときには、パラメーターを省く、もしくは `null` と入力します。

```
if(Advertisement.IsReady()) {
        Advertisement.Show();
    }
```

## ネイティブ iOS のコード例

ネイティブ SDK 統合では、`show` をコールする前に以下のどちらかをしなければなりません。
 
- 常にデフォルトの広告表示を使う
- 毎回 `setZone("")` をコールする



```
if ([[UnityAds sharedInstance] canShow]) {
    [[UnityAds sharedInstance] setZone:@"PLACEMENT ID"];
    [[UnityAds sharedInstance] show:....];
}
```

## Android のコード例

iOSと同じく、ネイティブ SDK 統合では、`show` をコールする前に以下のどちらかをしなければなりません。
 
- 常にデフォルトの広告表示を使う
- 毎回 `setZone("")` をコールする

```
if(UnityAds.canShow()){
    UnityAds.setZone("PLACEMENT ID");
    UnityAds.show(options);
}
```

