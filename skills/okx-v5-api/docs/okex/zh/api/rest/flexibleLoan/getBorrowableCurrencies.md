---
site: okex
lang: zh
name: GET / 可借币种列表
method: GET
path: /api/v5/finance/flexible-loan/borrow-currencies
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 可借币种列表 [financial-product-flexible-loan-get-borrowable-currencies]

获取可借币种列表

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/finance/flexible-loan/borrow-currencies`

> 请求示例

```shell
GET /api/v5/finance/flexible-loan/borrow-currencies
```

```python
from okx.Finance import FlexibleLoan

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

flexibleLoanAPI = FlexibleLoan.FlexibleLoanAPI(apikey, secretkey, passphrase, False, flag)
result = flexibleLoanAPI.borrow_currencies()
print(result)
```

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "borrowCcy": "USDT"
        },
        {
            "borrowCcy": "USDC"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**         | **类型**         | **描述**       |
|-----------------|----------------|--------------|
| borrowCcy       | String     | 可借币种，如 `BTC` |
