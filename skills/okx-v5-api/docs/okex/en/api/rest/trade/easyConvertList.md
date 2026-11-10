---
site: okex
lang: en
name: GET / Easy convert currency list
method: GET
path: /api/v5/trade/easy-convert-currency-list
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Easy convert currency list 

Get list of small convertibles and mainstream currencies. Only applicable to the crypto balance less than $10.

#### Rate Limit: 1 request per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/trade/easy-convert-currency-list`

> Request Example

```shell
GET /api/v5/trade/easy-convert-currency-list
```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# Get list of small convertibles and mainstream currencies
result = tradeAPI.get_easy_convert_currency_list()
print(result)
```

#### Request Parameters

| Parameters    | Type     | Required | Description                                                                           |
| :------- |:-------|:---------|:--------------------------------------------------------------------------------------|
| source  | String | No       | Funding source<br>`1`: Trading account<br>`2`: Funding account<br>The default is `1`. |
                                                                       

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "fromData": [
                {
                    "fromAmt": "6.580712708344864",
                    "fromCcy": "ADA"
                },
                {
                    "fromAmt": "2.9970000013055097",
                    "fromCcy": "USDC"
                }
            ],
            "toCcy": [
                "USDT",
                "BTC",
                "ETH",
                "OKB"
            ]
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter  | Type     | Description                                                                              |
| :--------- | :------- | :--------------------------------------------------------------------------------------- |
| fromData   | Array of objects    | Currently owned and convertible small currency list                                      |
| > fromCcy   | String   | Type of small payment currency convert from, e.g. `BTC`                                                  |
| > fromAmt   | String   | Amount of small payment currency convert from                                                          |
| toCcy      | Array of strings    | Type of mainstream currency convert to, e.g. `USDT`                                                   |
