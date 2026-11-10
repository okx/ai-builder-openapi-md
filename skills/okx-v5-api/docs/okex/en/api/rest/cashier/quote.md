---
site: okex
lang: en
name: Get buy/sell quote
method: POST
path: /api/v5/fiat/buy-sell/quote
module: fiat
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get buy/sell quote

#### Rate Limit: 10 requests per second
#### Rate limit rule: User ID
#### Rate Limit: 1 request per 5 seconds
#### Rate limit rule: Instrument ID

#### HTTP Request

`POST /api/v5/fiat/buy-sell/quote`

> Request Example

```shell
# Sell USD to buy 0.1 BTC
POST /api/v5/fiat/buy-sell/quote
body
{
    "side":"buy",
    "fromCcy": "USD",
    "toCcy": "BTC",
    "rfqAmt": "0.1",
    "rfqCcy": "BTC"
}

# Sell 30 USD to buy BTC
POST /api/v5/fiat/buy-sell/quote
body
{
    "side":"buy",
    "fromCcy": "USD",
    "toCcy": "BTC",
    "rfqAmt": "30",
    "rfqCcy": "USD"
}

# Sell BTC to buy 30 USD
POST /api/v5/fiat/buy-sell/quote
body
{
    "side":"sell",
    "fromCcy": "BTC",
    "toCcy": "USD",
    "rfqAmt": "30",
    "rfqCcy": "USD"
}

# Sell 0.1 BTC to buy USD
POST /api/v5/fiat/buy-sell/quote
body
{
    "side":"sell",
    "fromCcy": "BTC",
    "toCcy": "USD",
    "rfqAmt": "0.1",
    "rfqCcy": "BTC"
}
```

#### Request Parameters

| Parameters | Types  | Required | Description |
|:----------|:-------|:---------|:------------|
| side      | String | Yes      | Side  <br> `buy`: Buy Crypto / Fiat with Fiat  <br> `sell`: Sell Crypto to Crypto / Fiat |
| fromCcy   | String | Yes      | Currency to sell |
| toCcy     | String | Yes      | Currency to buy |
| rfqAmt    | String | Yes      | RFQ amount |
| rfqCcy    | String | Yes      | RFQ currency |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "quoteId": "quoterBTC-USD16461885104612381",
            "fromCcy": "USD",
            "toCcy": "BTC",
            "rfqAmt": "30",
            "rfqCcy": "USD",
            "quotePx": "2932.40104429",
            "quoteCcy": "USD",
            "quoteFromAmt": "30",
            "quoteToAmt": "30",
            "quoteTime": "1646188510461",
            "ttlMs": "10000"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter        | Type   | Description |
|-----------------|--------|-------------|
| quoteId         | String | Quote ID |
| side            | String | Side  <br> `buy`: Buy Crypto / Fiat with Fiat  <br> `sell`: Sell Crypto to Crypto / Fiat |
| fromCcy         | String | Currency to sell, e.g. `USD` |
| toCcy           | String | Currency to buy, e.g. `BTC` |
| rfqAmt          | String | RFQ amount |
| rfqCcy          | String | RFQ currency |
| quotePx         | String | Quote price |
| quoteCcy        | String | Quote price unit <br> e.g. `USD` |
| quoteFromAmt    | String | Quote amount, unit in `fromCcy` |
| quoteToAmt      | String | Quote amount, unit in `toCcy` |
| quoteTime       | String | Quotation generation time, Unix timestamp format in milliseconds, e.g. 1597026383085 |
| ttlMs           | String | The validity period of quotation in milliseconds <br> e.g. `10000` represents the quotation only valid for 10 seconds |

    <aside class="notice">
        This feature is only available to Bahamas institutional users at the moment.
    </aside>
