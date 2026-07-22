---
site: okex
lang: zh
name: POST / 停止信号策略
method: POST
path: /api/v5/tradingBot/signal/stop-order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 停止信号策略 [order-book-trading-signal-bot-trading-post-cancel-signal-bots]

每次最多可以撤销10个信号策略。

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/signal/stop-order-algo`

> 请求示例

```shell
POST /api/v5/tradingBot/signal/stop-order-algo
body
[
    {
        "algoId":"448965992920907776"
    }
]
```

#### 请求参数

| 参数名           | 类型       | 是否必须 | 描述                                                         |
|:--------------|:---------| :------- | :----------------------------------------------------------- |
| algoId        | String   | 是       | 策略ID                                                   |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "algoId": "448965992920907776",
            "sCode": "0",
            "sMsg": "",
            "algoClOrdId": ""

        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**     | **类型** | **描述**                          |
|:------------| :------- |:--------------------------------|
| algoId      | String   | 策略ID                          |
| sCode       | String   | 事件执行结果的code，0代表成功               |
| sMsg        | String   | 事件执行失败时的msg                     |
| algoClOrdId      | String   | 客户自定义订单ID                        |
