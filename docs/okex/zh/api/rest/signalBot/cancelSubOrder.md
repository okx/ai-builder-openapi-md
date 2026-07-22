---
site: okex
lang: zh
name: POST / 撤单
method: POST
path: /api/v5/tradingBot/signal/cancel-sub-order
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 撤单 [order-book-trading-signal-bot-trading-post-cancel-sub-order]

撤销之前下的未完成订单。

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/signal/cancel-sub-order`

> 请求示例

```shell
POST /api/v5/tradingBot/signal/cancel-sub-order
body
{
    "algoId":"91664",
    "signalOrdId":"590908157585625111",
    "instId":"BTC-USDT-SWAP"
}
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                            |
| :------ | :----- | :------- | :------------------------------ |
| algoId  | String | 是    | 策略ID                      |
| instId  | String | 是    | 产品ID，如 BTC-USDT-SWAP        |
| signalOrdId | String | 是     | 订单ID                    |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "signalOrdId":"590908157585625111",
            "sCode":"0",
            "sMsg":""
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                      |
| :--------- | :------- | :---------------------------- |
| code       | String   | 结果代码，`0`表示成功 |
| msg        | String   | 错误信息，代码为0时，该字段为空   |
| data       | Array of objects   | 包含结果的对象数组              |
| > signalOrdId      | String   | 订单ID                    |
| > sCode            | String   | 事件执行结果的code，0代表成功 |
| > sMsg             | String   | 事件执行失败时的msg          |

<aside class="notice">
撤单返回sCode等于0不能严格认为该订单已经被撤销，只表示您的撤单请求被系统服务器所接受，撤单结果以者查询订单状态为准<br>
</aside>
