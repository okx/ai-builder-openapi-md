---
site: okex
lang: zh
name: GET / 抵押物最大可赎回数量
method: GET
path: /api/v5/finance/flexible-loan/max-collateral-redeem-amount
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 抵押物最大可赎回数量 [financial-product-flexible-loan-get-maximum-collateral-redeem-amount]

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/finance/flexible-loan/max-collateral-redeem-amount`

> 请求示例

```shell
GET /api/v5/finance/flexible-loan/max-collateral-redeem-amount?ccy=USDT&ordId=12345

```

```python
from okx.Finance import FlexibleLoan

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

flexibleLoanAPI = FlexibleLoan.FlexibleLoanAPI(apikey, secretkey, passphrase, False, flag)
result = flexibleLoanAPI.max_collateral_redeem_amount(ordId="12345", ccy="USDT")
print(result)
```

#### 请求参数

| 参数        | 类型              | 是否必须 | 描述            |
|:----------|:----------------|:-----|:--------------|
| ccy       | String          | 是    | 抵押物币种，如 `USDT` |
| ordId   | String          | 否    | 活期借币订单 ID。<br>如果不传 `ordId`，系统将默认对起始时间最早的现存订单进行操作。<br>如果没有现存订单，系统将返回空数据。 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "USDT",
            "maxRedeemAmt": "1"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**        | **类型**          | **描述**         |
|----------------|-----------------|----------------|
| ccy            | String | 抵押物币种，如 `USDT` |
| maxRedeemAmt   | String | 抵押物最大可赎回数量     |
