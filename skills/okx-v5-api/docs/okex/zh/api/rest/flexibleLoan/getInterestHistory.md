---
site: okex
lang: zh
name: GET / 计息记录
method: GET
path: /api/v5/finance/flexible-loan/interest-accrued
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 计息记录 [financial-product-flexible-loan-get-accrued-interest]

获取最近30天的计息记录。

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/finance/flexible-loan/interest-accrued`

> 请求示例

```shell
GET /api/v5/finance/flexible-loan/interest-accrued
```

```python
from okx.Finance import FlexibleLoan

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

flexibleLoanAPI = FlexibleLoan.FlexibleLoanAPI(apikey, secretkey, passphrase, False, flag)
result = flexibleLoanAPI.interest_accrued()
print(result)
```

#### 请求参数

| 参数      | 类型               | 是否必须 | 描述                                          |
|:--------|:-----------------|:-----|:--------------------------------------------|
| ccy    | String    | 否    | 借贷币种，如 `BTC`                                |
| after   | String    | 否    | 请求此 ID 之前（更旧的数据）的分页内容，传的值为对应接口的`refId`（不包含） |
| before  | String    | 否    | 请求此 ID 之后（更新的数据）的分页内容，传的值为对应接口的`refId`（不包含） |
| limit   | String    | 否    | 返回结果的数量，最大为`100`，默认`100`条                   |
| ordId | String    | 否    | 活期借币订单 ID。<br>如果不传 `ordId`，系统将返回所有订单数据 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "USDC",
            "interest": "0.00004054",
            "interestRate": "0.41",
            "loan": "0.86599309",
            "refId": "17319133035195744",
            "ts": "1731913200000"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**        | **类型**      | **描述**                                |
|----------------|-------------|---------------------------------------|
| refId          | String      | 对应记录ID                                |
| ccy            | String      | 币种，如 `BTC`                            |
| loan           | String      | 计息时负债                                 |
| interest       | String      | 利息                                    |
| interestRate   | String      | 年化利率，如 `0.01`代表`1%`                   |
| ts             | String      | 计息时间，Unix 时间戳为毫秒数格式，如 `1597026383085` |
