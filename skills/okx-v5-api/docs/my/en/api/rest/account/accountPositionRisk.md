---
site: my
lang: en
name: Get account and position risk
method: GET
path: /api/v5/account/account-position-risk
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### Get account and position risk

Get account and position risk

<aside class="notice">
Obtain basic information about accounts and positions on the same time snapshot
</aside>

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/account-position-risk`

> Request Example

```shell
GET /api/v5/account/account-position-risk

```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Get account and position risk
result = accountAPI.get_account_position_risk()
print(result)
```

#### Request Parameters

| Parameter   | Type   | Required | Description                                                         |
| :------- | :----- | :------- | :----------------------------------------------------------- |
| instType | String | No      | Instrument type<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION` |

> Response Example

```json
{
    "code":"0",
    "data":[
        {
            "adjEq":"174238.6793649711331679",
            "balData":[
                {
                    "ccy":"BTC",
                    "disEq":"78846.7803721021362242",
                    "eq":"1.3863533369419636"
                },
                {
                    "ccy":"USDT",
                    "disEq":"73417.2495112863300127",
                    "eq":"73323.395564963177146"
                }
            ],
            "posData":[
                {
                    "baseBal": "0.4",
                    "ccy": "",
                    "instId": "BTC-USDT",
                    "instType": "MARGIN",
                    "mgnMode": "isolated",
                    "notionalCcy": "0",
                    "notionalUsd": "0",
                    "pos": "0",
                    "posCcy": "",
                    "posId": "310388685292318723",
                    "posSide": "net",
                    "quoteBal": "0"
                }
            ],
            "ts":"1620282889345"
        }
    ],
    "msg":""
}
```

#### Response Parameters

| **Parameters** | **Types** | **Description**                                                                                                                                                                                                                                                      |
| ----------- | -------- |----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ts          | String   | Update time of account information, millisecond format of Unix timestamp, e.g. `1597026383085`                                                                                                                                                                       |
| adjEq       | String   | Adjusted / Effective equity in `USD`<br>Applicable to `Multi-currency margin` and `Portfolio margin`                                                                                                                                                                  |
| balData     | Array of objects    | Detailed asset information in all currencies                                                                                                                                                                                                                         |
| > ccy       | String   | Currency                                                                                                                                                                                                                                                             |
| > eq        | String   | Equity of currency                                                                                                                                                                                                                                               |
| > disEq     | String   | Discount equity of currency in `USD`.                                                                                                                                                                                                                              |
| posData     | Array of objects    | Detailed position information in all currencies                                                                                                                                                                                                                      |
| > instType  | String   | Instrument type                                                                                                                                                                                                                                                      |
| > mgnMode   | String   | Margin mode<br>`cross`  <br>`isolated`                                                                                                                                                                                                                              |
| > posId     | String   | Position ID                                                                                                                                                                                                                                                          |
| > instId    | String   | Instrument ID, e.g. `BTC-USDT-SWAP`                                                                                                                                                                                                                                 |
| > pos       | String   | Quantity of positions `contract`. In the isolated margin mode, when doing manual transfers, a position with pos of `0` will be generated after the deposit is transferred                                                                                       |
| > baseBal       | String   | ~~Base currency balance, only applicable to `MARGIN`（Quick Margin Mode）~~(Deprecated)                                                                                                                                                                           |
| > quoteBal      | String   | ~~Quote currency balance, only applicable to `MARGIN`（Quick Margin Mode）~~(Deprecated)                                                                                                                                                                                               |
| > posSide   | String   | Position side<br>`long` <br>`short` <br>`net` (`FUTURES`/`SWAP`/`OPTION`: positive `pos` means long position and negative `pos` means short position. `MARGIN`: `posCcy` being base currency means long position, `posCcy` being quote currency means short position.) |
| > posCcy    | String   | Position currency, only applicable to `MARGIN` positions.                                                                                                                                                                                                            |
| > ccy       | String   | Currency used for margin                                                                                                                                                                                                                                             |
| > notionalCcy     | String   | Notional value of positions in `coin`                                                                                                                                                                                                                                |
| > notionalUsd     | String   | Notional value of positions in `USD`                                                                                                                                                                                                                                 |
