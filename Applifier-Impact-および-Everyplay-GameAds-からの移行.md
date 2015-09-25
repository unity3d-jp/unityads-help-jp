# Applifier Impact および Everyplay GameAds からの移行

Applifier Impact および Everyplay GameAds からの移行はとても簡単です。Android 用と iOS 用の互換レイヤーがあり、新しい Unity Ads インターフェイスをラップして古い Applifier Impact インターフェイスでシームレスに動作するようになっています。

# Android

Android では、Unity Ads を Android のライブラリプロジェクトとして利用することを推奨します。jar ファイルをインポートする従来の方法も利用可能です。

* [統合ガイド（Android向け）](統合ガイド（Android 向け）)

## Android ライブラリプロジェクトとしてインポートする

* プロジェクトから applifier-impact-android.jar への参照を削除する
* IDE に SDK フォルダから `unity-ads` フォルダをインポートする
* アプリケーション プロジェクトの project.properties ファイルに「manifestmerger.enabled=“true」を含めておく

## JAR ファイルとしてインポートする

* プロジェクトから applifier-impact-android.jar への参照を削除する
* IDE に SDK フォルダから  `unity-ads/libs/unity-ads.jar ` フォルダをインポートする
* [ここ](https://github.com/Applifier/unity-ads/blob/master/android/sources/AndroidManifest.xml) からマニフェストファイルを手動でプロジェクトに統合する 

# iOS

* プロジェクトから ApplifierImpact.framework と ApplifierImpact.bundle を削除する
* プロジェクトに UnityAds.framework と UnityAds.bundle を追加する
* #importを`<ApplifierImpact/ApplifierImpact.h>` から `<UnityAds/UnityAds.h>`に変更する
* [統合ガイド（iOS 向け）](統合ガイド（iOS 向け）)

# Unity プラグイン

Unity プロジェクトは、統合を容易かつシームレスに行えるよう再構成されています。しかし Applifier Impact 互換レイヤーがないため、新インターフェイスにアップグレードする必要があります。

* Plugins / UnityAds にスクリプトと UnityAds プレハブが入っており、これらは Applifier Impact と Everyplay GameAds ディストリビューションにおけるスクリプトとプレハブに近い作りのものです
* あらかじめ Unity 統合ガイドに目を通し、新インターフェイスの詳細を確認しておくことを推奨します
* [統合ガイド（Unity アセットストア パッケージ向け](統合ガイド（Unity アセットストア パッケージ向け） )
