---
site: hongkong
lang: en
name: Get fee rates
method: GET
path: /api/v5/account/trade-fee
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get fee rates

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/trade-fee`

> Request Example

```shell 
# Query trade fee rate of SPOT BTC-USDT
GET /api/v5/account/trade-fee?instType=SPOT&instId=BTC-USDT
```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Get trading fee rates of current account
result = accountAPI.get_fee_rates(
    instType="SPOT",
    instId="BTC-USDT"
)
print(result)
```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                                       |
|:-----------| :----- |:---------|:--------------------------------------------------------------------------------------------------|
| instType   | String | Yes      | Instrument type<br>`SPOT`                      |
| instId     | String | No       | Instrument ID, e.g. `BTC-USDT`<br>Applicable to `SPOT`                                     |
| groupId    | String | No       | Instrument trading fee group ID<br> Only one of groupId and instId can be passed in<br><br> Users can use instruments endpoint to fetch the mapping of an instrument ID and its trading fee group ID                                                                                                   |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "category": "1",
            "delivery": "",
            "exercise": "",
            "feeGroup": [
                {
                    "groupId": "1",
                    "maker": "-0.0008",
                    "taker": "-0.001"
                }
            ],
            "fiat": [],
            "instType": "SPOT",
            "level": "Lv1",
            "maker": "-0.0008",
            "makerU": "",
            "makerUSDC": "",
            "ruleType": "normal",
            "taker": "-0.001",
            "takerU": "",
            "takerUSDC": "",
            "ts": "1763979985847"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                                                               |
|---------------| -------- |-----------------------------------------------------------------------------------------------------------------------------------------------|
| level         | String   | Fee rate Level                                                                                                                                |
| feeGroup | Array of objects | Fee groups. <br>Applicable to `SPOT/MARGIN/SWAP/FUTURES/OPTION` |
| > taker | String | Taker fee |
| > maker | String | Maker fee |
| > groupId | String | Instrument trading fee group ID<br><br>**instType and groupId should be used together to determine a trading fee group. Users should use this endpoint together with [instruments endpoint](/docs-v5/en/#trading-account-rest-api-get-instruments) to get the trading fee of a specific symbol.** |
| delivery      | String   | Delivery fee rate                                                                                                                             |
| exercise      | String   | Fee rate for exercising the option                                                                                                            |
| instType      | String   | Instrument type                                                                                                                               |
| ts            | String   | Data return time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                 |
| taker         | String   | ~~For `SPOT`, it is taker fee rate of the USDT trading pairs. ~~(deprecated)|
| maker         | String   | ~~For `SPOT`, it is maker fee rate of the USDT trading pairs. ~~(deprecated) |
| takerU        | String   | ~~Taker fee rate of USDT-margined contracts, only applicable to `FUTURES`/`SWAP` ~~(deprecated)                                                                  |
| makerU        | String   | ~~Maker fee rate of USDT-margined contracts, only applicable to `FUTURES`/`SWAP` ~~(deprecated)                                                                  |
| takerUSDC        | String   | ~~For `SPOT`, it is taker fee rate of the USDⓈ&Crypto trading pairs. ~~(deprecated)     |
| makerUSDC        | String   | ~~For `SPOT`, it is maker fee rate of the USDⓈ&Crypto trading pairs. ~~(deprecated)     |
| category      | String   | ~~Currency category.~~(deprecated)                                                                                 |
| fiat   | Array of objects  | ~~Details of fiat fee rate~~(deprecated)                                                                                                                                 |
| > ccy   | String  | Fiat currency.                                                                                                                                           |
| > taker   | String  | Taker fee rate                                                                                                                                                   |
| > maker   | String  | Maker fee rate                                                                                                                                                   |

<aside class="notice">
Remarks: <br>
The fee rate like maker and taker: positive number, which means the rate of rebate; negative number, which means the rate of commission.
</aside>

<aside class="notice">
USDⓈ represent the stablecoin besides USDT
</aside>
