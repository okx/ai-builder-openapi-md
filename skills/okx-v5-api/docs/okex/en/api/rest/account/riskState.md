---
site: okex
lang: en
name: Get account risk state
method: GET
path: /api/v5/account/risk-state
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get account risk state

Only applicable to Portfolio margin account

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/risk-state`

> Request Example

```shell
GET /api/v5/account/risk-state
```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Get account risk state
result = accountAPI.get_account_position_risk()
print(result)
```

> Response Example

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

#### Response Parameters

| **Parameters** | **Types** | **Description**                                                                                                   |
| ----------- |-----------| ------------------------------------------------------------ |
| atRisk          | Boolean   | Account risk status in auto-borrow mode <br>true:  the account is currently in a specific risk state <br>false:  the account is currently not in a specific risk state |
| atRiskIdx       | Array of strings     | derivatives risk unit list        |
| atRiskMgn       | Array of strings     | margin risk unit list         |
| ts              | String    | Unix timestamp format in milliseconds, e.g.`1597026383085`                                            |
