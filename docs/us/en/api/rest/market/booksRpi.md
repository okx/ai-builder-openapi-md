---
site: us
lang: en
name: GET / RPI order book
method: GET
path: /api/v5/market/books-rpi
module: market
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### GET / RPI order book

Retrieve the consolidated order book of the instrument, combining organic depth with the currently tradeable Retail Price Improvement (RPI) depth at each price level. Non-tradeable RPI orders are filtered out platform-side and are not returned.<br>
The data is refreshed every 200 milliseconds. This endpoint does not return data immediately. Instead, it returns the latest data once the server-side cache has been updated.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/market/books-rpi`

> Request Example

```shell
GET /api/v5/market/books-rpi?instId=BTC-USDT-SWAP&sz=3

```

#### Request Parameters

| Parameter | Type   | Required | Description                                           |
| :----- | :----- | :------- | :--------------------------------------------- |
| instId | String | Yes      | Instrument ID, e.g. `BTC-USDT-SWAP` |
| sz     | String | No       | Order book depth per side. Maximum 400, e.g. 400 bids + 400 asks <br>Defaults to `1` depth level |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "asks": [
                [
                    "67855.2",
                    "0.5",
                    "0.5",
                    "1"
                ],
                [
                    "67856.0",
                    "1.3",
                    "1.0",
                    "4"
                ],
                [
                    "67860.5",
                    "0.3",
                    "0",
                    "1"
                ]
            ],
            "bids": [
                [
                    "67854.8",
                    "1.7",
                    "1.2",
                    "3"
                ],
                [
                    "67853.0",
                    "0.8",
                    "0.8",
                    "1"
                ]
            ],
            "ts": "1785310731002",
            "seqId": 332042172451
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**       |
| :--------- | :------- | :------------- |
| asks       | Array of Arrays    | Order book on sell side. Each element is `[price, totalQty, nonRpiQty, count]` |
| bids       | Array of Arrays    | Order book on buy side. Each element is `[price, totalQty, nonRpiQty, count]` |
| ts         | String   | Order book generation time, Unix timestamp format in milliseconds |
| seqId      | Integer  | Sequence ID of the current message. Provides sequencing parity with the `books-rpi` WebSocket channel |

<aside class="notice">
An example of the array of asks and bids values: ["67856.0", "1.3", "1.0", "4"]<br>
- "67856.0" is the depth price<br>
- "1.3" is <code>totalQty</code> — the total quantity at the price, organic plus currently tradeable RPI (number of contracts for derivatives, quantity in base currency for Spot and Spot Margin)<br>
- "1.0" is <code>nonRpiQty</code> — the organic (non-RPI) portion of the quantity at the price<br>
- "4" is the number of orders at the price, organic plus currently tradeable RPI.<br>
<br>
The tradeable RPI quantity at a price level is derived as <code>totalQty - nonRpiQty</code>. A taker with RPI access can execute against <code>totalQty</code>; a taker without RPI access can execute only against <code>nonRpiQty</code>, even though both read the same feed. Set `rpiTakerAccess` to `true` when placing an order to access RPI liquidity.<br>
<br>
Note that the third position carries <code>nonRpiQty</code> on this endpoint only. On the `books`, `books-full` and `books-lite` endpoints the same position is a deprecated field that is always "0".<br>
<br>
A level where <code>totalQty</code> equals <code>nonRpiQty</code> simply has no tradeable RPI depth at that price. This is expected for instruments with no RPI market maker quoting them, and for any level where resting RPI orders are currently hidden under the matching rules.
</aside>

<aside class="notice">
This endpoint returns no <code>checksum</code>. Use <code>seqId</code> for sequencing.<br>
If the RPI tradeability state is unavailable, the endpoint fails closed: RPI quantity is excluded and every level returns <code>totalQty</code> equal to <code>nonRpiQty</code>.
</aside>
