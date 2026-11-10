---
site: my
lang: zh
name: GET / 查看历史订单
method: GET
path: /api/v5/finance/staking-defi/orders-history
module: finance
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### GET / 查看历史订单 [financial-product-earn-get-order-history]

#### 限速：3次/s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/finance/staking-defi/orders-history`

> 请求示例

```shell
GET /api/v5/finance/staking-defi/orders-history

```

```python
import okx.Finance.StakingDefi as StakingDefi

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

StakingAPI = StakingDefi.StakingDefiAPI(apikey, secretkey, passphrase, False, flag)

result = StakingAPI.get_orders_history()
print(result)
```

#### 请求参数

| 参数 | 类型   | 是否必须 | 描述                    |
| :--- | :----- | :------- | :---------------------- |
| productId     | String | 否    | 项目ID  |
| protocolType  | String | 否    | 项目类型<br>`defi`：链上赚币 |
| ccy           | String | 否    | 投资币种，如 `BTC` |
| after         | String | 否    | 请求此ID之前（更旧的数据）的分页内容，传的值为对应接口的`ordId` |
| before        | String | 否    | 请求此ID之后（更新的数据）的分页内容，传的值为对应接口的`ordId` |
| limit         | String | 否    | 返回结果的数量，默认100条，最大值为100条         |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
       {
            "ordId": "1579252",
            "ccy": "DOT",
            "productId": "101",
            "state": "3",
            "protocol": "Polkadot",
            "protocolType": "defi",
            "term": "0",
            "apy": "0.1704",
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
                    "realizedEarnings": "0"
                }
            ],
            "purchasedTime": "1712908001000",
            "redeemedTime": "1712914294000",
            "tag": ""
       }
    ]
}
```

#### 返回参数

| 参数名                | 类型     | 描述                                             |
|--------------------|--------|------------------------------------------------|
| ccy                | String | 币种名称，如 `BTC`                                   |
| ordId              | String | 订单ID                                           |
| productId          | String | 项目ID                                           |
| state              | String | 订单状态<br>3: 订单已完成（包含撤销和已赎回两种状态）                 |
| protocol           | String | 项目名称                                           |
| protocolType       | String | 项目类型<br>`defi`：链上赚币                            |
| term               | String | 项目期限<br>活期为0，其他则显示定期天数                         |
| apy                | String | 预估年化<br>如果年化为7% ，则该字段为0.07<br>保留到小数点后4位（截位）    |
| investData         | Array of objects  | 用户投资信息                                         |
| > ccy              | String | 投资币种，如`BTC`                                    |
| > amt              | String | 已投资数量                                          |
| earningData        | Array of objects  | 收益信息                                           |
| > ccy              | String | 收益币种，如`BTC`                                    |
| > earningType      | String | 收益类型<br>`0`：预估收益<br>`1`：实际到账收益                 |
| > realizedEarnings | String | 已赎回订单累计收益<br>该字段仅在订单处于赎回状态时有效                  |
| purchasedTime      | String | 用户订单创建时间，值为时间戳，Unix时间戳为毫秒数格式，如 `1597026383085` |
| redeemedTime       | String | 用户订单赎回时间，值为时间戳，Unix时间戳为毫秒数格式，如 `1597026383085` |
| tag                | String | 订单标签                                           |
