---
site: us
lang: en
name: Estimate quote
method: POST
path: /api/v5/asset/convert/estimate-quote
module: asset
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### Estimate quote

#### Rate Limit: 10 requests per second 
#### Rate limit rule: User ID

#### Rate Limit: 1 request per 5 seconds 
#### Rate limit rule: Instrument ID

#### HTTP Request

`POST /api/v5/asset/convert/estimate-quote`

> Request Example

```shell
POST /api/v5/asset/convert/estimate-quote
body
{
	"baseCcy": "ETH",
	"quoteCcy": "USDT",
	"side": "buy",
	"rfqSz": "30",
	"rfqSzCcy": "USDT"
}
```

#### Request Parameters

| **Parameters** | **Types** | **Required** | **Description**  |
|:---------------| :----- | :------- | :----------------------------------------------------------- |
| baseCcy        | String | Yes       | Base currency, e.g. `BTC` in `BTC-USDT`                                             |
| quoteCcy       | String | Yes       | Quote currency, e.g. `USDT` in `BTC-USDT`                                                       |
| side           | String | Yes       | Trade side based on `baseCcy`<br>`buy` `sell`|
| rfqSz          | String | Yes       | RFQ amount |
| rfqSzCcy       | String | Yes       | RFQ currency                                                     |
| clQReqId       | String | No       | Client Order ID as assigned by the client<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| tag            | String | No       | Order tag<br>Applicable to broker user |
| convertMode    | String | No       | `0`: standard convert (default) <br> `1`: large order convert for VIP |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "baseCcy": "ETH",
            "baseSz": "0.01023052",
            "clQReqId": "",
            "cnvtPx": "2932.40104429",
            "origRfqSz": "30",
            "quoteCcy": "USDT",
            "quoteId": "quoterETH-USDT16461885104612381",
            "quoteSz": "30",
            "quoteTime": "1646188510461",
            "rfqSz": "30",
            "rfqSzCcy": "USDT",
            "side": "buy",
            "ttlMs": "10000"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter | Type    | Description                                                                            |
|:----------| :----- |:---------------------------------------------------------------------------------------|
| quoteTime | String | Quotation generation time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| ttlMs     | String | Validity period of quotation in milliseconds                                           |
| clQReqId  | String | Client Order ID as assigned by the client                                              |
| quoteId   | String | Quote ID                                                                               |
| baseCcy   | String | Base currency, e.g. `BTC` in `BTC-USDT`                                                |
| quoteCcy  | String | Quote currency, e.g. `USDT` in `BTC-USDT`                                              |
| side      | String | Trade side based on `baseCcy`                                                          |
| origRfqSz | String | Original RFQ amount                                                                    |
| rfqSz     | String | Real RFQ amount                                                                        |
| rfqSzCcy  | String | RFQ currency                                                                           |
| cnvtPx    | String | Convert price based on quote currency                                                  |
| baseSz    | String | Convert amount of base currency                                                        |
| quoteSz   | String | Convert amount of quote currency                                                       |
