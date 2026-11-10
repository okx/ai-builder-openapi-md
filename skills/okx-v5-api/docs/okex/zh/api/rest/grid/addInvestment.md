---
site: okex
lang: zh
name: POST / 加仓
method: POST
path: /api/v5/tradingBot/grid/adjust-investment
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 加仓 [order-book-trading-grid-trading-post-add-investment]

该接口用于加仓，仅适用于合约网格。

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/grid/adjust-investment`

> 请求示例

```shell
POST /api/v5/tradingBot/grid/adjust-investment
body
{
    "algoId":"448965992920907776",
    "amt":"12"
}
```

#### 请求参数

| 参数名           | 类型       | 是否必须 | 描述     |
|:--------------|:---------|:-----|:-------|
| algoId        | String   | 是    | 策略订单ID |
| amt        | String   | 是    | 加仓数量   |
| allowReinvestProfit        | String   | 否    | 是否复投利润，仅适用于现货网格。<br>`true` 或者 `false`。默认为 true。 |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "algoId":"448965992920907776"
        }
    ]
}
```

#### 返回参数

| **参数名**   | **类型** | **描述**              |
|:----------| :------- |:--------------------|
| algoId    | String   | 策略订单ID              |
