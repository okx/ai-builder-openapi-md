---
site: okex
lang: en
name: Get currencies
method: GET
path: /api/v5/asset/currencies
module: asset
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

## REST API
### Get currencies

Retrieve a list of all currencies available which are related to the current account's KYC entity.

#### Rate Limit: 6 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/asset/currencies`

> Request Example

```shell
GET /api/v5/asset/currencies

```

```python
import okx.Funding as Funding

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading: 0, Demo trading: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# Get currencies
result = fundingAPI.get_currencies()
print(result)
```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**                                                                       |
| :------------- | :-------- | :----------- |:--------------------------------------------------------------------------------------|
| ccy            | String    | No           | Single currency or multiple currencies separated with comma, e.g. `BTC` or `BTC,ETH`. |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "burningFeeRate": "",
        "canDep": true,
        "canInternal": true,
        "canWd": true,
        "ccy": "BTC",
        "chain": "BTC-Bitcoin",
        "ctAddr": "",
        "depEstOpenTime": "",
        "depQuotaFixed": "",
        "depQuoteDailyLayer2": "",
        "fee": "0.00005",
        "logoLink": "https://static.coinall.ltd/cdn/oksupport/asset/currency/icon/btc20230419112752.png",
        "mainNet": true,
        "maxFee": "0.00005",
        "maxFeeForCtAddr": "",
        "maxWd": "500",
        "minDep": "0.0005",
        "minDepArrivalConfirm": "1",
        "minFee": "0.00005",
        "minFeeForCtAddr": "",
        "minInternal": "0.0001",
        "minWd": "0.0005",
        "minWdUnlockConfirm": "2",
        "name": "Bitcoin",
        "needTag": false,
        "usedDepQuotaFixed": "",
        "usedWdQuota": "0",
        "wdEstOpenTime": "",
        "wdQuota": "10000000",
        "wdTickSz": "8"
    }
  ]
}
```

#### Response Parameters

| Parameter            | Type    | Description                                                                                                                                                                                                                                        |
|----------------------|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy                  | String  | Currency, e.g. `BTC`                                                                                                                                                                                                                               |
| name                 | String  | Name of currency. There is no related name when it is not shown.                                                                                                                                                                                   |
| logoLink             | String  | The logo link of currency                                                                                                                                                                                                                          |
| chain                | String  | Chain name, e.g. `USDT-ERC20`, `USDT-TRC20`                                                                                                                                                                                                        |
| ctAddr               | String  | Contract address                                                                                                                                                                                                                                   |
| canDep               | Boolean | The availability to deposit from chain <br>`false`: not available <br>`true`: available                                                                                                                                                            |
| canWd                | Boolean | The availability to withdraw to chain <br>`false`: not available <br>`true`: available                                                                                                                                                             |
| canInternal          | Boolean | The availability to internal transfer <br>`false`: not available <br>`true`: available                                                                                                                                                             |
| depEstOpenTime       | String | Estimated opening time for deposit, Unix timestamp format in milliseconds, e.g. `1597026383085`<br>if `canDep` is `true`, it returns `""`                                                                                                                                                    |
| wdEstOpenTime        | String | Estimated opening time for withdraw, Unix timestamp format in milliseconds, e.g. `1597026383085`<br>if `canWd` is `true`, it returns `""`                                                                                                                                                   |
| minDep               | String  | The minimum deposit amount of currency in a single transaction                                                                                                                                                                                     |
| minWd                | String  | The minimum `on-chain withdrawal` amount of currency in a single transaction                                                                                                                                                                       |
| minInternal          | String  | The minimum `internal transfer` amount of currency in a single transaction<br>No maximum `internal transfer` limit in a single transaction, subject to the withdrawal limit in the past 24 hours(`wdQuota`).                                       |
| maxWd                | String  | The maximum amount of currency `on-chain withdrawal` in a single transaction                                                                                                                                                                       |
| wdTickSz             | String  | The withdrawal precision, indicating the number of digits after the decimal point.<br>The withdrawal fee precision kept the same as withdrawal precision.<br>The accuracy of internal transfer withdrawal is 8 decimal places.                     |
| wdQuota              | String  | The withdrawal limit in the past 24 hours (including `on-chain withdrawal` and `internal transfer`), unit in `USD`                                                                                                                                 |
| usedWdQuota          | String  | The amount of currency withdrawal used in the past 24 hours, unit in `USD`                                                                                                                                                                         |
| fee                  | String  | The fixed withdrawal fee<br>Apply to `on-chain withdrawal`                                                                                                                                                                                         |
| minFee               | String  | <del>The minimum withdrawal fee for normal address<br>Apply to `on-chain withdrawal`</del><br>(Deprecated)                                                                                                                                         |
| maxFee               | String  | <del>The maximum withdrawal fee for normal address<br>Apply to `on-chain withdrawal`</del><br>(Deprecated)                                                                                                                                         |
| minFeeForCtAddr      | String  | <del>The minimum withdrawal fee for contract address<br>Apply to `on-chain withdrawal`</del><br>(Deprecated)                                                                                                                                       |
| maxFeeForCtAddr      | String  | <del>The maximum withdrawal fee for contract address<br>Apply to `on-chain withdrawal`</del><br>(Deprecated)                                                                                                                                       |
| burningFeeRate       | String  | Burning fee rate, e.g "0.05" represents "5%".<br>Some currencies may charge combustion fees. The burning fee is deducted based on the withdrawal quantity (excluding gas fee) multiplied by the burning fee rate.<br>Apply to `on-chain withdrawal` |
| mainNet              | Boolean | If current chain is main net, then it will return `true`, otherwise it will return `false`                                                                                                                                                         |
| needTag              | Boolean | Whether tag/memo information is required for withdrawal, e.g. `EOS` will return `true`                                                                                                                                                             |
| minDepArrivalConfirm | String  | The minimum number of blockchain confirmations to acknowledge fund deposit. The account is credited after that, but the deposit can not be withdrawn                                                                                               |
| minWdUnlockConfirm   | String  | The minimum number of blockchain confirmations required for withdrawal of a deposit                                                                                                                                                                |
| depQuotaFixed        | String  | The fixed deposit limit, unit in `USD`<br>Return empty string if there is no deposit limit                                                                                                                                                         |
| usedDepQuotaFixed    | String  | The used amount of fixed deposit quota, unit in `USD`<br>Return empty string if there is no deposit limit                                                                                                                                          |
| depQuoteDailyLayer2  | String  | The layer2 network daily deposit limit                                                                                                                                                                                                             |
