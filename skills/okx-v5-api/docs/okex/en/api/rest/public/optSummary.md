---
site: okex
lang: en
name: Get option market data
method: GET
path: /api/v5/public/opt-summary
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get option market data

Retrieve option market data.

#### Rate Limit: 20 requests per 2 seconds 
#### Rate limit rule: IP + instFamily

#### HTTP Request

`GET /api/v5/public/opt-summary`

> Request Example

```shell
GET /api/v5/public/opt-summary?uly=BTC-USD
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Retrieve option market data
result = publicDataAPI.get_opt_summary(
    uly="BTC-USD",
)
print(result)
```

#### Request Parameters

| Parameter     | Type   | Required      | Description                                                                                                                       |
|:--------------| :----- |:--------------|:----------------------------------------------------------------------------------------------------------------------------------|
| instFamily    | String | Yes      | Instrument family, only applicable to `OPTION`<br>                                                                                   |
| expTime       | String | No            | Contract expiry date, the format is "YYMMDD", e.g. "200527"                                                                       |

**Note**: This endpoint may not return data for every option listed in [`/api/v5/public/instruments`](#get-instruments). Data can be absent in two cases:
1. The option is listed but not yet tradeable (e.g. supplemental options may not become tradeable until a scheduled time; data will not be available before trading opens).
2. Implied volatility surface fitting fails due to insufficient market quotes. This is more likely to occur in demo trading; in live trading, market maker quotes are generally available to ensure fitting succeeds.

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "askVol": "3.7207056835937498",
            "bidVol": "0",
            "delta": "0.8310206676289528",
            "deltaBS": "0.9857332101544538",
            "fwdPx": "39016.8143629068452065",
            "gamma": "-1.1965483553276135",
            "gammaBS": "0.000011933182397798109",
            "instId": "BTC-USD-220309-33000-C",
            "instType": "OPTION",
            "lever": "0",
            "markVol": "1.5551965233045728",
            "realVol": "0",
            "volLv": "0",
            "theta": "-0.0014131955002093717",
            "thetaBS": "-66.03526900575946",
            "ts": "1646733631242",
            "uly": "BTC-USD",
            "vega": "0.000018173851073258973",
            "vegaBS": "0.7089307622132419"
        },
        {
            "askVol": "1.7968814062499998",
            "bidVol": "0",
            "delta": "-0.014668822072611904",
            "deltaBS": "-0.01426678984554619",
            "fwdPx": "39016.8143629068452065",
            "gamma": "0.49483062407551576",
            "gammaBS": "0.000011933182397798109",
            "instId": "BTC-USD-220309-33000-P",
            "instType": "OPTION",
            "lever": "0",
            "markVol": "1.5551965233045728",
            "realVol": "0",
            "volLv": "0",
            "theta": "-0.0014131955002093717",
            "thetaBS": "-54.93377294845015",
            "ts": "1646733631242",
            "uly": "BTC-USD",
            "vega": "0.000018173851073258973",
            "vegaBS": "0.7089307622132419"
        }
  ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                 |
| :--------- | :------- | :------------------------------------------------------- |
| instType   | String   | Instrument type<br>`OPTION`                                |
| instId      | String   | Instrument ID, e.g. `BTC-USD-200103-5500-C`                          |
| uly        | String   | Underlying                                                     |
| delta      | String   | Sensitivity of option price to `uly` price |
| gamma      | String   | The delta is sensitivity to `uly` price |
| vega       | String   | Sensitivity of option price to implied volatility |
| theta      | String   | Sensitivity of option price to remaining maturity |
| deltaBS  | String   | Sensitivity of option price to `uly` price in BS mode      |
| gammaBS  | String   | The delta is sensitivity to `uly ` price in BS mode       |
| vegaBS   | String   | Sensitivity of option price to implied volatility in BS mode       |
| thetaBS  | String   | Sensitivity of option price to remaining maturity in BS mode     |   
| lever      | String   | Leverage                                          |
| markVol    | String   | Mark volatility                              |
| bidVol     | String   | Bid volatility                                  |
| askVol     | String   | Ask volatility                                  |
| realVol    | String   | Realized volatility (not currently used) |
| volLv   | String | Implied volatility of at-the-money options |
| fwdPx       | String   | Forward price                       |
| ts         | String   | Data update time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
