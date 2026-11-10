---
site: okex
lang: en
name: POST / Amend recurring buy time
method: POST
path: /api/v5/tradingBot/recurring/amend-recurring-time
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Amend recurring buy time
#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP Request

`POST /api/v5/tradingBot/recurring/amend-recurring-time`

> Request Example

```shell
POST /api/v5/tradingBot/recurring/amend-recurring-time
body
{
    "algoId": "2837428373700509696",
    "recurringTimeType": "1",
    "period": "hourly",
    "recurringHour": "8",
    "recurringDay": "1",
    "recurringTime": "11",
    "timeZone": "8"
}
```

#### Request Parameters
| Parameter           | Type   | Required    | Description                                                                                                                                                                                   |
| :------------------ | :----- | :---------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| algoId              | String | Yes         | Algo ID                                                                                                                                                                                       |
| recurringTimeType   | String | Yes         | Recurring buy time type<br>`1`: Custom time<br>`2`: Immediate trigger                                                                                                                        |
| timeZone            | String | Yes         | UTC time zone, the value range is an integer of [-12,14]<br>e.g. `8` representing UTC+8 (East 8 District), Beijing Time                                                                      |
| period              | String | Yes         | Period<br>`monthly`<br>`weekly`<br>`daily`<br>`hourly`                                                                                                                                        |
| recurringHour       | String | Conditional | Recurring buy by hourly<br>`1`/`4`/`8`/`12`, e.g. `1` represents "recurring buy every 1 hour"<br>Required when `period` is `hourly`                                                          |
| recurringDay        | String | Conditional | Recurring buy date<br>When the period is `monthly`, the value range is an integer of [1,28]<br>When the period is `weekly`, the value range is an integer of [1,7]<br>When the period is `daily`/`hourly`, the parameter is not required<br>Only required when `recurringTimeType` is `1` |
| recurringTime       | String | Conditional | Recurring buy time, the value range is an integer of [0,23]<br>Only required when `recurringTimeType` is `1`                                                                                 |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "sCode": "0",
            "sMsg": ""
        }
    ]
}
```

#### Response Parameters
| Parameter | Type | Description |
| :--- | :--- | :--- |
| algoId | String | Algo order ID |
| sCode | String | Event execution status code, `0` indicates success |
| sMsg | String | Error message if the event execution failed |
