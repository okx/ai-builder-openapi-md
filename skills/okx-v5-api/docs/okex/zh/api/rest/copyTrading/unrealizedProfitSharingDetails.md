---
site: okex
lang: zh
name: GET / 交易员待分润明细
method: GET
path: /api/v5/copytrading/unrealized-profit-sharing-details
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 交易员待分润明细 [order-book-trading-copy-trading-get-unrealized-profit-sharing-details]

交易员获取预计在下一个周期分到的分润金额明细。<br>
当有跟单仓位平仓时，待分润明细会进行更新。

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/copytrading/unrealized-profit-sharing-details`

> 请求示例

```shell
GET /api/v5/copytrading/unrealized-profit-sharing-details

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
            "nickName": "Potato",
            "portLink": "",
            "ts": "1669901824779",
            "unrealizedProfitSharingAmt": "0.455472",
            "instType": "SWAP"
        },
        {
            "ccy": "USDT",
            "nickName": "Apple",
            "portLink": "",
            "ts": "1669460210113",
            "unrealizedProfitSharingAmt": "0.033608",
            "instType": "SWAP"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**  | **类型** | **描述**                         |
| :---------- | :------- |:-------------------------------|
| ccy      | String   | 分润币种，如：`USDT`                  |
| unrealizedProfitSharingAmt      | String   | 待分润额                           |
| nickName      | String   | 跟单人昵称                          |
| instType  | String     | 产品类型<br>SPOT：币币<br>SWAP：永续合约 |
| portLink     | String    | 跟单员头像的链接地址 |
| ts      | String   | 数据更新时间                         |
