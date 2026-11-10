---
site: okex
lang: zh
name: POST / 网格策略立即触发
method: POST
path: /api/v5/tradingBot/grid/order-instant-trigger
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 网格策略立即触发 [order-book-trading-grid-trading-post-instant-trigger-grid-algo-order]

#### 限速：20次/2s
#### 限速规则：User ID + Instrument ID

#### HTTP请求

`POST /api/v5/tradingBot/grid/order-instant-trigger`

> 请求示例

```shell
POST /api/v5/tradingBot/grid/order-instant-trigger
body
{
    "algoId":"561564133246894080"
}
```

#### 请求参数

| 参数名           | 类型       | 是否必须 | 描述                                                         |
|:--------------|:---------| :------- | :----------------------------------------------------------- |
| algoId        | String   | 是       | 策略订单ID                                                   |
| topUpAmt   | String | 否       | 增加的投资额，仅适用于现货网格                                                                          |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "561564133246894080"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**       | **类型** | **描述**                     |
|:--------------| :------- | :------------------------- |
| algoId        | String   | 策略订单ID                     |
| algoClOrdId   | String   | 用户自定义策略ID |
