---
site: okex
lang: zh
name: POST / 修改定投策略订单
method: POST
path: /api/v5/tradingBot/recurring/amend-order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 修改定投策略订单 [order-book-trading-recurring-buy-post-amend-recurring-buy-order]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/recurring/amend-order-algo`

> 请求示例

```shell
POST /api/v5/tradingBot/recurring/amend-order-algo
body
{
    "algoId":"448965992920907776",
    "stgyName":"stg1"
}
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                         |
| :-------- | :----- | :------- | :----------------------------------------------------------- |
| algoId    | String | 是       | 策略订单ID                                                   |
| stgyName    | String | 是       | 调整后的策略自定义名称                                                   |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "algoId":"448965992920907776",
            "algoClOrdId":"",
            "sCode":"0",
            "sMsg":""
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                      |
| :--------- | :------- | :---------------------------- |
| algoId      | String   | 策略订单ID                        |
| algoClOrdId      | String   | 客户自定义订单ID                        |
| sCode      | String   | 事件执行结果的code，0代表成功 |
| sMsg       | String   | 事件执行失败时的msg           |
