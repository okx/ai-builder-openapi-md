---
site: my
lang: en
name: POST / Trade one-click repay (New)
method: POST
path: /api/v5/trade/one-click-repay-v2
module: trade
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### POST / Trade one-click repay (New)

Trade one-click repay to repay debts. Only applicable to `SPOT mode`/`Multi-currency margin mode`/`Portfolio margin mode`.

#### Rate Limit: 1 request per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/trade/one-click-repay-v2`

> Request Example

```shell
POST /api/v5/trade/one-click-repay-v2
body
{
    "debtCcy": "USDC", 
    "repayCcyList": ["USDC","BTC"] 
}
```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"
flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag,debug=True)
result = tradeAPI.oneclick_repay_v2("USDC",["USDC","BTC"])
print(result)
```

#### Request Parameters

| Parameter    | Type            | Required | Description                                                                                                                                                        |
|:-------------|:----------------| :------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| debtCcy      | String          | Yes       | Debt currency                                                                                                                                                      |
| repayCcyList | Array of strings | Yes       | Repay currency list, e.g. ["USDC","BTC"]<br>The priority of currency to repay is consistent with the order in the array. (The first item has the highest priority) |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "debtCcy": "USDC",
            "repayCcyList": [
                "USDC",
                "BTC"
            ],
            "ts": "1742192217514"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter     | Type            | Description                                                                                        |
|:--------------|:----------------|:---------------------------------------------------------------------------------------------------|
| debtCcy       | String          | Debt currency                                                                                    |
| repayCcyList  | Array of strings | Repay currency list, e.g. ["USDC","BTC"]                                                                               |
| ts            | String          | Request time, Unix timestamp format in milliseconds, e.g. `1597026383085`                            |
