---
site: okex
lang: zh
name: GET / 交易员历史分润汇总
method: GET
path: /api/v5/copytrading/total-profit-sharing
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 交易员历史分润汇总 [order-book-trading-copy-trading-get-total-profit-sharing]

交易员获取自入驻平台以来，累计获得的总分润金额。

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/copytrading/total-profit-sharing`

> 请求示例

```shell
GET /api/v5/copytrading/total-profit-sharing

```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                         |
| :------ | :----- |:-----| :----------------------------------------------------------- |
| instType     | String | 否       | 产品类型<br>SPOT：币币<br>SWAP：永续合约<br>默认返回所有业务线的信息                                |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "USDT",
            "totalProfitSharingAmt": "0.6584928",
            "instType": "SWAP"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**  | **类型** | **描述**                                                                                                                                                                                       |
| :---------- | :------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy      | String   | 分润币种                                                                                                                                                                                         |
| totalProfitSharingAmt      | String   | 历史分润汇总                                                                                                                                                                                       |
| instType  | String     | 产品类型<br>SPOT：币币<br>SWAP：永续合约      |
