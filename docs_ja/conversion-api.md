# Conversion API

Yahoo!広告 ディスプレイ広告 コンバージョン計測API

## 仕様

### サーバ

<https://ads-cv.yahooapis.jp>

### POST /v1

#### リクエストヘッダ

| Key          | Value                |
|--------------|----------------------|
| `User-Agent`   | `Yahoo AppID: <AppID>` |
| `Content-Type` | `application/json`     |

`<AppID>` にはYahoo!デベロッパーネットワークで取得したアプリケーションID(Client ID)を指定してください。  
詳細は <https://developer.yahoo.co.jp/start/> を御覧ください。  
Conversion API ではアプリケーションID発行時の「ID連携利用有無」は「ID連携を利用しない」で利用可能です。  
**アプリケーションID(Client ID)は第三者に開示しないようにしてください。**

#### リクエストボディ

\* の記載があるパラメータは必須です。  
\** の記載があるパラメータは最低1つは必須です。

| Key                             | DataType | 入力値                                                                                                               | Value 例                                                                                                  |
|---------------------------------|---------|-------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| `hashed_email` **               | string  | SHA-256でハッシュ化されたEメール。半角小文字英数字で入力してください。                                                                           | `31c5543c1734d25c7206f5fd591525d0295bec6fe84ff82f946a34fe970a1e66`  ( Eメールが `example@example.com` の場合。 ) |
| `hashed_phone_number` **        | string  | SHA-256でハッシュ化されたハイフン無し半角数字の電話番号。半角小文字英数字で入力してください。                                                                | `4e6a6c5c6f8a086ce4babac3247364cc93a8a995a1968105d9b408f7e6b72e51`  ( 電話番号が `090xxxxyyyy` の場合。 )         |
| `yclid` **                      | string  | 広告をクリックしたユーザーを識別するパラメータ。`_ycl_yjad` Cookie の値を入力してください。                                                           | `YJAD.1234567890.1EALEzSJBNabcdEFGBE1`                                                                   |
| `yjr_yjad` **                   | string  | 広告をクリックしたユーザーを識別するパラメータ。`_yjr_yjad` Cookie の値を入力してください。                                                           | `1600000000.f0`                                                                                          |
| `event_time` *                  | number  | コンバージョンの発生日時。リクエスト日時より90日前〜現在時刻の間の10桁UNIX時間で入力してください。                                                             | `1600000000`                                                                                             |
| `yahoo_ydn_conv_io` *           | string  | お客様のアカウント専用の値。広告管理ツールで取得した「Conversion API リクエストパラメータ」の `yahoo_ydn_conv_io` の値をそのまま入力してください。                       | `1EALEzSJBNabcdEFGBE1`                                                                                   |
| `yahoo_ydn_conv_label` *        | string  | コンバージョンタグにおいてタグを識別するための項目。広告管理ツールで取得した「Conversion API リクエストパラメータ」の `yahoo_ydn_conv_label` の値をそのまま入力してください。        | `AB0ABCDEFGHIJKLMNOP123456`                                                                              |
| `yahoo_ydn_conv_transaction_id` | string  | コンバージョンの重複判定をするためのユニークなID。タグ計測において同じIDが入力されたコンバージョンが存在する場合、タグ計測側が優先されます。64byteを超えない範囲で、かつ前後にスペースが入らないように入力してください。 | `order1234`                                                                                              |
| `yahoo_ydn_conv_value`          | number  | 1コンバージョンあたりの価値。広告管理ツールで取得した「Conversion API リクエストパラメータ」の `yahoo_ydn_conv_value` の値、もしくは任意の値を入力してください。              | `10`                                                                                                     |

##### リクエストサンプル

curl を用いてリクエストする場合

```terminal
$ curl \
-H "User-Agent:Yahoo AppID: YAoCapisFYtPWI6Yiy6g4GGK7BA1AnlLF9cSqRKqhew5ksObrS5tjk9-" \
-H "Content-Type: application/json" \
-X POST \
-d '{
    "hashed_email": "31c5543c1734d25c7206f5fd591525d0295bec6fe84ff82f946a34fe970a1e66",
    "hashed_phone_number": "4e6a6c5c6f8a086ce4babac3247364cc93a8a995a1968105d9b408f7e6b72e51",
    "yclid": "YJAD.1234567890.1EALEzSJBNabcdEFGBE1",
    "yjr_yjad": "1600000000.f0",
    "event_time": 1600000000,
    "yahoo_ydn_conv_io": "1EALEzSJBNabcdEFGBE1",
    "yahoo_ydn_conv_label": "AB0ABCDEFGHIJKLMNOP123456",
    "yahoo_ydn_conv_transaction_id": "order1234",
    "yahoo_ydn_conv_value": 10
}' \
https://ads-cv.yahooapis.jp/v1/
```

#### レスポンス

##### 成功

| レスポンスコード | レスポンスボディ | 補足                                                 |
| --- | --- |----------------------------------------------------|
| 201 | (無し) | 本レスポンスの場合でも、不正な値の場合や重複判定となった場合はコンバージョンの計測対象外になります。 |

##### 失敗

| レスポンスコード | 主な原因                                                                         |
|----------|------------------------------------------------------------------------------|
| 400      | リクエストが不正な場合に発生します。特にリクエストボディがJSONとして解釈できない場合や入力値が不正な場合が考えられます。               |
| 401      | アプリケーションIDが正しく付与されていない場合に発生します。                                              |
| 403      | アプリケーションIDごとに設定されているアクセス回数上限に達した場合に発生します。 現時点でリクエスト制限を実施する目安は 500リクエスト/秒 です。 |
| 404      | リクエストパスが不正な場合に発生します。                                                         |
| 405      | POST 以外のメソッドでリクエストした場合に発生します。                                                |
| 415      | Content-Type の指定が不正な場合に発生します。                                                |
| 500      | 内部エラーが起きた場合に発生します。再度操作を実行してください。                                             |
| 503      | APIがメンテナンス中などの理由により利用できない状態です。                                               |

エラーレスポンスのボディには詳細な理由を含む場合があります。一部を除き以下の形式でレスポンスします。

```json
{
  "Error" : {
    "Message" : "<error message>"
  }
}
```
