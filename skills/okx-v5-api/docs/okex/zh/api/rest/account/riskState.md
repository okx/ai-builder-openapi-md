---
site: okex
lang: zh
name: 查看账户特定风险状态
method: GET
path: /api/v5/account/risk-state
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 查看账户特定风险状态 [trading-account-rest-api-get-account-risk-state]

仅适用于PM账户

#### 限速：10次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/risk-state`

> 请求示例

```shell
GET /api/v5/account/risk-state

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 查看账户持仓风险
result = accountAPI.get_account_position_risk()
print(result)
```

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "atRisk": false,
            "atRiskIdx": [],
            "atRiskMgn": [],
            "ts": "1635745078794"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**  | **类型**  | **描述**                                                     |
| ----------- |---------| ------------------------------------------------------------ |
| atRisk          | Boolean | 自动借币模式下的账户风险状态<br>true： 当前账户为特定风险状态<br>false： 当前不是特定风险状态   |
| atRiskIdx       | Array of strings   | 衍生品的risk unit列表          |
| atRiskMgn       | Array of strings   | 杠杆的risk unit列表            |
| ts              | String  | 接口数据返回时间 ，Unix时间戳的毫秒数格式，如 `1597026383085`                                            |

<aside class="notice">
当账户进入特定风险状态后，仅可以委托降低账户风险方向的IOC类型订单.
</aside>
