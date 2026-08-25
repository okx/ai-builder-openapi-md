---
site: okex
lang: zh
name: GET / 查看项目
method: GET
path: /api/v5/finance/staking-defi/offers
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 查看项目 [financial-product-earn-get-offers]

#### 限速：3次/s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/finance/staking-defi/offers`

> 请求示例

```shell
GET /api/v5/finance/staking-defi/offers

```

```python
import okx.Finance.StakingDefi as StakingDefi

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0" # 实盘:0 , 模拟盘:1

StakingAPI = StakingDefi.StakingDefiAPI(apikey, secretkey, passphrase, False, flag)

result = StakingAPI.get_offers(ccy="USDT")
print(result)

```

#### 请求参数

| 参数           | 类型     | 是否必须 | 描述                                 |
|:-------------|:-------|:-----|:-----------------------------------|
| productId    | String | 否    | 项目ID                               |
| protocolType | String | 否    | 项目类型<br>`defi`：链上赚币 |
| ccy          | String | 否    | 投资币种，如 `BTC`                       |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "DOT",
            "productId": "101",
            "protocol": "Polkadot",
            "protocolType": "defi",
            "term": "0",
            "apy": "0.1767",
            "earlyRedeem": false,
            "state": "purchasable",
            "investData": [
                {
                    "bal": "0",
                    "ccy": "DOT",
                    "maxAmt": "0",
                    "minAmt": "2"
                }
            ],
            "earningData": [
                {
                    "ccy": "DOT",
                    "earningType": "0"
                }
            ],
            "fastRedemptionDailyLimit": "",
            "redeemPeriod": [
                "28D",
                "28D"
            ]
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名                      | 类型              | 描述                                                                                                    |
|--------------------------|-----------------|-------------------------------------------------------------------------------------------------------|
| ccy                      | String          | 币种名称，如 `BTC`                                                                                          |
| productId                | String          | 项目ID                                                                                                  |
| protocol                 | String          | 项目名称                                                                                                  |
| protocolType             | String          | 项目类型<br>`defi`：链上赚币                                                                                   |
| term                     | String          | 项目期限<br>活期为0，其他则显示定期天数                                                                                |
| apy                      | String          | 预估年化<br>如果年化为`7%` ，则该字段为`0.07`                                                                        |
| earlyRedeem              | Boolean         | 项目是否支持提前赎回                                                                                            |
| investData               | Array of objects | 目前用户可用来投资的目标币种信息                                                                                      |
| > ccy                    | String          | 投资币种，如`BTC`                                                                                           |
| > bal                    | String          | 可投数量                                                                                                  |
| > minAmt                 | String          | 最小申购量                                                                                                 |
| > maxAmt                 | String          | 最大可申购量                                                                                                |
| earningData              | Array of objects | 收益信息                                                                                                  |
| > ccy                    | String          | 收益币种，如`BTC`                                                                                           |
| > earningType            | String          | 收益类型<br>`0`：预估收益<br>`1`：累计发放收益                                                                        |
| state                    | String          | 项目状态<br>`purchasable`：可申购<br>`sold_out`：售罄<br>`stop`：暂停申购                                             |
| redeemPeriod             | Array of strings | 赎回期，形式为 [最小赎回时间,最大赎回时间]<br>`H`：小时，`D`：天<br>例 ["1H","24H"] 表示赎回期时1小时到24小时。<br>["14D","14D"] 表示赎回期为14天。 |
| fastRedemptionDailyLimit | String          | 快速赎回每日最高限额<br>如果不支持快速赎回，则返回""                                                                                        |

````
