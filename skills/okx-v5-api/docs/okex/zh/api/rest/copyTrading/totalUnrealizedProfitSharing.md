---
site: okex
lang: zh
name: GET / 交易员待分润汇总
method: GET
path: /api/v5/copytrading/total-unrealized-profit-sharing
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 交易员待分润汇总 [order-book-trading-copy-trading-get-total-unrealized-profit-sharing]

交易员获取待分润汇总。

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/copytrading/total-unrealized-profit-sharing`

> 请求示例

```shell
GET /api/v5/copytrading/total-unrealized-profit-sharing

```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                     |
| :------ | :----- |:-----|:-----------------------|
| instType     | String | 否       | 产品类型<br>SWAP：永续合约，默认值 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "profitSharingTs": "1705852800000",
            "totalUnrealizedProfitSharingAmt": "0.114402985553185"
        }
    ],
    "msg": ""
}

```

#### 返回参数

| **参数名**  | **类型** | **描述**                                         |
| :---------- | :------- |:-----------------------------------------------|
| profitSharingTs      | String   | 当前周期待分润总额的结算时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| totalUnrealizedProfitSharingAmt      | String   | 待分润总额                                          |
