---
site: okex
lang: en
name: Position builder (new)
method: POST
path: /api/v5/account/position-builder
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Position builder (new)

Calculates portfolio margin information for virtual position/assets or current position of the user.<br>
You can add up to 200 virtual positions and 200 virtual assets in one request.

#### Rate Limit: 2 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP Request

`POST /api/v5/account/position-builder`

> Request Example

```shell
# Both real and virtual positions and assets are calculated 
POST /api/v5/account/position-builder
body
{
    "inclRealPosAndEq": false,
    "simPos":[
         {
            "pos":"-10",
            "instId":"BTC-USDT-SWAP",
            "avgPx":"100000"
         },
         {
            "pos":"10",
            "instId":"LTC-USDT-SWAP",
            "avgPx":"8000"
         }
    ],
    "simAsset":[
        {
            "ccy": "USDT",
            "amt": "100"
        }
    ],
    "greeksType":"CASH"
}

# Only existing real positions are calculated
POST /api/v5/account/position-builder
body
{
   "inclRealPosAndEq":true
}

# Only virtual positions are calculated
POST /api/v5/account/position-builder
body
{
    "acctLv": "4",
    "inclRealPosAndEq": false,
    "simPos":[
        {
            "pos":"10",
            "instId":"BTC-USDT-SWAP",
            "avgPx":"100000"
        },
        {
            "pos":"10",
            "instId":"LTC-USDT-SWAP",
            "avgPx":"8000"
        }
    ]
}

# Switch to Multi-currency margin mode
POST /api/v5/account/position-builder
body
{
    "acctLv": "3",
    "lever":"10",
    "simPos":[
        {
            "pos":"10",
            "instId":"BTC-USDT-SWAP",
            "avgPx":"100000",
            "lever":"5"
        },
        {
            "pos":"10",
            "instId":"LTC-USDT-SWAP",
            "avgPx":"8000"
        }
    ]
}
```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

result = accountAPI.position_builder(
    inclRealPosAndEq=True,
    simPos=[
        {
            "pos": "10",
            "instId": "BTC-USDT-SWAP"
        },
        {
            "pos": "10",
            "instId": "LTC-USDT-SWAP"
        }
    ]
)
print(result)
```

#### Request Parameters

| Parameter         | Type            | Required | Description                                                                                                                                                                                           |
|:------------------|:----------------|:---------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| acctLv            | String         | No       | Switch to account mode<br>`3`: Multi-currency margin<br>`4`: Portfolio margin<br>The default is `4`                                                                                                   |
| inclRealPosAndEq  | Boolean         | No       | Whether import existing positions and assets<br>The default is `true`                                                                                                                                 |
| lever             | String          | No       | Cross margin leverage in Multi-currency margin mode, the default is `1`.<br>If the allowed leverage is exceeded, set according to the maximum leverage.<br>Only applicable to `Multi-currency margin` |
| simPos            | Array of objects | No       | List of simulated positions                                                                                                                                                                           |
| > instId          | String          | Yes      | Instrument ID, e.g. `BTC-USDT-SWAP`<br>Applicable to `SWAP`/`FUTURES`/`OPTION`                                                                                                                        |
| > pos             | String          | Yes      | Quantity of positions                                                                                                                                                                                 |
| > avgPx           | String          | Yes      | Average open price                                                                                                                                                                                    |
| > lever           | String          | No       | leverage<br>Only applicable to `Multi-currency margin`<br>The default is `1`<br>If the allowed leverage is exceeded, set according to the maximum leverage.                                           |
| simAsset         | Array of objects | No       | List of simulated assets<br>When `inclRealPosAndEq` is `true`, only real assets are considered and virtual assets are ignored                                                                         |
| > ccy            | String          | Yes      | Currency, e.g. `BTC`                                                                                                                                                                                  |
| > amt            | String          | Yes       | Currency amount                                                                                                                                                                                       |
| greeksType       | String          | No       | Greeks type<br>`BS`: Black-Scholes Model Greeks<br>`PA`: Crypto Greeks<br>`CASH`: Empirical Greeks<br>The default is `BS`                                                                             |
| idxVol | String | No | Price volatility percentage, indicating what this price change means towards each of the values. In decimal form, range -0.99 ~ 1, in 0.01 increment.<br> Default 0 |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "acctLever": "-0.1364949794742562",
            "assets": [
                {
                    "availEq": "0",
                    "borrowImr": "0",
                    "borrowMmr": "",
                    "ccy": "BTC",
                    "spotInUse": "0"
                },
                {
                    "availEq": "0",
                    "borrowImr": "0",
                    "borrowMmr": "",
                    "ccy": "LTC",
                    "spotInUse": "0"
                },
                {
                    "availEq": "0",
                    "borrowImr": "0",
                    "borrowMmr": "",
                    "ccy": "USDC",
                    "spotInUse": "0"
                },
                {
                    "availEq": "-78589.37",
                    "borrowImr": "7855.32188898",
                    "borrowMmr": "",
                    "ccy": "USDT",
                    "spotInUse": "0"
                }
            ],
            "borrowMmr": "1571.064377796",
            "derivMmr": "1375.4837063088003",
            "eq": "-78553.21888979999",
            "marginRatio": "-25.95365779811705",
            "positions": [],
            "riskUnitData": [
                {
                    "delta": "-9704.903689800001",
                    "gamma": "0",
                    "imrBf": "",
                    "imr": "1538.9669514070802",
                    "mmrBf": "",
                    "mmr": "1183.8207318516002",
                    "mr1": "1164.4109244719994",
                    "mr1FinalResult": {
                        "pnl": "-1164.4109244719994",
                        "spotShock": "0.12",
                        "volShock": "up"
                    },
                    "mr1Scenarios": {
                        "volSame": {
                            "0": "0",
                            "0.08": "-776.2739496480004",
                            "-0.08": "776.2739496480004",
                            "0.04": "-388.1369748240002",
                            "0.12": "-1164.4109244719994",
                            "-0.12": "1164.4109244719994",
                            "-0.04": "388.1369748240002"
                        },
                        "volShockDown": {
                            "0": "0",
                            "0.08": "-776.2739496480004",
                            "-0.08": "776.2739496480004",
                            "0.04": "-388.1369748240002",
                            "0.12": "-1164.4109244719994",
                            "-0.12": "1164.4109244719994",
                            "-0.04": "388.1369748240002"
                        },
                        "volShockUp": {
                            "0": "0",
                            "0.08": "-776.2739496480004",
                            "-0.08": "776.2739496480004",
                            "0.04": "-388.1369748240002",
                            "0.12": "-1164.4109244719994",
                            "-0.12": "1164.4109244719994",
                            "-0.04": "388.1369748240002"
                        }
                    },
                    "mr2": "0",
                    "mr3": "0",
                    "mr4": "19.4098073796",
                    "mr5": "0",
                    "mr6": "1164.4109244720003",
                    "mr6FinalResult": {
                        "pnl": "-2328.8218489440005",
                        "spotShock": "0.24"
                    },
                    "mr7": "43.67206660410001",
                    "mr8": "1571.064377796",
                    "mr9": "0",
                    "portfolios": [
                        {
                            "amt": "-10",
                            "avgPx": "100000",
                            "delta": "-9704.903689800001",
                            "floatPnl": "290.6300000000003",
                            "gamma": "0",
                            "instId": "BTC-USDT-SWAP",
                            "instType": "SWAP",
                            "isRealPos": false,
                            "markPxBf": "",
                            "markPx": "97093.7",
                            "notionalUsd": "9703.22",
                            "posSide": "net",
                            "theta": "0",
                            "vega": "0"
                        }
                    ],
                    "riskUnit": "BTC",
                    "theta": "0",
                    "upl": "290.49631020000027",
                    "vega": "0"
                },
                {
                    "delta": "1019.5308",
                    "gamma": "0",
                    "imrBf": "",
                    "imr": "249.16186679436",
                    "mmrBf": "",
                    "mmr": "191.6629744572",
                    "mr1": "183.50672805719995",
                    "mr1FinalResult": {
                        "pnl": "-183.50672805719995",
                        "spotShock": "-0.18",
                        "volShock": "up"
                    },
                    "mr1Scenarios": {
                        "volSame": {
                            "0": "0",
                            "-0.06": "-61.168909352399936",
                            "0.06": "61.168909352399936",
                            "-0.18": "-183.50672805719995",
                            "0.18": "183.50672805719995",
                            "0.12": "122.33781870480001",
                            "-0.12": "-122.33781870480001"
                        },
                        "volShockDown": {
                            "0": "0",
                            "-0.06": "-61.168909352399936",
                            "0.06": "61.168909352399936",
                            "-0.18": "-183.50672805719995",
                            "0.18": "183.50672805719995",
                            "0.12": "122.33781870480001",
                            "-0.12": "-122.33781870480001"
                        },
                        "volShockUp": {
                            "0": "0",
                            "-0.06": "-61.168909352399936",
                            "0.06": "61.168909352399936",
                            "-0.18": "-183.50672805719995",
                            "0.18": "183.50672805719995",
                            "0.12": "122.33781870480001",
                            "-0.12": "-122.33781870480001"
                        }
                    },
                    "mr2": "0",
                    "mr3": "0",
                    "mr4": "8.1562464",
                    "mr5": "0",
                    "mr6": "183.5067280572",
                    "mr6FinalResult": {
                        "pnl": "-367.0134561144",
                        "spotShock": "-0.36"
                    },
                    "mr7": "7.1367156",
                    "mr8": "1571.064377796",
                    "mr9": "0",
                    "portfolios": [
                        {
                            "amt": "10",
                            "avgPx": "8000",
                            "delta": "1019.5308",
                            "floatPnl": "-78980",
                            "gamma": "0",
                            "instId": "LTC-USDT-SWAP",
                            "instType": "SWAP",
                            "isRealPos": false,
                            "markPxBf": "",
                            "markPx": "102",
                            "notionalUsd": "1018.9",
                            "posSide": "net",
                            "theta": "0",
                            "vega": "0"
                        }
                    ],
                    "riskUnit": "LTC",
                    "theta": "0",
                    "upl": "-78943.6692",
                    "vega": "0"
                }
            ],
            "totalImr": "9643.45070718144",
            "totalMmr": "2946.5480841048",
            "ts": "1736936801642",
            "upl": "-78653.1728898"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameters**   | **Types**       | **Description**                                                                                                                                                                                                                                                                                                |
|------------------|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| eq               | String          | Adjusted equity (`USD`) for the account                                                                                                                                                                                                                                                                        |
| totalMmr         | String          | Total MMR (`USD`) for the account                                                                                                                                                                                                                                                                              |
| totalImr         | String          | Total IMR (`USD`) for the account                                                                                                                                                                                                                                                                              |
| borrowMmr        | String          | Borrow MMR (`USD`) for the account                                                                                                                                                                                                                                                                             |
| derivMmr         | String          | Derivatives MMR (`USD`) for the account                                                                                                                                                                                                                                                                        |
| marginRatio      | String          | Cross maintenance margin ratio for the account                                                                                                                                                                                                                                                                 |
| upl              | String          | UPL for the account                                                                                                                                                                                                                                                                                            |
| acctLever        | String          | Leverage of the account                                                                                                                                                                                                                                                                                        |
| ts               | String          | Update time for the account, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                                       |
| assets           | Array of objects | Asset info                                                                                                                                                                                                                                                                                                     |
| > ccy            | String          | Currency, e.g. `BTC`                                                                                                                                                                                                                                                                                           |
| > availEq        | String          | Currency equity                                                                                                                                                                                                                                                                                                |
| > spotInUse      | String          | Spot in use                                                                                                                                                                                                                                                                                                    |
| > borrowMmr      | String          | <del>Borrowing MMR (`USD`)</del>(Deprecated)                                                                                                                                                                                                                                                                   |
| > borrowImr      | String          | Borrowing IMR (`USD`)                                                                                                                                                                                                                                                                                          |
| riskUnitData     | Array of objects | Risk unit info                                                                                                                                                                                                                                                                                                 |
| > riskUnit       | String          | Risk unit, e.g. `BTC`                                                                                                                                                                                                                                                                                          |
| > mmrBf | String | Risk unit MMR before volatility (`USD`)<br> Return "" if users don't pass in idxVol |
| > mmr            | String          | Risk unit MMR (`USD`)                                                                                                                                                                                                                                                                                          |
| > imrBf | String | Risk unit IMR before volatility (`USD`)<br> Return "" if users don't pass in idxVol |
| > imr            | String          | Risk unit IMR (`USD`)                                                                                                                                                                                                                                                                                          |
| > upl            | String          | Risk unit UPL (`USD`)                                                                                                                                                                                                                                                                                          |
| > mr1            | String          | Stress testing value of spot and volatility (all derivatives, and spot trading in spot-derivatives risk offset mode)                                                                                                                                                                                           |
| > mr2            | String          | Stress testing value of time value of money (TVM) (for options)                                                                                                                                                                                                                                                |
| > mr3            | String          | Stress testing value of volatility span (for options)                                                                                                                                                                                                                                                          |
| > mr4            | String          | Stress testing value of basis (for all derivatives)                                                                                                                                                                                                                                                            |
| > mr5            | String          | Stress testing value of interest rate risk (for options)                                                                                                                                                                                                                                                       |
| > mr6            | String          | Stress testing value of extremely volatile markets (for all derivatives, and spot trading in spot-derivatives risk offset mode)                                                                                                                                                                                |
| > mr7            | String          | Stress testing value of position reduction cost (for all derivatives)                                                                                                                                                                                                                                          |
| > mr8            | String          | Borrowing MMR/IMR                                                                                                                                                                                                                                                                                              |
| > mr9            | String          | USDT-USDC-USD hedge risk                                                                                                                                                                                                                                                                                       |
| > mr1Scenarios   | Object          | MR1 scenario analysis                                                                                                                                                                                                                                                                                          |
| >> volShockDown  | Object          | When volatility shocks down, the P&L of stress tests under different price volatility ratios, format in {`change`: `value`,...}<br>`change`: price volatility ratio (in percentage), e.g. `0.01` representing `1%`<br>`value`: P&L under stress tests, measured in ` USD`<br>e.g. {"-0.15":"-2333.23", ...}    |
| >> volSame       | Object          | When volatility keeps the same, the P&L of stress tests under different price volatility ratios, format in {`change`: `value`,...}<br>`change`: price volatility ratio (in percentage), e.g. `0.01` representing `1%`<br>`value`: P&L under stress tests, measured in ` USD`<br>e.g. {"-0.15":"-2333.23", ...} |
| >> volShockUp    | Object          | When volatility shocks up, the P&L of stress tests under different price volatility ratios, format in {`change`: `value`,...}<br>`change`: price volatility ratio (in percentage), e.g. `0.01` representing `1%`<br>`value`: P&L under stress tests, measured in ` USD`<br>e.g. {"-0.15":"-2333.23", ...}      |
| > mr1FinalResult | Object          | MR1 worst-case scenario                                                                                                                                                                                                                                                                                        |
| >> pnl           | String          | MR1 stress P&L (`USD`)                                                                                                                                                                                                                                                                                         |
| >> spotShock     | String          | MR1 worst-case scenario spot shock (in percentage), e.g. `0.01` representing `1%`                                                                                                                                                                                                                              |
| >> volShock      | String          | MR1 worst-case scenario volatility shock<br>`down`: volatility shock down<br>`unchange`: volatility unchanged<br>`up`: volatility shock up                                                                                                                                                                     |
| > mr6FinalResult | Object          | MR6 scenario analysis                                                                                                                                                                                                                                                                                          |
| >> pnl           | String          | MR6 stress P&L (`USD`)                                                                                                                                                                                                                                                                                         |
| >> spotShock     | String          | MR6 worst-case scenario spot shock (in percentage), e.g. `0.01` representing `1%`                                                                                                                                                                                                                              |
| > delta          | String          | (Risk unit) The rate of change in the contract’s price with respect to changes in the underlying asset’s price. <br>When the price of the underlying changes by x, the option’s price changes by delta multiplied by x.                                                                                        |
| > gamma          | String          | (Risk unit) The rate of change in the delta with respect to changes in the underlying price. <br>When the price of the underlying changes by x%, the option’s delta changes by gamma multiplied by x%.                                                                                                         |
| > theta          | String          | (Risk unit) The change in contract price each day closer to expiry.                                                                                                                                                                                                                                            |
| > vega           | String          | (Risk unit) The change of the option price when underlying volatility increases by 1%.                                                                                                                                                                                                                         |
| > portfolios     | Array of objects | Portfolios info<br>Only applicable to `Portfolio margin`                                                                                                                                                                                                                                                       |
| >> instId        | String          | Instrument ID, e.g. `BTC-USDT-SWAP`                                                                                                                                                                                                                                                                            |
| >> instType      | String          | Instrument type<br>`SPOT`<br>`SWAP`<br>`FUTURES`<br>`OPTION`                                                                                                                                                                                                                                                   |
| >> amt           | String          | When `instType` is `SPOT`, it represents spot in use.<br>When `instType` is `SWAP`/`FUTURES`/`OPTION`, it represents position amount.                                                                                                                                                                          |
| >> posSide       | String          | Position side<br>`long`<br>`short`<br>`net`                                                                                                                                                                                                                                                                    |
| >> avgPx         | String          | Average open price                                                                                                                                                                                                                                                                                             |
| >> markPxBf | String | Mark price before price volatility<br> Return "" if users don't pass in idxVol |
| >> markPx        | String          | Mark price                                                                                                                                                                                                                                                                                                     |
| >> floatPnl      | String          | Float P&L                                                                                                                                                                                                                                                                                                      |
| >> notionalUsd   | String          | Notional in `USD`                                                                                                                                                                                                                                                                                              |
| >> delta         | String          | When `instType` is `SPOT`, it represents asset amount.<br>When `instType` is `SWAP`/`FUTURES`/`OPTION`, it represents the rate of change in the contract’s price with respect to changes in the underlying asset’s price (by Instrument ID).                                                                   |
| >> gamma         | String          | The rate of change in the delta with respect to changes in the underlying price (by Instrument ID). <br>When `instType` is `SPOT`, it will return "".                                                                                                                                                         |
| >> theta         | String          | The change in contract price each day closer to expiry (by Instrument ID).<br>When `instType` is `SPOT`, it will return "".                                                                                                                                                                                   |
| >> vega          | String          | The change of the option price when underlying volatility increases by 1% (by Instrument ID).<br>When `instType` is `SPOT`, it will return "".                                                                                                                                                                |
| >> isRealPos     | Boolean         | Whether it is a real position<br>If `instType` is `SWAP`/`FUTURES`/`OPTION`, it is a valid parameter, else it will return `false`                                                                                                                                                                             |
| positions        | Array of objects | Position info<br>Only applicable to `Multi-currency margin`                                                                                                                                                                                                                                                    |
| > instId         | String          | Instrument ID, e.g. `BTC-USDT-SWAP`                                                                                                                                                                                                                                                                            |
| > instType       | String          | Instrument type<br>`SPOT`<br>`SWAP`<br>`FUTURES`<br>`OPTION`                                                                                                                                                                                                                                                   |
| > amt            | String          | When `instType` is `SPOT`, it represents spot in use.<br>When `instType` is `SWAP`/`FUTURES`/`OPTION`, it represents position amount.                                                                                                                                                                          |
| > posSide        | String          | Position side<br>`long`<br>`short`<br>`net`                                                                                                                                                                                                                                                                    |
| > avgPx          | String          | Average open price                                                                                                                                                                                                                                                                                             |
| > markPxBf | String | Mark price before price volatility<br> Return "" if users don't pass in idxVol |
| > markPx         | String          | Mark price                                                                                                                                                                                                                                                                                                     |
| > floatPnl       | String          | Float P&L                                                                                                                                                                                                                                                                                                      |
| > imrBf | String | IMR before price volatility|
| > imr            | String          | IMR                                                                                                                                                                                                                                                                                                            |
| > mgnRatio       | String          | Maintenance margin ratio                                                                                                                                                                                                                                                                                       |
| > lever          | String          | Leverage                                                                                                                                                                                                                                                                                                       |
| > notionalUsd    | String          | Notional in `USD`                                                                                                                                                                                                                                                                                              |
| > isRealPos      | Boolean         | Whether it is a real position<br>If `instType` is `SWAP`/`FUTURES`/`OPTION`, it is a valid parameter, else it will return `false`                                                                                                                                                                             |
