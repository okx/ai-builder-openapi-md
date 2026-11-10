---
site: okex
lang: en
name: POST / Amend grid algo order basic param
method: POST
path: /api/v5/tradingBot/grid/amend-algo-basic-param
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Amend grid algo order basic param

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/tradingBot/grid/amend-algo-basic-param`

> Request Example

```shell
POST /api/v5/tradingBot/grid/amend-algo-basic-param
body
    {
      "algoId": "448965992920907776",
      "maxPx": "100",
      "minPx": "10",
      "gridNum": "5"
      "topupAmount": "123.45"
    }
```

#### Request Parameters

| Parameter  | Type    | Required | Description           |
| :--------- | :------ | :------- |:--------------------  |
| algoId     | String  | Yes       | Algo ID              |
| minPx      | String  | Yes       | Minimum price range  |
| maxPx      | String  | Yes       | Maximum price range  |
| gridNum    | String     | Yes       | Grid quantity        |
| topupAmount    | String     | No       | Contract grid only. Optional client-supplied top up investment amount. If this is not supplied or explicitly supplied as "0", the required top up investment amount to edit grid parameters is topped up by default        |

> Response Example

```json
{
    "code": "55186",
    "msg": "Due to market fluctuations, your investment amount is too large to apply these modifications.",
    "data": [
        {
            "algoId": "4283223775520665600",
            "maxTopupAmount": "12456.78",
            "requiredTopupAmount": "12.34"
        }
    ]
}
```

#### Response Parameters

| **Parameter**       | **Type** | **Description**                                                           |
|:--------------      | :------- |:--------------------------------------------------------------------------|
| algoId              | String   | Algo ID                                                                   |
| requiredTopupAmount | String   | Required top up investment amount to edit grid parameters.                |
| maxTopupAmount | String   | Contract grid only. Max top up investment amount to edit grid parameters       |

#### Error Code
| **Error Code**  | **HTTP Status code** | **Error Message**                                                           |
|:--------------  | :-------      |:--------------------------------------------------------------------------|
| 51000           | 400   | Parameter {param} error. This would also be returned when a grid bot does not support the parameter
| 51346           | 400   | Upper limit must be greater than the lower limit price.
| 55123           | 400   | There is insufficient balance for transfer.      |
| 55124           | 200   | Due to market fluctuations, your investment amount is insufficient to apply these modifications.       |
| 55186           | 200   | Due to market fluctuations, your investment amount is too large to apply these modifications.      |
