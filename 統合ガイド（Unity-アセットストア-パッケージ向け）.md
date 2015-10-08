# 統合ガイド（Unity アセットストア パッケージ向け）

## クイックスタートガイド

* Advertisement.isSupported が true を返すかチェック
* Advertisement.Initialize を app id を使って呼び出す
* 広告が初期化されたら Advertisement.isReady が true を返す
* あとは広告を表示する時に Advertisement.Show を呼び出すだけ

## サンプルコード

広告が表示可能になったら広告を表示する単純なボタンを作成します。

```
using System;
using UnityEngine;
using UnityEngine.Advertisements;

public class AdvertisementTest : MonoBehaviour {
	void Awake() {
		if (Advertisement.isSupported) {
			Advertisement.Initialize (<YOUR GAME ID HERE>);
		} else {
			Debug.Log("Platform not supported");
		}
	}

	void OnGUI() {
		if(GUI.Button(new Rect(10, 10, 150, 50), Advertisement.IsReady() ? "Show Ad" : "Waiting...")) {
			// Show with default zone, pause engine and print result to debug log
			Advertisement.Show(null, new ShowOptions {
				resultCallback = result => {
					Debug.Log(result.ToString());
				}
			});
		}
	}
}
```

### 注意

#### ゾーンの準備

以下のコードでデフォルトのゾーンを呼び出します。`Advertisement.IsReady()`および`Advertisement.Show(null, ...)`

もし違ったゾーンだった場合は、**必ず** `IsReady`と`Show`メソッドのzoneIdを修正して下さい。

例:

```
if(Advertisement.IsReady("pictureZone")) {
	Advertisement.Show("pictureZone", ...);
}
```

####  "UnityAds" という名前の GameObject を使わないで下さい

AndroidとiOSにおける C# と、ネイティブな Android/iOS のやりとりのブリッジには、内部で "UnityAds" が呼び出されています。もしゲームオブジェクトに "UnityAds" が含まれていた場合、ゲームと広告の間のやりとりが失敗する可能性があります。


#### Unity forums について　

[Unity forums](http://forum.unity3d.com/forums/unity-ads.67/) では、Unity Ads について多くの疑問と解決法を見ることができます。また我々もこのフォーラムに挙がっている疑問を解決しようとしています。


## Unity Ads アセットストアパッケージ API リファレンス

すべての広告 API は UnityEngine.Advertisements 名前空間の下で利用可能です。

### Advertisement クラス

**static public DebugLevel debugLevel**

`public enum DebugLevel {
  None = 0,
  Error = 1,
  Warning = 2,
  Info = 4,
  Debug = 8
}`

Unity Ads におけるログの総量をコントロールする。デフォルトではError, Warning, Infoである。デバッグビルドのためのデフォルトは、Error, Warning, Info, Debugである。

**static public bool isSupported**

プラットフォームが広告をサポートしている場合は true を返し、そうでない場合は false を返す。

**static public bool isInitialized**

広告パッケージの初期化に成功している場合は true を返し、そうでない場合は false を返す。

**static public void Initialize(string appId, bool testMode = false)**

広告フレームワークを初期化する。利用できるネットワークがないと初期化が失敗する場合がある。初期化は一度だけで、現在は初期化失敗後の再初期化についてはサポート対象外である。`testMode` パラメータを使用することでテストモードが使用可能となる。これを使うことでサーバー側の広告フィルタリングの受信テストができる。



**static public bool IsReady(string zoneId = null)**

広告の準備ができていれば true を返す。zoneId が与えられていれば、そのゾーンに対してメソッド呼び出しが適用される。

**static public bool isShowing**

広告が現在表示されている場合は true を返し、そうでない場合は false を返す。

**static public void Show(string zoneId = null, ShowOptions options = null)**

広告を表示する。zoneId が与えられていれば、そのゾーンに対してメソッド呼び出しが適用される。このためには広告パッケージの初期化に成功していること、ゾーンの準備ができていることが必要となる。



### ShowOptions クラス

**public Action&lt;ShowResult&gt; resultCallback**

広告が表示された後に呼び出すメソッド。ShowResult の値は Failed、Skipped、Finished となる。

**public string gamerSid**

このプロパティを使うとクライアントなしでサーバー側に閲覧したユーザーのIDを明らかにすることができる。