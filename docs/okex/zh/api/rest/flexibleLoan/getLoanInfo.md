---
site: okex
lang: zh
name: GET / 借贷信息
method: GET
path: /api/v5/finance/flexible-loan/loan-info
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 借贷信息 [financial-product-flexible-loan-get-loan-info]

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/finance/flexible-loan/loan-info`

> 请求示例

```shell
GET /api/v5/finance/flexible-loan/loan-info
```

```python
from okx.Finance import FlexibleLoan

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

flexibleLoanAPI = FlexibleLoan.FlexibleLoanAPI(apikey, secretkey, passphrase, False, flag)
result = flexibleLoanAPI.loan_info()
print(result)
```

#### 请求参数

| 参数      | 类型     | 是否必须 | 描述                                                       |
|:--------|:-------|:-----|:---------------------------------------------------------|
| ordId | String | 否    | 活期借币订单 ID。<br>如果不传 `ordId`，系统将返回所有现存订单数据 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ordId": "12345",
            "collateralData": [
                {
                    "amt": "0.0000097",
                    "ccy": "COMP"
                },
                {
                    "amt": "0.78",
                    "ccy": "STX"
                },
                {
                    "amt": "0.001",
                    "ccy": "DOT"
                },
                {
                    "amt": "0.05357864",
                    "ccy": "LUNA"
                }
            ],
            "collateralNotionalUsd": "1.5078763",
            "curLTV": "0.5742",
            "liqLTV": "0.8374",
            "loanData": [
                {
                    "amt": "0.86590608",
                    "ccy": "USDC"
                }
            ],
            "loanNotionalUsd": "0.8661285",
            "marginCallLTV": "0.7374",
            "riskWarningData": {
                "instId": "",
                "liqPx": ""
            }
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**               | **类型**          | **描述**                                                                                         |
|-----------------------|-----------------|------------------------------------------------------------------------------------------------|
| ordId               | String | 订单 ID                                                                                                                                                                                   |
| loanNotionalUsd       | String | 借币资产美金价值                                                                                       |
| loanData              | Array of objects | 借币数据                                                                                           |
| > ccy                 | String | 借贷币种                                                                                           |
| > amt                 | String | 借贷数量                                                                                           |
| collateralNotionalUsd | String | 调整后的抵押物美金价值                                                                                    |
| collateralData        | Array of objects | 抵押资产数据                                                                                         |
| > ccy                 | String | 抵押币种                                                                                           |
| > amt                 | String | 抵押数量                                                                                           |
| riskWarningData       | Object | 风险预警信息                                                                                         |
| > instId              | String | 清算交易产品，如 `BTC-USDT`<br>仅当质押物和借币都只有一种时，该字段有效。其他情况返回""。                                          |
| > liqPx               | String | 清算价格<br>清算价格的单位为交易产品的计价币，如 `BTC-USDT`中的`USDT`。<br>仅当质押物和借币都只有一种时，该字段有效。其他情况返回""。               |
| curLTV                | String | 当前质押率，如 `0.1`代表`10%`<br>注：LTV(Loan-to-Value，贷款价值比)                                             |
| marginCallLTV         | String | 预警质押率，如 `0.1`代表`10%`<br>您的质押率达到预警质押率时，系统将会提示您当前质押率过高，即将触发强平。                              |
| liqLTV                | String | 强平质押率，如 `0.1`代表`10%`<br>若您的借贷达到强平质押率并被强平，您将损失质押物及已完成的还款。 |
