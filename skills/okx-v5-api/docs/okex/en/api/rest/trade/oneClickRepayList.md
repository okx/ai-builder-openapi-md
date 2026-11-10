---
site: okex
lang: en
name: GET / One-click repay currency list
method: GET
path: /api/v5/trade/one-click-repay-currency-list
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / One-click repay currency list

Get list of debt currency data and repay currencies. Debt currencies include both cross and isolated debts. Only applicable to `Multi-currency margin`/`Portfolio margin`.

#### Rate Limit: 1 request per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/trade/one-click-repay-currency-list`

> Request Example

```shell
GET /api/v5/trade/one-click-repay-currency-list
```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# Get list of debt currency data and repay currencies
result = tradeAPI.get_oneclick_repay_list()
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                         |
| :------- | :----- | :------- | :-------------------------------------------------------------------- |
| debtType | String | No        | Debt type <br>`cross`: cross <br>`isolated`: isolated        |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "debtData": [
                {
                    "debtAmt": "29.653478",
                    "debtCcy": "LTC"
                },
                {
                    "debtAmt": "237803.6828295906051002",
                    "debtCcy": "USDT"
                }
            ],
            "debtType": "cross",
            "repayData": [
                {
                    "repayAmt": "0.4978335419825104",
                    "repayCcy": "ETH"
                }
            ]
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter  | Type     | Description                                                                                                          |
| :--------- | :------- |:---------------------------------------------------------------------------------------------------------------------|
| debtData   | Array of objects    | Debt currency data list                                                                                              |
| > debtCcy   | String   | Debt currency                                                                                                        |
| > debtAmt   | String   | Debt currency amount <br>Including principal and interest                                                            |
| debtType   | String   | Debt type <br>`cross`: cross <br>`isolated`: isolated                                                                |
| repayData  | Array of objects    | Repay currency data list                                                                                             |
| > repayCcy  | String   | Repay currency                                                                                                       |
| > repayAmt  | String   | Repay currency's available balance amount                                                                            |
