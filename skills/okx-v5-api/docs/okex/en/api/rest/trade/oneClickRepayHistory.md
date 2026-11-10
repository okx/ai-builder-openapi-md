---
site: okex
lang: en
name: GET / One-click repay history
method: GET
path: /api/v5/trade/one-click-repay-history
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / One-click repay history

Get the history and status of one-click repay trades in the past 7 days. Only applicable to `Multi-currency margin`/`Portfolio margin`.

#### Rate Limit: 1 request per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/trade/one-click-repay-history`

> Request Example

```shell
GET /api/v5/trade/one-click-repay-history
```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# Get the history of one-click repay trades
result = tradeAPI.oneclick_repay_history()
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                         |
| :------- | :----- | :------- | :-------------------------------------------------------------------- |
| after    | String | No       | Pagination of data to return records earlier than the requested time, Unix timestamp format in milliseconds, e.g. 1597026383085   |
| before   | String | No       | Pagination of data to return records newer than the requested time, Unix timestamp format in milliseconds, e.g. 1597026383085  |
| limit    | String | No       | Number of results per request. The maximum is 100. The default is 100.  |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "debtCcy": "USDC",
            "fillDebtSz": "6950.4865447900000000",
            "fillRepaySz": "4.3067975995094930",
            "repayCcy": "ETH",
            "status": "filled",
            "uTime": "1661256148746"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter  | Type     | Description                                                                              |
| :--------- | :------- | :--------------------------------------------------------------------------------------- |
| debtCcy    | String   | Debt currency type                                                                |
| fillDebtSz     | String   | Amount of debt currency transacted                                                                |
| repayCcy   | String   | Repay currency type                                                                      |
| fillRepaySz    | String   | Amount of repay currency transacted                                                                      |
| status     | String   | Current status of one-click repay <br>`running`: Running <br>`filled`: Filled <br>`failed`: Failed             |
| uTime      | String   | Trade time, Unix timestamp format in milliseconds, e.g. 1597026383085                                 |
