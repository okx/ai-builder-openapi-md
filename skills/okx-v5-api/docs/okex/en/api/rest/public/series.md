---
site: okex
lang: en
name: Get series
method: GET
path: /api/v5/public/event-contract/series
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get series

Retrieve the list of series for OKX prediction markets.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/public/event-contract/series`

> Request Example

```shell
GET /api/v5/public/event-contract/series?seriesId=BTC-ABOVE-DAILY
```

#### Request Parameters

| Parameter | Type | Required | Description |
|:----------|:-----|:---------|:------------|
| seriesId | String | No | Series ID, e.g. `BTC-ABOVE-DAILY`. If not passed, all series will be returned. |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "seriesId": "BTC-ABOVE-DAILY",
            "freq": "daily",
            "title": "BTC price above 15k",
            "category": "Crypto",
            "settlement": {
                "method": "price_above",
                "closeEarly": false,
                "srcName": "okx_index",
                "underlying": "BTC-USDT"
            }
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter | Type | Description |
|:----------|:-----|:------------|
| seriesId | String | Series ID, e.g. `BTC-ABOVE-DAILY` |
| freq | String | Frequency of the series<br>`five_min`<br>`fifteen_min`<br>`hourly`<br>`daily`<br>`monthly` |
| title | String | Series title |
| category | String | Category which this series belongs to, e.g. `Crypto` |
| settlement | Object | Settlement information |
| > method | String | Settlement method.<br>`price_up_down`: Price up/down<br>`price_above`: Price above<br>`hit`: Hit (price touches strike level, settles immediately)<br>`between`: Between (settle price within [floorStrike, capStrike) range) |
| > closeEarly | Boolean | Whether the market can be settled earlier than the expiration time.<br>`true`<br>`false` |
| > srcName | String | Settlement source name, e.g. `okx_index`, `cf_benchmark_index` |
| > underlying | String | Price underlying in OKX trading symbol format, e.g. `BTC-USDT`. Only applicable to price-related settlement methods. |
