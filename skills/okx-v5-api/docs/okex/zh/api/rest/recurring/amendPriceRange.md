---
site: okex
lang: zh
name: POST / 编辑价格区间
method: POST
path: /api/v5/tradingBot/recurring/amend-price-range
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 编辑价格区间 [order-book-trading-recurring-buy-post-amend-price-range]
#### 限速：20次/2s
#### 限速规则：User ID
#### HTTP请求
`POST /api/v5/tradingBot/recurring/amend-price-range`

> 请求示例

```shell
POST /api/v5/tradingBot/recurring/amend-price-range
body
{
    "algoId": "2837428373700509696",
    "recurringList": [
        {
            "ccy": "BTC",
            "minPx": "80000",
            "maxPx": "120000"
        }
    ]
}
```

#### 请求参数
| 参数名 | 类型 | 是否必须 | 描述 |
| :--- | :--- | :--- | :--- |
| algoId | String | 是 | 策略订单ID |
| recurringList | Array | 是 | 价格区间设置，币种必须在策略定投币种范围内 |
| >ccy | String | 是 | 定投币种 |
| >minPx | String | 是 | 价格区间最低价，`""` 代表没有限制 |
| >maxPx | String | 是 | 价格区间最高价，`""` 代表没有限制 |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "algoId": "2837428373700509696",
            "sCode": "0",
            "sMsg": ""
        }
    ]
}
```

#### 返回参数

| 参数名 | 类型 | 描述 |
| :--- | :--- | :--- |
| algoId | String | 策略订单ID |
| sCode | String | 事件执行结果的 code，0 代表成功 |
| sMsg | String | 事件执行失败时的 msg |
