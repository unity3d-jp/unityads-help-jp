#Unityのゲームに組み込む

## 概要
- 組み込みの概要
- セットアップとイニシャライズ
- Services Window を使う
- Asset Package を使う
- テストモードで広告が表示されるか試す
- 広告を見せる
- ゲーム内リワード広告を見せる
- サーバー間のアイテム授受コールバックを使う
- Unity Ads で使える Scripting API

##組み込みの概要
詳細な部分の前に、単純な組み込みの概要です。以下の 3 ステップで Unity Ads を組み込むことができます。

1. セットアップとイニシャライズ
2. Verify ads are ready. 広告の認証?
3. 広告をユーザーに見せる

次の例は Unity Ads をイニシャライズし、デフォルトの広告表示で見せるまでを行うことができます。Unity Ads が Service window で利用可能となっている場合には、Unity が Unity Ads をイニシャライズしてくれています。残りの部分は、Service window と Unity Ads asset package どちらを使っていても違いありません。

C# Example – ShowAdOnStart.cs

```
using UnityEngine;
using System.Collections;
using UnityEngine.Advertisements; // Using the Unity Ads namespace.

public class ShowAdOnStart : MonoBehaviour
{
    #if !UNITY_ADS // If the Ads service is not enabled...
    public string gameId; // Set this value from the inspector.
    public bool enableTestMode = true;
    #endif

    IEnumerator Start ()
    {
        #if !UNITY_ADS // If the Ads service is not enabled...
        if (Advertisement.isSupported) { // If runtime platform is supported...
            Advertisement.Initialize(gameId, enableTestMode); // ...initialize.
        }
        #endif

        // Wait until Unity Ads is initialized,
        //  and the default ad placement is ready.
        while (!Advertisement.isInitialized || !Advertisement.IsReady()) {
            yield return new WaitForSeconds(0.5f);
        }

        // Show the default ad placement.
        Advertisement.Show();
    }
}
```

JavaScript Example – ShowAdOnStart.js

```
#pragma strict
import UnityEngine.Advertisements; // Import the Unity Ads namespace.

#if !UNITY_ADS // If the Ads service is not enabled...
public var gameId : String; // Set this value from the inspector.
public var enableTestMode : boolean = true;
#endif

function Start () : IEnumerator
{
    #if !UNITY_ADS // If the Ads service is not enabled...
    if (Advertisement.isSupported) { // If runtime platform is supported...
        Advertisement.Initialize(gameId, enableTestMode); // ...initialize.
    }
    #endif

    // Wait until Unity Ads is initialized,
    //  and the default ad placement is ready.
    while (!Advertisement.isInitialized || !Advertisement.IsReady()) {
        yield WaitForSeconds(0.5);
    }

    // Show the default ad placement.
    Advertisement.Show();
}
```

それでは Unity Ads 組み込みの詳細を見ていきましょう。

## セットアップとイニシャライズ
Unity に　Unity Ads を組み込むには、2 つの方法があります。

- Services Window を使う (Unity 5.2 以降)
- Asset Package を使う (Unity 4.3 以降)

Unity 5.2 以降では、Unity Ads API が既に Unity のエンジンに組み込まれています。これによって、セットアップとイニシャライズが非常に簡単になります。

もちろん、Unity 5.2 以降でも Service window を使わず Unity Ads asset package を使うこともできます。Unity Ads asset package は Unity 4.3 以降に互換性があります。


## Services Window を使う
このセクションでは、Unity 5.2 以降に導入されている Services window を使った Unity Ads の組み込みについてを説明します。

>注意：Service window から Unity Ads が利用可能である場合に、
Unity Ads asset package を使用されると、コンフリクトを起こす可能性があります。

>もし既に Unity Ads asset package より Unity Ads をプロジェクトに組み込んでいて、Unity 5.2 にアップグレードし、Service window から Unity Ads を使用されたい場合、今までの Unity Ads asset package は削除して下さい。

>これらの asset は以下のロケーションにあります。

>- Plugins/Android/unityads
- Plugins/iOS/UnityAds.bundle
- Plugins/iOS/UnityAds.framework
- Plugins/iOS/UnityAdsUnityWrapper.h
- Plugins/iOS/UnityAdsUnityWrapper.mm
- StandardAssets/Editor/UnityAds
- StandardAssets/UnityAds

>Package Uninstaller を使うと簡単に削除することができます。Asset Store より無料でご利用になれます。
>
Services window を使った Unity Ads の組み込みは、以下の Step で行うことができます。

Step 1：プラットフォームの選択( iOS, Android )

1. Unity のメニューより Select File > Build Settings... 
2. プラットフォームリストより iOS もしくは Android を選択
3. Switch Platform を選択

Step 2：Servises window を使って Unity Ads を利用可能にする

1. Select Window > Services から Services window を開く
2. Ads service の configure settings を選択
3. Ads のトグルスイッチを ONにする
4. 組み込もうとしているゲームの対象年齢が13歳以上であるかどうかを選択
5. Save Changes で保存

>13歳以下を対象とするゲームの場合、Unity Ads は Behaviorally targeting を行いません。
>Behavioral targeting は適切なユーザーを選んで広告を表示するため eCPM をより高めますが、COPPA レギュレーションによって 13歳以下のユーザーに Behavioral targeting を行うことは禁じられているためです。

Service window から Unity Ads が使用可能になると、ゲームスタート時に Unity Ads が自動的にイニシャライズされます。Game ID の確認も Service window より可能です。

Service window　のより詳しい情報は、Unity Manual の Unity Ads セクションをご覧ください。組み込みの続きは、Showing an Ad Placement をご覧ください。また、Using Test Mode の章もご確認下さい。

###ゲームスタート時のオートイニシャライズを無効化したい

Unity Ads のイニシャライズ時により多くの要求をしたい場合、editor script を使ってオートイニシャライズを無効化することができます。

>注意：Unity Ads のイニシャライズは、出来るだけ早いうちに行われることを推奨します。電波状況の良いところであっても、始めの動画広告をイニシャライズするのに 1~2分を要します。また、多くのサードパーティ SDK を同時にイニシャライズする場合にはより多くの時間と帯域が必要となります。

ゲームスタート時のオートイニシャライズを無効化するには、UnityAdsBuildProcessor という C# スクリプトを、プロジェクトのエディターフォルダに新規で作成する必要があります。以下をコピーして作成することができます。

```
using UnityEditor;
using UnityEditor.Callbacks;
using UnityEditor.Advertisements;

public class UnityAdsBuildProcessor : Editor
{
    [PostProcessScene]
    public static void OnPostprocessScene ()
    {
        AdvertisementSettings.enabled = true;
        AdvertisementSettings.initializeOnStartup = false;
    }
}
```

このスクリプトはゲームプレイやビルドの前に作動します。これを使うことで、Unity Ads を利用可能ですが、ゲームスタート時のオートイニシャライズは無効化されます。

## Asset Package を使う
このセクションでは、Unity 4.3 以降を対象とした Unity Ads asset package を使った Unity Ads の組み込みについてを説明します。

>注意：Unity 5.2 以降をお使いの場合でも、 Unity Ads asset package から Unity Ads を組み込むことができまが、Service window を使った組み込みを強く推奨します。
>Unity 5.2 以降において、Unity Ads asset package を使った組み込みをする場合には Service window は使用できなくなります。
>

Unity Ads asset package を使った Unity Ads の組み込みは以下のステップに従って行って下さい。

Step 1：プラットフォームの選択( iOS, Android )

1. Unity のメニューより Select File > Build Settings... 
2. プラットフォームリストより iOS もしくは Android を選択
3. Switch Platform を選択

Step 2：Unity Ads asset package をインポートする

Step 3：UnityExample という GameObject を作成する

1. Unity のメニューより GameObject を選択 > Create Empty
2. 作成した GameObject を Hierarchy window で選択
3. 右クリックで GameObject を選択し、名前を変更する
4. UnityAdsExample と入力し、Enterキーを押して変更 

Step 4：UnityAdsExample という C# script を作成

1. Unity メニューより Assets > Create > C# Script 
2. 名前を UnityAdsExample と入力
3. 右クリックで Open を選択し、エディトします
4. 以下のスクリプトをコピー & ペーストして下さい

C# Example – UnityAdsExample.cs

```
using UnityEngine;
using System.Collections;
using UnityEngine.Advertisements; // Declare the Unity Ads namespace.

public class UnityAdsExample : MonoBehaviour
{
    // Define gameId as a public field
    //  so it can be set from the Inspector.
    public string gameId;

    void Start ()
    {
        if (Advertisement.isSupported) { // If the platform is supported,
            Advertisement.Initialize(gameId); // initialize Unity Ads.
        }
    }
}
```
 
Step 5：UnityAdsExample GameObject にスクリプトを追加

1. UnityAdsExample GameObject を Hierarchy window で選択
2. Unity メニューより Component > Scripts > Unity Ads Example

Step 6：GameObject を選択し、Inspector window に Game ID を入力


>Game ID がわからない場合、Unity Ads dashboard の プラットフォームリストよりご確認いただけます。
>旧式の Unity Ads dashboard をお使いの場合には、左側メニューの "ゲーム" のリストよりご確認いただけます。 

##UnityAdsExample の拡張

以下の項目にも対応できるように機能を拡張します。

- Game ID の欄において iOS, Android どちらにも対応する
- Unity Ads の Test Mode を利用可能にする

C# Example – UnityAdsExample.cs

```
using UnityEngine;
using System.Collections;
using UnityEngine.Advertisements;

public class UnityAdsExample : MonoBehaviour
{
    // Serialize private fields
    //  instead of making them public.
    [SerializeField] string iosGameId;
    [SerializeField] string androidGameId;
    [SerializeField] bool enableTestMode;

    void Start ()
    {
        string gameId = null;

        #if UNITY_IOS // If build platform is set to iOS...
        gameId = iosGameId;
        #elif UNITY_ANDROID // Else if build platform is set to Android...
        gameId = androidGameId;
        #endif

        if (string.IsNullOrEmpty(gameId)) { // Make sure the Game ID is set.
            Debug.LogError("Failed to initialize Unity Ads. Game ID is null or empty.");
        } else if (!Advertisement.isSupported) {
            Debug.LogWarning("Unable to initialize Unity Ads. Platform not supported.");
        } else if (Advertisement.isInitialized) {
            Debug.Log("Unity Ads is already initialized.");
        } else {
            Debug.Log(string.Format("Initialize Unity Ads using Game ID {0} with Test Mode {1}.",
                gameId, enableTestMode ? "enabled" : "disabled"));
            Advertisement.Initialize(gameId, enableTestMode);
        }
    }
}
```


##テストモードで広告が表示されるか試す
開発中に、Unity Ads のテストモードの使用を強く推奨します。テストモードが有効になっている間は、ゲームにテスト用広告のみが流れます。テスト広告は、通常のリミットである 1日に 25 広告、という制限がありません。また、クリックしても Revenue を生み出すことはありません。


>注意：Unity Ads の Term of Service により、自分のゲームに実装した広告からゲームをインストールし、インプレッションを発生させることは禁じられています。詐欺行為としてアカウントが BAN される可能性もあります。これを避けるためにも、テストの際にはテストモードをご利用ください。

Unity 5.2 以降で Service window をご利用の場合には、Unity Ads はデフォルトでテストモードとなっています。設定は Service window の Ads 項目より変更できます。

Unity Ads のイニシャライズに `Advertisement.Initialize method` をお使いの場合、`Boolean value` を通すことでテストモードの設定が可能です。詳細は Scripting API をご覧ください。

ゲームが完成したら、ビルドする前にテストモードをオフにして下さい。もしくは、Unity Ads dashboard の platform tab よりオフにすることもできます。

旧管理画面をお使いの場合には、開発者用設定のタブよりテストモードの設定が行えます。

##広告を見せる
このセクションでは広告を見せる方法について紹介します。Unity Ads では広告の表示を `Ad placement` と呼びます。

UnityAdsButton という C# スクリプトを新規に作成し、対象の scene の新規 GameObject に追加します。以下のコードをコピーして、ご利用ください。

C# Example – UnityAdsButton.cs

```
using UnityEngine;
using System.Collections;
using UnityEngine.Advertisements;

public class UnityAdsButton : MonoBehaviour
{
    void OnGUI ()
    {
        Rect buttonRect = new Rect (10, 10, 150, 50);
        string buttonText = Advertisement.IsReady () ? "Show Ad" : "Waiting...";

        if (GUI.Button (buttonRect, buttonText)) {
            Advertisement.Show ();
        }
    }
}
```

##ゲーム内リワード広告を表示する
このセクションでは、ゲーム内リワード広告についてを紹介します。

UnityAdsRewardedButton という C# スクリプトを新規に作成し、対象の scene の新規 GameObject に追加します。以下のコードをコピーして、ご利用ください。

C# Example – UnityAdsRewardedButton.cs

```
using UnityEngine;
using System.Collections;
using UnityEngine.Advertisements;

public class UnityAdsRewardedButton : MonoBehaviour
{
    public string zoneId;
    public int rewardQty = 250;

    void OnGUI ()
    {
        if (string.IsNullOrEmpty (zoneId)) zoneId = null;

        Rect buttonRect = new Rect (10, 10, 150, 50);
        string buttonText = Advertisement.IsReady (zoneId) ? "Show Ad" : "Waiting...";

        ShowOptions options = new ShowOptions();
        options.resultCallback = HandleShowResult;

        if (GUI.Button (buttonRect, buttonText)) {
            Advertisement.Show (zoneId, options);
        }
    }

    private void HandleShowResult (ShowResult result)
    {
        switch (result)
        {
        case ShowResult.Finished:
            Debug.Log ("Video completed. User rewarded " + rewardQty + " credits.");
            break;
        case ShowResult.Skipped:
            Debug.LogWarning ("Video was skipped.");
            break;
        case ShowResult.Failed:
            Debug.LogError ("Video failed to show.");
            break;
        }
    }
}
```

##サーバー間のアイテム授受コールバックを使う
詳細は[こちら](https://github.com/unity3d-jp/unityads-help-jp/wiki/s2s-redeem-callbacks)をご覧ください。


##Unity Ads で使える Scripting API
Unity Ads で使用できる Scripting API は以下となります。

- Classes
UnityEngine.Advertisements.Advertisement
UnityEngine.Advertisements.ShowOptions

- Enumerations
UnityEngine.Advertisements.Advertisement.DebugLevel
UnityEngine.Advertisements.ShowResult

下記のクラスで Unity Ads の Services window の設定ができます。

- UnityEditor.Advertisements.AdvertisementSettings