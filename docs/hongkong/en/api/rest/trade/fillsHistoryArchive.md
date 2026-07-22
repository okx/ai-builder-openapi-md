---
site: hongkong
lang: en
name: POST / Transaction details (in the past 2 years)
method: POST
path: /api/v5/trade/fills-archive
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Transaction details (in the past 2 years)

Apply for recently-filled transaction details in the past 2 years except for last 3 months.

#### Rate Limit: 10 requests per day
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/trade/fills-archive`

> Request Example

```shell
POST /api/v5/trade/fills-archive
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

| **Parameter** | **Type** | **Description**                                                                                                                                                                                              |
| :------------ | :------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| result     | String | Whether there is already a download link for this section <br>`true`: Existed, can check from "Get download link". <br>`false`: Does not exist and is generating, can check the download link after 30 hours |
| ts    | String | Download link generation time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                   |

<aside class="notice">
Check the file link from the "Transaction details (in the past 2 years)" endpoint in 30 hours to allow for data generation. <br>
During peak demand, data generation may take longer. If the file link is still unavailable after 48 hours, reach out to customer support for assistance.
</aside>
