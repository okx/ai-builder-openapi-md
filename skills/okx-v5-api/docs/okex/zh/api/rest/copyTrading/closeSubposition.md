---
site: okex
lang: zh
name: POST / 平仓带单
method: POST
path: /api/v5/copytrading/close-subposition
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 平仓带单 [order-book-trading-copy-trading-post-close-lead-position]

一次仅可平仓一个带单仓位。<br>
`subPosId` 为必填参数，需要通过[交易员获取当前带单](/docs-v5/zh/#order-book-trading-copy-trading-get-existing-lead-positions)接口获取。

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/copytrading/close-subposition`

> 请求示例

```shell
POST /api/v5/copytrading/close-subposition
body
{
    "subPosId": "518541406042591232"
}
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                               |
| :------ | :----- |:-----|:-------------------------------------------------|
| instType     | String | 否       | 产品类型<br>SPOT：币币<br>SWAP：永续合约，默认值               |
| subPosId  | String | 是    | 带单仓位ID                                        |
| tag     | String | 否       | 订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间。 |
| ordType     | String | 否       | 订单类型<br>`market`：市价单<br>`limit`：限价单<br>默认为市价单                                 |
| px     | String | 否       | 委托价格，仅适用于`limit`类型的订单，且仅适用于现货交易员<br>委托价格为 0 代表撤销挂单<br>已经设置了限价单，仍为该条目设置价格时，视为改单。 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "subPosId": "518560559046594560",
            "tag":""
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**  | **类型** | **描述**    |
| :---------- | :------- |:----------|
| subPosId      | String   | 带单仓位ID |
| tag      | String   | 订单标签      |
