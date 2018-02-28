Unity Adsでは、パブリッシャーと広告主が収益化データをCSV形式で直接取得するためのAPIを提供しています。

収益化APIは [Unity Ads管理パネル][1] で入手できるものと同じ統計情報を、パブリッシャーはAPIを使用することでデータを取得できます。

### 概要
統計APIは次の2つの段階で動作します。

1. ユーザーは、認証サーバーに対してGETリクエストを実行します。認証に成功すると、サーバーは302 HTTPリダイレクトメッセージで応答します。このメッセージには、統計サーバーへの最終URLが含まれています。

2. ユーザーが署名されたURLへのGETリクエストを実行すると、サーバーは要求されたデータをCSV形式で返します。

### 認証
APIは [Unity Ads管理パネル][1] のAPIキーを使用します。APIキーは、[アカウント設定][2] ページで確認することが可能です。

必須事項として、APIキーは認証GETリクエストに `apikey` パラメータとして含める必要があります。

After a successful authentication, the server will respond with a 302 HTTP-redirect message with the data URL, located in the `Location` HTTP-header.

認証に成功すると、サーバーはデータURLを持つ302 HTTPリダイレクトメッセージで応答します。データURLは　`Location` HTTPヘッダーに含まれています。

実際のデータは、リダイレクトURLから取得できます。これは、すべてのHTTPクライアントでサポートされている標準のHTTP動作です。例えば `curl -L "https://gameads-admin.applifier.com/stats/monetization-api?apikey=APIKEY"` は、ファイルを直接コンソールに出力します。

有効なURL署名なしでアクセスされた場合、統計サーバは機能しません。認証に失敗すると、認証サーバーは本文にエラーメッセージを入れて、HTTP/1.1 200 OKヘッダーで応答します。

```
{"error":"Authentication error","responseCode":500,"status":"error"}
```

### リクエストフォーマット

##### リクエストフォーマット

収益化統計APIは、次のリクエストフォーマットをサポートしています:

```
https://gameads-admin.applifier.com/stats/monetization-api?apikey=<apikey>&fields=<fields>[&splitBy=<splitbyfields>][&scale=<scale>][&start=<startDate>][&end=<endDate>][&sourceIds=<sourceIds>]
```

例:

```
curl -L "https://gameads-admin.applifier.com/stats/monetization-api?apikey=a0db655ac99b68cb4d1835e878e06473277dd061782dbeec813cb3b14cb723ee&splitBy=zone,country&fields=adrequests,available,views,revenue&start=2016-01-01&end=2016-10-01&scale=day&sourceIds=1003843" > ~/Desktop/UnityAdsMonetization.csv
```

##### 定義:
データを分割は以下のパラメーターを使用することで定義することができます。

> 注釈: 複数の特性でデータを分割すると、CSVが指数関数的に増加します。その結果、一部の大きなデータセットがタイムアウトする可能性があります。
> サーバーがリクエストを処理するのに60秒以上かかる場合、タイムアウトします。

- `<apikey>` - **API Keys** タブの [Unity Ads管理パネル][1] から取得したAPI認証キー。

- `<fields>` - 使用可能なフィールドの列のコンマ区切りのリスト。
  - `adrequests` – キャンペーンリクエスト数（23回目の試行ごとにサンプリングされたもの）
  - `available` – 成功したキャンペーンリクエストの数（23回目の成功ごとにサンプリングされたもの）
  - `started` – 再生を開始した動画の数
  - `views` – 視聴が終了した動画の数
  - `revenue` - 総収入
  - `offers` – （非推奨）
  - `all` - 上記のすべてを含める
  > 空白にした場合、`<fields>` はデフォルトとして `all` になります。

- `<splitbyfields>` - 行展開したコンマ区切りのリスト。以下のフィールドでデータを分割します。
 - `source` – ソースゲームIDで分割します
 - `zone` – ソースゲームのプレースメントIDで分割します
 - `country` – ユーザーの国で分割します
 > 空白にした場合, `<splitbyfields>` はデフォルトとして `country` になります。データを分割したくない場合は `splitBy=none` を使用してください。

- `<scale>` – 時間分解能でデータを分割する値。毎日00:00に分割されます。
 - `hour`
 - `day`
 - `week`
 - `month`
 - `quarter`
 - `year`
 - `all` – 時間分解能での分割をせず、指定された期間内の合計値を返す
 > 空白にした場合, `<scale>` はデフォルトとして `day` になります。

- `<start>` & `<end>` – データセットの開始時刻と終了時刻。
 - 負の数値は、現在の日付との相対的な日数として扱われます。例えば, "`start=-7`" 7日前となります。
 - 日付文字列はISOフォーマットを使用します: `YYYY-MM-DDThh:mm:ss.sssZ`. 例えば: `2016-02-01T14:00:00.000Z`
 - 開始日と終了日は端数を切り上げた数値に変換されます。例えば、`14:00:05.000Z` は `15:00:00.000Z` にのような日付になります。
 - また、`<scale>=day` を使用する場合、午前0時でない `<start>` と `<end>' では、1日あたりではなく1時間あたりの範囲が選択されます。
 > 空のままにした場合、 `<start>`はデフォルトで `-7`、` <end> `はデフォルトで` 0` となり、過去の週が定義されます。

- `<sourceIds>` – 結果をフィルタリングするためのゲームIDのコンマ区切りのリスト。デフォルトでは、開発者のすべてのゲームが含まれます。
 - Example: `sourceIds=1003843`

その他のご質問は、unityads-support@unity3d.com までご連絡ください。

---

[1]:https://dashboard.unityads.unity3d.com
[2]:https://unityads.unity3d.com/admin/#/account/settings
