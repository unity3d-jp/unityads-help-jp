## 動画プレースメントとは

プレースメントは、動画の表示方法を制御できる[ダッシュボード](https://dashboard.unityads.unity3d.com) の設定です。

<!--- Insert dashboard screenshots -->

新たに作成されたゲームプロジェクトには、2つのプレースメントが含まれます:

- "動画" (5秒後にスキップする)
- "報酬型動画" (スキップできない - 30秒動画)

> 注: 広告を表示する場合（ユーザーが動画を視聴すする場合）、「rewardedVideo」プレースメントを使用してスキップができないようにすることをおすすめします。

**Project** - > **Game ID** - > **Settings** を選択すると、[Adsダッシュボード](https://dashboard.unityads.unity3d.com/) で新しいプレースメントを作成してプレースメント設定を変更できます。

> プレースメントを使用すると、ユーザーがどのように動画を視聴しているかを把握することができます。ユーザーが動画広告を操作する方法ごとに異なるプレースメントを使用することをおすすめします。

## コード内でプレースメントを選択する

プレースメントが選択されていない場合、ゲームIDのデフォルトプレースメントが選択されます。例えば、

```
	//show a video with the default placement
    if(Advertisement.IsReady()) {
        Advertisement.Show();
    }
```

Show() を呼び出すときに、プレースメント文字列をパラメータとして含めることで、特定のプレースメントを選択できます。

C＃、Obj-C、Swift、Javaで使うことができます。

### Unity (C#)

```
	//show a video with the "rewardedVideo" placement
    if(Advertisement.IsReady("rewardedVideo")) {
        Advertisement.Show("rewardedVideo");
    }
```

### iOS (Obj-C)

```
	if ([UnityAds isReady]) {
	    [UnityAds show:self placementId:@"rewardedVideo"];
	}
```

### Android (Java)

```
	if (UnityAds.isReady()) {
	    UnityAds.show(this, "rewardedVideo");
	}
```

実際に広告を表示するのはあなた次第ですが、さまざまなプレースメントの使用について検討する際には、[動画広告ガイド](https://blogs.unity3d.com/2015/04/15/a-designers-guide-to-using-video-ads) をご覧ください。
