---
site: okex
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

| Parameter  | Type   | Required | Description                                                                        |
|:-----------| :----- |:---------|:-----------------------------------------------------------------------------------|
| instType   | String | Yes      | Instrument type<br>`SPOT`<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`<br>`EVENTS` |
| instId     | String | No       | Instrument ID, e.g. `BTC-USDT`<br>Applicable to `SPOT`/`MARGIN`<br>Specifying this parameter returns the correct applicable fee rates (e.g., market maker rates for users in incentive programs).                      |
| instFamily | String | No       | Instrument family, e.g. `BTC-USD`<br>Applicable to `FUTURES`/`SWAP`/`OPTION`                           |                                                                                                                                                                      
| groupId    | String | No       | Instrument trading fee group ID<br> Only one of groupId and instId/instFamily can be passed in<br><br> Users can use instruments endpoint to fetch the mapping of an instrument ID and its trading fee group ID                                                                                                   |

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
                    "elpMaker": "-0.0008",
                    "rpiMaker": "-0.0008",
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
| feeGroup | Array of objects | Fee groups. <br>Applicable to `SPOT/MARGIN/SWAP/FUTURES/OPTION/EVENTS` |
| > taker | String | Taker fee<br>K1 parameter for `EVENTS` taker fee formula: `K1 × C × (P × (1-P))` (C = number of contracts, P = price) |
| > maker | String | Maker fee<br>K2 parameter for `EVENTS` maker fee formula: `K2 × C × (P × (1-P))` (C = number of contracts, P = price) |
| > groupId | String | Instrument trading fee group ID<br><br>**instType and groupId should be used together to determine a trading fee group. Users should use this endpoint together with [instruments endpoint](/docs-v5/en/#trading-account-rest-api-get-instruments) to get the trading fee of a specific symbol.** |
| > elpMaker | String | ELP Maker effective fee rate. Returns `""` if ELP is not applicable to the instrument. |
| > rpiMaker | String | RPI maker effective fee rate. Returns `""` if RPI is not applicable to the instrument.<br>`elpMaker` remains accepted as an alias until October 31, 2026. |
| delivery      | String   | Delivery fee rate                                                                                                                             |
| exercise      | String   | Fee rate for exercising the option                                                                                                            |
| instType      | String   | Instrument type                                                                                                                               |
| ts            | String   | Data return time, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                 |
| taker         | String   | ~~For `SPOT`/`MARGIN`, it is taker fee rate of the USDT trading pairs. <br>For `FUTURES`/`SWAP`/`OPTION`, it is the fee rate of crypto-margined contracts~~(deprecated) |
| maker         | String   | ~~For `SPOT`/`MARGIN`, it is maker fee rate of the USDT trading pairs. <br>For `FUTURES`/`SWAP`/`OPTION`, it is the fee rate of crypto-margined contracts~~(deprecated) |
| takerU        | String   | ~~Taker fee rate of USDT-margined contracts, only applicable to `FUTURES`/`SWAP`~~(deprecated)                                                                  |
| makerU        | String   | ~~Maker fee rate of USDT-margined contracts, only applicable to `FUTURES`/`SWAP`~~(deprecated)                                                                  |
| takerUSDC        | String   | ~~For `SPOT`/`MARGIN`, it is taker fee rate of the USDⓈ&Crypto trading pairs.<br>For `FUTURES`/`SWAP`, it is the fee rate of USDC-margined contracts~~(deprecated)      |
| makerUSDC        | String   | ~~For `SPOT`/`MARGIN`, it is maker fee rate of the USDⓈ&Crypto trading pairs.<br>For `FUTURES`/`SWAP`, it is the fee rate of USDC-margined contracts~~(deprecated)      |
| ruleType | String | ~~Trading rule types<br> `normal`: normal trading<br> `pre_market`: pre-market trading~~(deprecated)                                                                                                                                                                                                                                                  |
| category      | String   | ~~Currency category.~~(deprecated)                                                                                 |
| fiat   | Array of objects  | ~~Details of fiat fee rate~~(deprecated)                                                                                                                                 |
| > ccy   | String  | Fiat currency.                                                                                                                                           |
| > taker   | String  | Taker fee rate                                                                                                                                                   |
| > maker   | String  | Maker fee rate                                                                                                                                                   |
| settle | String | Settlement fee rate for users whose positions match the event contract settlement result. Users holding the opposite positions will not be charged during settlement. Only applicable to `EVENTS` |

<aside class="notice">
Remarks: <br>
The fee rate like maker and taker: positive number, which means the rate of rebate; negative number, which means the rate of commission.<br>
Exception: The values for delivery and exercise are positive numbers, representing the commission rate.
</aside>

<aside class="notice">
USDⓈ represent the stablecoin besides USDT
</aside>

<aside class="notice">
The Open API will not reflect zero-fee trading. For zero-fee pairs, please refer to <a href="https://www.okx.com/fees">https://www.okx.com/fees
</a>.
</aside>

<aside class="notice">
For users in market maker incentive programs: specifying <code>instId</code> (for <code>SPOT</code>/<code>MARGIN</code>) or <code>instFamily</code> (for <code>FUTURES</code>/<code>SWAP</code>/<code>OPTION</code>) returns the correct applicable fee rates. Without these parameters, the response reflects the organic base fee rates.
</aside>
