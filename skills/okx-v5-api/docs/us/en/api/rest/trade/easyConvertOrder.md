---
site: us
lang: en
name: POST / Place easy convert
method: POST
path: /api/v5/trade/easy-convert
module: trade
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### POST / Place easy convert 

Convert small currencies to mainstream currencies. 

#### Rate Limit: 1 request per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/trade/easy-convert`

> Request Example

```shell
POST /api/v5/trade/easy-convert
body
{
    "fromCcy": ["ADA","USDC"], //Seperated by commas
    "toCcy": "OKB" 
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

# Convert small currencies to mainstream currencies
result = tradeAPI.easy_convert(
    fromCcy=["ADA", "USDC"],
    toCcy="OKB"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                         |
|:----------| :----- | :------- | :-------------------------------------------------------------------- |
| fromCcy   | Array of strings  | Yes      | Type of small payment currency convert from <br>Maximum 5 currencies can be selected in one order. If there are multiple currencies, separate them with commas. |
| toCcy     | String | Yes      | Type of mainstream currency convert to <br>Only one receiving currency type can be selected in one order and cannot be the same as the small payment currencies.              |
| source    | String | No       | Funding source<br>`1`: Trading account<br>`2`: Funding account<br>The default is `1`. |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "fillFromSz": "6.5807127",
            "fillToSz": "0.17171580105126",
            "fromCcy": "ADA",
            "status": "running",
            "toCcy": "OKB",
            "uTime": "1661419684687"
        },
        {
            "fillFromSz": "2.997",
            "fillToSz": "0.1683755161661844",
            "fromCcy": "USDC",
            "status": "running",
            "toCcy": "OKB",
            "uTime": "1661419684687"
        }
    ],
    "msg": ""
}

```

#### Response Parameters

| Parameter  | Type     | Description                                                                             |
| :--------- | :------- | :--------------------------------------------------------------------------------------- |
| status     | String   | Current status of easy convert <br>`running`: Running <br>`filled`: Filled <br>`failed`: Failed         |
| fromCcy    | String   | Type of small payment currency convert from                                        |
| toCcy      | String   | Type of mainstream currency convert to                                                                      |
| fillFromSz | String   | Filled amount of small payment currency convert from                                                                |
| fillToSz   | String   | Filled amount of mainstream currency convert to                                                                      |
| uTime      | String   | Trade time, Unix timestamp format in milliseconds, e.g. 1597026383085                               |
