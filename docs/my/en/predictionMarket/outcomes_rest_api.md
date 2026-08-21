---
site: my
lang: en
name: 1.1 Get events
method: GET
path: /api/v5/predictions/events
module: predictions
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
multi_endpoint: true
---

# REST API

## Endpoint sections

This generated file documents multiple REST endpoints. For a single-endpoint lookup, use this table or `rg -n -F "<search key>" <this-file>` to locate the target section, then read only that section instead of loading the whole file.

| Method | Path | Search key | Section |
| --- | --- | --- | --- |
| GET | `/api/v5/predictions/events` | `GET /api/v5/predictions/events` | 1.1 Get events |
| GET | `/api/v5/predictions/events/{eventId}` | `GET /api/v5/predictions/events/{eventId}` | 1.2 Get a single event |
| GET | `/api/v5/predictions/events/{eventId}/markets` | `GET /api/v5/predictions/events/{eventId}/markets` | 1.3 Get markets |
| GET | `/api/v5/predictions/markets/{marketId}` | `GET /api/v5/predictions/markets/{marketId}` | 1.4 Get a single market |
| GET | `/api/v5/predictions/events/search` | `GET /api/v5/predictions/events/search` | 1.5 Search events |
| GET | `/api/v5/market/ticker` | `GET /api/v5/market/ticker?instId={yesAssetId}` | 2.1 Get ticker |
| GET | `/api/v5/market/candles` | `GET /api/v5/market/candles?instId={yesAssetId}&bar=1m&limit=100` | 2.2 Get candlesticks |
| GET | `/api/v5/market/pm-books` | `GET /api/v5/market/pm-books?instId={yesAssetId}&sz=400` | 2.3 Get order book |
| POST | `/api/v5/predictions/orders` | `POST /api/v5/predictions/orders` | 3.1 Place order |
| POST | `/api/v5/predictions/orders/cancel` | `POST /api/v5/predictions/orders/cancel` | 3.2 Cancel a single order |
| GET | `/api/v5/predictions/orders/{orderId}` | `GET /api/v5/predictions/orders/{orderId}` | 3.3 Get a single order |
| GET | `/api/v5/predictions/orders` | `GET /api/v5/predictions/orders` | 3.4 Get user orders |
| POST | `/api/v5/predictions/orders/cancel-all` | `POST /api/v5/predictions/orders/cancel-all` | 3.5 Cancel all / market orders |
| POST | `/api/v5/predictions/heartbeat` | `POST /api/v5/predictions/heartbeat` | 3.6 Send heartbeat |
| POST | `/api/v5/predictions/positions/split` | `POST /api/v5/predictions/positions/split` | 4.1 Split (xp → YES + NO) |
| POST | `/api/v5/predictions/positions/merge` | `POST /api/v5/predictions/positions/merge` | 4.2 Merge (YES + NO → xp) |
| POST | `/api/v5/predictions/positions/redeem` | `POST /api/v5/predictions/positions/redeem` | 4.3 Redeem (redeem xp after settlement) |
| GET | `/api/v5/predictions/trades` | `GET /api/v5/predictions/trades` | 5.1 Query trades |
| GET | `/api/v5/predictions/positions` | `GET /api/v5/predictions/positions?status=open` | 6.1 Query current positions |
| GET | `/api/v5/predictions/positions` | `GET /api/v5/predictions/positions?status=closed` | 6.2 Query closed positions |
| GET | `/api/v5/predictions/positions` | `GET /api/v5/predictions/positions?marketId={marketId}` | 6.3 Query positions in a specified market |
| GET | `/api/v5/predictions/positions` | `GET /api/v5/predictions/positions` | 6.4 Unified endpoint |
| GET | `/api/v5/predictions/balance` | `GET /api/v5/predictions/balance` | 7.1 Query account balance |


The REST API lets you query events, markets, tickers, order books, orders, trades, positions, and balances, and submit write operations such as place order, cancel order, Split, Merge, and Redeem. Developers can complete the full trading loop through the REST API: discover markets, obtain the `assetId`, submit orders, query order status, manage positions, and handle settlement-related operations. The base URL for all endpoints is `https://www.okx.com`.

## 1. Events & Markets API

**Data Structures**

**OutcomeResp — Outcome option**

| Field | Type | Description |
| --- | --- | --- |
| tokenId | string / null | Conditional token contract address; null before deployment |
| assetId | string / null | TradeZone asset ID (used when placing orders); null before deployment |
| name | string | Outcome name, e.g. `"Yes"`, `"No"` |
| price | string | Current price (string in [0, 1], e.g. "0.82") |
| bgColor | string | Button background color (Hex); for a sports moneyline Yes outcome this is the home-team theme color; otherwise null |

**MarketResp — Market object**

| Field | Type | Description |
| --- | --- | --- |
| id | string | Globally unique market ID |
| marketId | string | Market's unique ID in TradeZone (used when placing orders) |
| negRisk | boolean | Whether this is a mutually exclusive (negRisk) market |
| status | string | Market status: `active` / `paused` / `settling` / `resolved` |
| settleStage | int | Settlement stage: 0=not started, 1=first-round publication, 2=first-round dispute, 3=second-round publication, 4=second-round dispute, 5=settled |
| question | string | Full market question |
| shortQuestion | string / null | Short market question |
| description | string | Market description |
| marketIcon | string / null | Market icon URL |
| bestBid | string / null | Best bid price (0–1); null if there are no bids |
| bestAsk | string / null | Best ask price (0–1); null if there are no asks |
| lastTradePrice | string / null | Last trade price (0–1); null if never traded |
| volume | string | Total volume (xp) |
| probability | string / null | Market Yes probability (decimal in [0, 1]); null before deployment |
| resolutionSources | string[] | List of resolution data source URLs |
| yesOutcome | OutcomeObject | Yes outcome option |
| noOutcome | OutcomeObject | No outcome option |
| startTime | string | Expected market start time (Unix ms) |
| endTime | string | Expected market end time (Unix ms) |
| resolveStartAt | string | First time the market entered the start_resolve state (Unix ms) |
| resolveAt | string | First time the market entered the resolved state (Unix ms) |

**EventResp — Event object**

| Field | Type | Description |
| --- | --- | --- |
| id | string | Globally unique event ID |
| eventId | string | Event's unique ID in TradeZone |
| negRisk | boolean | Whether this is a negRisk event |
| status | string | Event status: `active` / `paused` / `resolved` |
| eventTitle | string | Event title |
| description | string | Event description |
| eventIcon | string / null | Event icon URL |
| volume | string | Sum of volume across all markets in the event (xp) |
| startTime | string / null | Trading start timestamp (ms) |
| endTime | string / null | Trading stop timestamp (ms) |
| createdAt | string | Event creation timestamp (ms) |
| totalMarketsCount | int | Total number of markets under the event |
| finalOutcomesMarketId | string / null | Winning market ID after settlement; null before settlement |
| markets | array\<MarketResp\> | Market list (the list endpoint returns at most the first 2; get the full list via `GET /api/v5/predictions/events/{eventId}/markets`) |

**PaginationResp — Cursor pagination**

| Field | Type | Description |
| --- | --- | --- |
| nextCursor | string / null | Next-page cursor; null means this is the last page |
| hasMore | boolean | Whether more data is available |
| pageSize | int | Number of items returned this time |

### 1.1 Get events

Get the prediction market event list, with multi-dimensional filtering and sorting by status, etc.

#### HTTP Request

`GET /api/v5/predictions/events`

#### Request Parameters (Query)

| Parameter | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| status | string | No | `active` | Event status filter: `active` / `resolved` |
| sort | string | No | `volume_24h` | Sort order: `volume` / `volume_24h` / `ending_soon` / `newest` |
| tag | string | No | - | Sports tag ID filter (from GET /api/v5/predictions/sports/tags) |
| leagueId | string | No | - | League ID filter (from GET /api/v5/predictions/sports/tags/{tagId}/leagues) |
| cursor | string | No | - | Pagination cursor; omit on the first request |
| pageSize | int | No | 10 | Items per page (max 50) |

**Response** (`data`):

> Response example

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "events": [
      {
        "id": "100",
        "eventId": "100",
        "negRisk": false,
        "status": "active",
        "eventTitle": "Will BTC exceed $100k by end of 2026?",
        "description": "This event tracks whether Bitcoin will cross $100,000.",
        "eventIcon": "https://cdn.example.com/events/100.png",
        "volume": "1250000.00",
        "startTime": 1700000000000,
        "endTime": 1798761600000,
        "createdAt": 1710000000000,
        "totalMarketsCount": 1,
        "finalOutcomesMarketId": null,
        "markets": [
          {
            "id": "1",
            "marketId": "1",
            "oddsType": "points",
            "negRisk": false,
            "status": "active",
            "settleStage": 0,
            "question": "Will BTC exceed $100k by end of 2026?",
            "shortQuestion": "BTC > $100k?",
            "description": "Resolves YES if BTC price exceeds $100,000 before Jan 1, 2027.",
            "marketIcon": null,
            "bestBid": "0.64",
            "bestAsk": "0.66",
            "lastTradePrice": "0.65",
            "volume": "1250000.00",
            "probability": "0.65",
            "resolutionSources": ["https://coinmarketcap.com"],
            "yesOutcome": {
              "tokenId": "0xabc...001",
              "assetId": "1",
              "name": "Yes",
              "price": "0.65",
              "finalResult": null
            },
            "noOutcome": {
              "tokenId": "0xabc...002",
              "assetId": "2",
              "name": "No",
              "price": "0.35",
              "finalResult": null
            }
          }
        ]
      }
    ],
    "pagination": {
      "nextCursor": "eyJpZCI6MTAwfQ",
      "hasMore": true,
      "pageSize": 10
    }
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| events | array\<EventResp\> | Event list; each event's `markets` returns at most the first 2 markets |
| pagination | PaginationResp | Cursor pagination info |

### 1.2 Get a single event

Get the full information for a specified event. The `markets` field returns all markets under the event (not truncated).

#### HTTP Request

`GET /api/v5/predictions/events/{eventId}`

#### Path Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| eventId | string | Yes | Event ID |

**Response** (`data`):

> Response example

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "id": "100",
    "eventId": "100",
    "negRisk": false,
    "status": "active",
    "eventTitle": "NBA Finals 2026 - Lakers vs Celtics",
    "description": "Who will win the 2026 NBA Championship?",
    "eventIcon": "https://cdn.example.com/events/100.png",
    "volume": "3200000.00",
    "startTime": 1748000000000,
    "endTime": 1750000000000,
    "createdAt": 1710000000000,
    "totalMarketsCount": 3,
    "finalOutcomesMarketId": null,
    "markets": [ ]
  }
}
```

Returns the full EventResp, with `markets` containing all markets under the event.

### 1.3 Get markets

Get all markets under a specified event, without pagination.

#### HTTP Request

`GET /api/v5/predictions/events/{eventId}/markets`

#### Path Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| eventId | string | Yes | Event ID |

**Response** (`data`):

> Response example

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "markets": [
      {
        "id": "1",
        "marketId": "1",
        "negRisk": false,
        "status": "active",
        "settleStage": 0,
        "question": "Will BTC exceed $100k by end of 2026?",
        "shortQuestion": "BTC > $100k?",
        "description": "Resolves YES if BTC price exceeds $100,000 before Jan 1, 2027.",
        "marketIcon": null,
        "bestBid": "0.64",
        "bestAsk": "0.66",
        "lastTradePrice": "0.65",
        "volume": "1250000.00",
        "probability": "0.65",
        "resolutionSources": ["https://coinmarketcap.com"],
        "yesOutcome": {
          "tokenId": "0xabc...001",
          "assetId": "1",
          "name": "Yes",
          "price": "0.65",
          "finalResult": null
        },
        "noOutcome": {
          "tokenId": "0xabc...002",
          "assetId": "2",
          "name": "No",
          "price": "0.35",
          "finalResult": null
        }
      }
    ]
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| markets | array\<MarketResp\> | All markets under the event |

### 1.4 Get a single market

Get the detailed information for a specified market.

#### HTTP Request

`GET /api/v5/predictions/markets/{marketId}`

#### Path Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| marketId | string | Yes | Market TradeZone ID |

**Response** (`data`):

> Response example

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "id": "1",
    "marketId": "1",
    "negRisk": false,
    "status": "resolved",
    "settleStage": 5,
    "question": "Will BTC exceed $100k by end of 2026?",
    "shortQuestion": "BTC > $100k?",
    "description": "Resolves YES if BTC price exceeds $100,000 before Jan 1, 2027.",
    "marketIcon": null,
    "bestBid": null,
    "bestAsk": null,
    "lastTradePrice": "0.98",
    "volume": "1250000.00",
    "probability": null,
    "resolutionSources": ["https://coinmarketcap.com"],
    "yesOutcome": {
      "tokenId": "0xabc...001",
      "assetId": "1",
      "name": "Yes",
      "price": "1.00",
      "finalResult": true
    },
    "noOutcome": {
      "tokenId": "0xabc...002",
      "assetId": "2",
      "name": "No",
      "price": "0.00",
      "finalResult": false
    }
  }
}
```

Returns the full MarketResp.

### 1.5 Search events

Full-text search of events by keyword. Returns the matching event list, with cursor pagination.

#### HTTP Request

`GET /api/v5/predictions/events/search`

#### Request Parameters (Query)

| Parameter | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| keyword | string | Yes | - | Search keyword, matched against event title and description |
| cursor | string | No | - | Pagination cursor; omit on the first request |
| pageSize | int | No | 10 | Items per page (max 50) |

**Response** (`data`):

> Response example

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "events": [
      {
        "id": "100",
        "eventId": "100",
        "negRisk": false,
        "status": "active",
        "eventTitle": "Will BTC exceed $100k by end of 2026?",
        "description": "This event tracks whether Bitcoin will cross $100,000.",
        "eventIcon": "https://cdn.example.com/events/100.png",
        "volume": "1250000.00",
        "startTime": 1700000000000,
        "endTime": 1798761600000,
        "createdAt": 1710000000000,
        "totalMarketsCount": 1,
        "finalOutcomesMarketId": null,
        "markets": [
          {
            "id": "1",
            "marketId": "1",
            "oddsType": "points",
            "negRisk": false,
            "status": "active",
            "settleStage": 0,
            "question": "Will BTC exceed $100k by end of 2026?",
            "shortQuestion": "BTC > $100k?",
            "description": "Resolves YES if BTC price exceeds $100,000 before Jan 1, 2027.",
            "marketIcon": null,
            "bestBid": "0.64",
            "bestAsk": "0.66",
            "lastTradePrice": "0.65",
            "volume": "1250000.00",
            "probability": "0.65",
            "resolutionSources": ["https://coinmarketcap.com"],
            "yesOutcome": {
              "tokenId": "0xabc...001",
              "assetId": "1",
              "name": "Yes",
              "price": "0.65",
              "finalResult": null
            },
            "noOutcome": {
              "tokenId": "0xabc...002",
              "assetId": "2",
              "name": "No",
              "price": "0.35",
              "finalResult": null
            }
          }
        ]
      }
    ],
    "pagination": {
      "nextCursor": "eyJpZCI6MTAwfQ",
      "hasMore": true,
      "pageSize": 10
    }
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| events | array\<EventResp\> | Event list |
| pagination | PaginationResp | Cursor pagination info |

## 2. Price API

<aside class="notice">
<b>Note:</b> the <code>instId</code> parameter in all of the following endpoints is the prediction market's <code>yesAssetId</code>. The <code>yesAssetId</code> is provided by the prediction-market backend.
</aside>

### 2.1 Get ticker

Get the latest ticker for a single prediction market instrument, used to display the last trade price in the order book.

#### Request Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `instId` | String | Yes | Prediction market yesAssetId |

#### HTTP Request

`GET /api/v5/market/ticker?instId={yesAssetId}`

#### Response Parameters

> Response example

```json
{
  "code": "0",
  "msg": "",
  "data": [{
    "instType": "SPOT",
    "instId": "{yesAssetId}",
    "last": "0.65",
    "lastSz": "100",
    "askPx": "0.66",
    "askSz": "500",
    "bidPx": "0.64",
    "bidSz": "300",
    "open24h": "0.60",
    "high24h": "0.70",
    "low24h": "0.55",
    "vol24h": "10000",
    "volCcy24h": "6500",
    "sodUtc0": "0.62",
    "sodUtc8": "0.61",
    "ts": "1711900800000"
  }]
}
```

| Field | Type | Description |
| --- | --- | --- |
| `instType` | String | Instrument type |
| `instId` | String | Instrument ID |
| `last` | String | Last trade price |
| `lastSz` | String | Last trade size |
| `askPx` | String | Best ask price |
| `askSz` | String | Size at the best ask |
| `bidPx` | String | Best bid price |
| `bidSz` | String | Size at the best bid |
| `open24h` | String | 24h open price |
| `high24h` | String | 24h high price |
| `low24h` | String | 24h low price |
| `vol24h` | String | 24h volume (in contracts) |
| `volCcy24h` | String | 24h volume (in quote currency) |
| `sodUtc0` | String | Open price at UTC 0 |
| `sodUtc8` | String | Open price at UTC+8 |
| `ts` | String | Data update time (Unix ms timestamp) |

### 2.2 Get candlesticks

Query historical candlestick data for a prediction market instrument. Candlesticks are grouped and returned by the requested bar size; at most 1,440 candles are available per bar size.

#### Request Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `instId` | String | Yes | Prediction market yesAssetId |
| `bar` | String | No | Bar size, default `1m`. Allowed values: `1m` `3m` `5m` `15m` `30m` `1H` `2H` `4H` `6H` `12H` `1D` `1W` `1M` |
| `after` | String | No | Pagination cursor; return data before this timestamp (Unix ms) |
| `before` | String | No | Pagination cursor; return data after this timestamp (Unix ms) |
| `limit` | String | No | Items per page, max 100, default 100 |

#### HTTP Request

`GET /api/v5/market/candles?instId={yesAssetId}&bar=1m&limit=100`

**Response Data**

(data is a 2D array; each record's field order is as follows)

> Response example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    ["1711900800000", "0.65", "0.67", "0.63", "0.66", "2000", "1300", "1300", "1"],
    ["1711900740000", "0.63", "0.66", "0.62", "0.65", "1800", "1170", "1170", "1"]
  ]
}
```

| Index | Field | Type | Description |
| --- | --- | --- | --- |
| 0 | `ts` | String | Candle open time (Unix ms timestamp) |
| 1 | `o` | String | Open price |
| 2 | `h` | String | High price |
| 3 | `l` | String | Low price |
| 4 | `c` | String | Close price |
| 5 | `vol` | String | Volume (in contracts) |
| 6 | `volCcy` | String | Volume (in quote currency) |
| 7 | `volCcyQuote` | String | Volume (in quote currency) |
| 8 | `confirm` | String | Candle state: `0` not confirmed, `1` confirmed |

<aside class="notice">
<b>Pagination:</b> data is returned in reverse chronological order. To page, take the <code>ts</code> of the last record as the <code>after</code> parameter of the next request.
</aside>

### 2.3 Get order book

Query a bid/ask depth snapshot for a prediction market instrument.

#### Request Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `instId` | String | Yes | Prediction market yesAssetId |
| `sz` | String | No | Number of depth levels; max 400 per side, i.e. up to 800 across both sides. If omitted, defaults to `1` level. |

#### HTTP Request

`GET /api/v5/market/pm-books?instId={yesAssetId}&sz=400`

#### Response Parameters

> Response example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "asks": [
        ["67364.1","0.45478048","5"]
      ],
      "bids": [
        ["67364","1.72315936", "17"]
      ],
      "ts": "1774943488756",
      "seqId": 74487243135
    }
  ]
}

```

| Field | Type | Description |
| --- | --- | --- |
| `asks` | Array | Ask levels, sorted by price **low to high** |
| `bids` | Array | Bid levels, sorted by price **high to low** |
| `ts` | String | Depth snapshot time (Unix ms timestamp) |
| `seqId` | Number | Order book version number; not relevant for prediction markets |

Each record in the `asks` / `bids` arrays has the format: `[price, size, order count]`

## 3. Orders API

### 3.1 Place order

Submit a signed order request. The developer constructs the calldata and signs it with their own wallet private key (ECDSA); after the server validates it, the order is written to the database and forwarded directly to TradeZone (it does not go through the AA Wallet).

#### HTTP Request

`POST /api/v5/predictions/orders`

#### Request Body

> Request example

```json
{
  "action": {
    "type": "placeOrder",
    "grouping": "na",
    "orders": [{
      "assetId": "1",
      "marketType": "prediction",
      "side": "buy",
      "price": "0.65",
      "size": "100",
      "reduceOnly": false,
      "clientOrderId": "0x0197a98c91312671ca83f15ccbd5186f",
      "orderType": { "limit": { "tif": "gtc" } }
    }]
  },
  "nonce": 1708929600000,
  "signature": {
    "Ecdsa": {
      "r": "0x...",
      "s": "0x...",
      "v": 1
    }
  }
}
```

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| action | object | Yes | Order action |
| action.type | string | Yes | Fixed `"placeOrder"` |
| action.grouping | string | Yes | Fixed `"na"` |
| action.orders | array | Yes | Order list (one order per request) |
| action.orders[].assetId | string | Yes | TradeZone asset ID (e.g. `"1"`) |
| action.orders[].marketType | string | Yes | Fixed `"prediction"` |
| action.orders[].side | string | Yes | `"buy"` / `"sell"` |
| action.orders[].price | string | Yes | Order price (e.g. `"0.65"`) |
| action.orders[].size | string | Yes | Order size (e.g. `"100"`) |
| action.orders[].clientOrderId | string | Yes | Client order ID; used to distinguish region |
| action.orders[].reduceOnly | boolean | No | Whether reduce-only, default `false` |
| action.orders[].sizeType | string | No | `"base"` (default) / `"quote"` |
| action.orders[].orderType | object | Yes | Order type |
| action.orders[].orderType.limit.tif | string | Yes | `"gtc"` / `"gtd"` / `"ioc"` / `"fok"` / `"alo"` |
| nonce | long | Yes | Request timestamp (ms), anti-replay |
| signature | object | Yes | Signature object |
| signature.Ecdsa | object | Yes | ECDSA signature components |
| signature.Ecdsa.r | string | Yes | Signature r value (`"0x..."`) |
| signature.Ecdsa.s | string | Yes | Signature s value (`"0x..."`) |
| signature.Ecdsa.v | int | Yes | Signature v value (`0` or `1`) |

**TIF (Time In Force)**

| JSON value | Trade structure | Description |
| --- | --- | --- |
| `"gtc"` | "tif": "gtc" | Good-Til-Cancel; the order stays until filled or cancelled |
| `"gtd"` | "tif": { "gtd": { "expiresAfter": 1700000005000 } }, | Good-Til-Date; the order stays until it expires at expiresAfter, or is filled/cancelled |
| `"ioc"` | "tif": "ioc" | Immediate-Or-Cancel (i.e. FAK); fills as much as possible immediately, then cancels the remainder. **For simulated market orders only**; must be paired with a price protection upper/lower bound (`price`) |
| `"fok"` | "tif": "fok" | Fill-Or-Kill; fully filled or fully cancelled. **For simulated market orders only**; must be paired with a price protection upper/lower bound (`price`) |
| `"alo"` | "tif": "alo" | Add-Liquidity-Only (Post-Only); maker-only order, rejected if it would fill immediately |

**Response** (`data`):

> Response example

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "txHash": "0xdef...789"
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| txHash | string | TradeZone transaction hash |

### 3.2 Cancel a single order

Cancel an active order. The developer constructs the cancel calldata and signs it; after validating the order's ownership and status, the server submits it to TradeZone.

#### HTTP Request

`POST /api/v5/predictions/orders/cancel`

#### Request Body

> Request example

```json
{
  "action": {
    "type": "cancel",
    "cancels": [{
      "assetId": "1",
      "marketType": "prediction",
      "oid": "12345",
      "clientOrderId": "0x"
    }]
  },
  "nonce": 1708929600000,
  "signature": {
    "Ecdsa": {
      "r": "0x...",
      "s": "0x...",
      "v": 1
    }
  }
}
```

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| action | object | Yes | Cancel action |
| action.type | string | Yes | Fixed `"cancel"` |
| action.cancels | array | Yes | Cancel list (one per request) |
| action.cancels[].assetId | string | Yes | TradeZone asset ID (e.g. `"1"`) |
| action.cancels[].marketType | string | Yes | Fixed `"prediction"` |
| action.cancels[].oid | string | No | Order ID; exactly one of oid and clientOrderId is required |
| action.cancels[].clientOrderId | string | No | Client order ID; exactly one of oid and clientOrderId is required |
| nonce | long | Yes | Request timestamp (ms) |
| signature | object | Yes | Signature object |
| signature.Ecdsa | object | Yes | ECDSA signature components |
| signature.Ecdsa.r | string | Yes | Signature r value (`"0x..."`) |
| signature.Ecdsa.s | string | Yes | Signature s value (`"0x..."`) |
| signature.Ecdsa.v | int | Yes | Signature v value (`0` or `1`) |

**Response** (`data`):

> Response example

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "txHash": "0xdef...abc"
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| txHash | string | TradeZone transaction hash |

### 3.3 Get a single order

Get the full details of a specified order (active or historical).

#### HTTP Request

`GET /api/v5/predictions/orders/{orderId}`

#### Path Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| orderId | string | Yes | Order ID |

**Response** (`data`):

> Response example

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "id": "1",
    "oid": "1",
    "clientOrderId": "1",
    "marketId": "1",
    "assetId": "1",
    "side": "BUY",
    "orderType": "GTC",
    "sizeType": "BASE",
    "size": "100",
    "price": "0.65",
    "expiration": null,
    "txHash": "0xdef...789",
    "status": "ACTIVE",
    "filledSize": "40",
    "filledAmount": "26",
    "failReason": null,
    "cancelReason": null,
    "oddsType": "points",
    "createdAt": "1710000000000",
    "updatedAt": "1710000000000"
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| id | string | Order ID |
| oid | string | Order oid |
| clientOrderId | string | Order clientOrderId |
| marketId | string | Market ID |
| tokenId | string | YES/NO token address |
| assetId | string | TradeZone asset ID |
| side | string | `BUY` / `SELL` |
| orderType | string | `GTC` / `GTD` / `FOK` / `IOC` / `POST_ONLY` |
| sizeType | string | `BASE` / `QUOTE` |
| size | string | Original order size |
| price | string | Order price |
| expiration | string | GTD expiry timestamp (ms); null for non-GTD |
| txHash | string | Transaction hash for the current stage |
| status | string | Order status (see table below) |
| filledSize | string | Filled token amount |
| filledAmount | string | Filled xp amount |
| failReason | string | Failure reason (present only when status = `FAILED`) |
| cancelReason | string | Cancel reason (system-initiated cancellations) |
| oddsType | string | Odds type: `points` = points odds |
| createdAt | string | Order creation timestamp (ms) |
| updatedAt | string | Last update timestamp (ms) |

**Order Status**

| Status | Description |
| --- | --- |
| PENDING_PLACE | Submitted, waiting for on-chain confirmation |
| ACTIVE | Active open order |
| PENDING_CANCEL | Cancellation submitted, waiting for on-chain confirmation |
| FILLED | Fully filled |
| PARTIALLY_FILLED | Partially filled then cancelled or expired |
| FAILED | On-chain transaction failed |
| CANCELLED | Cancelled by user or system |
| EXPIRED | GTD order expired |

### 3.4 Get user orders

Query the current authenticated user's order list, with filtering and pagination.

#### HTTP Request

`GET /api/v5/predictions/orders`

#### Request Parameters (Query)

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| marketId | string | No | Filter by market ID |
| status | string | No | `open` (PENDING_PLACE / ACTIVE / PENDING_CANCEL), `closed` (FILLED / PARTIALLY_FILLED / CANCELLED / EXPIRED / FAILED); defaults to `open` |
| cursor | string | No | Pagination cursor |
| limit | int | No | Items per page (default 20, max 50) |

**Response** (`data`):

> Response example

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "list": [
      {
        "id": "1",
        "oid": "1",
        "clientOrderId": "1",
        "marketId": "string",
        "assetId": "1",
        "side": "BUY",
        "orderType": "GTC",
        "sizeType": "BASE",
        "size": "100",
        "price": "0.65",
        "expiration": null,
        "txHash": "0xdef...789",
        "status": "ACTIVE",
        "filledSize": "40",
        "filledAmount": "26",
        "failReason": null,
        "cancelReason": null,
        "oddsType": "points",
        "createdAt": "1710000000000",
        "updatedAt": "1710000000000"
      }
    ],
    "nextCursor": "eyJpZCI6MTIzfQ",
    "hasNext": false
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| list[].id | string | Order ID |
| list[].marketId | string | Market ID |
| list[].oid | string | Order oid |
| list[].clientOrderId | string | Order clientOrderId |
| list[].tokenId | string | YES/NO token address |
| list[].assetId | string | TradeZone asset ID |
| list[].side | string | `BUY` / `SELL` |
| list[].orderType | string | `GTC` / `GTD` / `FOK` / `IOC` / `POST_ONLY` |
| list[].sizeType | string | `BASE` / `QUOTE` |
| list[].size | string | Original order size |
| list[].price | string | Order price |
| list[].expiration | string | GTD expiry timestamp (ms); null for non-GTD |
| list[].txHash | string | Transaction hash for the current stage |
| list[].status | string | Order status: `PENDING_PLACE` / `ACTIVE` / `PENDING_CANCEL` / `FILLED` / `PARTIALLY_FILLED` / `FAILED` / `CANCELLED` / `EXPIRED` |
| list[].filledSize | string | Filled token amount |
| list[].filledAmount | string | Filled xp amount |
| list[].failReason | string | Failure reason (present only when status = `FAILED`) |
| list[].cancelReason | string | Cancel reason (system-initiated cancellations) |
| list[].oddsType | string | Odds type: `points` = points odds |
| list[].createdAt | String | Order creation timestamp (ms) |
| list[].updatedAt | string | Last update timestamp (ms) |
| nextCursor | string | Next-page cursor |
| hasNext | boolean | Whether more data is available |

### 3.5 Cancel all / market orders

Cancel the current authenticated user's active orders. When `assetIds` is an empty list, cancel orders across all markets; when specific values are passed, cancel only the orders in the markets of the corresponding assets.

#### HTTP Request

`POST /api/v5/predictions/orders/cancel-all`

#### Request Body

> Request example (cancel by market)

```json
{
  "action": {
    "type": "cancelAll",
    "assetIds": [1, 2, 5],
    "marketType": "prediction"
  },
  "nonce": 1708929600000,
  "expiresAfter": 1708929660000,
  "signature": {
    "Ecdsa": {
      "r": "0x...",
      "s": "0x...",
      "v": 1
    }
  }
}
```

> Request example (cancel all)

```json
{
  "action": {
    "type": "cancelAll",
    "assetIds": [],
    "marketType": "prediction"
  },
  "nonce": 1708929600000,
  "expiresAfter": 1708929660000,
  "signature": {
    "Ecdsa": {
      "r": "0x...",
      "s": "0x...",
      "v": 1
    }
  }
}
```

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| action | object | Yes | Cancel action |
| action.type | string | Yes | Fixed `"cancelAll"` |
| action.assetIds | array | Yes | Asset ID list; pass an empty list to cancel all orders, or specific values to cancel by the corresponding markets |
| action.marketType | string | Yes | Market type, fixed `"prediction"` |
| nonce | long | Yes | Request timestamp (ms) |
| expiresAfter | long | Yes | Expiry timestamp (ms) |
| signature | object | Yes | Signature object |
| signature.Ecdsa | object | Yes | ECDSA signature components |
| signature.Ecdsa.r | string | Yes | Signature r value (`"0x..."`) |
| signature.Ecdsa.s | string | Yes | Signature s value (`"0x..."`) |
| signature.Ecdsa.v | int | Yes | Signature v value (`0` or `1`) |

**Response** (`data`):

> Response example

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "txHash": "0xdef...abc"
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| txHash | string | TradeZone transaction hash |

### 3.6 Send heartbeat

A heartbeat mechanism that protects the developer's active orders. The request body carries a pre-signed `cancelAll` action with `nonce` set to the current time + 5 minutes. The server stores this signature; if the developer does not renew the heartbeat within 5 minutes, the system automatically uses this signature to cancel all orders.

The developer should keep calling this endpoint (recommended interval < 5 minutes), each time overwriting the previous pre-signature with a new `nonce` (current time + 5 minutes).

#### HTTP Request

`POST /api/v5/predictions/heartbeat`

#### Request Body

> Request example

```json
{
  "action": {
    "type": "cancelAll",
    "assetIds": [],
    "marketType": "prediction"
  },
  "nonce": 1708929900000,
  "expiresAfter": 1708929960000,
  "signature": {
    "Ecdsa": {
      "r": "0x...",
      "s": "0x...",
      "v": 1
    }
  }
}
```

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| action | object | Yes | Cancel action; same structure as `POST /api/v5/predictions/orders/cancel-all` |
| action.type | string | Yes | Fixed `"cancelAll"` |
| action.assetIds | array | Yes | Always pass an empty list `[]`; cancels all orders when the heartbeat fires |
| action.marketType | string | Yes | Market type, fixed `"prediction"` |
| nonce | long | Yes | Timestamp of current time + 5 minutes (ms) |
| expiresAfter | long | Yes | Expiry timestamp (ms) |
| signature | object | Yes | Signature object |
| signature.Ecdsa | object | Yes | ECDSA signature components |
| signature.Ecdsa.r | string | Yes | Signature r value (`"0x..."`) |
| signature.Ecdsa.s | string | Yes | Signature s value (`"0x..."`) |
| signature.Ecdsa.v | int | Yes | Signature v value (`0` or `1`) |

**Response** (`data`):

> Response example

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "serverTimestamp": 1708929600000,
    "expireAt": 1708929900000
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| serverTimestamp | long | Server current timestamp (ms) |
| expireAt | long | This heartbeat's expiry timestamp (ms) |

<aside class="notice">
<code>nonce</code> = 1708929900000, i.e. current time 1708929600000 + 300000 (5 minutes).
</aside>

**Order Semantics**

<aside class="notice">
The system prevents self-trading: buy and sell orders from the same wallet address will not match against each other.
</aside>

**Order Mode Support Matrix**

**Support matrix**

| Scenario | sizeType | tif | size | price semantics | Supported |
| --- | --- | --- | --- | --- | --- |
| Limit buy | `base` | `gtc` / `gtd` / `ioc` / `fok` / `alo` | user-entered shares | user-entered limit price | ✅ |
| Limit sell | `base` | `gtc` / `gtd` / `ioc` / `fok` / `alo` | user-entered shares | user-entered limit price | ✅ |
| Market buy (by size) | `base` | `ioc` / `fok` | user-entered shares | system-simulated **worst fill price** (protection price) from the book | ✅ |
| Market buy (by amount) | `quote` | `ioc` | user-entered notional amount (xp) | system-simulated **worst fill price** (protection price) from the book | ✅ |
| Market sell (by size) | `base` | `ioc` / `fok` | user-entered shares | system-simulated **worst fill price** (protection price) from the book | ✅ |
| Market buy (by amount) + FOK | `quote` | `fok` | — | — | ❌ **Not supported** |

**Key rules**

1. **In the prediction market, `ioc` is equivalent to the commonly used `fak`**: it fills as much as possible, cancels the remainder, and never leaves a resting order.

2. **`fok` is only supported when ordering by shares**: the combination of `sizeType=quote` and `tif=fok` is rejected by the server.

3. **`alo` (Post Only) applies only to limit orders**: combining it with `sizeType=quote` or a market order is rejected.

4. **A market order is not an "unprotected order"**: at order time the system simulates a `worstPrice` from the book and submits it to TradeZone as the `price`. The actual fill price will **never be worse than `worstPrice`**.

5. **`tif=gtd` must include `expiration`**: the field path is `action.orders[].orderType.limit.expiration`, of type `String` (Unix ms timestamp, 13 digits), an **absolute time**. If missing or invalid, it currently throws `10001 PARAM_ERROR`.

**Error returns for unsupported combinations**

| Trigger | Current error code |
| --- | --- |
| `sizeType=quote` and `tif=fok` | `10001` PARAM_ERROR |
| `sizeType=quote` and `tif ∈ {gtc, gtd, alo}` | `10001` PARAM_ERROR |
| `tif=gtd` without `expiration` | `10001` PARAM_ERROR |

**Minimum Order Amount & Liquidation Exception**

**Minimum order amount rule**

- The prediction market enforces a minimum notional amount per order: 1 xp.

**Sell-All Exception**

- After a user sells a position in one direction, if the remaining **total position** for that assetId (= available + frozen) is 0, it is treated as a **sell-all (liquidation)**.

- In the sell-all scenario, the order is allowed even if the notional fill amount is below 1 xp.

**Price Unit & Precision Rules**

**Valid range**

| Condition | Allowed |
| --- | --- |
| `price ≤ 0` | Rejected |
| `price ≥ 1` | Rejected |
| `0 < price < 1` | Allowed, proceeds to precision validation |

**Segmented precision rules**

Precision is split into two segments by `[0.04, 0.96]` and outside it:

| Range | Max decimal places | Equivalent cent range | Example (allowed) | Example (rejected) |
| --- | --- | --- | --- | --- |
| `0 < price < 0.04` | **3 places** | `(0, 4)` cents | `0.001` / `0.025` / `0.039` | `0.0125` (4 places) |
| `0.04 ≤ price ≤ 0.96` | **2 places** (tick=0.01) | `[4, 96]` cents | `0.04` / `0.25` / `0.96` | `0.045` / `0.123` / `0.961` |
| `0.96 < price < 1` | **3 places** | `(96, 100)` cents | `0.962` / `0.9875` / `0.999` | `0.99875` (4 places) |

<aside class="notice">
<b>Boundary values:</b> both <code>0.04</code> and <code>0.96</code> belong to the middle range (the closed interval <code>[0.04, 0.96]</code>) and are subject to the <b>2-decimal-place limit</b>.
</aside>

**ClientOrderId Generation Specification**

`clientOrderId` (cloid for short) is the order's unique identifier on chain. On-chain events are broadcast globally, and consumers in each region use the region/env prefix in the cloid to filter out the orders that belong to them. Any cloid generated by any integrator must follow this specification, otherwise orders will be misattributed.

**Format**

The format is `0x{region}{env}{random}`:

| Segment | Length (chars) | Content | Description |
| --- | --- | --- | --- |
| prefix | 2 | `0x` | Fixed literal |
| region | 1 | 1 hex char | Region code |
| env | 1 | 1 hex char | Environment code |
| random | 30 | 30 hex chars | Random number (lowercase) |

The total length is fixed at **34 characters**, all lowercase hex (`0-9`, `a-f`).

**region / env code table**

| region | Meaning |
| --- | --- |
| `0` | HK |
| `1` | US |
| `2` | EU |

| env | Meaning |
| --- | --- |
| `1` | Production |

Each value is a single hex char; currently 0/1/2 are used, and future expansion will not exceed `f` (15).

**random generation requirements**

- **Entropy**: ≥ 120 bits (30 hex = 120 bits)
- **Source**: must use a cryptographically secure random source or equivalent strength (e.g. UUIDv4, `crypto.randomBytes`, `secrets`, etc.)
- **Encoding**: lowercase hex, zero-padded on the high bits if short

**Reference implementations**

> Java (UUID-derived)

```java
UUID u = UUID.randomUUID();
long hi = u.getMostSignificantBits();
long lo = u.getLeastSignificantBits();
StringBuilder sb = new StringBuilder("0x");
sb.append(Integer.toHexString(region & 0xF));
sb.append(Integer.toHexString(env & 0xF));
for (int i = 14; i >= 0; i--) {
    sb.append(Character.forDigit((int)((hi >>> (i * 4)) & 0xF), 16));
}
for (int i = 14; i >= 0; i--) {
    sb.append(Character.forDigit((int)((lo >>> (i * 4)) & 0xF), 16));
}
return sb.toString();
```

> Node.js

```javascript
const crypto = require('crypto');
function generate(region, env) {
  const rand = crypto.randomBytes(15).toString('hex'); // 30 hex
  return `0x${region.toString(16)}${env.toString(16)}${rand}`;
}
```

```python
import secrets
def generate(region: int, env: int) -> str:
    return f"0x{region:x}{env:x}{secrets.token_hex(15)}"
```

> Go

```go
import (
    "crypto/rand"
    "encoding/hex"
    "fmt"
)

func Generate(region, env int) (string, error) {
    b := make([]byte, 15)
    if _, err := rand.Read(b); err != nil {
        return "", err
    }
    return fmt.Sprintf("0x%x%x%s", region, env, hex.EncodeToString(b)), nil
}
```

**Server-side parsing and attribution**

The receiver decides whether a cloid belongs to the current environment using the following rules:

```text
if cloid is null/empty
  or length < 4
  or does not start with "0x"
  or cloid[2], cloid[3] are not valid hex chars:
    treat as HK-staging (region=0, env=0)  <- fallback rule
else:
    region = parse_hex(cloid[2])
    env    = parse_hex(cloid[3])
    check whether it equals the current environment's (region, env)
```

**FAQ**

- Heartbeat: if you run a bot, set up a heartbeat to automatically cancel all orders when the bot disconnects. Send a pre-signed cancel-all every < 5 minutes; see the API docs for details.

- Nonce: use the current millisecond timestamp. Each nonce can be used only once.

- Timestamp drift: OK-ACCESS-TIMESTAMP must not differ from server time by more than 30 seconds, otherwise error code 50102 is returned.

## 4. Positions Operations API

### 4.1 Split (xp → YES + NO)

Split xp into an equal amount of YES and NO conditional token pairs. The developer constructs and signs the calldata; after validation, the server submits it directly to TradeZone.

#### HTTP Request

`POST /api/v5/predictions/positions/split`

#### Request Body

> Request example

```json
{
  "action": {
    "type": "predictionSplit",
    "marketId": "1",
    "size": "100000000"
  },
  "nonce": 1708929600000,
  "signature": {
    "Ecdsa": {
      "r": "0x...",
      "s": "0x...",
      "v": 1
    }
  }
}
```

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| action | object | Yes | Split action |
| action.type | string | Yes | Fixed `"predictionSplit"` |
| action.marketId | string | Yes | Market ID (e.g. `"1"`) |
| action.size | string | Yes | xp amount (minimal-unit string, e.g. `"100000000"`) |
| nonce | long | Yes | Request timestamp (ms), anti-replay |
| signature | object | Yes | Signature object |
| signature.Ecdsa | object | Yes | ECDSA signature components |
| signature.Ecdsa.r | string | Yes | Signature r value (`"0x..."`) |
| signature.Ecdsa.s | string | Yes | Signature s value (`"0x..."`) |
| signature.Ecdsa.v | int | Yes | Signature v value (`0` or `1`) |

**Response** (`data`):

> Response example

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "txHash": "0xdef...abc"
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| txHash | string | TradeZone transaction hash |

### 4.2 Merge (YES + NO → xp)

Merge equal amounts of YES and NO conditional tokens back into xp (the inverse of Split). The developer constructs and signs the calldata; after validation, the server submits it directly to TradeZone.

#### HTTP Request

`POST /api/v5/predictions/positions/merge`

#### Request Body

> Request example

```json
{
  "action": {
    "type": "predictionMerge",
    "marketId": "1",
    "size": "100000000"
  },
  "nonce": 1708929600000,
  "signature": {
    "Ecdsa": {
      "r": "0x...",
      "s": "0x...",
      "v": 1
    }
  }
}
```

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| action | object | Yes | Merge action |
| action.type | string | Yes | Fixed `"predictionMerge"` |
| action.marketId | string | Yes | Market ID (e.g. `"1"`) |
| action.size | string | Yes | Merge amount (minimal-unit string, e.g. `"100000000"`) |
| nonce | long | Yes | Request timestamp (ms), anti-replay |
| signature | object | Yes | Signature object |
| signature.Ecdsa | object | Yes | ECDSA signature components |
| signature.Ecdsa.r | string | Yes | Signature r value (`"0x..."`) |
| signature.Ecdsa.s | string | Yes | Signature s value (`"0x..."`) |
| signature.Ecdsa.v | int | Yes | Signature v value (`0` or `1`) |

**Response** (`data`):

> Response example

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "txHash": "0xdef...abc"
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| txHash | string | TradeZone transaction hash |

### 4.3 Redeem (redeem xp after settlement)

After a market settles, redeem xp 1:1 with the winning conditional tokens. The redeem amount is not passed in; by default it redeems all of the user's winning tokens.

<aside class="notice">
After a market settles, the system <b>automatically</b> redeems winning tokens to xp for all users (this takes some time). <b>In most cases you do not need to call this endpoint</b>; manual redeem is only needed as a fallback when auto-redeem fails, or when you do not want to wait for the automatic process to complete.
</aside>

#### HTTP Request

`POST /api/v5/predictions/positions/redeem`

#### Request Body

> Request example

```json
{
  "action": {
    "type": "predictionRedeem",
    "marketId": "1"
  },
  "nonce": 1708929600000,
  "signature": {
    "Ecdsa": {
      "r": "0x...",
      "s": "0x...",
      "v": 1
    }
  }
}
```

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| action | object | Yes | Redeem action |
| action.type | string | Yes | Fixed `"predictionRedeem"` |
| action.marketId | string | Yes | Market ID (e.g. `"1"`) |
| nonce | long | Yes | Request timestamp (ms), anti-replay |
| signature | object | Yes | Signature object |
| signature.Ecdsa | object | Yes | ECDSA signature components |
| signature.Ecdsa.r | string | Yes | Signature r value (`"0x..."`) |
| signature.Ecdsa.s | string | Yes | Signature s value (`"0x..."`) |
| signature.Ecdsa.v | int | Yes | Signature v value (`0` or `1`) |

<aside class="warning">
Note: unlike Split/Merge, the Redeem action has <b>no <code>size</code> field</b>; it redeems all winning tokens by default.
</aside>

**Response** (`data`):

> Response example

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "txHash": "0xdef...abc"
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| txHash | string | TradeZone transaction hash |

## 5. Trade History API

### 5.1 Query trades

Query the current authenticated user's trade (fill) records. The server resolves the userId from the login session and internally converts it to an address for the query.

#### HTTP Request

`GET /api/v5/predictions/trades`

#### Request Parameters (Query)

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| marketId | string | No | Filter by market ID |
| side | string | No | Filter by side: `BUY` / `SELL` |
| startTime | long | No | Start timestamp (ms), inclusive |
| endTime | long | No | End timestamp (ms), exclusive |
| cursor | string | No | Pagination cursor |
| limit | int | No | Items per page (default 20, max 100) |

**Response** (`data`):

> Response example

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "list": [
      {
        "tradeId": "1",
        "orderId": "1",
        "marketId": "1",
        "tokenId": "0xabc...001",
        "side": "BUY",
        "size": "50",
        "amount": "32.5",
        "price": "0.65",
        "fee": "0.065",
        "role": "TAKER",
        "txHash": "0xdef...789",
        "createdAt": 1710000030000
      }
    ],
    "nextCursor": "eyJpZCI6MTAwMDF9",
    "hasNext": false
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| list[].tradeId | string | Trade record ID |
| list[].orderId | string | Associated order ID |
| list[].marketId | string | Market ID |
| list[].tokenId | string | Token address (YES or NO) |
| list[].side | string | `BUY` / `SELL` |
| list[].size | string | Filled token amount |
| list[].amount | string | Filled xp amount |
| list[].price | string | Fill price |
| list[].fee | string | Fee (xp) |
| list[].role | string | `MAKER` / `TAKER` |
| list[].txHash | string | On-chain transaction hash |
| list[].createdAt | string | Trade timestamp (ms) |
| nextCursor | string | Next-page cursor |
| hasNext | boolean | Whether more data is available |

## 6. Positions Query API

### 6.1 Query current positions

Query the current authenticated user's active positions.

#### HTTP Request

`GET /api/v5/predictions/positions?status=open`

### 6.2 Query closed positions

Query the current authenticated user's exited or settled positions.

#### HTTP Request

`GET /api/v5/predictions/positions?status=closed`

### 6.3 Query positions in a specified market

Query the current authenticated user's positions in a specified market.

#### HTTP Request

`GET /api/v5/predictions/positions?marketId={marketId}`

### 6.4 Unified endpoint

All three scenarios above are implemented through the same endpoint + query parameter combinations:

#### HTTP Request

`GET /api/v5/predictions/positions`

#### Request Parameters (Query)

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| status | string | No | `open` (active positions), `closed` (liquidated); returns all if omitted |
| marketId | long | No | Filter by market ID |
| cursor | string | No | Pagination cursor |
| limit | int | No | Items per page (default 20, max 100) |

**Response** (`data`):

> Response example

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "list": [
      {
        "id": "100001",
        "tokenId": "0xabc...001",
        "marketId": "1",
        "tokenIndex": "1",
        "tokenName": "Yes",
        "size": "500",
        "availableSize": "480",
        "value": "325",
        "avgPrice": "0.62",
        "unRealizedPnl": "15",
        "unRealizedPnlPercentage": "0.048",
        "title": "Will BTC exceed $100k by end of 2026?",
        "icon": "https://cdn.example.com/markets/1.png",
        "eventId": "100",
        "winningToken": null,
        "positionStatus": 1,
        "oddsType": "points",
        "curPrice": "0.65",
        "realizedPnl": "12.5",
        "realizedPnlPercentage": "0.04"
      }
    ],
    "nextCursor": "eyJpZCI6MTAwMDAxfQ",
    "hasNext": false
  }
}
```

| Field | Type | Description |
| --- | --- | --- |
| list[].id | string | Position ID |
| list[].tokenId | string | Token ID |
| list[].marketId | string | Market ID |
| list[].tokenIndex | string | Token direction (`"1"` = YES, `"2"` = NO) |
| list[].tokenName | string | Token name (`"Yes"` / `"No"`) |
| list[].size | string | Position size (= remain) |
| list[].availableSize | string | Available position size (= remain − frozen, the amount not locked by resting SELL orders) |
| list[].value | string | Current value (curPrice × size) |
| list[].avgPrice | string | Weighted average entry cost |
| list[].unRealizedPnl | string | Unrealized PnL |
| list[].unRealizedPnlPercentage | string | Unrealized PnL percentage |
| list[].title | string | Market question text |
| list[].icon | string | Market icon URL |
| list[].eventId | string | Parent event ID |
| list[].winningToken | string | Winning token ID after settlement; null before settlement |
| list[].positionStatus | integer | Position status code (see PositionStatusEnum) |
| list[].oddsType | string | Odds type: `points` = points odds |
| list[].curPrice | string | Current real-time token price |
| list[].realizedPnl | string | Realized PnL |
| list[].realizedPnlPercentage | string | Realized PnL percentage |
| nextCursor | string | Next-page cursor |
| hasNext | boolean | Whether more data is available |

## 7. Account Balance API

### 7.1 Query account balance

Query the current authenticated user's xp balance.

#### HTTP Request

`GET /api/v5/predictions/balance`

**Request parameters**: none

**Response** (`data`):

> Response example

```json
{
  "code": 0,
  "message": "OK",
  "data": [
    { "oddsType": "points", "balance": "2.2",  "available": "2.2"  }
  ]
}
```

`data` is an array; each element corresponds to the balance of one odds type:

| Field | Type | Description |
| --- | --- | --- |
| oddsType | string | Odds type: `points` = points odds |
| balance | string | Total balance |
| available | string | Available balance (total balance − frozen amount) |

## 8. Rate Limits

**Events & Markets API**

| Endpoint | Limit |
| --- | --- |
| `GET /api/v5/predictions/events` | 20 times / 1s |
| `GET /api/v5/predictions/events/{eventId}` | 20 times / 1s |
| `GET /api/v5/predictions/events/{eventId}/markets` | 20 times / 1s |
| `GET /api/v5/predictions/markets/{marketId}` | 20 times / 1s |
| `GET /api/v5/predictions/events/search` | 10 times / 1s |

**Price API (market data)**

| Endpoint | Limit |
| --- | --- |
| `GET /api/v5/market/ticker` | 10 times / 1s |
| `GET /api/v5/market/candles` | 20 times / 1s |
| `GET /api/v5/market/pm-books` | 20 times / 1s |

**Orders API**

**Query**

| Endpoint | Limit |
| --- | --- |
| `GET /api/v5/predictions/orders/{orderId}` | 20 times / 1s |
| `GET /api/v5/predictions/orders` | 20 times / 1s |

**Write**

| Endpoint | Limit |
| --- | --- |
| `POST /api/v5/predictions/orders` | 50 times / 1s |
| `POST /api/v5/predictions/orders/cancel` | 20 times / 1s |
| `POST /api/v5/predictions/orders/cancel-all` | 1 time / 1h |
| `POST /api/v5/predictions/heartbeat` | 1 time / 1s |

**Positions Operations API**

| Endpoint | Limit |
| --- | --- |
| `POST /api/v5/predictions/positions/split` | 5 times / 1s |
| `POST /api/v5/predictions/positions/merge` | 5 times / 1s |
| `POST /api/v5/predictions/positions/redeem` | 5 times / 1s |

**Trade History / Positions Query / Balance**

| Endpoint | Limit |
| --- | --- |
| `GET /api/v5/predictions/trades` | 10 times / 1s |
| `GET /api/v5/predictions/positions` | 10 times / 1s |
| `GET /api/v5/predictions/balance` | 10 times / 1s |

**Rate-limit response**

- HTTP Status: `429 Too Many Requests`
- Business error code: `50011 RATE_LIMIT_EXCEEDED`
- Clients are advised to retry with **exponential backoff**: 1s first, doubling each time, up to a max of 30s; if 5 consecutive retries still return 429, stop and alert.
