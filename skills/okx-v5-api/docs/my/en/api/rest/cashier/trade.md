---
site: my
lang: en
name: Buy/sell trade
method: POST
path: /api/v5/fiat/buy-sell/trade
module: fiat
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### Buy/sell trade

#### Rate Limit: 1 request per 5 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/fiat/buy-sell/trade`

> Request Example

```shell
# Sell 30 USD to buy BTC
POST /api/v5/fiat/buy-sell/trade
body
{
    "clOrdId":"123456",
    "side":"sell",
    "fromCcy": "USD",
    "toCcy": "BTC",
    "rfqAmt": "30",
    "rfqCcy": "USD",
    "paymentMethod":"balance",
    "quoteId": "quoterETH-USDT16461885104612381"
}
```

#### Request Parameters

| Parameters     | Types  | Required | Description |
|:--------------|:-------|:---------|:------------|
| quoteId       | String | Yes      | Quote ID<br>Get from Buy/Sell quote API |
| side          | String | Yes      | Side  <br> `buy`: Buy Crypto / Fiat with Fiat  <br> `sell`: Sell Crypto to Crypto / Fiat <br>Should be the same as the Quote request |
| fromCcy       | String | Yes      | Currency to sell <br>Should be the same as the Quote request |
| toCcy         | String | Yes      | Currency to buy <br>Should be the same as the Quote request |
| rfqAmt        | String | Yes      | RFQ amount <br>Should be the same as the Quote request |
| rfqCcy        | String | Yes      | RFQ currency <br>Should be the same as the Quote request |
| paymentMethod | String | Yes      | paymentMethod <br> `balance` |
| clOrdId       | String | Yes      | Client Order ID as assigned by the client <br> A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "ordId": "1234",
            "clOrdId": "",
            "quoteId": "quoterBTC-USD16461885104612381",
            "side":"buy",
            "fromCcy": "USD",
            "toCcy": "BTC",
            "rfqAmt": "30",
            "rfqCcy": "USD",
            "fillPx": "2932.40104429",
            "fillQuoteCcy": "USD",
            "fillFromAmt": "30",
            "fillToAmt": "0.01",
            "cTime": "1646188510461"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter      | Type   | Description |
|---------------|--------|-------------|
| ordId         | String | Order ID |
| clOrdId       | String | Client Order ID as assigned by the client |
| quoteId       | String | Quote ID |
| state         | String | Trade state <br> `processing` <br> `completed` <br> `failed` |
| side          | String | Side  <br> `buy`: Buy Crypto / Fiat with Fiat  <br> `sell`: Sell Crypto to Crypto / Fiat |
| fromCcy       | String | Currency to sell |
| toCcy         | String | Currency to buy |
| rfqAmt        | String | RFQ amount |
| rfqCcy        | String | RFQ currency |
| fillPx        | String | Filled price based on quote currency |
| fillQuoteCcy  | String | Filled price quote currency <br> e.g. `USD` |
| fillFromAmt   | String | Sold amount, unit in `fromCcy` |
| fillToAmt     | String | Bought amount, unit in `toCcy` |
| cTime         | String | Request time, Unix timestamp format in milliseconds, e.g. `1597026383085` |

    <aside class="notice">
        This feature is only available to EU users at the moment.
    </aside>
