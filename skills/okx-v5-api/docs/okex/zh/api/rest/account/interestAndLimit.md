---
site: okex
lang: zh
name: 获取借币利率与限额
method: GET
path: /api/v5/account/interest-limits
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取借币利率与限额 [trading-account-rest-api-get-borrow-interest-and-limit]

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/interest-limits`

> 请求示例

```shell
GET /api/v5/account/interest-limits?ccy=BTC

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 获取借币利率与限额
result = accountAPI.get_interest_limits(
    ccy="BTC"
)
print(result)
```

#### 请求参数

| 参数名   | 类型   | 是否必须 | 描述                         |
| :------- | :----- | :------- |:---------------------------|
| type | String | 否       | 借币类型<br>`2`：市场借币<br>默认为`2` |
| ccy | String | 否      | 借贷币种，如 `BTC`               |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "debt": "0.85893159114900247077000000000000",
            "interest": "0.00000000000000000000000000000000",
            "loanAlloc": "",
            "nextDiscountTime": "1729490400000",
            "nextInterestTime": "1729490400000",
            "records": [
                {
                    "availLoan": "",
                    "avgRate": "",
                    "ccy": "BTC",
                    "interest": "0",
                    "loanQuota": "175.00000000",
                    "posLoan": "",
                    "rate": "0.0000276",
                    "surplusLmt": "175.00000000",
                    "surplusLmtDetails": {},
                    "usedLmt": "0.00000000",
                    "usedLoan": "",
                    "interestFreeLiab": "",
                    "potentialBorrowingAmt": ""
                }
            ]
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**                    | **类型** | **描述**                                                                                                                |
|----------------------------|--------|-----------------------------------------------------------------------------------------------------------------------|
| debt                       | String | 当前负债，单位为`USD`                                                                                                         |
| interest                   | String | 当前记息，单位为`USD`<br>仅适用于`市场借币`                                                                                           |
| nextDiscountTime           | String | 下次扣息时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                |
| nextInterestTime           | String | 下次计息时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                |
| loanAlloc                  | String | <del>当前交易账户尊享借币可用额度的比率（百分比）<br>1. 范围为[0, 100]. 精度为 0.01% (2位小数)<br>2. 0 代表母账户没有为子账户分配；<br>3. "" 代表母子账户共享</del><br>已废弃 |
| records                    | Array of objects  | 各币种详细信息                                                                                                               |
| > ccy                      | String | 借贷币种，如 `BTC`                                                                                                          |
| > rate                     | String | 当前日借币利率                                                                                                            |
| > loanQuota                | String | 母账户维度借币限额<br>如果已配置可用额度，该字段代表当前交易账户的借币限额                                                                               |
| > usedLmt                  | String | 当前账户已借额度<br>如果已配置可用额度，该字段代表当前交易账户的已借额度	                                                                               |
| > interest                 | String | 已计未扣利息<br>仅适用于`市场借币`                                                                                                  |
| > interestFreeLiab               | String          | 当前账户免息负债                                 |
| > potentialBorrowingAmt               | String          | 当前账户潜在借币量                                 |
| > surplusLmt               | String | 母子账户剩余可借<br>如果已配置可用额度，该字段代表当前交易账户的剩余可借                                                                                |
| > surplusLmtDetails        | Object  | <del>母子账户剩余可借额度详情，母子账户剩余可借额度的值取该数组中的最小值，可以用来判断是什么原因导致可借额度不足<br>仅适用于`尊享借币`</del><br>已废弃	                               |
| >> allAcctRemainingQuota   | String | 母子账户剩余额度	                                                                                                             |
| >> curAcctRemainingQuota   | String | 当前账户剩余额度<br>仅适用于为子账户分配限额的场景	                                                                                          |
| >> platRemainingQuota      | String | 平台剩余额度，当平台剩余额度大于`curAcctRemainingQuota`或者`allAcctRemainingQuota`时，会显示大于某个值，如">1000"	                                  |
| > posLoan                  | String | <del>当前账户负债占用（锁定额度内）<br>仅适用于`尊享借币`</del><br>已废弃	                                                                      |
| > availLoan                | String | <del>当前账户剩余可用（锁定额度内）<br>仅适用于`尊享借币`</del><br>已废弃	                                                                      |
| > usedLoan                 | String | <del>当前账户已借额度<br>仅适用于`尊享借币`</del><br>已废弃	                                                                             |
| > avgRate                  | String | <del>已借币种平均每小时利率，仅适用于`尊享借币`</del><br>已废弃                                                                             |
