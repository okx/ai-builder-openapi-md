---
site: okex
lang: en
name: POST / Set instruments
method: POST
path: /api/v5/tradingBot/signal/set-instruments
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / Set instruments

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/signal/set-instruments`

> Request Example

```shell
POST /api/v5/tradingBot/signal/set-instruments
body
{
    "algoId": "637039348240277504",
    "instIds": [
        "SHIB-USDT-SWAP",
        "ETH-USDT-SWAP"
    ]
}
```

#### Request Parameters

| Parameter  | Type    | Required | Description                                                                                                               |
| :--------- | :------ | :------- | :------------------------------------------------------------------------------------------------------------------------ |
| algoId            | String    | Yes       | Algo ID                                                            |
| instIds           | Array of strings     | Yes       | Instrument IDs. When `includeAll` is `true`, it is ignored         |
| includeAll        | Boolean   | Yes       | Whether to include all USDT-margined contract.The default value is `false`. `true`: include `false` : exclude   |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoId": "637039348240277504"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                            |
|:--------------| :------- | :--------------------------------------------------------- |
| algoId        | String   | Algo ID                        |
