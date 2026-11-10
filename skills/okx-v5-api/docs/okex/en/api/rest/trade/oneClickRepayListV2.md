---
site: okex
lang: en
name: GET / One-click repay currency list (New)
method: GET
path: /api/v5/trade/one-click-repay-currency-list-v2
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / One-click repay currency list (New)

Get list of debt currency data and repay currencies. Only applicable to `SPOT mode`/`Multi-currency margin mode`/`Portfolio margin mode`.

#### Rate Limit: 1 request per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/trade/one-click-repay-currency-list-v2`

> Request Example

```shell
GET /api/v5/trade/one-click-repay-currency-list-v2
```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"
flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag,debug=True) 
result = tradeAPI.get_oneclick_repay_list_v2()
print(result)
```

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "debtData": [
                {
                    "debtAmt": "100",
                    "debtCcy": "USDC"
                }
            ],
            "repayData": [
                {
                    "repayAmt": "1.000022977",
                    "repayCcy": "BTC"
                },
                {
                    "repayAmt": "4998.0002397",
                    "repayCcy": "USDT"
                },
                {
                    "repayAmt": "100",
                    "repayCcy": "OKB"
                },
                {
                    "repayAmt": "1",
                    "repayCcy": "ETH"
                },
                {
                    "repayAmt": "100",
                    "repayCcy": "USDC"
                }
            ]
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter  | Type             | Description                                                                                                         |
| :--------- |:-----------------|:--------------------------------------------------------------------------------------------------------------------|
| debtData   | Array of objects | Debt currency data list                                                                                             |
| > debtCcy   | String           | Debt currency                                                                                                       |
| > debtAmt   | String           | Debt currency amount<br>Including principal and interest                                                            |
| repayData  | Array of objects | Repay currency data list                                                                                            |
| > repayCcy  | String           | Repay currency                                                                                                      |
| > repayAmt  | String           | Repay currency's available balance amount                                                                           |
