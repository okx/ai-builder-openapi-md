---
site: okex
lang: zh
name: POST / 最大可借
method: POST
path: /api/v5/finance/flexible-loan/max-loan
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 最大可借 [financial-product-flexible-loan-post-maximum-loan-amount]

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/finance/flexible-loan/max-loan`

> 请求示例

```shell
POST /api/v5/finance/flexible-loan/max-loan
body
{
    "ordId": "12345",
    "borrowCcy": "USDT"
}

```

```python
from okx.Finance import FlexibleLoan

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

flexibleLoanAPI = FlexibleLoan.FlexibleLoanAPI(apikey, secretkey, passphrase, False, flag)
result = flexibleLoanAPI.max_loan(ordId="12345", borrowCcy="USDT")
print(result)
```

#### 请求参数

| 参数        | 类型               | 是否必须 | 描述             |
|:----------|:-----------------|:-----|:---------------|
| borrowCcy      | String           | 是    | 借币币种，如 `USDT`  |
| ordId        | String           | 否    | 活期借币订单 ID。<br>如果不传 `ordId`，系统将默认对起始时间最早的现存订单进行操作。<br>如果没有现存订单，系统将返回空数据。 |
| supCollateral  | Array of objects  | 否    | 补充抵押资产信息       |
| > ccy          | String           | 是    | 币种，如 `BTC`     |
| > amt          | String           | 是     | 数量             |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "borrowCcy": "USDT",
            "maxLoan": "0.01113",
            "notionalUsd": "0.01113356",
            "remainingQuota": "3395000"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**            | **类型**          | **描述**        |
|--------------------|-----------------|---------------|
| borrowCcy          | String | 借币币种，如 `USDT` |
| maxLoan            | String | 最大可借数量        |
| notionalUsd        | String | 最大可借美元价值      |
| remainingQuota     | String | 剩余可借额度，单位为`borrowCcy`    |
