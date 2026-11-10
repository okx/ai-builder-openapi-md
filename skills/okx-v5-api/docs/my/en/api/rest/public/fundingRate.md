---
site: my
lang: en
name: Get funding rate
method: GET
path: /api/v5/public/funding-rate
module: public
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### Get funding rate

Retrieve funding rate.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: IP + Instrument ID

#### HTTP Request

`GET /api/v5/public/funding-rate`

> Request Example

```shell
GET /api/v5/public/funding-rate?instId=BTC-USD-SWAP
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Retrieve funding rate
result = publicDataAPI.get_funding_rate(
    instId="BTC-USD-SWAP",
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                          |
| :----- | :----- | :------- | :-------------------------------------------- |
| instId | String | Yes       | Instrument ID, e.g. `BTC-USD-SWAP` or X-Perps futures instId, or `ANY` to return the funding rate info of all perpetual and X-Perps futures contracts<br>Applicable to `SWAP` and X-Perps `FUTURES` |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "formulaType": "noRate",
            "fundingRate": "0.0000182221218054",
            "fundingTime": "1743609600000",
            "impactValue": "",
            "instId": "BTC-USDT-SWAP",
            "instType": "SWAP",
            "interestRate": "",
            "maxFundingRate": "0.00375",
            "method": "current_period",
            "minFundingRate": "-0.00375",
            "nextFundingRate": "",
            "nextFundingTime": "1743638400000",
            "premium": "0.0000910113652644",
            "settFundingRate": "0.0000145824401745",
            "settState": "settled",
            "ts": "1743588686291"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**      | **Type** | **Description**                                                |
| :-------------- | :------- | :------------------------------------------------------ |
| instType        | String   | Instrument type<br>`SWAP`: Perpetual futures<br>`FUTURES`: X-Perps futures |
| instId           | String   | Instrument ID, e.g. `BTC-USD-SWAP` or `ANY`                                 |
| method | String | Funding rate mechanism <br>`current_period` ~~<br>`next_period`~~(no longer supported) |
| formulaType | String | Formula type<br>`noRate`: old funding rate formula<br>`withRate`: new funding rate formula |
| fundingRate     | String   | Predicted funding rate for the upcoming settlement period. Sign: positive = long positions pay short positions at the next `fundingTime`; negative = short positions pay long positions. This is a forecast — the final settled rate may differ. See `settFundingRate` for the last settled rate. Note: the settlement interval is typically 8 hours but may be adjusted; use the difference between `fundingTime` and `nextFundingTime` to determine the actual interval.                           |
| nextFundingRate | String | ~~Forecasted funding rate for the next period <br>The nextFundingRate will be "" if the method is `current_period`~~(no longer supported) |
| fundingTime     | String   | Settlement time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| nextFundingTime  | String   | Forecasted funding time for the next period , Unix timestamp format in milliseconds, e.g. `1597026383085`|
| minFundingRate | String | The lower limit of the funding rate |
| maxFundingRate | String | The upper limit of the funding rate |
| interestRate | String | Interest rate |
| impactValue | String | Depth weighted amount (in the unit of quote currency) |
| settState | String | Settlement state of funding rate <br>`processing` <br>`settled` |
| settFundingRate | String | If settState = `processing`, it is the funding rate that is being used for current settlement cycle. <br>If settState = `settled`, it is the funding rate that is being used for previous settlement cycle |
| premium | String | Premium index<br> formula: [Max (0, Impact bid price – Index price) – Max (0, Index price – Impact ask price)] / Index price |
| ts | String | Data return time, Unix timestamp format in milliseconds, e.g. `1597026383085` |

<aside class="notice">
For some altcoins perpetual swaps with significant fluctuations in funding rates, OKX will closely monitor market changes. When necessary, the funding rate collection frequency, currently set at 8 hours, may be adjusted to higher frequencies such as 6 hours, 4 hours, 2 hours, or 1 hour. Thus, users should focus on the difference between `fundingTime` and `nextFundingTime` fields to determine the funding fee interval of a contract.
</aside>
