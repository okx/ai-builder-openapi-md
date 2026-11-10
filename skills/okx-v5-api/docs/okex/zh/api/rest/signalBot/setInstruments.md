---
site: okex
lang: zh
name: POST / 设置币对
method: POST
path: /api/v5/tradingBot/signal/set-instruments
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 设置币对 [order-book-trading-signal-bot-trading-post-set-instruments]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/signal/set-instruments`

> 请求示例

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

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                         |
| :-------- | :----- | :------- | :----------------------------------------------------------- |
| algoId            | String    | 是       | 策略ID                                                            |
| instIds           | Array of strings     | 是       | 产品Id 列表，当 includeAll 为 true 时，忽略此参数。                    |
| includeAll        | Boolean   | 是       | 是否包含所有USDT 本位永续合约，默认false `true`: 包含 `false` : 不包含   |

> 返回结果

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

#### 返回参数

| **参数名**       | **类型** | **描述**                      |
|:--------------| :------- | :---------------------------- |
| algoId        | String   | 策略ID                        |
