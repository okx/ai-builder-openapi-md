---
site: okex
lang: zh
name: 获取买卖交易币对
method: GET
path: /api/v5/fiat/buy-sell/currency-pair
module: fiat
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取买卖交易币对 [funding-account-rest-api-get-buy-sell-currency-pair]

#### 限速：6次/s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/fiat/buy-sell/currency-pair`

> 请求示例

```shell
GET /api/v5/fiat/buy-sell/currency-pair?fromCcy=USD&toCcy=BTC
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                  |
|:----------|:-------|:---------|:-----------------------------|
| fromCcy    | String | 是      | 卖出币种，如 `USD` |
| toCcy      | String | 是      | 买入币种，如 `BTC`  |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "side": "buy",
            "fromCcy": "USD",
            "toCcy": "BTC",
            "singleTradeMax": "1",
            "singleTradeMin": "0.01",
            "fixedPxRemainingDailyQuota": "", 
            "fixedPxDailyLimit": "", 
            "paymentMethods":["balance"]
        }
    ],
    "msg": ""
}

{
    "code": "0",
    "data": [
        {
            "side": "sell",
            "fromCcy": "BTC",
            "toCcy": "USD",
            "singleTradeMax": "1",
            "singleTradeMin": "0.01",
            "fixedPxRemainingDailyQuota": "", 
            "fixedPxDailyLimit": "", 
            "paymentMethods":["balance"]
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名                  | 类型              | 描述                                                                                                                        |
|----------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| side                       | String            | 交易方向<br>`buy`: 使用法币购买加密货币/法币<br>`sell`: 将加密货币出售为加密货币/法币<br>未来可能同时支持双向交易，以逗号分隔，如 `buy,sell` |
| fromCcy                    | String            | 卖出币种，如 `USD`                                                                                           |
| toCcy                      | String            | 买入币种，如 `BTC`                                                                                            |
| singleTradeMax             | String            | 单笔交易最大数量，单位为 `fromCcy`                                                   |
| singleTradeMin             | String            | 单笔交易最小数量，单位为 `fromCcy`                                                   |
| fixedPxDailyLimit          | String            | 固定价格每日限额<br>仅适用于法币间交易，否则返回空字符串<br>当`side` = `buy`时，单位为 `fromCcy`<br>当`side` = `sell`时，单位为 `toCcy` |
| fixedPxRemainingDailyQuota | String            | 固定价格剩余每日限额<br>仅适用于法币间交易，否则返回空字符串<br>当`side` = `buy`时，单位为 `fromCcy`<br>当`side` = `sell`时，单位为 `toCcy` |
| paymentMethods             | Array of strings  | 支持的支付方式<br>`balance`<br>例如：["balance"]                                                          |

<aside class="notice">
此功能目前仅对巴哈马机构用户开放。
</aside>
