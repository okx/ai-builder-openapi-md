---
site: okex
lang: zh
name: POST / 定投策略停止
method: POST
path: /api/v5/tradingBot/recurring/stop-order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 定投策略停止 [order-book-trading-recurring-buy-post-stop-recurring-buy-order]

每次最多可以撤销10个定投策略订单。

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/recurring/stop-order-algo`

> 请求示例

```shell
POST /api/v5/tradingBot/recurring/stop-order-algo
body
[
    {
        "algoId":"560472804207104000"
    }
]
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                         |
| :-------- | :----- | :------- | :----------------------------------------------------------- |
| algoId    | String | 是       | 策略订单ID                                                   |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "1839309556514557952",
            "sCode": "0",
            "sMsg": "",
            "tag": ""
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                      |
| :--------- | :------- | :---------------------------- |
| algoId      | String   | 策略订单ID                        |
| algoClOrdId      | String   | 客户自定义订单ID                        |
| sCode      | String   | 事件执行结果的code，0代表成功 |
| sMsg       | String   | 事件执行失败时的msg           |
| tag         | String   | <del>订单标签</del>（已废弃）                      |
