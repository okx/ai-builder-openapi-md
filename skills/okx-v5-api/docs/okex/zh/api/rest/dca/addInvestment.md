---
site: okex
lang: zh
name: POST / 手动加仓
method: POST
path: /api/v5/tradingBot/dca/orders/manual-buy
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 手动加仓 [order-book-trading-dca-trading-post-dca-manual-buy]
#### 限速：20次/2s
#### 限速规则：User ID
#### HTTP请求
`POST /api/v5/tradingBot/dca/orders/manual-buy`

> 请求示例

```shell
POST /api/v5/tradingBot/dca/orders/manual-buy
body
{
    "algoId": "2833925189933756416",
    "algoOrdType": "contract_dca",
    "price": "41000",
    "amt": "100"
}
```

#### 请求参数
| 参数名         | 类型   | 是否必须 | 描述                                                                        |
| :----------- | :----- | :------- | :-------------------------------------------------------------------------- |
| algoId       | String | 是       | 策略订单 ID                                                                 |
| algoOrdType  | String | 是       | 策略订单类型<br>`contract_dca`：合约马丁委托<br>`spot_dca`：现货马丁委托    |
| price        | String | 是       | 加仓价格                                                                    |
| amt          | String | 是       | 增加的投资额                                                                |
| ordType      | String | 否       | 订单类型<br>`limit`：限价单<br>`market`：市价单<br>仅适用于 `spot_dca`      |
| tradeQuoteCcy| String | 否       | 指定交易计价货币<br>仅适用于 `spot_dca`                                     |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "algoId": "2833925189933756416",
            "algoClOrdId": "",
            "algoOrdType": "contract_dca",
            "tag": "",
            "diffAmount": "100",
            "sCode": "0",
            "sMsg": ""
        }
    ]
}
```

#### 返回参数
| 参数名       | 类型   | 描述                                                                        |
| :----------- | :----- | :-------------------------------------------------------------------------- |
| algoId       | String | 策略订单 ID                                                                 |
| algoClOrdId  | String | 客户端自定义策略单ID                                                        |
| algoOrdType  | String | 策略订单类型<br>`contract_dca`：合约马丁委托<br>`spot_dca`：现货马丁委托    |
| tag          | String | 订单标签                                                                    |
| diffAmount   | String | 手动加仓转入虚拟子账户的资金<br>仅适用于 `contract_dca`                     |
| sCode        | String | 事件执行结果的 code，0 代表成功                                              |
| sMsg         | String | 事件执行失败时的 msg                                                        |
