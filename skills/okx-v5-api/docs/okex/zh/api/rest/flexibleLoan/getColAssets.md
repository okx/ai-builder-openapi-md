---
site: okex
lang: zh
name: GET / 可抵押资产
method: GET
path: /api/v5/finance/flexible-loan/collateral-assets
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 可抵押资产 [financial-product-flexible-loan-get-collateral-assets]

获取活期借币现存订单下持有的抵押资产信息（仅支持资金账户中的资产）。该接口查询的是订单下实际持有的抵押资产，若不存在订单，则无法返回泛指的、用户可用作抵押的资产列表

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/finance/flexible-loan/collateral-assets`

> 请求示例

```shell
GET /api/v5/finance/flexible-loan/collateral-assets?ordId=12345
```

```python
from okx.Finance import FlexibleLoan

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

flexibleLoanAPI = FlexibleLoan.FlexibleLoanAPI(apikey, secretkey, passphrase, False, flag)
result = flexibleLoanAPI.collateral_assets(ordId="12345")
print(result)
```

#### 请求参数

| 参数        | 类型   | 是否必须 | 描述         |
|:----------| :----- |:-----|:-----------|
| ccy     | String    | 否    | 币种，如 `BTC` |
| ordId | String    | 否    | 活期借币订单 ID。<br>如果不传 `ordId`，系统将默认对起始时间最早的现存订单进行操作。<br>如果没有现存订单，系统将返回空数据。 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "assets": [
                {
                    "amt": "1.7921483143067599",
                    "ccy": "BTC",
                    "notionalUsd": "158292.621793314105231"
                },
                {
                    "amt": "1.9400755578876945",
                    "ccy": "ETH",
                    "notionalUsd": "6325.6652712507628946"
                },
                {
                    "amt": "63.9795959720319628",
                    "ccy": "USDT",
                    "notionalUsd": "64.3650372635940345"
                }
            ]
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**       | **类型**          | **描述**  |
|---------------|-----------------|---------|
| assets        | Array of objects | 可抵押资产信息 |
| > ccy         | String          | 币种，如 `BTC` |
| > amt         | String          | 可用数量 |
| > notionalUsd | String          | 可抵押资产的美金价值 |
