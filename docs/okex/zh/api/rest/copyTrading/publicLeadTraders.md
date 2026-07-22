---
site: okex
lang: zh
name: GET / 获取交易员排名
method: GET
path: /api/v5/copytrading/public-lead-traders
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取交易员排名 [order-book-trading-copy-trading-get-lead-trader-ranks]

公共接口，获取交易员排名信息。

#### 限速：5次/2s  
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/copytrading/public-lead-traders`

> 请求示例

```shell
GET /api/v5/copytrading/public-lead-traders?instType=SWAP

```

#### 请求参数

| 参数名         | 类型   | 是否必须 | 描述                                                                                                                                                                   |
|:------------| :----- |:-----|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType    | String | 否    | 产品类型<br>`SWAP`：永续合约，默认值                                                                                                                                             |
| sortType    | String | 否       | 排名类型<br>`overview`: 综合排序，默认值<br>`pnl`: 按照交易员收益额排序<br>`aum`: 按照带单规模排序<br>`win_ratio`: 胜率<br>`pnl_ratio`: 收益率<br>`current_copy_trader_pnl`: 当前跟单人的收益额        |
| state       | String | 否       | 交易员的状态<br>`0`: 所有交易员，默认值，包括有空缺和没有空缺 <br>`1`: 有空缺的交易员                                                                                                               |
| minLeadDays | String | 否       | 最短带单时长<br>`1`: 7 天<br>`2`: 30 天<br>`3`: 90 天<br>`4`: 180天                                                                                                       |
| minAssets   | String | 否       | 交易员资产范围的最小值，单位为 USDT                                                                                                                                                 |
| maxAssets   | String | 否       | 交易员资产范围的最大值，单位为 USDT                                                                                                                                                 |
| minAum      | String | 否       | 带单规模的最小值，单位为 USDT                                                                                                                                                    |
| maxAum      | String | 否       | 带单规模的最大值，单位为 USDT                                                                                                                                                    |
| dataVer     | String | 否       | 排名数据的版本，14 位数字，如：20231010182400，主要在分页时使用 <br>每10分钟生成一版，仅保留最新的5个版本<br>默认使用最近的版本；不存在时不会报错，会使用最近的版本。                                                                  |
| page        | String | 否       | 查询页数                                                                                                                                                                 |
| limit       | String | 否       | 分页返回的结果集数量，最大为 20，不填默认返回 10 条                                                                                                                                        |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "dataVer": "20231129213200",
            "ranks": [
                {
                    "accCopyTraderNum": "3536",
                    "aum": "1509265.3238761567721365",
                    "ccy": "USDT",
                    "copyState": "0",
                    "copyTraderNum": "999",
                    "leadDays": "156",
                    "maxCopyTraderNum": "1000",
                    "nickName": "Crypto to the moon",
                    "pnl": "48805.1105999999972258",
                    "pnlRatio": "1.6898",
                    "pnlRatios": [
                        {
                            "beginTs": "1701187200000",
                            "pnlRatio": "1.6744"
                        },
                        {
                            "beginTs": "1700755200000",
                            "pnlRatio": "1.649"
                        }
                    ],
                    "portLink": "https://static.okx.com/cdn/okex/users/headimages/20230624/f49a683aaf5949ea88b01bbc771fb9fc",
                    "traderInsts": [
                        "ICP-USDT-SWAP",
                        "MINA-USDT-SWAP"
                        
                    ],
                    "uniqueCode": "540D011FDACCB47A",
                    "winRatio": "0.6957"
                }
            ],
            "totalPage": "1"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**            | **类型** | **描述**                             |
|:-------------------|:-------|:-----------------------------------|
| dataVer            | String | 排名数据的版本                            |
| totalPage          | String | 总的页数                               |
| ranks              | Array of objects  | 交易员排名信息                            |
| > aum              | String | 带单规模，单位为USDT                       |
| > copyState        | String | 当前跟单状态 <br>`0`: 没在跟单<br>`1`：在跟单 |
| > maxCopyTraderNum | String | 最大跟单人数                             |
| > copyTraderNum    | String | 跟单人数                               |
| > accCopyTraderNum | String | 累计跟单人数                             |
| > portLink         | String | 头像                                 |
| > nickName         | String | 昵称                                 |
| > ccy              | String | 保证金币种                              |
| > uniqueCode       | String | 交易员唯一标识码                           |
| > winRatio         | String | 胜率，0.1 代表 10%                      |
| > leadDays         | String | 带单天数                               |
| > traderInsts      | Array of strings  | 交易员带单的合约列表                         |
| > pnl              | String | 近90日交易员收益，单位为 USDT                 |
| > pnlRatio         | String | 近90日交易员收益率，0.1 代表 10%              |
| > pnlRatios        | Array of objects  | 收益率数据                              |
| >> beginTs       | String  | 当天收益率的开始时间                         |
| >> pnlRatio       | String  | 当天收益率                              |
