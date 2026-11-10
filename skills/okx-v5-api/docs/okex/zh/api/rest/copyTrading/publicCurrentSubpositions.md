---
site: okex
lang: zh
name: GET / 获取交易员当前带单
method: GET
path: /api/v5/copytrading/public-current-subpositions
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取交易员当前带单 [order-book-trading-copy-trading-get-lead-trader-current-lead-positions]

公共接口，获取交易员当前带单。<br>

#### 限速：5次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/copytrading/public-current-subpositions`

> 请求示例

```shell
GET /api/v5/copytrading/public-current-subpositions?instType=SWAP&uniqueCode=D9ADEAB33AE9EABD

```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                      |
| :------ | :----- |:-----|:----------------------------------------|
| instType     | String | 否    | 产品类型<br>SWAP：永续合约，默认值                   |
| uniqueCode     | String | 是    | 交易员唯一标识码                         |
| after    | String | 否    | 请求此id之前（更旧的数据）的分页内容，传的值为对应接口的`subPosId` |
| before   | String | 否    | 请求此id之后（更新的数据）的分页内容，传的值为对应接口的`subPosId` |
| limit    | String | 否    | 分页返回的结果集数量，最大为100，不填默认返回100条            |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "USDT",
            "instId": "ETH-USDT-SWAP",
            "instType": "SWAP",
            "lever": "5",
            "margin": "16.23304",
            "markPx": "2027.31",
            "mgnMode": "isolated",
            "openAvgPx": "2029.13",
            "openTime": "1701144639417",
            "posSide": "short",
            "subPos": "4",
            "subPosId": "649582930998104064",
            "uniqueCode": "D9ADEAB33AE9EABD",
            "upl": "0.0728",
            "uplRatio": "0.0044846806266725"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**  | **类型** | **描述**                                                                                                                                                                                       |
| :---------- | :------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId      | String   | 产品ID                                                                                                                                                                                         |
| subPosId      | String   | 带单仓位ID                                                                                                                                                                                       |
| posSide         | String   | 持仓方向<br>long：开平仓模式开多 <br>short：开平仓模式开空 <br>net：买卖模式（subPos为正代表开多，subPos为负代表开空）                                                                                                                             |
| mgnMode       | String   | 保证金模式，`isolated`：逐仓 ；`cross`：全仓                                                                                                                                                                                         |
| lever     | String   | 杠杆倍数                                                                                                                                                                                    |
| openAvgPx          | String   | 开仓均价                                                                                                                                                                                         |
| openTime          | String   | 开仓时间                                                                                                                                                                                         |
| subPos         | String   | 持仓张数                                                                                                                                                                       |
| instType  | String     | 产品类型<br>SPOT：币币<br>SWAP：永续合约      |
| margin     | String     | 保证金           |
| upl     | String     | 未实现收益         |
| uplRatio     | String      | 未实现收益率        |
| markPx     | String       | 最新标记价格，仅适用于合约 |
| uniqueCode     | String       | 交易员唯一标识代码     |
| ccy     | String       | 币种            |
