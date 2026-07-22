---
site: okex
lang: zh
name: POST / 撤销合约网格平仓单
method: POST
path: /api/v5/tradingBot/grid/cancel-close-order
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 撤销合约网格平仓单 [order-book-trading-grid-trading-post-cancel-close-position-order-for-contract-grid]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/grid/cancel-close-order`

> 请求示例

```shell
POST /api/v5/tradingBot/grid/cancel-close-order
body
{
    "algoId":"448965992920907776",
    "ordId":"570627699870375936"
}
```

#### 请求参数

| 参数名         | 类型       | 是否必须 | 描述     |
|:------------|:---------| :------- |:-------|
| algoId      | String   | 是       | 策略订单ID |
| ordId       | String   | 是       | 平仓单ID  |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "algoClOrdId": "",
            "algoId": "448965992920907776",
            "ordId": "570627699870375936",
            "tag": ""
        }
    ]
}
```

#### 返回参数

| **参数名**   | **类型** | **描述**    |
|:----------| :------- |:----------|
| algoId    | String   | 策略订单ID    |
| ordId     | String   | 平仓单ID     |
| algoClOrdId   | String   | 用户自定义策略ID |
| tag         | String   | 订单标签      |
