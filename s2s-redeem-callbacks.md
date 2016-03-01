# サーバー間のアイテム授受コールバック

>注意: デフォルトではサーバー間でのコールバックは利用できません。あなたのゲームでサーバー間のコールバックを可能にするには、[unityads-support@unity3d.com[英語]](mailto:unityads-support@unity3d.com) にメールでお問い合わせください。

ユーザーが動画を見終わると、Unity Ads サーバーはパブリッシャーが指定したバーチャルアイテムのコールバック URL へコールバックを実行します。これは動画が実際に終了する前に行われるため、報酬の授受サイクルはユーザーがゲームに復帰する前に完了します。なおクライアントは、Unity Ads ダイアログが閉じられた旨のコールバックを受け取るまで、ユーザーに通知を表示するべきではありません。動画が実際に終了するまで、ユーザーの邪魔にならないようにするためです。

#### コールバック発信元
コールバックは[こちら](http://static.applifier.com/public_ips.json)のリストにある IP アドレス/ネットワークから発信されます。リストは毎月1日にアップデートされます。こちらをご覧いただくことで、他からのコールバックを無視、あるいはブロックすることができます。

#### コールバック URL のフォーマット
リクエストは HTTP/1.1 GET リクエストで以下のフォーマットの URL に送信されます。
> 
> [CALLBACK_URL][SEPARATOR1]sid=[SID][SEPARATOR]oid=[OID][SEPARATOR]hmac=[SIGNATURE]
> 

各パラメータとその内容を以下に説明します。

|パラメータ|内容|
|---------|-------|
|CALLBACK_URL|コールバック URL のベース URL。例えば `http://developer.example.com/award.php?productid=1234` のようになる。これを変更する場合は [unityads-support@unity3d.com[英語]](unityads-support@unity3d.com) へお問い合わせください。 |
|SEPARATOR1|`&` または `?`。URL 内でこれより前に `?` が存在しない場合は `?` を使う。そうでない場合は `&` を使う。|
|SID|Unity Ads ダイアログ初期化の際に与えられたユーザー ID。例: `1234567890`|
|SEPARATOR|`&`|
|OID|一意のオファー ID。Unity Ads の初期化時にパブリッシャーが指定するか、その時点で指定されなかった場合は Unity Ads サーバーが生成する。例: `0987654321`|
|SEPARATOR|`&`|
|SIGNATURE|パラメータ文字列の HDMAC-MD5 ハッシュ。詳しくは下記を参照。例: `106ed4300f91145aff6378a355fced73`|


例:

```
https://developer.example.com/award.php?productid=1234&sid=1234567890&oid=0987654321&hmac=106ed4300f91145aff6378a355fced73.
```

### コールバック URL の署名
コールバック URL リクエストの URL パラメータには署名が付きます。この署名は、（hmacを除いて）すべての URL パラメータを「キー=値」の形でカンマ区切りでアルファベット順に並べ連結して作ったパラメータ文字列の、HDMAC-MD5 ハッシュです。

例えば、SID と OID を持つコールバック URL `https://developer.example.com/award.php?productid=1234&sid=1234567890&oid=0987654321` のパラメータ文字列は「`oid=0987654321,productid=1234,sid=1234567890`」となり、これがハッシュキー `xyzKEY`（共有ハッシュキーは Unity Ads 管理パネルで設定可能）でハッシュされて、「`106ed4300f91145aff6378a355fced73`」というハッシュが生成されます。この結果、報酬コールバックが送信される URL は以下のようになります。


```
https://developer.example.com/award.php?productid=1234&sid=1234567890&oid=0987654321&hmac=106ed4300f91145aff6378a355fced73
```



>**注意:** コールバック URL に含まれるパラメータはすべて、署名の算出にアルファベット順で含まれる必要があります。これ以外の方法では署名が一致しません。


#### コールバックへの応答
リクエストがすべてのチェックをパスし、ユーザーに報酬アイテムが渡されると、URL は HTTP/1.1 200 OK を返します。その HTTP リクエストの本文には「 1 」が含まれます。例えば次のようになります。


```
HTTP/1.1 200 OK
Date: Wed, 22 Feb 2012 23:59:59 GMT
Content-Length: 8

1
```


エラーが発生した場合、サーバーは HTTP エラー 400 番台か 500 番台を人間に読めるエラーメッセージ付きで返します。例えば、OID が使用済み、署名が一致しないなどのエラーにより、ユーザーが約束のアイテムを受け取れなかった場合は、次のようになります。


```
HTTP/1.1 400 ERROR
Date: Wed, 22 Feb 2012 23:59:59 GMT
Content-Length: 12

Duplicate order
```


#### PHP でのコールバック サンプル
以下のサンプルは、PHP で署名を検証する方法を示すものです。


```
<?php
function generate_hash($params, $secret) {
   ksort($params); // All parameters are always checked in alphabetical order
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
$signature = generate_hash($_GET, 'xyzKEY'); // insert here the shared hash key from the Unity Ads Admin Panel
error_log("req hmac".$hash);
error_log("sig hmac".$signature);

// check signature
if($hash != $signature) { header('HTTP/1.1 403 Forbidden'); echo "Signature did not match"; exit; }

// check duplicate orders
if(check_duplicate_orders($_GET['oid']) { header('HTTP/1.1 403 Forbidden'); echo "Duplicate order"; exit; }

// if not then give the player the item and check that it succeeds.
if(!give_item_to_player($_GET['sid'], $_GET['product']) { header('HTTP/1.1 500 Internal Server Error'); echo "Failed to give item to the player"; exit; }

// save the order ID for dublicate checking
if(save_order_number($_GET['oid']) { header('HTTP/1.1 500 Internal Server Error'); echo "Order ID saving failed, user granted item"; exit; }

// everything OK, return "1"
header('HTTP/1.1 200 OK');
echo "1";
?>
```