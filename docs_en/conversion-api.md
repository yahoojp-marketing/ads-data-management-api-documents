# Conversion API

Yahoo! JAPAN Ads Display Ads Conversion API

## Specification

### Server

<https://ads-cv.yahooapis.jp>

### POST /v1

#### Request Header

| Key          | Value                |
|--------------|----------------------|
| `User-Agent`   | `Yahoo AppID: <AppID>` |
| `Content-Type` | `application/json`     |

For `<AppID>`, use Application ID　(Client ID) issued on Yahoo! Developer Network.  
Refer to <https://developer.yahoo.co.jp/start/> for further details.  
Conversion API is available even for users who have selected "ID連携を利用しない (or No ID Linkage)" for "ID連携利用有無 (or ID Linkage options)" when issuing Application ID on Yahoo! Developer Network.  
**DO NOT share your Application ID (Client ID) with others.**

#### Request Body

Request Body must contain parameters with \* symbols.  
Request Body must contain at least one of the parameters with \** symbols.

| Key                             | DataType | Input                                                                                                              | Value (example)                                                                                               |
|---------------------------------|---------|-------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| `hashed_email` **               | string  | Hashed email address using SHA-256. Use lower case alphanumeric characters.                                                                        | `31c5543c1734d25c7206f5fd591525d0295bec6fe84ff82f946a34fe970a1e66`  ( For email address: `example@example.com` before hashed with SHA-256 ) |
| `hashed_phone_number` **        | string  | Hashed phone number without hyphen using SHA-256. Use lower case alphanumeric characters.                                                              | `4e6a6c5c6f8a086ce4babac3247364cc93a8a995a1968105d9b408f7e6b72e51`  ( For phone number: `090xxxxyyyy` before hashed with SHA-256 )         |
| `yclid` **                      | string  | Parameter to identify users who have clicked ads. Use the value of `_ycl_yjad` Cookie.                                                     | `YJAD.1234567890.1EALEzSJBNabcdEFGBE1`                                                                   |
| `yjr_yjad` **                   | string  | Parameter to identify users who have clicked ads. Use the value of `_yjr_yjad` Cookie.                                                          | `1600000000.f0`                                                                                          |
| `event_time` *                  | number  | Date and time conversions occurred. Use 10-digit Unix Time between current time and 90 days before requests.                                                           | `1600000000`                                                                                             |
| `yahoo_ydn_conv_io` *           | string  | Value specific to your account. Copy and paste the value of `yahoo_ydn_conv_io` at "Conversion API リクエストパラメータ (or Conversion API Request Parameter)" on Yahoo! Ads UI.                          | `1EALEzSJBNabcdEFGBE1`                                                                                   |
| `yahoo_ydn_conv_label` *        | string  | Label to identify tags. Copy and paste the value of `yahoo_ydn_conv_label` at "Conversion API リクエストパラメータ (or Conversion API Request Parameter)" on Yahoo! Ads UI.        | `AB0ABCDEFGHIJKLMNOP123456`                                                                              |
| `yahoo_ydn_conv_transaction_id` | string  | A unique id to check for duplicated conversions. In the case there are multiple conversions with the same ID, the ones fired by tags win. Make sure not to exceed 64 bytes and not to have space before and after the value. | `order1234`                                                                                              |
| `yahoo_ydn_conv_value`          | number  | Figures that represent how much one conversion is worth. Use either a value of your choice or the value of `yahoo_ydn_conv_value` at "Conversion API リクエストパラメータ (or Conversion API Request Parameter)" on Yahoo! Ads UI.            | `10`                                                                                                     |

##### Sample Request

API Request for curl

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

#### Response

##### Success

| Response Code | Response Body | note                                                 |
| --- | --- |----------------------------------------------------|
| 201 | N/A | Even with 201 response code, conversions with invalid inputs or duplicated conversions will be excluded from measurement.|

##### Failure

| Response Code | Common causes                                                                                                                                                     |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 400           | Occurs when the request is invalid. Common causes include when the body can not be interpreted as JSON or when inputs are invalid.                                |
| 401           | Occurs when Application ID is not properly set.                                                                                                                   |
| 403           | Occurs when Application ID is invalid.                                                                                                                            |
| 404           | Occurs when the request path is invalid.                                                                                                                          |
| 405           | Occurs when the request method is not POST.                                                                                                                       |
| 415           | Occurs when the input for Content-Type is invalid.                                                                                                                |
| 429           | Occurs when the number of request reaches its designated limit per Application ID. Currently, we put limits on API requests with around 500+ requests per second. |
| 500           | Occurs when internal errors happen. Try calling the api again.                                                                                                    |
| 503           | Occurs when the API is not available due to maintenance and other reasons.                                                                                        |

Some error responses may contain further details. Most will be returned in the following format.

```json
{
  "Error" : {
    "Message" : "<error message>"
  }
}
```


## Contact

Please refer to the following.  
https://ads-promo.yahoo.co.jp/support/contact/