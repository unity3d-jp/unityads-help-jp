#Unity Ads SDK 2.0 へのアップグレード

Unity Ads SDK 2.0 は、いままでの Unity Ads 1.x プラグイン API をサポートしているため、いままでのコードを使いつづけることができます。しかしながら、Unity Ads SDK 2.0 をインポートする前に、以前のバージョンの Unity Ads を削除してください。

>注意: Unity Ads 2.0 SDK プラグインは、Unity 5.0.1 以降をサポートしています。もしそれ以前のバージョンの Unity をお使いの場合には、Unity もまたアップグレードする必要があります。

##Step 1: Ads service の無効化

Unity Services window から Ads を無効化、もしくはこれまでの Unity Ads Assets を削除します。

###お使いの Unity バージョンが 5.2 以降の場合 

1. Window > Services をクリック
2. Services window から Ads をクリック
3. トグルスイッチをクリックして、Ads を無効化します。

###Unity Ads プラグインを削除する場合

[Package Uninstaller](https://www.assetstore.unity3d.com/en/#!/content/35439) を使用することで、自動的に削除されます。ご自身で削除される場合には、以下の assets をプロジェクトから削除して下さい。

`/Assets/Plugins/Android/unityads/*`
`/Assets/Plugins/iOS/UnityAds.framework`
`/Assets/Plugins/iOS/UnityAds.bundle`
`/Assets/Plugins/iOS/UnityAdsUnityWrapper.h`
`/Assets/Plugins/iOS/UnityAdsUnityWrapper.mm`
`/Assets/Standard Assets/Editor/UnityAds/*`
`/Assets/Standard Assets/UnityAds/*`

##Step 2: [Unity Ads SDK プラグイン](https://www.assetstore.unity3d.com/en/#!/content/66123)のダウンロード、インポート

##Step 3: スクリプトを追加

以前に Ads サービスメニューの統合を使用していた場合は、ゲームが読み込まれたときに Unity Ads をイニシャライズするためのスクリプトを追加します。イニシャライズの API は、以前のバージョン [Unity Ads API](https://docs.unity3d.com/540/Documentation/ScriptReference/Advertisements.Advertisement.Initialize.html) と同様です。

1. ゲームの始めに読み込まれるシーンを開きます。
2. 空の GameObject を作成します。
3. 作成した GameObject に以下のスクリプトを加えます。
4. Game IDはご自身の対象プロジェクトのものに置き換えて下さい。

```
using UnityEngine;
using UnityEngine.Advertisements;

public class UnityAdsInitializer : MonoBehaviour
{
    [SerializeField]
    private string
        androidGameId = "18658",
        iosGameId = "18660";

    [SerializeField]
    private bool testMode;

    void Start ()
    {
        string gameId = null;

        #if UNITY_ANDROID
        gameId = androidGameId;
        #elif UNITY_IOS
        gameId = iosGameId;
        #endif

        if (Advertisement.isSupported && !Advertisement.isInitialized) {
            Advertisement.Initialize(gameId, testMode);
        }
    }
}
```

##Step 4: プロジェクトをビルド、実行します。広告を表示するためのAPIは、以前の Unity Ads API と同様です。