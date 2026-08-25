---
site: okex
lang: zh
name: GET / 借贷历史
method: GET
path: /api/v5/finance/flexible-loan/loan-history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 借贷历史 [financial-product-flexible-loan-get-loan-history]

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/finance/flexible-loan/loan-history`

> 请求示例

```shell
GET /api/v5/finance/flexible-loan/loan-history
```

```python
from okx.Finance import FlexibleLoan

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

flexibleLoanAPI = FlexibleLoan.FlexibleLoanAPI(apikey, secretkey, passphrase, False, flag)
result = flexibleLoanAPI.loan_history()
print(result)
```

#### 请求参数

| 参数      | 类型               | 是否必须 | 描述                                                                                                                                                                                                                                                                                                                                                        |
|:--------|:-----------------|:-----|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| type    | String    | 否    | 操作类型<br>`borrowed`：借入<br>`repaid`：还币<br>`collateral_locked`：锁定质押物<br>`collateral_released`：释放质押物<br>`forced_repayment_buy`：自动换币买入<br>`forced_repayment_sell`：自动换币卖出<br>`forced_liquidation`：强制平仓<br>`partial_liquidation`：强制减仓<br>`sell_collateral`：卖出质押资产<br>`buy_transition_coin`：购买中介币种<br>`sell_transition_coin`：卖出中介币种<br>`buy_borrowed_coin`：购买借币币种 |
| after   | String    | 否    | 请求此 ID 之前（更旧的数据）的分页内容，传的值为对应接口的`refId`（不包含）                                                                                                                                                                                                                                                                                                               |
| before  | String    | 否    | 请求此 ID 之后（更新的数据）的分页内容，传的值为对应接口的`refId`（不包含）                                                                                                                                                                                                                                                                                                               |
| limit   | String    | 否    | 返回结果的数量，最大为`100`，默认`100`条                                                                                                                                                                                                                                                                                                                                 |
| ordId | String    | 否    | 活期借币订单 ID。<br>如果不传 `ordId`，系统将返回所有订单数据 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "amt": "-0.001",
            "ccy": "DOT",
            "refId": "17316594851045086",
            "ts": "1731659485000",
            "type": "collateral_locked"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**               | **类型**          | **描述**                                |
|-----------------------|-----------------|---------------------------------------|
| refId         | String               | 对应记录ID                                |
| type          | String               | 操作类型                                  |
| ccy           | String               | 币种，如 `BTC`                            |
| amt           | String               | 数量                                    |
| ts           | String               | 操作发生时间，Unix 时间戳为毫秒数格式，如 `1597026383085` |
