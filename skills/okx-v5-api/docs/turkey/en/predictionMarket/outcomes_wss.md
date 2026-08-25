---
site: turkey
lang: en
name: WebSocket
type: guide
---

# WebSocket

## 1. WebSocket Login Authentication

WebSocket login authentication is required only before subscribing to private channels. Public channels do not require login.

See [WebSocket login documentation](https://www.okx.com/docs-v5/en/#overview-websocket-login).

> Generic subscribe format

```json
{
  "op": "subscribe",
  "args": [
     { "channel": "<channel1>", "instId": "<yesAssetId1>" },
     { "channel": "<channel2>", "instId": "<yesAssetId2>" }
  ]
}
```

> Generic unsubscribe format

```json
{
  "op": "unsubscribe",
  "args": [
      { "channel": "<channel1>", "instId": "<yesAssetId1>" },
      { "channel": "<channel2>", "instId": "<yesAssetId2>" }
   ]
}
```

## 2. WebSocket Private Channels

| Channel | Subscribe params | Auth required | Description |
| --- | --- | --- | --- |
| pm-order | channelName | Yes | User order data push |
| pm-position | channelName | Yes | Position changes |
| pm-user-trade | channelName | Yes | User trade history push |
| pm-balance | channelName | Yes | Balance changes |
| pm-pnl | channelName | Yes | Unrealized PnL of current positions |

### 2.1 Order Status Push

**Push frequency:** Event-driven, pushed when an order status changes

> Subscribe example

```json
{
  "op": "subscribe",
  "args": [{ "channel": "pm-order" }]
}
```

> Push data format

```json
{
  "arg": { "channel": "pm-order", "uid": "{cexUserId}" },
  "data": [{
    "orderId": "307173036051017730",
    "clientOrderId": "cli-abc-123",
    "marketId": "100001",
    "status": "FILLED",
    "assetId": "71",
    "side": "BUY",
    "direction": "YES",
    "filledSize": "10",
    "orderSize": "10",
    "avgPrice": "0.57",
    "amount": "5.7",
    "limitPrice": "0.45",
    "failMessage": null,
    "oddsType": "points",
    "txHash": "0xdef...",
    "tradeId": "9876543210"
  }]
}
```

#### Push Field Descriptions

| Field | Type | Description |
| --- | --- | --- |
| `orderId` | string | Order ID |
| `clientOrderId` | string / null | Client order ID (cloid); null if not provided by the client |
| `marketId` | string | Market ID |
| `status` | string | Push event type (see the enum table below) |
| `assetId` | string | Outcome asset ID (yesAssetId or noAssetId) |
| `side` | string | Trade direction: `BUY` / `SELL` |
| `direction` | string | Position direction: `YES` / `NO` |
| `filledSize` | string / null | Cumulative filled size; null if no fill |
| `orderSize` | string | Order size |
| `avgPrice` | string / null | Cumulative average fill price (= amount / filledSize); null if no fill |
| `amount` | string / null | Cumulative filled amount (xp), BUY = spent / SELL = received; null if no fill |
| `limitPrice` | string / null | Limit order price (limit scenario only); null for market orders |
| `failMessage` | string / null | Failure message; only populated for `PLACE_FAILED` / `CANCEL_FAILED` |
| `oddsType` | string | Odds type: `points` |
| `txHash` | string / null | On-chain transaction hash; null for events not yet on-chain (e.g. `PLACE_FAILED`) |
| `tradeId` | string / null | TradeZone trade ID; only populated for limit order partial fills (status=`ACTIVE`) |

#### status Enum

| code | Description | Required fields |
| --- | --- | --- |
| `ACTIVE` | Active (limit order on-chain / partially filled with remainder valid) | `orderSize`, `limitPrice` (partial fills also include `filledSize`, `avgPrice`, `amount`, `tradeId`) |
| `FILLED` | Fully filled | `filledSize`, `avgPrice`, `amount`, `txHash` |
| `PARTIALLY_FILLED` | Cancelled after partial fill | `filledSize`, `orderSize`, `avgPrice`, `amount`, `txHash` |
| `PLACE_FAILED` | Order placement failed | `orderSize`, `failMessage` |
| `CANCEL_FAILED` | Cancellation failed (unexpected) | `orderId`, `failMessage` |
| `CANCELLED` | Cancelled by user / batch-cancelled by system | `orderSize`, `limitPrice` |
| `EXPIRED` | Order expired | `orderSize`, `limitPrice` |

### 2.2 Position Change Push

**Push frequency:** Event-driven

> Subscribe example

```json
{
  "op": "subscribe",
  "args": [{ "channel": "pm-position" }]
}
```

### 2.3 Position

(status = FILL / REDEEM / FILL_FAILED / REDEEM_FAILED)

> Push data format

```json
{
  "arg": { "channel": "pm-position", "uid": "{cexUserId}" },
  "data": [{
    "id": "307173036051017730",
    "marketId": "100001",
    "tokenId": "20000",
    "assetId": "71",
    "amount": "10",
    "timestamp": "1712736000000",
    "unRealizedPnl": "5.20",
    "unRealizedPnlPercentage": "1.05",
    "value": "100.00",
    "avgPrice": "0.57",
    "status": "FILL",
    "tradeId": "9876543210",
    "oddsType": "points"
  }]
}
```

#### Field Descriptions

| Field | Type | Description |
| --- | --- | --- |
| `id` | String | Position ID |
| `marketId` | String | Market ID |
| `tokenId` | String | YES/NO token on-chain ID |
| `assetId` | String | Outcome asset ID (yesAssetId or noAssetId) |
| `amount` | String | Current position size (`remain` snapshot); `"0"` for REDEEM |
| `timestamp` | String | Event timestamp (ms) |
| `unRealizedPnl` | String | Unrealized PnL = (currentPrice − avgCost) × remain; `"0"` for REDEEM |
| `unRealizedPnlPercentage` | String | Unrealized PnL percentage (8-digit precision); `"0"` for REDEEM |
| `value` | String | Position market value = currentPrice × remain; `"0"` for REDEEM |
| `avgPrice` | String | Weighted average cost basis; `"0"` for REDEEM |
| `status` | String | `FILL` / `REDEEM` |
| `tradeId` | String / null | TradeZone trade ID; only populated for `FILL` (null for `REDEEM`) |
| `oddsType` | String | Odds type: `points` |

### 2.4 status Scenarios

| code | Description | Pushes per event |
| --- | --- | --- |
| `FILL` | Position change caused by an order fill | 1 (single tokenId position) |
| `REDEEM` | Settlement redemption; the market's position is cleared | N (one per tokenId the market originally had, with `amount=0`) |

### 2.5 Position

(status = SPLIT / MERGE / DEPOSIT / WITHDRAW / SPLIT_FAILED / MERGE_FAILED / DEPOSIT_FAILED / WITHDRAW_FAILED)

> Push data format

```json
{
  "arg": { "channel": "pm-position", "uid": "{cexUserId}" },
  "data": [{
    "marketId": "100001",
    "status": "DEPOSIT",
    "amount": "100",
    "txHash": "0xdef...",
    "oddsType": "points",
    "ext": {
      "toTxHash": "0xabc..."
    }
  }]
}
```

#### Field Descriptions

| Field | Type | Description |
| --- | --- | --- |
| `marketId` | String | Market ID |
| `status` | String | `SPLIT` / `MERGE` / `DEPOSIT` / `WITHDRAW` |
| `amount` | String | Amount |
| `txHash` | String | On-chain transaction hash. For DEPOSIT, the original XLayer transaction hash; otherwise the on-chain hash of the corresponding operation |
| `oddsType` | String | Odds type: `points` |
| `ext` | Object / null | Extended info; only populated for `DEPOSIT`, null otherwise |
| `ext.toTxHash` | String / null | The TZ-side crediting transaction hash; only populated for `DEPOSIT` |

#### status Enum

| code | Description |
| --- | --- |
| `FILL` | Position change caused by an order fill |
| `SPLIT` | Split succeeded |
| `MERGE` | Merge succeeded |
| `REDEEM` | Settlement redemption succeeded |
| `DEPOSIT` | Deposit succeeded |
| `WITHDRAW` | Withdrawal succeeded |
| `FILL_FAILED` | Fill failed |
| `SPLIT_FAILED` | Split failed |
| `MERGE_FAILED` | Merge failed |
| `REDEEM_FAILED` | Redemption failed |
| `DEPOSIT_FAILED` | Deposit failed |
| `WITHDRAW_FAILED` | Withdrawal failed |

### 2.6 Trade Fill Push

Each FILL event pushes one trade record; the front end uses it to display the trade list in real time.

**Push frequency:** Event-driven, one per fill

> Subscribe example

```json
{
  "op": "subscribe",
  "args": [{ "channel": "pm-user-trade" }]
}
```

> Push data format

```json
{
  "arg": { "channel": "pm-user-trade", "uid": "{cexUserId}" },
  "data": [{
    "orderId": "307173036051017730",
    "clientOrderId": "cli-abc-123",
    "marketId": "100001",
    "tokenId": "20000",
    "assetId": "71",
    "side": "BUY",
    "size": "10",
    "price": "0.57",
    "txhash": "0xdef...",
    "timestamp": "1712736000000",
    "tradeId": "9876543210"
  }]
}
```

#### Push Field Descriptions

| Field | Type | Description |
| --- | --- | --- |
| `orderId` | string | TradeZone order ID |
| `clientOrderId` | string / null | Client order ID; null if not provided by the client |
| `marketId` | string | Market ID |
| `tokenId` | string | YES/NO token on-chain ID |
| `assetId` | string | Outcome asset ID (yesAssetId or noAssetId) |
| `side` | string | Fill direction: `BUY` / `SELL` |
| `size` | string | Fill size |
| `price` | string | Fill price |
| `txhash` | string | On-chain transaction hash |
| `timestamp` | string | Event timestamp (ms) |
| `tradeId` | string | TradeZone trade ID |

### 2.7 Balance Change Push

Pushed after a balance sync succeeds; the front end uses it to update the balance display in real time.

**Push frequency:** Event-driven, pushed on balance changes (place / cancel / fill / Split / Merge / redeem / deposit / withdraw)

> Subscribe example

```json
{
  "op": "subscribe",
  "args": [{ "channel": "pm-balance" }]
}
```

> Push data format

```json
{
  "arg": { "channel": "pm-balance", "uid": "{cexUserId}" },
  "data": [{
    "walletAddress": "0x1234abcd5678ef901234abcd5678ef901234abcd",
    "available": "950.5",
    "total": "1000",
    "frozen": "49.5",
    "tokenId": "0",
    "changeType": "FILL",
    "changeAmount": "100",
    "updateTime": "1712736000000",
    "oddsType": "points"
  }]
}
```

#### Push Field Descriptions

| Field | Type | Description |
| --- | --- | --- |
| `walletAddress` | String | User's AA wallet address |
| `available` | String | Available balance |
| `total` | String | Total balance (including frozen) |
| `frozen` | String | Frozen amount (= total − available) |
| `tokenId` | String | xp token on-chain ID (corresponds to `oddsType`) |
| `changeType` | String | Trigger reason (see the enum table below) |
| `changeAmount` | String / null | Amount changed this time; null for non-applicable scenarios |
| `updateTime` | String | Event timestamp (ms) |
| `oddsType` | String | Odds type: `points` |

### 2.8 changeType Enum

| code | Trigger scenario |
| --- | --- |
| `PLACE` | Funds frozen on order placement |
| `CANCEL` | Funds unfrozen on cancellation |
| `FILL` | Balance change caused by a fill |
| `SPLIT` | Split deducts xp |
| `MERGE` | Merge adds xp |
| `REDEEM` | Settlement redemption |
| `DEPOSIT` | Deposit |
| `WITHDRAW` | Withdrawal |

### 2.9 PnL Change Push

Pushes both the PnL curve and the overview.

> Push data format

```json
{
  "arg": { "channel": "pm-pnl", "uid": "{cexUserId}" },
  "data": [{
    "portfolioValue": "1234.56",
    "periods": [
      { "period": "1D", "periodPnl": "12.34",  "pnlPercent": "1.01" },
      { "period": "1W", "periodPnl": "56.78",  "pnlPercent": "4.83" },
      { "period": "1M", "periodPnl": "120.45", "pnlPercent": "10.81" },
      { "period": "6M", "periodPnl": "320.10", "pnlPercent": "35.02" },
      { "period": "1Y", "periodPnl": "500.00", "pnlPercent": "67.93" }
    ]
  }]
}
```

#### Field Descriptions

| Field | Type | Description |
| --- | --- | --- |
| `portfolioValue` | String | Current total portfolio value (xp available balance + position market value) |
| `periods` | Array | Multi-period PnL summary array |
| `periods[].period` | String | Period: `1D` / `1W` / `1M` / `6M` / `1Y` |
| `periods[].periodPnl` | String | Absolute PnL within the period |
| `periods[].pnlPercent` | String | PnL percentage within the period |

> Push data format

```json
{
  "arg": { "channel": "pm-pnl", "uid": "{cexUserId}" },
  "data": [{
    "period": "0",
    "interval": "600000",
    "points": [
      { "time": "1712707200000", "pnl": "1000.00" },
      { "time": "1712728800000", "pnl": "1010.50" },
      { "time": "1712750400000", "pnl": "1023.75" }
    ],
    "currentPnl": "1023.75",
    "high": "1030.00",
    "low": "995.00"
  }]
}
```

#### Field Descriptions

| Field | Type | Description |
| --- | --- | --- |
| `period` | String | Period code: `0`=1D / `1`=1W / `2`=1M / `3`=6M / `4`=1Y |
| `interval` | String | Data point interval (ms), corresponding to the period (see the table below) |
| `points` | Array | Time-series data points |
| `points[].time` | String | Data point timestamp (ms) |
| `points[].pnl` | String | Total assets at that moment (= xp balance + position market value) |
| `currentPnl` | String | Current total assets (compatibility field) |
| `high` | String | Highest total assets within the period |
| `low` | String | Lowest total assets within the period |

### 2.10 Period Code and interval Mapping

| period code | Period | interval (ms) | Meaning |
| --- | --- | --- | --- |
| `0` | 1D | `600000` | 10 minutes |
| `1` | 1W | `1800000` | 30 minutes |
| `2` | 1M | `3600000` | 1 hour |
| `3` | 6M | `86400000` | 1 day |
| `4` | 1Y | `86400000` | 1 day |

## 3. WebSocket Public Channels

### 3.1 Order Book Push

### 3.2 Push Frequency

- Minimum push interval 100ms — the shortest interval between two incremental pushes

- Maximum push interval 60,000ms (60s) — the longest interval when the order book does not change

> Subscribe example

```json
{
  "op": "subscribe",
  "args": [{
    "channel": "pm-books",
    "instId": "{yesAssetId}"
  }]
}
```

> Subscribe success response

```json
{
  "event": "subscribe",
  "arg": { "channel": "pm-books", "instId": "{yesAssetId}" }
}
```

> Push data format

```json
{
      "arg": {"channel": "pm-books", "instId": "{yesAssetId}"},
      "action": "snapshot",
      "data": [
          {
              "asks": [
                  ["67300.1", "201.18", "25"],
                  ["67300.2", "421.45", "5"]
              ],
              "bids": [
                  ["67300", "525.41", "34"],
                  ["67299.9", "0.17", "7"]
              ],
              "ts": "1774944028506",
              "checksum": -702280706,
              "seqId": 308306650401,
              "prevSeqId": -1
          }
      ]
  }
```

### 3.3 Trade Push

Pushes each trade record in real time.

**Push frequency:** push interval 200ms, up to 20 trades per push, encoded as aggregated trades

> Subscribe example

```json
{
  "op": "subscribe",
  "args": [{
    "channel": "pm-trades",
    "instId": "{yesAssetId}"
  }]
}
```

> Push data format

```json
{
  "arg": { "channel": "pm-trades", "instId": "{yesAssetId}" },
  "data": [{
    "instId": "{yesAssetId}",
    "tradeId": "123456789",
    "px": "0.65",
    "sz": "100",
    "side": "buy",
    "ts": "1711900800123"
  }]
}
```

#### Push Field Descriptions

| Field | Type | Description |
| --- | --- | --- |
| `instId` | String | yesAssetId |
| `fId` | String | First trade ID within the aggregation window |
| `lId` | String | Last trade ID within the aggregation window |
| `px` | String | Trade price |
| `sz` | String | Trade size |
| `side` | String | Trade direction: `buy` / `sell` |
| `ts` | String | Trade time (Unix ms timestamp) |

### 3.4 Candlestick Push

Pushes candlestick data in real time; the current candle keeps updating until it closes. Clients can draw a price trend chart based on the candle close price; the subscription granularity is determined by the client.

**Channel naming rule:** `pm-candle` + period, e.g. `pm-candle1m`, `pm-candle15m`

> Subscribe example (1-minute and 5-minute as examples)

```json
{
  "op": "subscribe",
  "args": [
    { "channel": "pm-candle1m", "instId": "{yesAssetId}" },
    { "channel": "pm-candle15m", "instId": "{yesAssetId}" }
  ]
}
```

> Push data format

```json
{
  "arg": { "channel": "pm-candle15m", "instId": "{yesAssetId}" },
  "data": [
    ["1711900800000", "0.65", "0.67", "0.63", "0.66", "2000", "1300", "1300", "0"]
  ]
}
```

### 3.5 Push Field Descriptions

(each record in the data array is in index order)

| Index | Field | Description |
| --- | --- | --- |
| 0 | `ts` | Candle start time (Unix ms timestamp) |
| 1 | `o` | Open price |
| 2 | `h` | High price |
| 3 | `l` | Low price |
| 4 | `c` | Close price |
| 5 | `vol` | Volume (in contracts) |
| 6 | `volCcy` | Volume (in the base currency) |
| 7 | `volCcyQuote` | Volume (in the quote currency) |
| 8 | `confirm` | Candle status: `0` unconfirmed (current candle), `1` confirmed (closed) |

### 3.6 Ticker Push

Pushes the latest trade price, best bid, best ask, 24h volume, and other info in real time.

**Push frequency:** up to once every 100ms, triggered by trades or best-bid/best-ask changes; nothing is pushed without a trigger event

> Subscribe example

```json
{
  "op": "subscribe",
  "args": [{ "channel": "pm-tickers", "instId": "{yesAssetId}" }]
}
```

> Subscribe success response

```json
{
  "event": "subscribe",
  "arg": { "channel": "pm-tickers", "instId": "{yesAssetId}" },
  "connId": "accb8e21"
}
```

> Push data format

```json
{
  "arg": { "channel": "pm-tickers", "instId": "{yesAssetId}" },
  "data": [{
    "instType": "PREDICTIONS",
    "instId": "{yesAssetId}",
    "last": "0.4999",
    "lastSz": "1",
    "askPx": "0.6",
    "askSz": "30",
    "bidPx": "0.4",
    "bidSz": "30",
    "open24h": "0.5001",
    "high24h": "0.5001",
    "low24h": "0.4999",
    "vol24h": "372",
    "volCcy24h": "186",
    "sodUtc0": "0.4999",
    "sodUtc8": "0.4999",
    "ts": "1774948808119"
  }]
}
```

#### Push Field Descriptions

| Field | Type | Description |
| --- | --- | --- |
| instType | String | Instrument type |
| instId | String | yesAssetId |
| last | String | Latest trade price |
| lastSz | String | Latest trade size |
| askPx | String | Best ask price |
| askSz | String | Size at the best ask |
| bidPx | String | Best bid price |
| bidSz | String | Size at the best bid |
| open24h | String | 24h open price |
| high24h | String | 24h high price |
| low24h | String | 24h low price |
| vol24h | String | 24h volume (in contracts) |
| volCcy24h | String | 24h volume (in the base currency) |
| sodUtc0 | String | Open price at UTC 0 |
| sodUtc8 | String | Open price at UTC+8 |
| ts | String | Data push time (Unix ms timestamp) |

### 3.7 Probability Price Push

Pushes prediction market probability prices, including market probability, cumulative volume, best bid/ask, latest trade price, etc.

**Push frequency:** pushed every 3s on a timer

> Subscribe example

```json
{
  "op": "subscribe",
  "args": [{
    "channel": "prediction-market-prices",
    "instId": "{yesAssetId}"
  }]
}
```

> Push data format

```json
{
    "arg": {
        "channel": "prediction-market-prices",
        "instId": "{yesAssetId}"
    },
    "data": [
        {
            "yesAssetId": "71",
            "eventId": "1774875348987717503",
            "bestBid": "0.4896",
            "bestAsk": "0.6134",
            "lastTradePrice": "0.5848",
            "probability": "5515",
            "marketVolume": "17.1463",
            "eventVolume": "773.6524",
            "timestamp": "1775036647300"
        }
    ]
}
```

#### Push Field Descriptions

| Field | Type | Description |
| --- | --- | --- |
| yesAssetId | String | Prediction market yesAssetId |
| eventId | String | The event the market belongs to |
| bestBid | String | Best bid price |
| bestAsk | String | Best ask price |
| lastTradePrice | String | Latest trade price |
| probability | String | Market probability for the Yes side, pushed as a basis-point integer (e.g. 0.6500 → 6500) |
| marketVolume | String | Current cumulative market volume |
| eventVolume | String | Current cumulative event volume |
| timestamp | String | Event timestamp (ms) |

### 3.8 Event Status Push

Event status changes, used to display an event's final settlement result.

**Push frequency:** event-driven, pushed when an event reaches its final result. During the World Cup there are 2-3 matches per day; pushed when a NegRisk or single binary event reaches its final result.

> Subscribe example

> **Note:** When subscribing to this channel, `instId` is `event-{eventId}`.

```json
{
  "op": "subscribe",
  "args": [{
    "channel": "pm-event-status",
    "instId": "event-{eventId}"
  }]
}
```

> Push data format

```json
{
  "arg": {
    "channel": "pm-event-status",
    "instId": "event-{eventId}"
  },
  "data": [{
    "eventId": "{eventId}",
    "status": "resolved",
    "marketId":"marketId",
    "outcomeOption":"yes | no | others | team name | draw",
    "timestamp": "1672290687"
  }]
}
```

#### Push Field Descriptions

| Field | Type | Description |
| --- | --- | --- |
| eventId | string | Event ID |
| status | string | Event status |
| marketId | string | The marketId of the winning market |
| outcomeOption | string | Display of the final winning outcome |
| timestamp | string | Event timestamp (ms) |
