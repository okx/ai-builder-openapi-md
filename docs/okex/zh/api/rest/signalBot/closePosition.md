---
site: okex
lang: zh
name: POST / 市价仓位全平
method: POST
path: /api/v5/tradingBot/signal/close-position
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 市价仓位全平 [order-book-trading-signal-bot-trading-post-close-position]

市价平掉指定交易产品的持仓

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/signal/close-position`

> 请求示例

```shell
POST /api/v5/tradingBot/signal/close-position
body
{
    "instId":"BTC-USDT-SWAP",
    "algoId":"448965992920907776"
}
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                         |
| :------ | :----- |:-----| :----------------------------------------------------------- |
| algoId  | String | 是   | 策略ID |
| instId  | String | 是   | 产品ID    |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "algoId": "448965992920907776"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述**   |
| :--------- | :------- | :--------- |
| algoId     | String   | 策略ID |
