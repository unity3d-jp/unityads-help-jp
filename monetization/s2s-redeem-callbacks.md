![S2Sコールバックの図解](https://cdn.applifier.com/files/unityads_s2s_illustration.png)

サーバーからのコールバックは、ユーザーが広告を視聴した際にあなたのサーバーに送信されます。これらのコールバックを使用して、不正な行為などを検出することができます。

注：デフォルトでは、サーバー間のコールバックは使用できません。ゲームでそれらを有効にしたい場合は、unityads-support@unity3d.comにメールでサポート担当者までお問い合わせください。ゲームIDとそれぞれのコールバックURLをメッセージに入力すると、コールバックに署名して検証するために使用されるハッシュを送信します。

プレーヤーが動画視聴を完了すると、Unity Adsサーバーは指定されたURLに署名付きコールバックを送信します。これは動画が実際に終了の前に行われるので、比較的正確にユーザーの動画へのアクセスの記録を完了することができます。広告の視聴が終了した後、報酬通知を表示します。これは、ビデオが実際に終了するまで、ユーザーの気を散らすことを避けるためです。

通信環境によっては、コールバックまでのラグが発生し時間がかかることがあります。スムーズなゲームプレイを実現するために、S2Sコールバックを使用して不正行為に対するチェックを行う方法があります。

S2Sコールバックを使用するには、広告を表示する前にサーバーID（sid）を設定する必要があります。

Unityでのソースコード実装の例:

```
using UnityEngine;
using System.Collections;
using UnityEngine.Advertisements;

public class UnityAdsManager : MonoBehaviour
{
    // an integer string of usually 5-7 digits, example: "12345"
    public string gameId;
    public string placement = "rewardedVideo"

    // Call this function when Advertisement.IsReady == true
    public void showAd() {

        ShowOptions options = new ShowOptions();

        // setting the server ID
        options.gamerSid = "example";

        Advertisement.Show (placement, options);
    }
}

```


ネイティブのUnity Ads SDKでは、layerMetaData APIクラスで行われます。

Androidの例:
```
    if(UnityAds.isReady()) {
        PlayerMetaData playerMetaData = new PlayerMetaData(context);
        playerMetaData.setServerId("example");
        playerMetaData.commit();

        UnityAds.show(activity);
    }
```

iOSの例:

```
    if([UnityAds isReady]) {
        id playerMetaData = [[UADSPlayerMetaData alloc] init];
        [playerMetaData setServerId:@"example"];
        [playerMetaData commit];

        [UnityAds show:self];
    }
```

#### コールバックの起点
コールバックは、[https://static.applifier.com/public_ips.json](https://static.applifier.com/public_ips.json) に記載されているIPアドレス/ネットワークから取得されます。リストは、毎月1日に更新されます。パブリッシャーは、どこからでもコールバックを確認と制御することができます。

#### コールバックURLのフォーマット
リクエストは、次のフォーマットURLに対するHTTP/1.1 GETリクエストです。
>
> [CALLBACK_URL][SEPARATOR1]sid=[SID][SEPARATOR]oid=[OID][SEPARATOR]hmac=[SIGNATURE]
>

パラメータとその内容に関する説明は以下です:

|Parameter|Content|
|---------|-------|
|CALLBACK_URL|コールバックURLのベースURL, 例えば: `https://developer.example.com/award.php?productid=1234`. これを設定するにはunityads-support@unity3d.comまでお問い合わせください。 |
|SEPARATOR1|`&` or `?`: URL内に `?` はまだ存在しない場合は `?` が使用され, そうでない場合は `&` が使用されます。|
|SID|ユーザーID、またはエンドポイントに送信する任意のカスタムデータ。 上記の例は、異なるプラットフォームでサーバーIDを設定する方法を示しています。例: `1234567890`|
|SEPARATOR|`&`|
|OID|Unity Adsサーバーによって生成された固有のOffer ID, 例: `0987654321`|
|SEPARATOR|`&`|
|SIGNATURE|以下で説明するようなパラメータ文字列のHDMAC-MD5ハッシュ, 例: `106ed4300f91145aff6378a355fced73`|

例:
https://developer.example.com/award.php?productid=1234&sid=1234567890&oid=0987654321&hmac=106ed4300f91145aff6378a355fced73.

### コールバックURLの設定
コールバックURLリクエストには、URLパラメータに署名が付けられます。署名は、hmacを除いたkey=valeu形式のすべてのURLパラメータをコンマで区切られたアルファベット順に連結して作成されたパラメータ文字列のHDMAC-MD5ハッシュです。

例えば, `https://developer.example.com/award.php?productid=1234&sid=1234567890&oid=0987654321` のSIDとOIDを持つコールバックURLでは、パラメータ文字列 ‘`oid=0987654321,productid=1234,sid=1234567890`‘ となり、秘密のハッシュキー ‘`xyzKEY`‘ でハッシュすると(コールバックを有効にすると、サポートエンジニアから秘密のハッシュを取得できます) ‘`106ed4300f91145aff6378a355fced73`‘ が与えられます。その結果、報酬コールバックはURLを生成します:

```
https://developer.example.com/award.php?productid=1234&sid=1234567890&oid=0987654321&hmac=106ed4300f91145aff6378a355fced73
```

**注意:** コールバックURLに含まれるすべてのパラメータは、アルファベット順で署名の算出に含める必要があります。他の方法では、署名は一致しません。

#### コールバックに対する応答
リクエストがすべてのチェックを通過し、ユーザーにアイテムが与えられた場合、URLはHTTP/1.1 200 OKで応答します。HTTPリクエストの本文には文字「1」が含まれます。例:

```
HTTP/1.1 200 OK
Date: Wed, 22 Feb 2012 23:59:59 GMT
Content-Length: 8

1
```
エラーが発生した場合、サーバーは400番台または500番台のHTTPエラーを人間が判読可能なエラーとして返します。例えば、OIDがすでに使用されている場合、署名が一致しない場合、またはユーザが約束されたアイテムを持っていない場合などのエラーです。例:

```
HTTP/1.1 400 ERROR
Date: Wed, 22 Feb 2012 23:59:59 GMT
Content-Length: 12

Duplicate order
```

#### node.jsでのコールバックの例
次の例は、node.js + expressを使用して署名を検証する方法を示しています。
```
// NODE.js S2Sコールバック・エンドポイントのサンプル実装
// Unity Ads

var express = require('express');
var crypto = require('crypto')
var app = express();


app.listen(process.env.PORT || 3412);

function getHMAC(parameters, secret) {
	var sortedParameterString = sortParams(parameters);
	return crypto.createHmac('md5', secret).update(sortedParameterString).digest('hex');
}

function sortParams(parameters) {
	var params = parameters || {};
	return Object.keys(params)
		.filter(key => key !== 'hmac')
		.sort()
		.map(key => params[key] === null ? `${key}=` : `${key}=${params[key]}`)
		.join(',');
}

app.get('/', function (req, res) {

	var sid = req.query.sid;
	var oid = req.query.oid;
	var hmac = req.query.hmac;

	// 環境変数としてsecretを保存します。設定がない場合はデフォルトとしてxyzKEYです
	var secret = process.env.UNITYADSSECRET || 'xyzKEY';

	var newHmac = getHMAC(req.query, secret);

	if (hmac === newHmac) {
		// 署名が一致する

		// ここで、重複したoidがある（プレーヤーはすでに報酬を受け取っている）かどうかを確認し、存在する場合は403を返します

		// 重複がない場合 - プレーヤーにバーチャルグッズを提供します。 失敗した場合は500を返します。

		// 重複チェックのためにoidを保存します。 失敗した場合は500を返します。

		// コールバックが成功し、200を返すと、メッセージの本文に「1」が含まれます。
		res.status(200).send('1');

	} else {
		// no match
		res.sendStatus(403);
	}

});
```

#### PHPでのコールバックの例
次の例は、PHPで署名を検証する方法を示しています。
```
<?php
function generate_hash($params, $secret) {
   ksort($params); // すべてのパラメータは常にアルファベット順にチェックされます
   $s = '';
   foreach ($params as $key => $value) {
     $s .= "$key=$value,";
   }
   $s = substr($s, 0, -1);
   $hash = hash_hmac('md5', $s, $secret);
   return $hash;
}

$hash = $_GET['hmac'];
unset($_GET['hmac']);
$signature = generate_hash($_GET, 'xyzKEY'); // Unity Adsサポートから受け取った秘密のハッシュキーをここに挿入します
error_log("req hmac".$hash);
error_log("sig hmac".$signature);

// 署名を確認する
if($hash != $signature) { header('HTTP/1.1 403 Forbidden'); echo "Signature did not match"; exit; }

// 重複オーダーを確認する
if(check_duplicate_orders($_GET['oid']) { header('HTTP/1.1 403 Forbidden'); echo "Duplicate order"; exit; }

// 重複がなければ、プレイヤーにアイテムを渡し、成功したかどうかチェックします。
if(!give_item_to_player($_GET['sid'], $_GET['product']) { header('HTTP/1.1 500 Internal Server Error'); echo "Failed to give item to the player"; exit; }

// 重複チェックのオーダーIDを保存する
if(save_order_number($_GET['oid']) { header('HTTP/1.1 500 Internal Server Error'); echo "Order ID saving failed, user granted item"; exit; }

// すべてOKで、"1"を返します。
header('HTTP/1.1 200 OK');
echo "1";
?>
```

[1000]: https://blogs.unity3d.com/2016/08/22/unity-ads-platform-update-integration-tutorials/
