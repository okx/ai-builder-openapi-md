---
site: okex
lang: zh
name: 获取买卖交易币种
method: GET
path: /api/v5/fiat/buy-sell/currencies
module: fiat
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取买卖交易币种 [funding-account-rest-api-get-buy-sell-currencies]

#### 限速：6次/s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/fiat/buy-sell/currencies`

> 请求示例

```shell
GET /api/v5/fiat/buy-sell/currencies
```

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
           "fiatCcyList":[
                {
                    "ccy": "USD"
                },
                {
                    "ccy": "EUR"
                },
                ...
            ],
            "cryptoCcyList":[
                {
                    "ccy": "BTC"
                },
                ...
            ],
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名      | 类型              | 描述            |
|:--------------|:------------------|:-----------------------|
| fiatCcyList    | Array of objects  | 法币列表     |
| >ccy           | String            | 币种，如 `BTC`   |
| cryptoCcyList  | Array of objects  | 加密货币列表   |
| >ccy           | String            | 币种，如 `USD`   |

<aside class="notice">
    此功能目前仅对巴哈马机构用户开放。
</aside>
