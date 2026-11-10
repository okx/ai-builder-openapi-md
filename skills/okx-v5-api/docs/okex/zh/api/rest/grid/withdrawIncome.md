---
site: okex
lang: zh
name: POST / 现货网格提取利润
method: POST
path: /api/v5/tradingBot/grid/withdraw-income
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 现货网格提取利润 [order-book-trading-grid-trading-post-spot-grid-withdraw-income]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/grid/withdraw-income`

> 请求示例

```shell
POST /api/v5/tradingBot/grid/withdraw-income
body
{
    "algoId":"448965992920907776"
}
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                         |
| :-------- | :----- | :------- | :----------------------------------------------------------- |
| algoId    | String | 是       | 策略订单ID                                                   |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "algoClOrdId": "",
            "algoId":"448965992920907776",
            "profit":"100"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                      |
| :--------- | :------- | :---------------------------- |
| algoId      | String   | 策略订单ID                        |
| algoClOrdId   | String   | 用户自定义策略ID                        |
| profit      | String   | 提取的利润   |
