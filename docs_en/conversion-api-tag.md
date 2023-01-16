# Yahoo! JAPAN Ads Display Ads Conversion API Tag (for Server-side Google Tag Manager)

The documentation shows the procedure to trigger web page conversions using Yahoo! JAPAN Ads Display Ads Conversion API Tag on the community template gallery in [Server-side Google Tag Manager](https://developers.google.com/tag-platform/tag-manager/server-side).

Please refer to the [API Reference](./conversion-api.md) for further details. 

## Preparations

### Preparation 1: Server-side Google Tag Manager

[Google Tag Manager](https://developers.google.com/tag-platform/tag-manager) is a tag management system offered by Google. 
[Server-side Google Tag Manager](https://developers.google.com/tag-platform/tag-manager/server-side) allows tags to be processed in containers on platforms such as Google Cloud.

Yahoo! JAPAN Ads Display Ads Conversion API Tag (Yahoo!広告 ConversionAPIタグ) enables conversion measurements for Yahoo! JAPAN Ads Display Ads by collecting conversion data sent to containers on Server-side Google Tag Manager and requesting the data to Conversion API.

Please complete the following configurations.

- Please complete your Server-side Google Tag Manager configurations by referring to sections such as "Set up server-side tagging" in the [documentation](https://developers.google.com/tag-platform/tag-manager/server-side/app-engine-configuration).
- Please complete configurations for Google analytics 4 (GA4), tag configurations on web pages, and other necessary configurations by referring to the [documentation](https://developers.google.com/tag-platform/tag-manager/server-side/send-data).

### Preparation 2: Conversion API

"Yahoo! JAPAN Ads Display Ads Conversion API Tag" uses Conversion API.
Please refer to [Yahoo! JAPAN Ads Help](https://ads-help.yahoo.co.jp/yahooads/display/articledetail?lan=en&aid=64946) and the [API Reference](./conversion-api.md), and prepare the following items.
- Client ID
- Request parameters for Conversion API

## Configurations: Yahoo! JAPAN Ads Display Ads Conversion API Tag

### Configuration 1: Variables

This section shows how to define variables for input values for Yahoo! JAPAN Ads Display Ads Conversion API Tag.   
Please refer to the [documentation](https://support.google.com/tagmanager/topic/7683268) for Variables configuration.

The following is the example for registering the value of quary parameter `buy_time` as the variable `cv_event_time`.

1. Open workspace of the containers for setting tags on Server-Side Google Tag Manager.
2. Select "Variables" on the side menu and open the configuration for new variables by clicking "New" in "User-Defined Variables".
3. Select variable type "Query Parameters" in "Variable Configuration".
4. Input `buy_time` to "Parameter Name".
5. Select "Save" and input `cv_event_time` in "Variable Name" in the "Change Variable" section before saving.

### Configuration 2: Tags

This section covers how to set parameters and firing triggers for Yahoo! JAPAN Ads Display Ads Conversion API Tag.

1. Open workspace of the containers for setting tags on Server-Side Google Tag Manager.
2. Select "Tags" on the side menu and open the configuration for new tags by clicking "New".
3. Select "Tag Configuration" and then click "Discover more tag types in the Community Template Gallery".
4. Choose "Yahoo!広告 ConversionAPIタグ" on the list.
5. Set each parameter in fixed values or variables for requesting Conversion API.
   - Input Client ID from "Preparation 2" as a fixed value for "Application ID (Client ID)".
   - Set other parameters in fixed values or variables as needed.
6. Use "Triggers" to select conditions to fire tags.
7. Select "Save" to save tag configurations.

### Configuration 3: Testing

Select "Preview" button to test tags using the preview feature on Server-side Google Tag Manager. 
To make the testing process easier, check the following.

1. Make sure the tag configurations for "Yahoo!広告 ConversionAPIタグ" exists in "Tags Fired".
2. Check the following after selecting "Outgoing HTTP Requests from Server" in "Tag Details" shown when "Tags Fired" is selected.
   - Requests with expected parameters have been sent to Conversion API.
   - Response code is 201.
3. Logs in the console tab contain `success: {.....}`.

## About how Yahoo! JAPAN Ads Display Ads Conversion API Tag works
 
When fired, requests to Conversion API are based on the values set in Configuration 2.  
For some parameters, the system tries to retrieve values from "input(secondary)" shown in the chart below in case that no value is set or the value is invalid.

| Parameter Name                        | input(primary)   | input(secondary)                               |
|-------------------------------|-------|------------------------------------|
| Application ID (Client ID)         | Values set in Tags  |                                    |
| hashed_email                  | Values set in Tags  | Values of `user_data.email_address` event |
| hashed_phone_number           | Values set in Tags  | Values of `user_data.phone_number` event |
| yclid                         | Values set in Tags | Values of Cookie `_ycl_yjad`             |
| yjr_yjad                      | Values set in Tags | Values of Cookie `_yjr_yjad`             |
| event_time                    | Values set in Tags | Current Time                               |
| yahoo_ydn_conv_io             | Values set in Tags |                                    |
| yahoo_ydn_conv_label          | Values set in Tags |                                    |
| yahoo_ydn_conv_transaction_id | Values set in Tags |                                    |
| yahoo_ydn_conv_value          | Values set in Tags |                                    |

To use the event data of `user_data.email_address` and `user_data.phone_number`, `first_party_collection` flag needs to be set to `true` on Google tag as described [here](https://developers.google.com/tag-platform/tag-manager/server-side/send-data).  
[Custom domain configuration](https://developers.google.com/tag-platform/tag-manager/server-side/custom-domain) is required to retrieve the values of Cookie `_ycl_yjad` and `yjr_yjad`.

The values of hashed_email and hashed_phone_number will be hashed as described below.

| Parameter Name                       | Note                                                                                                         |
|-------------------------------|-------------------------------------------------------------------------------------------------------------|
| hashed_email                  | Plain email addresses will be hashed using SHA-256. Invalid email addresses and hashed values using other than SHA-256 will be ignored.                           |
| hashed_phone_number           | After removing `-` and replacing `+81` with `0`, phone numbers will be hashed only if they are made up of numbers. The values will be ignored if the previous conditions are not met or the values are not hashed with SHA-256. |

Requests will be made based on the input values for Conversion API Tag. The response code 201 means the tag fires are successful.  
Please refer to the [API Refecrence](./conversion-api.md) for further details of Conversion API.

## Warnings

### DO NOT send the values of Conversion API to Google Analytics.

[Google Analytics Terms of Service](https://marketingplatform.google.com/about/analytics/terms/us/) does not allow information that can be used and identified as personal information to be sent.
Therefore, **NEVER** send the values of Conversion API to Google Analytics.
  
Especially when using "Google Analytics GA4" tags concurrently, all event parameters and user properties will be sent to Google Analytics by default. 
By using the proper configurations of firing triggers and trigger exceptions, make sure not to send the values of Conversion API to Google Analytics.

### DO NOT share Application ID (Client ID) with others.
  
DO NOT share Application ID (Client ID) with others.  
Please use fixed values when setting tags to prevent information from being shared unintentionally.
