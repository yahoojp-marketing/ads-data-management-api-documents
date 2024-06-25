# Yahoo!広告 ConversionAPIタグ (サーバーサイドGoogleタグマネージャー用)

本ドキュメントでは [サーバーサイドGoogleタグマネージャー](https://developers.google.com/tag-platform/tag-manager/server-side) のコミュニティテンプレートギャラリーから「Yahoo!広告 ConversionAPIタグ」を利用してウェブページコンバージョンを実施する手順を示します。

ConversionAPI の詳細については [こちらのドキュメント](./conversion-api.md) をご参照ください。 

## 準備

### 準備1: サーバーサイドGoogleタグマネージャー

[Googleタグマネージャー](https://developers.google.com/tag-platform/tag-manager) はGoogle社が提供するタグ管理システムです。  
[サーバーサイドGoogleタグマネージャー](https://developers.google.com/tag-platform/tag-manager/server-side) ではタグの処理が Google Cloud 等のプラットフォーム上のコンテナで実行されます。

Yahoo!広告 ConversionAPIタグ では サーバーサイドGoogleタグマネージャー のコンテナに送信されたコンバージョンデータを収集しConversionAPIにリクエストすることでYahoo!広告へのコンバージョン測定を実施します。

以下のセットアップを実施してください。

- [こちら](https://developers.google.com/tag-platform/tag-manager/server-side/app-engine-setup) の「Set up server-side tagging」セクションなどをご確認の上、サーバーサイドGoogleタグマネージャーのセットアップをお願いします。
- [こちら](https://developers.google.com/tag-platform/tag-manager/server-side/send-data) を参考に Google アナリティクス4(GA4)クライアントの設定、およびWebページへのタグ設置等のセットアップをお願いします。

### 準備2: Conversion API

「Yahoo!広告 ConversionAPIタグ」ではConversion APIを利用します。
[Yahoo!広告ヘルプ](https://ads-help.yahoo.co.jp/yahooads/display/articledetail?lan=ja&aid=119783) および [Conversion APIのドキュメント](./conversion-api.md) をご確認の上、以下をご用意ください。
- Client ID
- ConversionAPI用のリクエストパラメータ

## セットアップ: Yahoo!広告 ConversionAPIタグ

### セットアップ1: 変数の設定

本セクションでは「Yahoo!広告 ConversionAPIタグ」の入力値として設定するための変数を定義します。    
変数については [こちら](https://support.google.com/tagmanager/topic/7683268) をご確認ください。

一例として「クエリパラメータ `buy_time` の値を変数 `cv_event_time` として登録する」例を示します。

1. サーバーサイドGoogleタグマネージャーでタグを設定するコンテナのワークスペースを開きます。
2. サイドメニューの「変数」を選択し、「ユーザー定義変数」の「新規」ボタンから変数の新規登録画面を開きます。
3. 「変数の設定」から変数タイプ「クエリパラメータ」を選択します。
4. 「パラメータ名」に `buy_time` を入力します。
5. 「保存」ボタンを選択し、変数名の変更で「変数名」に `cv_event_time` を入力して保存します。

### セットアップ2: タグの設定

本セクションでは「Yahoo!広告 ConversionAPIタグ」のパラメータと配信トリガーを設定します。

1. サーバーサイドGoogleタグマネージャーでタグを設定するコンテナのワークスペースを開きます。
2. サイドメニューの「タグ」を選択し、「新規」ボタンからタグの新規追加画面を開きます。
3. 「タグの設定」を選択し、「コミュニティ テンプレート ギャラリーでタグタイプをさらに見つけましょう」を選択します。
4. 一覧から「Yahoo!広告 ConversionAPIタグ」を選択します。
5. ConversionAPIにリクエストする際の各種パラメータを固定値および変数で設定します。
   - 「アプリケーションID(ClientID)」 は「準備2」で取得した Client ID を固定値で入力してください。
   - その他のパラメータは必要に応じて固定値および変数で設定してください。
6. 「トリガー」でタグが実行される条件を選択してください。
7. 「保存」ボタンを選択してタグを保存してください。

### セットアップ3: 動作確認

ワークスペースの「プレビュー」ボタンからサーバーサイドGoogleタグマネージャーのプレビュー機能を用いて動作確認を実施してください。  
プレビューについては [こちら](https://developers.google.com/tag-platform/tag-manager/server-side/debug) をご確認ください。  
以下の点をご確認いただくとスムーズに動作確認が実施可能です。

1. Tagsタブの「Tags Fired」に「Yahoo!広告 ConversionAPIタグ」を利用したタグが存在する
2. 上記を選択すると表示される「Tag Details」の「Outgoing HTTP Requests from Server」を選択し、以下を確認する
   - 想定したリクエストがConversionAPIに送信されている
   - レスポンスコードが201になっている
3. Consoleタブのログに `success: {.....}` のログが出力されている

## Yahoo!広告 ConversionAPIタグ の動作について

タグ発火時、ConversionAPIへのリクエストは「セットアップ2」で入力した値が反映されます。  
一部のパラメータではセットアップ2の入力値がない場合や無効な値の場合は、以下の表「予備入力値」の取得を試行します。

| パラメータ名                        | 入力値    | 予備入力値                                 |
|-------------------------------|--------|---------------------------------------|
| アプリケーションID (ClientID)         | タグ設定の値 |                                       |
| hashed_email                  | タグ設定の値 | イベントデータの `user_data.email_address` の値 |
| hashed_phone_number           | タグ設定の値 | イベントデータの `user_data.phone_number` の値  |
| yclid                         | タグ設定の値 | Cookieの `_ycl_yjad` の値                |
| yjr_yjad                      | タグ設定の値 | Cookieの `_yjr_yjad` の値                |
| ifa                           | タグ設定の値 |                                       |
| event_time                    | タグ設定の値 | 現在時刻                                  |
| yahoo_ydn_conv_io             | タグ設定の値 |                                       |
| yahoo_ydn_conv_label          | タグ設定の値 |                                       |
| yahoo_ydn_conv_transaction_id | タグ設定の値 |                                       |
| yahoo_ydn_conv_value          | タグ設定の値 |                                       |
| ip                            | タグ設定の値 | イベントデータの `ip_override` の値             |
| user_agent                    | タグ設定の値 | イベントデータの `user_agent` の値              |
| yjsu_yjad                     | タグ設定の値 | Cookieの `_yjsu_yjad` の値               |
| url                           | タグ設定の値 | イベントデータの `page_location` の値           |
| referrer                      | タグ設定の値 | イベントデータの `page_referrer` の値           |

メールアドレスおよび電話番号の予備入力値である、イベントデータの `user_data.email_address` と `user_data.phone_number` の値の取得は [こちら](https://developers.google.com/tag-platform/tag-manager/server-side/send-data) の Google tag において `first_party_collection` フラグを `true` に設定する必要があります。
イベントデータについては [こちら](https://developers.google.com/tag-platform/tag-manager/server-side/common-event-data) もご確認ください。
Cookieの `_ycl_yjad`, `_yjr_yjad`, `_yjsu_yjad` の値の取得については [カスタムドメインの設定](https://developers.google.com/tag-platform/tag-manager/server-side/custom-domain) が必要となります。

hashed_email および hashed_phone_number の値については以下の通りハッシュ化が実行されます。

| パラメータ名                        | 補足                                                                                                          |
|-------------------------------|-------------------------------------------------------------------------------------------------------------|
| hashed_email                  | SHA-256でハッシュ化されていないメールアドレスはハッシュ化されます。 メールアドレス形式ではない場合やSHA-256の値ではない場合は無視されます。                               |
| hashed_phone_number           | 電話番号に `-` が含まれる場合は削除、`+81` が含まれる場合は `0` に置換し、数値のみになっている場合はハッシュ化されます。前述の条件に当てはまらない場合やSHA-256の値ではない場合は無視されます。 |

ConversionAPIタグ への入力値を元に ConversionAPI にリクエストを実施し、レスポンスコードが201の場合はタグ実行が成功となります。  
ConversionAPI の詳細については [こちらのドキュメント](./conversion-api.md) をご参照ください。

## 注意点

### ConversionAPI と同様の制限を受けます

本タグは ConversionAPI を経由してリクエストを送信しているため、リクエスト数制限等は ConversionAPI に依存いたします。  
ConversionAPI の詳細については [こちらのドキュメント](./conversion-api.md) をご参照ください。

### ConversionAPI への入力値を Google アナリティクス に送信しないでください

[Google アナリティクス利用規約](https://marketingplatform.google.com/about/analytics/terms/jp/) において、個人情報として使用または認識できる情報を送信することを禁止しています。
そのため、ConversionAPI への入力値は **絶対に** Google アナリティクス に送信しないでください。

特に「Google アナリティクス: GA4」タグを併用している場合、デフォルト設定では全イベントパラメータやユーザプロパティが Google アナリティクス に送信されます。  
必ず除外設定やトリガーの適切な設定によってConversionAPI への入力値を Google アナリティクス に送信されないようにしてください。

### アプリケーションID (Client ID) は第三者に開示しないでください

アプリケーションID (Client ID) は第三者に開示しないようにしてください。  
意図せず開示してしまうことを防ぐために、タグの設定時に固定値での入力をお願いいたします。


## お問い合わせ

お問い合わせについてはこちらをご参照下さい。  
https://www.lycbiz.com/jp/contact/support/yahoo-ads/
