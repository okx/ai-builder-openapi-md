---
site: us
lang: en
name: Get funding rate history
method: GET
path: /api/v5/public/funding-rate-history
module: public
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### Get funding rate history

Retrieve funding rate history. This endpoint can return data up to three months.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: IP + Instrument ID

#### HTTP Request

`GET /api/v5/public/funding-rate-history`

> Request Example

```shell
GET /api/v5/public/funding-rate-history?instId=BTC-USD-SWAP
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Retrieve funding rate history
result = publicDataAPI.funding_rate_history(
    instId="BTC-USD-SWAP",
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                         |
| :----- | :----- | :------- | :----------------------------------------------------------- |
| instId | String | Yes       | Instrument ID, e.g. `BTC-USD-SWAP` or X-Perps futures instId<br>Applicable to `SWAP` and X-Perps `FUTURES` |
| before  | String | No       | Pagination of data to return records newer than the requested `fundingTime` |
| after | String | No       | Pagination of data to return records earlier than the requested `fundingTime` |
| limit  | String | No       | Number of results per request. The maximum is `400`; The default is `400`           |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "formulaType": "noRate",
            "fundingRate": "0.0000746604960499",
            "fundingTime": "1703059200000",
            "instId": "BTC-USD-SWAP",
            "instType": "SWAP",
            "method": "next_period",
            "realizedRate": "0.0000746572360545"
        },
        {
            "formulaType": "noRate",
            "fundingRate": "0.000227985782722",
            "fundingTime": "1703030400000",
            "instId": "BTC-USD-SWAP",
            "instType": "SWAP",
            "method": "next_period",
            "realizedRate": "0.0002279755647389"
        }
  ]
}
```

#### Response Parameters

| **Parameter**   | **Type** | **Description**                                               |
| :----------- | :------- | :----------------------------------------------------- |
| instType     | String   | Instrument type<br>`SWAP`: Perpetual futures<br>`FUTURES`: X-Perps futures |
| instId        | String   | Instrument ID, e.g. `BTC-USD-SWAP`                              |
| formulaType | String | Formula type<br>`noRate`: old funding rate formula<br>`withRate`: new funding rate formula |
| fundingRate  | String   | Predicted funding rate                          |
| realizedRate | String   | Actual funding rate                        |
| fundingTime  | String   | Settlement time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| method | String | Funding rate mechanism <br>`current_period` <br>`next_period` |

<aside class="notice">
For some altcoins perpetual swaps with significant fluctuations in funding rates, OKX will closely monitor market changes. When necessary, the funding rate collection frequency, currently set at 8 hours, may be adjusted to higher frequencies such as 6 hours, 4 hours, 2 hours, or 1 hour. Thus, users should focus on the difference between `fundingTime` and `nextFundingTime` fields to determine the funding fee interval of a contract.
</aside>
