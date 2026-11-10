---
site: okex
lang: en
name: POST / Close position for contract grid
method: POST
path: /api/v5/tradingBot/grid/close-position
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Close position for contract grid

Close position when the contract grid stop type is 'keep position'.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/grid/close-position`

> Request Example

```shell
POST /api/v5/tradingBot/grid/close-position
body
{
    "algoId":"448965992920907776",
    "mktClose":true
}
```

#### Request Parameters

| Parameter | Type    | Required   | Description                                                                                                |
|:----------| :------ |:-----------|:-----------------------------------------------------------------------------------------------------------|
| algoId    | String | Yes        | Algo ID                                                                                                    |
| mktClose  | Boolean | Yes        | Market close all the positions or not<br>`true`: Market close all position, `false`: Close part of position |
| sz        | String | Conditional   | Close position amount, with unit of `contract`<br>If `mktClose` is `false`, the parameter is required.     |
| px        | String | Conditional   | Close position price<br>If `mktClose` is `false`, the parameter is required.                               |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "448965992920907776",
            "ordId": "",
            "tag": ""
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                   |
|:--------------| :------- |:----------------------------------------------------------------------------------|
| algoId        | String   | Algo ID                                                                           |
| ordId         | String   | Close position order ID<br>If `mktClose` is `true`, the parameter will return "". |
| algoClOrdId        | String   | Client-supplied Algo ID                                                    |
| tag           | String   | Order tag                                                  |
