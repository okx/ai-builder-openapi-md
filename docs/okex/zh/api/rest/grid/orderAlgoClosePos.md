---
site: okex
lang: zh
name: POST / 合约网格平仓
method: POST
path: /api/v5/tradingBot/grid/close-position
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 合约网格平仓 [order-book-trading-grid-trading-post-close-position-for-contract-grid]

只有处于已停止未平仓状态合约网格可使用该接口

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/grid/close-position`

> 请求示例

```shell
POST /api/v5/tradingBot/grid/close-position
body
{
    "algoId":"448965992920907776",
    "mktClose":true
}
```

#### 请求参数

| 参数名           | 类型       | 是否必须 | 描述                                 |
|:--------------|:---------|:-----|:-----------------------------------|
| algoId        | String   | 是    | 策略订单ID                             |
| mktClose        | Boolean   | 是    | 是否市价全平<br>`true`：市价全平，`false`：部分平仓 |
| sz        | String   | 可选   | 平仓数量,单位为张<br>部分平仓时必传              |
| px        | String   | 可选     | 平仓价格 <br>部分平仓时必传                  |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "algoClOrdId": "",
            "algoId":"448965992920907776",
            "ordId":"",
            "tag": ""
        }
    ]
}
```

#### 返回参数

| **参数名**   | **类型** | **描述**              |
|:----------| :------- |:--------------------|
| algoId    | String   | 策略订单ID              |
| ordId     | String   | 平仓单ID<br>市价全平时，该字段为"" |
| algoClOrdId   | String   | 用户自定义策略ID |
| tag         | String   | 订单标签      |
