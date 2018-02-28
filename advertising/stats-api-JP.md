Unity Adsでは、パブリッシャーと広告主が収益化と獲得の統計データを直接CSV形式で取得するためのAPIを提供しています。Unity Ads Statistics APIを使用すると、ゲームやキャンペーンのデータを取得を行いレポートシステムに定期的にロードすることができます。実際の統計APIは、[Unity Ads Admin Panel][1]で利用可能な同じ統計ファイルを自動的に取得するためのマシンツーマシンインタフェースです。

### 概要
統計APIは、2つのステージで動作します。まず、認証サーバーにGETリクエストを行い、認証を成功させます。その後に302 HTTPリダイレクトメッセージでレスポンスを返します。この時の情報には統計サーバーの最終的に確定されたURLが含まれています。

次に、ユーザが署名されたURLへのGETリクエストを実行すると該当するサーバからメッセージ本体のCSV形式のリクエストされたデータで応答します。

```
日付,ターゲットキャンペーンID,ターゲット名,クリック数
2013-02-28 00:00:00,"5065e1f1fdeb285e4d0430ce","Campaign 1",129
2013-02-28 00:00:00,"50ed569d57fe1e324415fbf7","Campaign 2",428
2013-02-28 00:00:00,"50eeb7c39610c9d21c0225cb","Campaign 3",812
2013-02-28 00:00:00,"511e5f7a73452a3363062d5d","Campaign 4",130
...
```

### 認証
Unity Ads Statistics APIを使用するには、[Unity Ads Admin Panel][1]からAPIキーを取得する必要があります。APIキーは、[アカウント設定][2]ページにあります。

APIキーは、認証リクエストからHTTP GETパラメータの`apikey`に置く必要があります。（placed in A to Bがうまく訳せていません）

認証に成功すると、サーバは「Location」HTTPヘッダにある統計サーバに、データURLを持つ302 URLリダイレクトメッセージで応答します。実際のデータはこのリダイレクトURLから取得されます。これは標準のHTTP動作であり、すべてのHTTPクライアントでサポートされています。例えば、`curl -L "https://gameads-admin.applifier.com/stats/acquisition-api?apikey=APIKEY"` はファイルをコンソールに直接出力します。

統計サーバーには署名されたURLが常に必要となります。もし、有効な署名なしでアクセスされた場合は機能しません。認証に失敗すると、認証サーバーはHTTP/1.1 200 OKヘッダーで応答し、本文にエラーメッセージが表示されます。

```
{"error":"Authentication error","responseCode":500,"status":"error"}
```

---

### リクエストフォーマット

### 収益化統計（このAPIを使用して収益化ゲームの統計情報を取得）

収益化統計APIに関するドキュメントはこちらご覧ください:  [Unity Ads statistics API for monetisation](../Documentation for Publishers/Statistics-API-for-monetization)


### 獲得統計（このAPIを使用して広告キャンペーンの統計情報を取得する）

取得統計APIは収益化統計APIと似ており、次のリクエスト形式をサポートしています:

```
https://gameads-admin.applifier.com/stats/acquisition-api?apikey=<apikey>&fields=<fields>[&splitBy=<splitbyfields>][&scale=<scale>][&start=<startDate>][&end=<endDate>]([&targetIds=<targetIds>]|[&campaigns=<campaignIds>]
```
ここで:

- `<apikey>` は [Unity Ads Admin Panel][1] から取得したAPI認証キー
- `<fields>` には、コンマで区切られた使用可能なフィールドのリストが含まれます
 - started - インプレッション数
 - views - ビュー数
 - clicks – クリック数
 - installs – インストール数
 - spend – 費やされた金額

デフォルトのフィールドセットは上記全てです: "`clicks`,`installs`,`spend`"（all of the aboveがどれを指しているのか不明です。←の3つを指している可能性が高いのですが、「上記」と言われると↑の箇条書きの5つの可能性もあります。）

- `<splitbyfields>` には、データを分割する特性のコンマ区切りのリストが含まれます
 - `target` – データはターゲットゲームによって分割されます
 - `campaign` – データはキャンペーンによって分割されます
 - `country` – データはユーザーの国によって分割されます

デフォルトの分割基準は `country`です。 データを分割したくない場合は、`splitBy = none`と書くことで使用できます。ターゲットまたはキャンペーンの分割は同時に許可されます。どちらも、国の分割の有無にかかわらず実行可能です。

> 注: 統計データを複数の特性で同時に分割すると、データサイズが指数関数的に増加します。この増加が爆発的な場合は処理時間が長くなりリクエストが失敗する可能性があります。データを生成するために1分以上かかるすべてのリクエストは60秒で終了します。

- `<scale>` – にはデータの時間分解能が含まれています。 それぞれの日は00:00 UTCで分割されます。スケールに使用できる値は次のとおりです。
 - `all` – は時間分解能を完全に取り除き、定義された期間内の合計値を提供します
 - `hour`
 - `day`
 - `week`
 - `month`
 - `quarter`
 - `year`

デフォルトは `day`です。

- `<startDate>`& `<endDate>`は、データの開始時刻（包含的）と終了時刻（排他的）を含みます。日付は次の形式で受け入れられます：
 - 負の数は、現在の日付に対する日数として扱われます。 例えば： `-7` は1週間前
 - 日付文字列はISOフォーマット `YYYY-MM-DDTHH:mm:SS:hhhZ` です。例えば、 `2013-02-01T14:00:00.000Z`

過去1週間のデータを取得するには、開始日は「-7」で、終了日は「0」です。

> 注: 開始日と終了日は、端数切り上げで、次の1時間に変換されます。例えば, 14:00:05.000Z` は, `15:00:00.000Z`に変換されます。また、`<scale>=day`を使用する場合、午前0時でない` <startDate> `および` <endDate> 'を使用すると、1日あたりではなく1時間あたりの範囲が選択されます。

- `<targetIds>` – 結果をフィルタリングするためにコンマで区切ったターゲットゲームIDのリスト。デフォルトでは、広告主のすべてのゲームが含まれます。
- `<campaigns>` – 結果をフィルタリングするためにコンマで区切ったキャンペーンIDのリスト。デフォルトでは、広告主のすべてのキャンペーンが含まれます。
> 注: 結果は、ターゲットゲームまたはキャンペーンのいずれかでフィルタリングできますが、同時に両方をフィルタリングすることはできません。

例:
```
curl -L "https://gameads-admin.applifier.com/stats/acquisition-api?apikey=c4ca4238a0b923820dcc509a6f75849bc81e728d9d4c2f636f067f89cc14862c&splitBy=campaign,country&fields=views,clicks&start=-31&scale=all&targetIds=8234,7432"
```

### 応答フォーマット
#### 一般的なCSV形式
統計サーバーは、リクエストされた統計データをHTTPレスポンス本体にCSV形式で出力します。フィールド区切り文字としてカンマ ','が使用され、小数点記号として '.'が使用されます。テキストフィールドはダブルクォーテーション（‘”‘）で囲まれます。純粋整数フィールドにはダブルクォーテーションはありませんが、小数（収益や支出など）はダブルクォーテーションで囲みます。unix改行文字（`0x0D`）が行区切り文字として使用されます。出力の最初の行にはフィールド名が含まれます。

#### フィールド
左端のフィールドは常に「Date」です。日付はISOフォーマット `YYYY-MM-DD HH:mm:SS` になります。

次のフィールドは、以下の順で分割する特性（選択されている場合）になります。

- `target` で分割したデータの場合、`"Target game id"` と `"Target name"` の2つのフィールドが表示されます。
- `campaign` で分割したデータの場合、`"Target campaign id" `と `"Target name"` の2つのフィールドが表示されます。
- `country` で分割したデータの場合、`"Country code"` と `"Country tier"` の2つのフィールドが表示されます。国コードは、ユーザーのGeoIPデータに基づいて、大文字の `ISO 3166-1`アルファ2国コードです。GeoIPの場所が不明だったユーザーIPに対しては `"--"` が表示されます。 - - 国の階層は、Applifierの階層構造における国の分類を示す整数です。

右端のフィールドは、リクエストと同じ順序で要求されたデータフィールドです。

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
