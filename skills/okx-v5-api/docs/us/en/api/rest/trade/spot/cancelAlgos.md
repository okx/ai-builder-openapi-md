---
site: us
lang: en
name: POST / Cancel algo order
method: POST
path: /api/v5/trade/cancel-algos
module: trade
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### POST / Cancel algo order

Cancel unfilled algo orders. A maximum of 10 orders can be canceled per request. Request parameters should be passed in the form of an array.

#### Rate Limit: 20 orders per 2 seconds
#### Rate limit rule: User ID + Instrument ID

#### HTTP Request

`POST /api/v5/trade/cancel-algos`

> Request Example

```shell
POST /api/v5/trade/cancel-algos
body
[
    {
        "algoId":"198273485",
        "instId":"BTC-USDT"
    },
    {
        "algoId":"198273485",
        "instId":"BTC-USDT"
    }
]
```

#### Request Parameters

| Parameter | Type   | Required | Description                    |
| :----- | :----- | :------- |:-------------------------------|
| algoId | String | Yes       | Algo ID                        |
| instId | String | Yes       | Instrument ID, e.g. `BTC-USDT` |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "1836489397437468672",
            "clOrdId": "",
            "sCode": "0",
            "sMsg": "",
            "tag": ""
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
|:--------------|:---------|:-----------------------------------------------------------|
| algoId        | String   | Algo ID                                                    |
| sCode         | String   | The code of the event execution result, `0` means success. |
| sMsg          | String   | Rejection message if the request is unsuccessful.          |
| clOrdId       | String   | <del>Client Order ID as assigned by the client</del>(Deprecated)     |
| algoClOrdId     | String   | <del>Client-supplied Algo ID</del>(Deprecated)                                    |
| tag           | String   | <del>Order tag  </del>(Deprecated)                                                                                                                                                                                                                                                                  |
