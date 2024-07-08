# Conversion API

Yahoo! JAPAN Ads Display Ads Conversion API

In case there are any discrepancies between the Japanese and English references, the Japanese version shall be considered correct.

## Specification

### Server

<https://ads-cv.yahooapis.jp>

### Request Limit

500 requests / sec

Requests exceeding the threshold will be ignored and will result in error responses.

### POST /v1

#### Request Header

| Key            | Value                  |
|----------------|------------------------|
| `User-Agent`   | `Yahoo AppID: <AppID>` |
| `Content-Type` | `application/json`     |

For `<AppID>`, use Application ID　(Client ID) issued on Yahoo! Developer Network.  
Refer to <https://developer.yahoo.co.jp/start/> for further details.  
Conversion API is available even for users who have selected "ID連携を利用しない (or No ID Linkage)" for "ID連携利用有無 (or ID Linkage options)" when issuing Application ID on Yahoo! Developer Network.  
**DO NOT share your Application ID (Client ID) with others.**

#### Request Body

Request Body must contain parameters with \* symbols.  
Request Body must contain at least one of the parameters with \** symbols.

| Key                             | DataType | Input                                                                                                                                                                                                                                                                                                                 | Value (example)                                                                                                                             |
|---------------------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| `hashed_email` **               | string   | Hashed email address using SHA-256. Use lower case alphanumeric characters.                                                                                                                                                                                                                                           | `31c5543c1734d25c7206f5fd591525d0295bec6fe84ff82f946a34fe970a1e66`  ( For email address: `example@example.com` before hashed with SHA-256 ) |
| `hashed_phone_number` **        | string   | Hashed phone number without hyphen using SHA-256. Use lower case alphanumeric characters.                                                                                                                                                                                                                             | `4e6a6c5c6f8a086ce4babac3247364cc93a8a995a1968105d9b408f7e6b72e51`  ( For phone number: `090xxxxyyyy` before hashed with SHA-256 )          |
| `yclid` **                      | string   | Parameter to identify users who have clicked ads. Use the value of `_ycl_yjad` Cookie or `yclid` URL parameter.                                                                                                                                                                                                       | `YJAD.1234567890.1EALEzSJBNabcdEFGBE1`                                                                                                      |
| `yjr_yjad` **                   | string   | Parameter to identify users who have clicked ads. Use the value of `_yjr_yjad` Cookie or `yj_r` URL parameter.                                                                                                                                                                                                        | `1600000000.f0`                                                                                                                             |
| `ifa` **                        | string   | Value of IDFA or AAID. Please input in uppercase for IDFA, and in lowercase for AAID.                                                                                                                                                                                                                                 | `ABCDEF00-ABCD-4DEF-1234-1234567890AB`                                                                                                      |
| `event_time` *                  | number   | Date and time conversions occurred. Use 10-digit Unix Time between current time and 90 days before requests. A 13-digit UNIX timestamp, when entered, will have its last three digits ignored and be treated as a 10-digit UNIX timestamp. Requests near the current time might be subject to adjustments by the API. | `1600000000`                                                                                                                                |
| `yahoo_ydn_conv_io` *           | string   | Value specific to your account. Copy and paste the value of `yahoo_ydn_conv_io` at "Conversion API リクエストパラメータ (or Conversion API Request Parameter)" on Yahoo! JAPAN Ads UI.                                                                                                                                          | `1EALEzSJBNabcdEFGBE1`                                                                                                                      |
| `yahoo_ydn_conv_label` *        | string   | Label to identify tags. Copy and paste the value of `yahoo_ydn_conv_label` at "Conversion API リクエストパラメータ (or Conversion API Request Parameter)" on Yahoo! JAPAN Ads UI. This parameter is only valid when the "Conversion type" in the "Conversion setting" is set to "Web".                                          | `AB0ABCDEFGHIJKLMNOP123456`                                                                                                                 |
| `yahoo_ydn_conv_transaction_id` | string   | A unique id to check for duplicated conversions. In the case there are multiple conversions with the same ID, the ones fired by tags win. Make sure not to exceed 64 bytes and not to have space before and after the value.                                                                                          | `order1234`                                                                                                                                 |
| `yahoo_ydn_conv_value`          | number   | Figures that represent how much one conversion is worth. Use either a value of your choice or the value of `yahoo_ydn_conv_value` at "Conversion API リクエストパラメータ (or Conversion API Request Parameter)" on Yahoo! JAPAN Ads UI.                                                                                        | `10`                                                                                                                                        |
| `ip`                            | string   | The IP address of the user who triggered the conversion.                                                                                                                                                                                                                                                              | `203.0.113.10`                                                                                                                              |
| `user_agent`                    | string   | The user agent of the user who triggered the conversion.                                                                                                                                                                                                                                                              | `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/500.00 (KHTML, like Gecko) Chrome/50.0.0000.000 Safari/500.0`                        |
| `yjsu_yjad`                     | string   | Unique identifier within the website domain. Use the value of `_yjsu_yjad` Cookie.                                                                                                                                                                                                                                    | `1234567890.12345678-abcd-4bcd-1234-123456789012`                                                                                           |
| `url`                           | string   | The URL where the conversion occurred.                                                                                                                                                                                                                                                                                | `https://example.com/registered/?utm_source=yahoo&utm_medium=display`                                                                       |
| `referrer`                      | string   | The referrer where the conversion occurred.                                                                                                                                                                                                                                                                           | `https://example.com/ad/?utm_source=yahoo&utm_medium=display`                                                                               |

##### Sample Request

API Request for curl

```shell
$ curl \
-H "User-Agent:Yahoo AppID: YAoCapisFYtPWI6Yiy6g4GGK7BA1AnlLF9cSqRKqhew5ksObrS5tjk9-" \
-H "Content-Type: application/json" \
-X POST \
-d '{
    "hashed_email": "31c5543c1734d25c7206f5fd591525d0295bec6fe84ff82f946a34fe970a1e66",
    "hashed_phone_number": "4e6a6c5c6f8a086ce4babac3247364cc93a8a995a1968105d9b408f7e6b72e51",
    "yclid": "YJAD.1234567890.1EALEzSJBNabcdEFGBE1",
    "yjr_yjad": "1600000000.f0",
    "ifa": "ABCDEF00-ABCD-4DEF-1234-1234567890AB",
    "event_time": 1600000000,
    "yahoo_ydn_conv_io": "1EALEzSJBNabcdEFGBE1",
    "yahoo_ydn_conv_label": "AB0ABCDEFGHIJKLMNOP123456",
    "yahoo_ydn_conv_transaction_id": "order1234",
    "yahoo_ydn_conv_value": 10,
    "ip": "203.0.113.10",
    "user_agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/500.00 (KHTML, like Gecko) Chrome/50.0.0000.000 Safari/500.0",
    "yjsu_yjad": "1234567890.12345678-abcd-4bcd-1234-123456789012",
    "url": "https://example.com/registered/?utm_source=yahoo&utm_medium=display",
    "referrer": "https://example.com/ad/?utm_source=yahoo&utm_medium=display"
}' \
https://ads-cv.yahooapis.jp/v1/
```

#### Response

##### Success

| Response Code  | Response Body  | note                                                                                                                      |
|----------------|----------------|---------------------------------------------------------------------------------------------------------------------------|
| 201            | N/A            | Even with 201 response code, conversions with invalid inputs or duplicated conversions will be excluded from measurement. |

##### Failure

| Response Code | Common causes                                                                                                                      |
|---------------|------------------------------------------------------------------------------------------------------------------------------------|
| 400           | Occurs when the request is invalid. Common causes include when the body can not be interpreted as JSON or when inputs are invalid. |
| 401           | Occurs when Application ID is not properly set.                                                                                    |
| 403           | Occurs when Application ID is invalid.                                                                                             |
| 404           | Occurs when the request path is invalid.                                                                                           |
| 405           | Occurs when the request method is not POST.                                                                                        |
| 415           | Occurs when the input for Content-Type is invalid.                                                                                 |
| 429           | Occurs when the number of request reaches its designated limit per Application ID.                                                 |
| 500           | Occurs when internal errors happen. Try calling the api again.                                                                     |
| 503           | Occurs when the API is not available due to maintenance and other reasons.                                                         |

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
https://global-marketing.yahoo.co.jp/contact/