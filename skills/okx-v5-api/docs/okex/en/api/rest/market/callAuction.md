---
site: okex
lang: en
name: GET / Call auction details
method: GET
path: /api/v5/market/call-auction-details
module: market
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Call auction details
Retrieve call auction details.

#### Rate Limit: 20 requests per 2 seconds 
#### Rate limit rule: IP
#### HTTP Request

`GET /api/v5/market/call-auction-details`

> Request Example

```shell
GET /api/v5/market/call-auction-details?instId=ONDO-USDC

```

#### Request Parameters

| Parameter | Type   | Required | Description                                           |
| :----- | :----- | :------- | :--------------------------------------------- |
| instId | String | Yes      | Instrument ID, e.g. `BTC-USDT`         |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "instId": "ONDO-USDC",
            "unmatchedSz": "9988764",
            "eqPx": "0.6",
            "matchedSz": "44978",
            "state": "continuous_trading",
            "auctionEndTime": "1726542000000",
            "ts": "1726542000007"
        }
    ]
}
```

#### Response Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| instId | String | Instrument ID |
| eqPx | String | Equilibrium price |
| matchedSz | String | Matched size for both buy and sell<br>The unit is in base currency |
| unmatchedSz | String | Unmatched size |
| auctionEndTime | String | Call auction end time. Unix timestamp in milliseconds. |
| state | String | Trading state of the symbol<br>`call_auction`<br>`continuous_trading`  |
| ts | String | Data generation time. Unix timestamp in millieseconds. |

<aside class="notice">
During call auction, users can get the updates of equilibrium price, matched size, unmatched size, and auction end time. The data will be updated around once a second. The endpoint returns the actual open price, matched size, and unmatched size when the call auction ends.
<br>
For symbols that never go through call auction, the endpoint will also return results but with state always as `continuous_trading` and other fields as 0 or empty.
</aside>
