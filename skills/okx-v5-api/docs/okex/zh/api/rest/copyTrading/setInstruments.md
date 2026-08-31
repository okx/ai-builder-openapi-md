---
site: okex
lang: zh
name: POST / 交易员修改带单产品
method: POST
path: /api/v5/copytrading/set-instruments
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 交易员修改带单产品 [order-book-trading-copy-trading-post-amend-leading-instruments]

交易员修改带单产品的设置。初始带单产品在申请带单交易员时进行设置。<br>
非带单产品修改为带单产品时，该次请求中所有的非带单产品不能有持仓或者挂单。

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/copytrading/set-instruments`

> 请求示例

```shell
POST /api/v5/copytrading/set-instruments
body
{
    "instId": "BTC-USDT-SWAP,ETH-USDT-SWAP"
}
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                           |
| :-------- | :----- | :------- |:---------------------------------------------|
| instType     | String | 否       | 产品类型<br>SPOT：币币<br>SWAP：永续合约，默认值           |
| instId    | String | 是       | 产品ID，如 BTC-USDT-SWAP，多个产品用半角逗号隔开 |

<aside class="notice">
如果进行多个产品带单，`instId`传值需要包括所有将要带单的产品，因为当前请求设置成功后，之前的设置会被覆盖掉<br>
</aside>

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "enabled": true,
            "instId": "BTC-USDT-SWAP"
        },
        {
            "enabled": true,
            "instId": "ETH-USDT-SWAP"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型**  | **描述**                                                        |
| :--------- |:--------|:--------------------------------------------------------------|
| instId      | String  | 产品id， 如 BTC-USDT-SWAP                                         |
| enabled    | Boolean | `true` 或 `false`<br>`true` 代表设置成功<br>`false` 代表设置失败                  |
