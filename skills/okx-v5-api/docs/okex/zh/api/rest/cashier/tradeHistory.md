---
site: okex
lang: zh
name: 获取买卖交易历史
method: GET
path: /api/v5/fiat/buy-sell/history
module: fiat
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取买卖交易历史 [funding-account-rest-api-get-buy-sell-trade-history]

#### 限速：6次/s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/fiat/buy-sell/history`

> 请求示例

```shell
GET /api/v5/fiat/buy-sell/history
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述 |
|:----------|:-------|:---------|:------------|
| ordId     | String | 否       | 订单ID |
| clOrdId   | String | 否       | 用户自定义的订单标识<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间 |
| state     | String | 否       | 交易状态<br>`processing`：处理中<br>`completed`：已完成<br>`failed`：失败 |
| begin     | String | 否       | 开始时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| end       | String | 否       | 结束时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| limit     | String | 否       | 返回的结果集数量，默认为100，最大为100 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ordId": "1234",
            "clOrdId": "",
            "quoteId": "quoterBTC-USD16461885104612381",
            "state":"completed",
            "side":"buy",
            "fromCcy": "USD",
            "toCcy": "BTC",
            "rfqAmt": "30",
            "rfqCcy": "USD",
            "fillPx": "2932.40104429",
            "fillQuoteCcy": "USD",
            "fillFromAmt": "30",
            "fillToAmt": "0.01",
            "cTime": "1646188510461",
            "uTime": "1646188510461"
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
| fromCcy       | String | 卖出币种 |
| toCcy         | String | 买入币种 |
| rfqAmt        | String | 询价数量 |
| rfqCcy        | String | 询价币种 |
| fillPx        | String | 成交价格，单位为报价币种 |
| fillQuoteCcy  | String | 成交价格报价币种<br>如 `USD` |
| fillFromAmt   | String | 成交数量，单位为 `fromCcy` |
| fillToAmt     | String | 成交数量，单位为 `toCcy` |
| cTime         | String | 请求时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| uTime         | String | 更新时间，Unix时间戳的毫秒数格式，如 `1597026383085` |

<aside class="notice">
此功能目前仅对巴哈马机构用户开放。
</aside>
