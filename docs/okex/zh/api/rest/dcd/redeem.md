---
site: okex
lang: zh
name: POST / 赎回
method: POST
path: /api/v5/finance/sfp/dcd/redeem
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 赎回 [financial-product-dual-investment-post-redeem]

使用有效的赎回报价确认提前赎回。这是两步赎回流程的第二步。

#### 限速：2次/60s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/finance/sfp/dcd/redeem`

> 请求示例

```shell
POST /api/v5/finance/sfp/dcd/redeem
body
{
    "ordId": "987654321",
    "quoteId": "quoterbcDCD-REDEEM17732116652401234"
}
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
| :---- | :--- | :------- | :--- |
| ordId | String | 是 | 订单ID |
| quoteId | String | 是 | 报价ID |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ordId": "987654321",
            "state": "pending_redeem_booking"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| :-------- | :------ | :------- |
| ordId | String | 订单ID |
| state | String | 订单状态<br>`pending_redeem_booking`：赎回请求已接收，等待流动性提供商确认<br>`pending_redeem`：流动性提供商已确认，等待资金划转<br>`redeeming`：赎回处理中<br>`redeemed`：赎回完成 |
