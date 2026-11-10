---
site: okex
lang: en
name: Get Greeks
method: GET
path: /api/v5/account/greeks
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get Greeks

Retrieve a greeks list of all assets in the account.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/greeks`

> Request Example

```shell
# Get the greeks of all assets in the account
GET /api/v5/account/greeks

# Get the greeks of BTC assets in the account
GET /api/v5/account/greeks?ccy=BTC

```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Retrieve a greeks list of all assets in the account
result = accountAPI.get_greeks()
print(result)
```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**                                                                                         |
| :------------- | :-------- | :----------- | :------------------------------------------------------------------------------------------------------ |
| ccy            | String    | No           | Single currency, e.g. `BTC`. |

> Response Example

```json
{
    "code":"0",
    "data":[
        {            
           "thetaBS": "",
           "thetaPA":"",
           "deltaBS":"",
           "deltaPA":"",
           "gammaBS":"",
           "gammaPA":"",
           "vegaBS":"",    
           "vegaPA":"",
           "ccy":"BTC",
           "ts":"1620282889345"
        }
    ],
    "msg":""
}
```

#### Response Parameters

| **Parameters** | **Types** | **Description**                                                                                                   |
| :----------- | :------- | :----------------------------------------------------------- |
| deltaBS       | String   | delta: Black-Scholes Greeks in dollars     |
| deltaPA       | String   | delta: Greeks in coins                     |
| gammaBS       | String   | gamma: Black-Scholes Greeks in dollars, only applicable to OPTION |
| gammaPA       | String   | gamma: Greeks in coins, only applicable to OPTION        |
| thetaBS       | String   | theta: Black-Scholes Greeks in dollars, only applicable to `OPTION`          |
| thetaPA       | String   | theta: Greeks in coins, only applicable to `OPTION`          |
| vegaBS        | String   | vega: Black-Scholes Greeks in dollars, only applicable to `OPTION`           |
| vegaPA        | String   | vega：Greeks in coins, only applicable to `OPTION`            |
| ccy           | String   | Currency  |
| ts            | String   | Time of getting Greeks, Unix timestamp format in milliseconds, e.g. 1597026383085     |
