---
site: okex
lang: zh
name: GET / 获取订单状态
method: GET
path: /api/v5/finance/sfp/dcd/order-status
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取订单状态 [financial-product-dual-investment-get-order-state]

返回双币赢订单的当前状态。

#### 限速：3次/s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/finance/sfp/dcd/order-status`

> 请求示例

```shell
GET /api/v5/finance/sfp/dcd/order-status?ordId=987654321
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
| :---- | :--- | :------- | :--- |
| ordId | String | 是 | 订单ID |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ordId": "987654321",
            "state": "live"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
| :-------- | :------ | :------- |
| ordId | String | 订单ID |
| state | String | 订单状态<br>`initial`<br>`live`<br>`pending_settle`<br>`settled`<br>`pending_redeem`<br>`redeemed`<br>`rejected` |
