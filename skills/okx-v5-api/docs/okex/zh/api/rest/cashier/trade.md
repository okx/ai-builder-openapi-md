---
site: okex
lang: zh
name: 买卖交易
method: POST
path: /api/v5/fiat/buy-sell/trade
module: fiat
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 买卖交易 [funding-account-rest-api-buy-sell-trade]

#### 限速：1次/5s
#### 限速规则：User ID

#### HTTP 请求

`POST /api/v5/fiat/buy-sell/trade`

> 请求示例

```shell
# 卖出30 USD买入BTC
POST /api/v5/fiat/buy-sell/trade
body
{
    "clOrdId":"123456",
    "side":"sell",
    "fromCcy": "USD",
    "toCcy": "BTC",
    "rfqAmt": "30",
    "rfqCcy": "USD",
    "paymentMethod":"balance",
    "quoteId": "quoterETH-USDT16461885104612381"
}
```

#### 请求参数

| 参数名     | 类型   | 是否必须 | 描述 |
|:--------------|:-------|:---------|:------------|
| quoteId       | String | 是      | 报价ID<br>从获取买卖交易报价API获取 |
| side          | String | 是      | 交易方向<br>`buy`: 使用法币购买加密货币/法币<br>`sell`: 将加密货币出售为加密货币/法币<br>必须与报价请求一致 |
| fromCcy       | String | 是      | 卖出币种<br>必须与报价请求一致 |
| toCcy         | String | 是      | 买入币种<br>必须与报价请求一致 |
| rfqAmt        | String | 是      | 询价数量<br>必须与报价请求一致 |
| rfqCcy        | String | 是      | 询价币种<br>必须与报价请求一致 |
| paymentMethod | String | 是      | 支付方式<br>`balance` |
| clOrdId       | String | 是      | 用户自定义的订单标识<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ordId": "1234",
            "clOrdId": "",
            "quoteId": "quoterBTC-USD16461885104612381",
            "side":"buy",
            "fromCcy": "USD",
            "toCcy": "BTC",
            "rfqAmt": "30",
            "rfqCcy": "USD",
            "fillPx": "2932.40104429",
            "fillQuoteCcy": "USD",
            "fillFromAmt": "30",
            "fillToAmt": "0.01",
            "cTime": "1646188510461"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名      | 类型   | 描述 |
|---------------|--------|-------------|
| ordId         | String | 订单ID |
| clOrdId       | String | 用户自定义的订单标识 |
| quoteId       | String | 报价ID |
| state         | String | 交易状态<br>`processing`：处理中<br>`completed`：已完成<br>`failed`：失败 |
| side          | String | 交易方向<br>`buy`: 使用法币购买加密货币/法币<br>`sell`: 将加密货币出售为加密货币/法币 |
| fromCcy       | String | 卖出币种 |
| toCcy         | String | 买入币种 |
| rfqAmt        | String | 询价数量 |
| rfqCcy        | String | 询价币种 |
| fillPx        | String | 成交价格，单位为报价币种 |
| fillQuoteCcy  | String | 成交价格报价币种<br>如 `USD` |
| fillFromAmt   | String | 卖出数量，单位为 `fromCcy` |
| fillToAmt     | String | 买入数量，单位为 `toCcy` |
| cTime         | String | 请求时间，Unix时间戳的毫秒数格式，如 `1597026383085` |

<aside class="notice">
此功能目前仅对巴哈马机构用户开放。
</aside>
