---
site: my
lang: zh
name: GET / 查看活跃订单
method: GET
path: /api/v5/finance/staking-defi/orders-active
module: finance
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### GET / 查看活跃订单 [financial-product-earn-get-active-orders]

#### 限速：3次/s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/finance/staking-defi/orders-active`

> 请求示例

```shell
GET /api/v5/finance/staking-defi/orders-active

```

```python
import okx.Finance.StakingDefi as StakingDefi

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

StakingAPI = StakingDefi.StakingDefiAPI(apikey, secretkey, passphrase, False, flag)

result = StakingAPI.get_activity_orders()
print(result)
```

#### 请求参数

| 参数           | 类型     | 是否必须 | 描述                                                                       |
|:-------------|:-------|:-----|:-------------------------------------------------------------------------|
| productId    | String | 否    | 项目ID                                                                     |
| protocolType | String | 否    | 项目类型<br>`defi`：链上赚币                                    |
| ccy          | String | 否    | 投资币种，如 `BTC`                                                             |
| state        | String | 否    | 订单状态<br>`8`: 待上车（预约中）<br>`13`: 订单取消中<br>`9`: 上链中<br>`1`: 收益中<br>`2`: 赎回中 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ordId": "2413499",
            "ccy": "DOT",
            "productId": "101",
            "state": "1",
            "protocol": "Polkadot",
            "protocolType": "defi",
            "term": "0",
            "apy": "0.1014",
            "investData": [
                {
                    "ccy": "DOT",
                    "amt": "2"
                }
            ],
            "earningData": [
                {
                    "ccy": "DOT",
                    "earningType": "0",
                    "earnings": "0.10615025"
                }
            ],
            "purchasedTime": "1729839328000",
            "tag": "",
            "estSettlementTime": "",
            "cancelRedemptionDeadline": "",
            "fastRedemptionData": []
        },
        {
            "ordId": "2213257",
            "ccy": "USDT",
            "productId": "4005",
            "state": "1",
            "protocol": "On-Chain Defi",
            "protocolType": "defi",
            "term": "0",
            "apy": "0.0323",
            "investData": [
                {
                    "ccy": "USDT",
                    "amt": "1"
                }
            ],
            "earningData": [
                {
                    "ccy": "USDT",
                    "earningType": "0",
                    "earnings": "0.02886582"
                },
                {
                    "ccy": "COMP",
                    "earningType": "1",
                    "earnings": "0.0000627"
                }
            ],
            "purchasedTime": "1725345790000",
            "tag": "",
            "estSettlementTime": "",
            "cancelRedemptionDeadline": "",
            "fastRedemptionData": []
        },
        {
            "ordId": "2210943",
            "ccy": "USDT",
            "productId": "4005",
            "state": "1",
            "protocol": "On-Chain Defi",
            "protocolType": "defi",
            "term": "0",
            "apy": "0.0323",
            "investData": [
                {
                    "ccy": "USDT",
                    "amt": "1"
                }
            ],
            "earningData": [
                {
                    "ccy": "USDT",
                    "earningType": "0",
                    "earnings": "0.02891823"
                },
                {
                    "ccy": "COMP",
                    "earningType": "1",
                    "earnings": "0.0000632"
                }
            ],
            "purchasedTime": "1725280801000",
            "tag": "",
            "estSettlementTime": "",
            "cancelRedemptionDeadline": "",
            "fastRedemptionData": []
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名                      | 类型              | 描述                                                                  |
|--------------------------|-----------------|---------------------------------------------------------------------|
| ccy                      | String          | 币种名称，如 `BTC`                                                        |
| ordId                    | String          | 订单ID                                                                |
| productId                | String          | 项目ID                                                                |
| state                    | String          | 订单状态<br>`8`：待上车（预约中）<br>`13`：订单取消中<br>`9`：上链中<br>`1`：收益中<br>`2`：赎回中 |
| protocol                 | String          | 项目名称                                                                |
| protocolType             | String          | 项目类型<br>`defi`：链上赚币                                                 |
| term                     | String          | 项目期限<br>活期为0，其他则显示定期天数                                              |
| apy                      | String          | 预估年化<br>如果年化为7% ，则该字段为0.07<br>保留到小数点后4位（截位）                         |
| investData               | Array of objects | 用户投资信息                                                              |
| > ccy                    | String          | 投资币种，如 `BTC`                                                        |
| > amt                    | String          | 已投资数量                                                               |
| earningData              | Array of objects | 收益信息                                                                |
| > ccy                    | String          | 收益币种，如 `BTC`                                                        |
| > earningType            | String          | 收益类型<br>`0`：预估收益<br>`1`：实际到账收益                                      |
| > earnings               | String          | 收益数量                                                                |
| fastRedemptionData       | Array of objects | 快速赎回信息                                                              |
| > ccy                    | String          | 快速赎回币种，如 `BTC`                                                      |
| > redeemingAmt           | String          | 赎回中的数量                                                              |
| purchasedTime            | String          | 用户订单创建时间，值为时间戳，Unix时间戳为毫秒数格式，如 `1597026383085`                      |
| estSettlementTime        | String          | 预估赎回到账时间                                                            |
| cancelRedemptionDeadline | String          | 撤销赎回申请截止时间                                                          |
| tag                      | String          | 订单标签                                                                |
