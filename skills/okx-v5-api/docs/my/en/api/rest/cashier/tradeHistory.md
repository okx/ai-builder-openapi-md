---
site: my
lang: en
name: Get buy/sell trade history
method: GET
path: /api/v5/fiat/buy-sell/history
module: fiat
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### Get buy/sell trade history

#### Rate Limit: 6 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/fiat/buy-sell/history`

> Request Example

```shell
GET /api/v5/fiat/buy-sell/history
```

#### Request Parameters

| Parameters | Types  | Required | Description |
|:----------|:-------|:---------|:------------|
| ordId     | String | No       | Order ID |
| clOrdId   | String | No       | Client Order ID as assigned by the client <br> A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| state     | String | No       | Trade state <br> `processing` <br> `completed` <br> `failed` |
| begin     | String | No       | Filter with a begin timestamp. Unix timestamp format in milliseconds, e.g. `1597026383085` |
| end       | String | No       | Filter with an end timestamp. Unix timestamp format in milliseconds, e.g. `1597026383085` |
| limit     | String | No       | Number of results per request. The maximum is 100. The default is 100. |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "ordId": "1234",
            "clOrdId": "",
            "quoteId": "quoterBTC-USD16461885104612381",
            "state":"completed",
            "side":"buy",
            "fromCcy": "USD",
            "toCcy": "BTC",
            "rfqAmt": "30",
            "rfqCcy": "USD",
            "fillPx": "2932.40104429",
            "fillQuoteCcy": "USD",
            "fillFromAmt": "30",
            "fillToAmt": "0.01",
            "cTime": "1646188510461",
            "uTime": "1646188510461"
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
| fromCcy       | String | Currency to sell |
| toCcy         | String | Currency to buy |
| rfqAmt        | String | RFQ amount |
| rfqCcy        | String | RFQ currency |
| fillPx        | String | Filled price based on quote currency |
| fillQuoteCcy  | String | Filled price quote currency <br> e.g. `USD` |
| fillFromAmt   | String | Filled amount unit in fromCcy |
| fillToAmt     | String | Filled amount unit in toCcy |
| cTime         | String | Request time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
| uTime         | String | Updated time, Unix timestamp format in milliseconds, e.g. `1597026383085` |

    <aside class="notice">
        This feature is only available to EU users at the moment.
    </aside>
