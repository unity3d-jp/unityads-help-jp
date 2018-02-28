このガイドでは、UnityEngineにUnity Adsを組み込むための2つの方法について説明します:

* Services Windowを使用する
* Asset Packageを使用する

**コンテンツ**

* ビルドターゲットを設定する
* 広告サービスを有効にする
    * Services Windowでの方法
    * Asset Packageでの方法
* 広告を表示する
* 広告を視聴するプレーヤーに報酬を与える
    * 広告表示のボタンのソースコード例
* 広告ダッシュボードで設定を管理する

## ビルドターゲットを設定する

[Build Settingsウィンドウ]（BuildSettings）で、サポート対象のプラットフォームにプロジェクトを設定します。

プラットフォームを __iOS__ または __Android__ に設定し、__Switch Platform__ をクリックします。

![Build Settingsウィンドウ](https://docs.unity3d.com/uploads/Main/BuildTargets.png "Build Settingsウィンドウ")

## 広告サービスを有効にする

このプロセスは、組み込みの方法（Services Window もしくは Asset Package）によって若干異なります。

### Services Windowでの方法
広告を有効にするには、Unity Services用にプロジェクトを設定する必要があります。これには、__Organization__ と __Project Name__ を設定する必要があります。[サービスの設定](https://docs.unity3d.com/Manual/SettingUpProjectServices.html) を参照してください。

ここまででサービスを設定すると、Unity Adsを有効にすることができます：

1. Unityエディタで __Window__ > __Services__ を選択して、Servicesウィンドウを開きます。
2. Servicesウィンドウのメニューから __Ads__ を選択します。
3. 右側のトグルをクリックして、広告サービスを有効にします（下の画像を参照）。
4. チェックボックスにチェックを入れ、 __Continue__ をクリックして、13歳未満の子供をターゲットにするかどうかを指定します。

![Servicesウィンドウ](https://docs.unity3d.com/uploads/Main/ServicesWindow.png "UnityエディタのServicesウィンドウ（左）とServicesウィンドウのAds項目でトグルを有効にする（真ん中, 右）")

### Asset Packageでの方法
Asset PackageにUnity Adsを組み込む前に、Unity Ads Game IDを作成する必要があります。このあとの説明でアカウントを使用します。

**Unity Ads Game IDを作成する**

1. Webブラウザで、Unity Developer Network [UDN]（https://id.unity.com/account/new）アカウントを使用して、[Unity Ads Dashboard]（https://dashboard.unityads.unity3d.com/）に移動し、__Add new project__ を選択します。<br/><br/>![Add a new project](https://docs.unity3d.com/uploads/Main/NewProject.png)

2. 該当するプラットフォーム（iOS、Android、またはその両方）を選択します。<br/><br/>![Select your platform](https://docs.unity3d.com/uploads/Main/SelectPlatforms.png)

3. プラットフォーム固有の `Game ID` を探し出し、後のためにそれをコピーします。<br/><br/>![Locate your Game ID](https://docs.unity3d.com/uploads/Main/CopyGameID.png)

**Asset PackageにAdsを組み込む**

1. スクリプトのヘッダーにUnity Adsの名前空間 `UnityEngine.Advertisements` を宣言してください([UnityEngine.Advertisements](https://docs.unity3d.com/ScriptReference/Advertisements.Advertisement.html) ドキュメントを参照)。 <br/>

    ```
        using UnityEngine.Advertisements;
    ```

2. コピーしたgameIDの文字列を使用して、スクリプト内のUnity Adsを初期化します: <br/>

	```
	    Advertisement.Initialize(string gameId)
	```

**注釈**: 初期化の呼び出しは通常、コードのStart関数で行います。

## 広告を表示する

**Note**: Unity AdsはUnity 5.2以降のServicesウィンドウから利用できます。

サービスを有効にすると、任意のスクリプトでコードを実装して広告を表示できます。

1. スクリプトのヘッダーにUnity Adsの名前空間 `UnityEngine.Advertisements` を宣言してください([ UnityEngine.Advertisements](https://docs.unity3d.com/ScriptReference/Advertisements.Advertisement.html) ドキュメントを参照)。: <br/>

   ```
   using UnityEngine.Advertisements;   
   ```

2. `Show()` 関数を呼び出して広告を表示する: <br/>

    ```
    Advertisement.Show()
    ```

## 広告を視聴するプレーヤーに報酬を与える

広告を視聴するプレーヤーに報酬を与えることで、ユーザーエンゲージメントが向上し、収益が増加します。例えば、ゲームでは、ゲーム内の通貨、消耗品、追加のライフ、または経験値の乗算をプレイヤーに報酬として与えることができます。

動画広告を視聴したプレーヤーに報酬を与えるには、以下の例の `HandleShowResult`コールバックメソッドを使用します。ユーザーが広告をスキップしていないことを確認するには、`result` が ` ShowResult.Finished` に等しいことを確認してください。

1. スクリプトにコールバックメソッドを追加する。
2. `Show()` を呼び出すときに、このメソッドをパラメータとして渡します。
3. ビデオをスキップ不可能にするには、`"rewardedVideo"`プレースメントで `Show（）`を呼び出します。

**注釈**: 「プレースメント」の詳細については、Unity Adsドキュメント](https://unityads.unity3d.com/help/monetization/placements) を参照してください。

```cs
void ShowRewardedVideo ()
{
    ShowOptions options = new ShowOptions();
	options.resultCallback = HandleShowResult;

	Advertisement.Show("rewardedVideo", options);
}

void HandleShowResult (ShowResult result)
{
	if(result == ShowResult.Finished) {
		Debug.Log("Video completed - Offer a reward to the player");

	}else if(result == ShowResult.Skipped) {
		Debug.LogWarning("Video was skipped - Do NOT reward the player");

	}else if(result == ShowResult.Failed) {
		Debug.LogError("Video failed to show");
	}
}
```

### 報酬型広告のボタンのコード例

広告ボタンを作成するには、下のコードを使用してください。広告ボタンは、設定を行なっている状態で押されると広告を表示します。

1. __Game Object__ > __UI__ > __Button__ を選択し、[シーン]にボタンを追加します。(https://docs.unity3d.com/Manual/UsingTheSceneView.html).
2. シーンに追加したボタンを選択し、[インスペクター]からスクリプトコンポーネントを追加します。 (https://docs.unity3d.com/Manual/UsingTheInspector.html). (インスペクターで __Add Component__ > __New Script__ を選択します。)
3. 作成したスクリプトを開き(https://docs.unity3d.com/Manual/CreatingAndUsingScripts.html)、以下のコードを追加します: <br/> <br/>**注釈**: Asset Packageへの組み込みに固有であるコード2箇所にコメントがついています。 <br/>

``` cs
 using UnityEngine;
using UnityEngine.UI;
using UnityEngine.Advertisements;

//---------- ONLY NECESSARY FOR ASSET PACKAGE INTEGRATION: ----------//

#if UNITY_IOS
private string gameId = "1486551";
#elif UNITY_ANDROID
private string gameId = "1486550";
#endif

//-------------------------------------------------------------------//

[RequireComponent(typeof(Button))]
public class UnityAdsButton : MonoBehaviour
{
    Button m_Button;

    public string placementId = "rewardedVideo";

    void Start ()
    {
        m_Button = GetComponent<Button>();
        if (m_Button) m_Button.onClick.AddListener(ShowAd);

        //---------- ONLY NECESSARY FOR ASSET PACKAGE INTEGRATION: ----------//

        if (Advertisement.isSupported) {
            Advertisement.Initialize (gameId, true);
        }

        //-------------------------------------------------------------------//

    }

    void Update ()
    {
        if (m_Button) m_Button.interactable = Advertisement.IsReady(placementId);
    }

    void ShowAd ()
    {
        ShowOptions options = new ShowOptions();
        options.resultCallback = HandleShowResult;

        Advertisement.Show(placementId, options);
    }

    void HandleShowResult (ShowResult result)
    {
        if(result == ShowResult.Finished) {
        Debug.Log("Video completed - Offer a reward to the player");

        }else if(result == ShowResult.Skipped) {
            Debug.LogWarning("Video was skipped - Do NOT reward the player");

        }else if(result == ShowResult.Failed) {
            Debug.LogError("Video failed to show");
        }
    }
}
```

4. Unityエディタで __Play__ を押して、広告ボタンが組み込まれているかを検証します。

詳細なガイダンスについては、[Unity Adsフォーラム](https://forum.unity3d.com/forums/unity-ads.67) を参照してください。

-------------------------------------------------------------------------

## Adsのダッシュボードで設定を管理する

settingsを使用して、プロジェクト内のプレースメントやその他のゲーム固有の設定を変更します（プレースメントの詳細については、[Unity Adsドキュメント](https://unityads.unity3d.com/help/monetization/placements) を参照してください）。

1. Webブラウザで、Unity Developer Network [UDN](https://id.unity.com/account/new) アカウントを使用して、[Unity Ads Dashboard](https://dashboard.unityads.unity3d.com/) に移動し、ゲームのプロジェクトを探します。 <br/> <br/>
    ![プロジェクト選択をハイライトしたUnity Ads Dashboard](https://docs.unity3d.com/uploads/Main/DashSelectProject.png "プロジェクト選択がハイライトされたUnity Ads Dashboard")

2. アプリケーションのプラットフォームを選択します (iOSまたはAndroid). <br/><br/>
    ![プラットフォーム選択をハイライトしたUnity Ads Dashboard](https://docs.unity3d.com/uploads/Main/DashSelectStore.png "プラットフォーム選択をハイライトしたUnity Ads Dashboard")

3. プレースメントを選択します ([Unity Ads documentation](https://unityads.unity3d.com/help/monetization/placements) を参照してください)。 <br/><br/>
    ![プレースメント情報を示すUnity Ads Dashboard](https://docs.unity3d.com/uploads/Main/DashSelectPlacement.png "プレースメント情報を示すUnity Ads Dashboard")
