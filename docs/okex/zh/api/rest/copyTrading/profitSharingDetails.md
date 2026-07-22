---
site: okex
lang: zh
name: GET / 交易员历史分润明细
method: GET
path: /api/v5/copytrading/profit-sharing-details
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 交易员历史分润明细 [order-book-trading-copy-trading-get-profit-sharing-details]

交易员获取最近三个月的分润明细。

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/copytrading/profit-sharing-details`

> 请求示例

```shell
GET /api/v5/copytrading/profit-sharing-details?limit=2

```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                         |
| :------ | :----- |:-----| :----------------------------------------------------------- |
| instType     | String | 否       | 产品类型<br>SPOT：币币<br>SWAP：永续合约<br>默认返回所有业务线的信息                                |
| after    | String | 否       | 请求此id之前（更旧的数据）的分页内容，传的值为对应接口的`profitSharingId` |
| before   | String | 否       | 请求此id之后（更新的数据）的分页内容，传的值为对应接口的`profitSharingId` |
| limit    | String | 否       | 分页返回的结果集数量，最大为100，不填默认返回100条         |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "USDT",
            "nickName": "Potato",
            "profitSharingAmt": "0.00536",
            "profitSharingId": "148",
            "portLink": "",
            "ts": "1723392000000",
            "instType": "SWAP"
        },
        {
            "ccy": "USDT",
            "nickName": "Apple",
            "profitSharingAmt": "0.00336",
            "profitSharingId": "20",
            "portLink": "",
            "ts": "1723392000000",
            "instType": "SWAP"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**  | **类型** | **描述**                         |
| :---------- | :------- |:-------------------------------|
| ccy      | String   | 分润币种                           |
| profitSharingAmt      | String   | 分润额，没有分润时，默认返回0                |
| nickName         | String   | 跟单人的昵称                         |
| profitSharingId       | String   | 分润ID                           |
| instType  | String     | 产品类型<br>SPOT：币币<br>SWAP：永续合约 |
| portLink     | String    | 跟单员头像的链接地址 |
| ts     | String   | 分润时间                           |
