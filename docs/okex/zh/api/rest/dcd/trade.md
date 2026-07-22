---
site: okex
lang: zh
name: POST / 下单
method: POST
path: /api/v5/finance/sfp/dcd/trade
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 下单 [financial-product-dual-investment-post-trade]

使用有效报价下单双币赢。

#### 限速：2次/60s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/finance/sfp/dcd/trade`

> 请求示例

```shell
POST /api/v5/finance/sfp/dcd/trade
body
{
    "quoteId": "quoterbpDCD-QUOTE17732116652401234"
}
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
| :---- | :--- | :------- | :--- |
| quoteId | String | 是 | 报价ID |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "quoteId": "quoterbpDCD-QUOTE17732116652401234",
            "ordId": "987654321",
            "state": "live"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| :-------- | :------ | :------- |
| quoteId | String | 报价ID |
| ordId | String | 订单ID |
| state | String | 订单状态<br>`initial`：系统已接收请求，待处理<br>`pending_book`：流动性提供商已接收请求，待处理<br>`live`：交易已生效<br>`rejected`：交易已拒绝 |
