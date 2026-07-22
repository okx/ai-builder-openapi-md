---
site: okex
lang: zh
name: POST / 撤销赎回
method: POST
path: /api/v5/finance/staking-defi/eth/cancel-redeem
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 撤销赎回 [financial-product-eth-staking-post-cancel-redeem]

#### 限速：2 次/s
#### 限速规则：User ID

#### HTTP 请求

`POST /api/v5/finance/staking-defi/eth/cancel-redeem`

> 请求示例

```shell
POST /api/v5/finance/staking-defi/eth/cancel-redeem
body
{
    "ordId": "1234567890"
}
```

#### 请求参数

| 参数    | 类型   | 是否必须 | 描述   |
|:-------|:------|:-------|:------|
| ordId  | String | 是      | 订单ID |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ordId": "1234567890"
        }
    ],
    "msg": ""
}
```

#### 返回参数
| 参数名 | 类型   | 描述   |
|-------|--------|--------|
| ordId | String | 订单ID |
