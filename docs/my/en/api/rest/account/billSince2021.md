---
site: my
lang: en
name: Apply bills details (since 2021)
method: POST
path: /api/v5/account/bills-history-archive
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### Apply bills details (since 2021) 

Apply for bill data since 1 February, 2021 except for the current quarter.

#### Rate Limit: 1 request per 10 seconds
#### Rate limit rule: User ID
#### HTTP Request

`POST /api/v5/account/bills-history-archive`

> Request Example

```shell
POST /api/v5/account/bills-history-archive
body
{
    "year":"2023",
    "quarter":"Q1"
}
```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                         |
|:-----------| :----- | :------- | :----------------------------------------------------------- |
| year   | String | Yes       | 4 digits year  |
| quarter| String | Yes       | Quarter, valid value is `Q1`, `Q2`, `Q3`, `Q4`      |
| type | String | No | Bill type. Multiple values are supported, separated by commas, e.g. `1,2,3`. If not specified, all types are returned.<br>Please refer to [Get bill types](/docs-v5/en/#trading-account-rest-api-get-bill-types) for the list of available types. |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "result": "true",
            "ts": "1646892328000"
        }
    ],
    "msg": ""
}
```
#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                                                                                                                                                                                                                 |
| :------------ | :------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| result     | String | Whether there is already a download link for this section <br>`true`: Existed, can check from "Get bills details (since 2021)". <br>`false`: Does not exist and is generating, can check the download link after 2 hours<br> The data of file is in reverse chronological order using `billId`. |
| ts    | String | The first request time when the server receives. Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                    |

<aside class="notice">

The rule introduction, only applicable to the file generated after 11 October, 2024<br>
1. Taking 2024 Q2 as an example. The date range are [2024-07-01, 2024-10-01). The begin date is included, The end date is excluded.<br>
2. The data of file is in reverse chronological order using `billId`
</aside>

<aside class="notice">
Check the file link from the "Get bills details (since 2021)" endpoint in 2 hours to allow for data generation. <br>
During peak demand, data generation may take longer. If the file link is still unavailable after 3 hours, reach out to customer support for assistance.
</aside>

<aside class="notice">
It is only applicable to the data from the unified account.
</aside>
