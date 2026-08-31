---
site: turkey
lang: en
name: Convert trade
method: POST
path: /api/v5/asset/convert/trade
module: asset
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---


### Convert trade

You should make [estimate quote](/docs-v5/en/#funding-account-rest-api-estimate-quote) before convert trade. 

<aside class="warning">
Only assets in the trading account supported convert.
</aside>

#### Rate Limit: 10 requests per second
#### Rate limit rule: User ID

For the same side (buy/sell), there's a trading limit of 1 request per 5 seconds.

#### HTTP Request

`POST /api/v5/asset/convert/trade`

> Request Example

```shell
POST /api/v5/asset/convert/trade
body
{
	"baseCcy": "ETH",
	"quoteCcy": "USDT",
	"side": "buy",
	"sz": "30",
	"szCcy": "USDT",
	"quoteId": "quoterETH-USDT16461885104612381"
}
```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**  |
| :----- | :----- | :------- | :----------------------------------------------------------- |
| quoteId    | String | Yes       | Quote ID                                             |
| baseCcy    | String | Yes       | Base currency, e.g. `BTC` in `BTC-USDT`                |
| quoteCcy    | String | Yes       | Quote currency, e.g. `USDT` in `BTC-USDT`             |
| side   | String | Yes       | Trade side based on `baseCcy`<br>`buy` `sell`  |
| sz     | String | Yes       | Quote amount<br>The quote amount should no more then RFQ amount  |
| szCcy  | String | Yes       | Quote currency                              |
| clTReqId    | String | No       | Client Order ID as assigned by the client<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| tag    | String | No       | Order tag<br>Applicable to broker user |
| convertMode | String | No       | `0`: standard convert (default) <br> `1`: large order convert for VIP |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "baseCcy": "ETH",
            "clTReqId": "",
            "fillBaseSz": "0.01023052",
            "fillPx": "2932.40104429",
            "fillQuoteSz": "30",
            "instId": "ETH-USDT",
            "quoteCcy": "USDT",
            "quoteId": "quoterETH-USDT16461885104612381",
            "side": "buy",
            "state": "fullyFilled",
            "tradeId": "trader16461885203381437",
            "ts": "1646188520338"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter   | Type    | Description                                                                     |
|:------------| :----- |:--------------------------------------------------------------------------------|
| tradeId     | String | Trade ID                                                                        |
| quoteId     | String | Quote ID                                                                        |
| clTReqId    | String | Client Order ID as assigned by the client                                       |
| state       | String | Trade state<br>`fullyFilled`: success<br>`rejected`: failed                     |
| instId      | String | Currency pair, e.g. `BTC-USDT`                                                  |
| baseCcy     | String | Base currency, e.g. `BTC` in `BTC-USDT`                                         |
| quoteCcy    | String | Quote currency, e.g. `USDT` in `BTC-USDT`                                       |
| side        | String | Trade side based on `baseCcy`<br>`buy`<br>`sell`                                |
| fillPx      | String | Filled price based on quote currency                                            |
| fillBaseSz  | String | Filled amount for base currency                                                 |
| fillQuoteSz | String | Filled amount for quote currency                                                |
| ts          | String | Convert trade time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
