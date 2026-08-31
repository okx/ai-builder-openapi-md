---
site: okex
lang: zh
name: POST / 修改止盈止损
method: POST
path: /api/v5/tradingBot/signal/amendTPSL
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 修改止盈止损 [order-book-trading-signal-bot-trading-post-amend-tpsl]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/signal/amendTPSL`

> 请求示例

```shell
POST /api/v5/tradingBot/signal/amendTPSL
body
{
    "algoId": "637039348240277504",
    "exitSettingParam": {
        "tpSlType": "pnl",
        "tpPct": "0.01",
        "slPct": "0.01"
    }
}
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                         |
| :-------- | :----- | :------- | :----------------------------------------------------------- |
| algoId                | String | 是       | 策略ID  |
| exitSettingParam      | String | 是       | 离场参数设定 |
| > tpSlType            | String | 是       | 止盈止损类型 |
| > tpPct               | String | 否       | 止盈百分比  |
| > slPct               | String | 否       | 止损百分比  |

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
