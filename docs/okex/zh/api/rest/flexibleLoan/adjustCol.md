---
site: okex
lang: zh
name: POST / 调整抵押物
method: POST
path: /api/v5/finance/flexible-loan/adjust-collateral
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 调整抵押物 [financial-product-flexible-loan-post-adjust-collateral]

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/finance/flexible-loan/adjust-collateral`

> 请求示例

```shell
POST /api/v5/finance/flexible-loan/adjust-collateral
body
{
    "type":"add",
    "ordId": "12345",
    "collateralCcy": "BTC",
    "collateralAmt": "0.1"
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
result = flexibleLoanAPI.adjust_collateral(type="add", ordId="12345", collateralCcy="USDT", collateralAmt="1")
print(result)
```

#### 请求参数

| 参数            | 类型               | 是否必须 | 描述                                    |
|:--------------|:-----------------|:-----|:--------------------------------------|
| type          | String           | 是    | 操作类型<br>`add`：补充抵押物<br>`reduce`：减少抵押物 |
| collateralCcy | String           | 是    | 抵押物币种，如 `BTC`                         |
| collateralAmt | String           | 是    | 抵押物数量                                 |
| ordId       | String           | 否    | 活期借币订单 ID。<br>如果不传 `ordId`，系统将默认对起始时间最早的现存订单进行操作。<br>如果没有现存订单，系统将返回错误 `51063` |

对于 E-Mode 订单，`add` 请求的抵押币种必须与订单记录的 E-Mode 配对抵押币种一致；否则将返回错误码 `51785`。

> 返回结果

```json
{
    "code": "0",
    "data": [
    ],
    "msg": ""
}
```

#### 返回参数

code = `0` 代表请求已被接受(不代表处理成功)
