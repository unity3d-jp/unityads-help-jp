# 広告主のための統計 API
Unity Ads では、パブリッシャーは収益化の、広告主はユーザー獲得の、統計データをそれぞれ CSV 形式で直接取得できる API を提供しています。この Unity Ads 統計 API を使うと、ゲームやキャンペーンのデータを取得して、定期的に提携レポートシステムに読み込ませることができます。実質上、統計 API は [Unity Ads 管理パネル][1] で取得できるものと同じ統計ファイルを、自動的にフェッチするためのマシン間インターフェイスです。

### 概要
統計 API は 2 段階で動作します。まず、ユーザーが認証サーバーに対して GET リクエストを実行します。認証に成功すれば、サーバーは 302 HTTP リダイレクトメッセージで応答します。この応答には統計サーバーへの最終的な URL が含まれています

次に、ユーザーが署名済み URL に対して GET リクエストを実行すると、宛先サーバーはリクエストされたデータを CSV 形式でメッセージの本文に入れて返します。

```
Date,Target campaign id,Target name,clicks
2013-02-28 00:00:00,"5065e1f1fdeb285e4d0430ce","Campaign 1",129
2013-02-28 00:00:00,"50ed569d57fe1e324415fbf7","Campaign 2",428
2013-02-28 00:00:00,"50eeb7c39610c9d21c0225cb","Campaign 3",812
2013-02-28 00:00:00,"511e5f7a73452a3363062d5d","Campaign 4",130
...
```

### 認証
Unity Ads 統計 API を使用するには、[Unity Ads 管理パネル][1] から API キーを取得する必要があります。API は [アカウント][2] ページの "APIキー" から確認できます。

この API キーを `apikey` HTTP GET パラメータへの認証リクエストに含めます。

認証に成功すれば、サーバーは 302 HTTP リダイレクトメッセージで応答します。この応答の Location HTTP ヘッダに、統計サーバーのデータへの URL が含まれています。実際のデータはこのリダイレクト URL から取得することになります。これは標準的な HTTP の動作であり、すべての HTTP クライアントでサポートされています。例えば

`curl -L "https://gameads-admin.applifier.com/stats/acquisition-api?apikey=APIKEY"`

はファイルを直接コンソールに出力します

統計サーバーは常に署名済み URL を要求し、有効な署名なしでアクセスされても動作しません。認証に失敗した場合、認証サーバーは HTTP/1.1 200 OK のヘッダで本文に次のようなエラーメッセージを入れて応答します。

```
{"error":"Authentication error","responseCode":500,"status":"error"}
```

---

### リクエストフォーマット

### 収益化統計

パブリッシャーのための収益化統計については[こちら](https://github.com/unity3d-jp/unityads-help-jp/wiki/stats-api)を参照してください。



### ユーザー獲得統計
ユーザー獲得統計 API は、基本的には収益化統計 API と共通で、以下のリクエストフォーマットをサポートします。

```
https://gameads-admin.applifier.com/stats/acquisition-api?apikey=<apikey>&fields=<fields>[&splitBy=<splitbyfields>][&scale=<scale>][&start=<startDate>][&end=<endDate>]([&targetIds=<targetIds>]|[&campaigns=<campaignIds>]
```
このうち

- `<apikey>` は [Unity Ads 管理パネル][1] から取得した API キーです。
- `<fields>` には使用可能なフィールドのカンマ区切りリストが入ります。
 - clicks – 記録されたクリック数
 - installs – 記録されたインストール数
 - spend – これまでに支払った金額

デフォルトのフィールドセットは上記すべてで、"`clicks`,`installs`,`spend`" となります。

- `<splitbyfields>` には、データを分割するディメンションのカンマ区切りリストが入ります。
 - `target` – データはターゲットゲーム別に分割されます
 - `campaign` – データはキャンペーン別に分割されます
 - `country` – データはユーザーの国/地域別に分割されます

デフォルト設定は `country` です。データを一切分割したくない場合は `splitBy=none` と記述します。ターゲットとキャンペーンの両方で同時に分割することもできます。どちらも国での分割あり/なしの両方で使用可能です。

> 注意: 統計データを同時に複数のディメンションで分割すると、データサイズが飛躍的に増加します。その結果、処理時間が長くなりすぎてリクエストが失敗する場合があります。すべてのリクエストは、データ生成にかかる時間が 1 分を超える場合、60 秒で中止されます。

- `<scale>` – データの時間単位1 日の区切りは `UTC 00:00` 時となります。時間単位として指定できる値は以下のとおりです。
 - `all` –（時間単位で区切らず、指定期間内の総計を出力する）
 - `hour`
 - `day`
 - `week`
 - `month`
 - `quarter`
 - `year`

デフォルトは `day` です。

- `<startDate>` & `<endDate>` – データの開始時点と終了時点を指定します。受け付ける日付形式は以下のとおりです。
 -  負の数は現在を基準とした相対日付として扱います。例: `-7` は 1 週間前を指します。
 - 日付文字列は ISO 形式で `YYYY-MM-DDTHH:mm:SS:hhhZ` としてください。 例: `2013-02-01T14:00:00.000Z`

デフォルトでは開始日 `-7`、終了日 `0` で、過去 1 週間のデータを取得します。

> 注意: 開始時点と終了時点が時間単位の区切りと一致しない場合、直前の区切りに修正されます。例えば、時間単位が 1 日の場合、上記の `14:00:00.000Z` は `00:00:00.000Z` に修正されます。

- `<targetIds>` – game id のカンマ区切りリストです。結果をフィルタするのに用います。デフォルトでは、広告主のすべてのゲームが含まれます。
- `<campaigns>` – キャンペーン ID のカンマ区切りリストです。結果をフィルタするのに用います。デフォルトでは、広告主のすべてのキャンペーンが含まれます。
> 注意: 結果はターゲットゲームまたはキャンペーンのどちらかでフィルタできますが、両方同時にフィルタすることはできません。

例:

```
curl -L "https://gameads-admin.applifier.com/stats/acquisition-api?apikey=c4ca4238a0b923820dcc509a6f75849bc81e728d9d4c2f636f067f89cc14862c&splitBy=campaign,country&fields=views,clicks&start=-31&scale=all&targetIds=8234,7432"
```

### 応答フォーマット
#### CSV フォーマットについて
統計サーバーはリクエストされた統計データを HTTP レスポンスの本文に出力します。データは CSV 形式です。フィールド区切りにはカンマ「,」を、小数点区切りには「.」を使用します。文字列フィールドはダブルクォート「”」で囲まれます。純粋な整数フィールドはダブルクォートが付きませんが、小数（収益額や支出額など）はダブルクォートで囲まれます。改行コードには UNIX の改行文字（`0x0D`）を使用します。出力の 1 行目には各フィールド名が含まれます。

#### フィールド
一番左のフィールドは常に `Date` となります。日付は ISO 形式で `YYYY-MM-DD HH:mm:SS` フォーマットです。

次にくるフィールドは分割ディメンション（選択していれば）で、順序は以下のようになります。

- `ソース`で分割されたデータの場合、`Source game id` および `Source game name` の 2 つのフィールドが出力されます。Game id は 1 個の整数です。
- `ターゲット`で分割されたデータの場合、 `Target game id` および `Target name` の 2 つのフィールドが出力されます。
- `キャンペーン`で分割されたデータの場合、 `Target campaign id` および `Target name` の 2 つのフィールドが出力されます。
- `国/地域`で分割されたデータの場合、 `Country code` および `Country tier` の 2 つのフィールドが出力されます。国/地域コードは `ISO 3166-1` alpha-2 の国名コード（大文字）で、ユーザーの GeoIP データに基づいたものです。GeoIP の位置が不明なユーザーの IP には `–`が表示されます。- - Country tier は Applifier の tier 区分における国の分類を示す整数です。

これより右側のフィールドは、リクエストされたデータフィールドがリクエストに記述されたのと同じ順序で並びます。

例:

```
$ curl -L "https://gameads-admin.applifier.com/stats/acquisition-api?apikey=c4ca4238a0b923820dcc509a6f75849bc81e728d9d4c2f636f067f89cc14862c&splitBy=country,campaign"
Date,Target campaign id,Target name,Country code,Country tier,clicks,installs,spend
2013-03-01 00:00:00,"50ed569d57fe1f324a15fbf7","Campaign #5","AU",2,71,30,"45.00"
2013-03-01 00:00:00,"50ed569d57fe1f324a15fbf7","Campaign #5","CA",2,129,88,"132.00"
2013-03-01 00:00:00,"50ed569d57fe1f324a15fbf7","Campaign #5","US",1,1745,855,1282.50
2013-03-01 00:00:00,"50eeb7339e10c9d21c0225cb","Campaign #6","AT",3,39,19,"28.50"
2013-03-01 00:00:00,"50eeb7339e10c9d21c0225cb","Campaign #6","AU",2,16,10,"15.00"
2013-03-01 00:00:00,"50eeb7339e10c9d21c0225cb","Campaign #6","BE",3,209,120,"180.00"
2013-03-01 00:00:00,"50eeb7339e10c9d21c0225cb","Campaign #6","CA",2,284,179,"268.50"
2013-03-01 00:00:00,"50eeb7339e10c9d21c0225cb","Campaign #6","CH",3,15,7,"10.50"
...

```

[1]: https://unityads.unity3d.com/admin
[2]: https://unityads.unity3d.com/admin/#/account/settings
