---
site: okex
lang: en
name: POST / Trade one-click repay
method: POST
path: /api/v5/trade/one-click-repay
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Trade one-click repay

Trade one-click repay to repay cross debts. Isolated debts are not applicable.
The maximum repayment amount is based on the remaining available balance of funding and trading accounts.
Only applicable to `Multi-currency margin`/`Portfolio margin`.

#### Rate Limit: 1 request per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/trade/one-click-repay`

> Request Example

```shell
POST /api/v5/trade/one-click-repay
body
{
    "debtCcy": ["ETH","BTC"], 
    "repayCcy": "USDT" 
}
```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# Trade one-click repay to repay cross debts
result = tradeAPI.oneclick_repay(
    debtCcy=["ETH", "BTC"],
    repayCcy="USDT"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                         |
| :------- | :----- | :------- | :-------------------------------------------------------------------- |
| debtCcy  | Array of strings  | Yes       | Debt currency type <br>Maximum 5 currencies can be selected in one order. If there are multiple currencies, separate them with commas.        |
| repayCcy | String | Yes       | Repay currency type <br>Only one receiving currency type can be selected in one order and cannot be the same as the small payment currencies.               |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "debtCcy": "ETH", 
            "fillDebtSz": "0.01023052",
            "fillRepaySz": "30", 
            "repayCcy": "USDT", 
            "status": "filled",
            "uTime": "1646188520338"
        },
        {
            "debtCcy": "BTC", 
            "fillFromSz": "3",
            "fillToSz": "60,221.15910001",
            "repayCcy": "USDT",
            "status": "filled",
            "uTime": "1646188520338"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter  | Type     | Description                                                                              |
| :--------- | :------- | :--------------------------------------------------------------------------------------- |
| status     | String   | Current status of one-click repay <br>`running`: Running <br>`filled`: Filled <br>`failed`: Failed             |
| debtCcy    | String   | Debt currency type                                                                |
| repayCcy   | String   | Repay currency type                                                                      |
| fillDebtSz | String   | Filled amount of debt currency                                                                |
| fillRepaySz| String   | Filled amount of repay currency                                                                       |
| uTime      | String   | Trade time, Unix timestamp format in milliseconds, e.g. 1597026383085                                 |
