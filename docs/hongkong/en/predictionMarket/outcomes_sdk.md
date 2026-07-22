---
site: hongkong
lang: en
name: SDK API Reference
type: guide
---

# SDK API Reference

A complete reference for every public method, request body, response struct, error variant, and WebSocket channel exposed by the `okx-outcomes-sdk` Rust crate. The README is a quick start; this document is the detailed version.

Conventions used throughout:

- All Rust types are `pub` types re-exported from `okx_outcomes_sdk::*` (the specific module path is given at the top of each section).
- "Authentication" for each REST call refers to the OKX REST `OK-ACCESS-*` headers written after signing locally with HMAC-SHA256 via `ApiCredentials`. The SDK signs locally; the secret never leaves the process.
- "Write" operations (place / cancel order, split / merge / redeem / heartbeat) additionally require an EIP-712 ECDSA signature over the typed action. See the **Signing** section below.
- All decimal values (price, size, balance, PnL) are exchanged as decimal **strings** to avoid floating-point precision loss.
- All timestamps are Unix milliseconds.
- Wire field names use camelCase; Rust struct fields use snake_case, converted via `serde(rename_all = "camelCase")`.

## 1. Installation

Add the dependency via Git in your `Cargo.toml`:

> Cargo.toml dependencies

```toml
[dependencies]
okx-outcomes-sdk = { git = "https://github.com/okx/outcomes-sdk.git", features = ["signing", "websocket"] }
```

## 2. Client Construction

Module: `okx_outcomes_sdk::{OutcomesSdkClient, ApiCredentials}`.

```rust
pub struct ApiCredentials {
    pub api_key:    String, // value of the OK-ACCESS-KEY header
    pub secret_key: String, // HMAC-SHA256 signing secret; never transmitted
    pub passphrase: String, // value of the OK-ACCESS-PASSPHRASE header
}

impl OutcomesSdkClient {
    pub fn with_credentials(creds: ApiCredentials) -> Self;
    pub fn with_credentials_and_url(creds: ApiCredentials, base_url: impl Into<String>) -> Self;
}
```

Base URL resolution order: the explicit argument passed to `with_credentials_and_url` > the `PREDICTIONS_API_BASE` environment variable > `https://www.okx.com`. Endpoint constants are full absolute paths (`/api/v5/predictions/...`, `/api/v5/market/...`) concatenated with the base URL, so a single host configuration covers both prediction and market-data calls.

## 3. Errors

Module: `okx_outcomes_sdk::SdkError`.

Every fallible call returns `Result<T, SdkError>`. The enum has seven variants:

```rust
pub enum SdkError {
    /// Network failure: connection refused, DNS, timeout, TLS handshake failure.
    /// Transport layer; retrying is usually safe.
    Http(reqwest::Error),

    /// The server returned a non-zero business error code in the response envelope.
    /// `code` is the upstream OKX business code; decide retry / backoff / abort based on it.
    Api { code: i64, message: String },

    /// The response body could not be deserialized to the expected schema.
    /// Usually means the SDK and server versions are mismatched.
    Deserialize(serde_json::Error),

    /// WS connect, send, login, or close failed.
    /// Includes login rejection (`60xxx` error codes) and timeout during login.
    WebSocket { message: String },

    /// Reserved for callers that bypass the public constructors and end up without credentials.
    /// Clients constructed via `with_credentials` / `with_credentials_and_url` never
    /// return this variant — they always carry credentials.
    NotAuthenticated { hint: String },

    /// Invalid URL, missing state, or an unexpected internal precondition.
    /// Almost always a programming error by the caller.
    Internal { message: String },

    /// Serializing the request body or WS payload failed.
    /// Rare in practice (only triggered by, e.g., non-finite floats).
    Serialization { message: String },
}
```

Each variant implements `Display` via `thiserror`, so `format!("{e}")` produces a readable one-line log.

## 4. Events and Markets

Module: `okx_outcomes_sdk::models::event::*`. The API is implemented in `okx_outcomes_sdk::api::events`.

### 4.1 `get_events`

Get a paginated list of prediction market events.

- **Endpoint:** `GET /api/v5/predictions/events`
- **Auth:** Required

```rust
pub async fn get_events(
    &self,
    status:    Option<&str>,   // "active" (default) | "resolved"
    tag:       Option<&str>,   // sports tag ID
    league_id: Option<&str>,   // sports league ID
    sort:      Option<&str>,   // "volume" | "volume_24h" (default) | "ending_soon" | "newest"
    cursor:    Option<&str>,   // pagination cursor from the previous `EventsResponse.pagination.next_cursor`
    page_size: Option<i32>,    // items per page, max 50 (default 10)
) -> Result<EventsResponse, SdkError>;

pub struct EventsResponse {
    events:     Vec<EventObject>,
    pagination: Pagination,    // see Common Types
}

pub struct EventObject {
    id:                       String,         // globally unique event ID
    event_id:                 String,         // event ID
    neg_risk:                 bool,           // mutually exclusive (negRisk) event
    status:                   EventStatus,    // Active / Paused / Resolved / Unknown
    event_title:              String,         // display title
    description:              String,         // long description
    event_icon:               Option<String>, // icon URL
    volume:                   String,         // total volume across all markets
    start_time:               Option<i64>,    // trading start time (ms)
    end_time:                 Option<i64>,    // trading end time (ms)
    created_at:               i64,            // creation timestamp (ms)
    total_markets_count:      i32,            // number of markets under this event
    final_outcomes_market_id: Option<String>, // winning market ID after settlement
    markets:                  Vec<MarketObject>,      // the list endpoint returns at most 2 markets;
                                                      // call `get_event_markets` for the full list
}
```

### 4.2 `search`

Search events and markets by keyword.

- **Endpoint:** `GET /api/v5/predictions/events/search`
- **Auth:** Required

```rust
pub async fn search(
    &self,
    keyword:   &str,           // free-text query (required)
    cursor:    Option<&str>,   // pagination cursor
    page_size: Option<i32>,    // default 10
) -> Result<EventsResponse, SdkError>;
```

Response: `EventsResponse` (same shape as `get_events`).

### 4.3 `get_event`

Get a single event, with its full market list inlined.

- **Endpoint:** `GET /api/v5/predictions/events/{eventId}`
- **Auth:** Required

```rust
pub async fn get_event(&self, event_id: &str) -> Result<EventObject, SdkError>;
```

Returns `Api { code: 40404, ... }` if the event ID does not exist.

### 4.4 `get_event_markets`

Get all markets of an event (no pagination, no list cap).

- **Endpoint:** `GET /api/v5/predictions/events/{eventId}/markets`
- **Auth:** Required

```rust
pub async fn get_event_markets(&self, event_id: &str) -> Result<MarketsResponse, SdkError>;

pub struct MarketsResponse {
    markets: Vec<MarketObject>,
}

pub struct MarketObject {
    id:                        String,         // globally unique market ID
    market_id:                 String,         // market ID
    neg_risk:                  bool,           // negRisk market flag
    status:                    MarketStatus,   // Active / Paused / Settling / Resolved / Unknown
    settle_stage:              i32,            // 0 = not started, 5 = settled
    question:                  String,         // full market question
    short_question:            Option<String>, // short question
    description:               String,         // long description
    market_icon:               Option<String>, // icon URL
    start_time:                String,         // trading start time (ms, string)
    end_time:                  String,         // trading end time (ms, string)
    resolve_start_at:          String,         // settlement window start time (ms, string)
    resolve_at:                String,         // settlement time (ms, string)
    best_bid:                  Option<String>, // decimal in [0, 1]; None if no bids
    best_ask:                  Option<String>, // decimal in [0, 1]; None if no asks
    last_trade_price:          Option<String>, // None before the first trade
    volume:                    String,         // market volume
    probability:               Option<String>, // YES outcome probability in [0, 1]
    resolution_sources:        Vec<String>,    // source URLs used for resolution
    yes_outcome:               OutcomeObject,
    no_outcome:                OutcomeObject,
}

pub struct OutcomeObject {
    token_id:     Option<String>, // conditional token address; None before deployment
    asset_id:     Option<String>, // asset ID; used as `inst_id` in order placement / market data
    name:         String,         // "Yes" or "No"
    price:        String,         // decimal in [0, 1]
    final_result: Option<bool>,   // Some(true) = won, Some(false) = lost, None = not settled
}
```

### 4.5 `get_market`

Get a single market.

- **Endpoint:** `GET /api/v5/predictions/markets/{marketId}`
- **Auth:** Required

```rust
pub async fn get_market(&self, market_id: &str) -> Result<MarketObject, SdkError>;
```

## 5. Account: Balance

Module: `okx_outcomes_sdk::models::balance::*`. The API is in `okx_outcomes_sdk::api::balance`.

### 5.1 `get_balance`

Returns the authenticated user's available balance grouped by odds type.

- **Endpoint:** `GET /api/v5/predictions/balance`
- **Auth:** Required

```rust
pub async fn get_balance(&self) -> Result<BalanceResponse, SdkError>;

pub type BalanceResponse = Vec<BalanceEntry>;

pub struct BalanceEntry {
    odds_type: OddsType, // Spots / Points / Unknown (spots = real, points = points)
    balance:   String, // total balance (unit determined by odds_type)
    available: String, // available balance (total balance - amount frozen by open orders)
}
```

## 6. Account: Orders

Module: `okx_outcomes_sdk::models::order::*`. The API is in `okx_outcomes_sdk::api::orders`.

### 6.1 `place_order`

Submit a signed limit (or trigger) order.

- **Endpoint:** `POST /api/v5/predictions/orders`
- **Auth:** Required (REST credentials) + EIP-712 signature

```rust
pub async fn place_order(&self, req: &PlaceOrderRequest) -> Result<TxHashResponse, SdkError>;

struct PlaceOrderRequest {
    action:    PlaceOrderAction,
    nonce:     i64,                // millisecond timestamp, anti-replay
    signature: SignatureWrapper,   // { Ecdsa: { r, s, v } }
}

struct PlaceOrderAction {
    action_type: String,           // always "placeOrder"
    grouping:    String,           // always "na"
    orders:      Vec<OrderItem>,
}

struct OrderItem {
    asset_id:        String,           // outcome assetId
    side:            SigningOrderSide,  // Buy / Sell (wire side lowercase; bytes used for the EIP-712 hash)
    market_type:     String,           // always "prediction"
    client_order_id: Option<String>,   // 34-char client order ID; see Signing > Client Order ID
    price:           String,           // decimal in [0, 1]
    reduce_only:     bool,
    size:            String,           // decimal
    size_type:       SizeType,         // Base (default, omitted on wire) / Quote
    order_type:      OrderTypeSpec,    // { limit: { tif } }
}

struct OrderTypeSpec   { limit: LimitOrderType }
struct LimitOrderType  { tif: LimitTif /* Gtc | Gtd { expires_after } | Ioc | Fok | Alo */ }
```

Response: `TxHashResponse { tx_hash: String }`.

Build a typed `signing::types::OrderRequest`, sign it with `signing::sign_to_wrapper`, then derive the wire-side `OrderItem` via `OrderItem::from(&OrderRequest)` so the signed bytes and the JSON body cannot drift. See **Signing**.

### 6.2 `cancel_order`

Cancel an active order (by server ID or client order ID).

- **Endpoint:** `POST /api/v5/predictions/orders/cancel`
- **Auth:** Required + EIP-712 signature

```rust
pub async fn cancel_order(&self, req: &CancelOrderRequest) -> Result<TxHashResponse, SdkError>;

struct CancelOrderRequest {
    action:    CancelOrderAction,
    nonce:     i64,
    signature: SignatureWrapper,
}

struct CancelOrderAction {
    action_type: String,           // always "cancel"
    cancels:     Vec<CancelItem>,
}

struct CancelItem {
    asset_id:     String,
    market_type:  String,           // "prediction"
    // exactly one of the two:
    by: CancelBy,                   // flattened-serialized as { "oid": ... } or { "clientOrderId": ... }
}

enum CancelBy {
    Oid           { oid: String },          // server-assigned, decimal string
    ClientOrderId { client_order_id: String }, // 34-char client order ID, hex with 0x prefix
}
```

Response: `TxHashResponse { tx_hash: String }`.

### 6.3 `cancel_all`

Cancel all active orders, or all active orders under a given set of asset IDs.

- **Endpoint:** `POST /api/v5/predictions/orders/cancel-all`
- **Auth:** Required + EIP-712 signature

```rust
pub async fn cancel_all(&self, req: &CancelAllRequest) -> Result<TxHashResponse, SdkError>;

struct CancelAllRequest {
    action:        CancelAllAction,
    nonce:         i64,
    expires_after: i64,            // expiry timestamp (ms), required
    signature:     SignatureWrapper,
}

struct CancelAllAction {
    action_type: String,            // always "cancelAll"
    asset_ids:   Vec<String>,       // empty = all markets; non-empty = filter
    market_type: String,            // "prediction"
}
```

Response: `TxHashResponse`.

### 6.4 `heartbeat`

Refresh the dead-man's switch that protects active orders (auto-cancels on disconnect).

- **Endpoint:** `POST /api/v5/predictions/heartbeat`
- **Auth:** Required + EIP-712 signature

```rust
pub async fn heartbeat(&self, req: &CancelAllRequest) -> Result<HeartbeatResponse, SdkError>;

struct HeartbeatResponse {
    server_timestamp: i64, // current server time (ms)
    expire_at:        i64, // time this heartbeat expires (ms)
}
```

The request body uses the same `CancelAllRequest` shape: the signed payload **is** a pre-authorized cancel-all — the server executes it on your behalf after the heartbeat times out. Set `nonce` to `now_ms` and `expires_after` to `now_ms + 300_000` (5 minutes). Heartbeats should be sent more frequently than once every 5 minutes.

### 6.5 `get_order`

Query a single order by its server-assigned ID.

- **Endpoint:** `GET /api/v5/predictions/orders/{orderId}`
- **Auth:** Required

```rust
pub async fn get_order(&self, order_id: &str) -> Result<OrderRecord, SdkError>;

pub struct OrderRecord {
    id:              String,          // server-assigned order ID
    oid:             String,          // order oid (distinct from `id`)
    market_id:       String,
    token_id:        String,          // YES/NO token contract address
    asset_id:        String,          // YES or NO outcome asset ID
    client_order_id: Option<String>,  // client order ID provided at placement (if any)
    side:            OrderSide,        // Buy / Sell / Unknown
    order_type:      TimeInForce,      // Gtc / Gtd / Ioc / Fok / PostOnly / Unknown
    size_type:       OrderSizeType,    // Base / Quote / Unknown
    size:            String,           // decimal
    price:           String,           // decimal
    expiration:      Option<String>,   // GTD expiry (ms, string); None for non-GTD
    tx_hash:         String,           // submission transaction hash
    status:          RestOrderStatus,  // PendingPlace / Active / PendingCancel / Filled /
                                       // PartiallyFilled / Failed / Cancelled / Expired / Unknown
    filled_size:     String,           // decimal
    filled_amount:   String,           // decimal
    fail_reason:     Option<String>,   // present only when status == RestOrderStatus::Failed
    cancel_reason:   Option<String>,   // set when the server initiates the cancel (heartbeat timeout, market settlement, etc.)
    odds_type:       OddsType,         // Spots / Points / Unknown
    created_at:      String,           // Unix ms (string)
    updated_at:      String,           // Unix ms (string)
}
```

### 6.6 `list_orders`

List the authenticated user's orders.

- **Endpoint:** `GET /api/v5/predictions/orders`
- **Auth:** Required

```rust
pub async fn list_orders(
    &self,
    market_id: Option<&str>,   // filter by market ID
    status:    Option<&str>,   // "open" (pending + active) | "closed" (filled / cancelled / expired / failed)
    cursor:    Option<&str>,   // pagination cursor
    limit:     Option<i32>,    // max 50, default 20
) -> Result<OrdersResponse, SdkError>;

// Type alias over the shared paginated-list response wrapper.
pub type OrdersResponse = PagedListResponse<OrderRecord>;
// pub struct PagedListResponse<T> { list: Vec<T>, next_cursor: Option<String>, has_next: bool }
```

## 7. Account: Positions

Module: `okx_outcomes_sdk::models::position::*`. The API is in `okx_outcomes_sdk::api::positions`.

### 7.1 `get_positions`

Query the authenticated user's positions.

- **Endpoint:** `GET /api/v5/predictions/positions`
- **Auth:** Required

```rust
pub async fn get_positions(
    &self,
    status:    Option<&str>,   // "open" | "closed"; all if omitted
    market_id: Option<&str>,
    cursor:    Option<&str>,   // pagination cursor
    limit:     Option<i32>,    // max 100, default 20
) -> Result<PositionsResponse, SdkError>;

pub type PositionsResponse = PagedListResponse<PositionRecord>;

pub struct PositionRecord {
    id:                         String,         // identifier
    token_id:                   String,
    market_id:                  String,
    token_index:                String,         // "1" = YES, "2" = NO
    token_name:                 String,         // "Yes" or "No"
    size:                       String,         // current remaining size
    available_size:             String,         // available size (= size − the portion frozen by sell orders)
    value:                      String,         // cur_price * size
    avg_price:                  String,         // weighted average entry cost
    un_realized_pnl:            String,         // unrealized PnL
    un_realized_pnl_percentage: String,
    title:                      String,         // display string
    icon:                       String,         // display string
    event_id:                   String,
    winning_token:              Option<String>, // winning token ID after settlement; None before settlement
    position_status:            i32,            // position status code (full enum in the API reference)
    cur_price:                  String,         // current token price
    realized_pnl:               String,         // realized PnL
    realized_pnl_percentage:    String,
    odds_type:                  OddsType,       // Spots / Points / Unknown
                                                // (verified in production: wire value is "points" or "spots", not "real")
}
```

## 8. Account: Trades

Module: `okx_outcomes_sdk::models::trade::*`. The API is in `okx_outcomes_sdk::api::trades`.

### 8.1 `get_trades`

Query the authenticated user's trade history.

- **Endpoint:** `GET /api/v5/predictions/trades`
- **Auth:** Required

```rust
pub async fn get_trades(
    &self,
    market_id:  Option<&str>,   // filter by market ID
    side:       Option<&str>,   // "BUY" | "SELL"
    start_time: Option<i64>,    // start time (inclusive, ms)
    end_time:   Option<i64>,    // end time (exclusive, ms)
    cursor:     Option<&str>,   // pagination cursor
    limit:      Option<i32>,    // max 100, default 20
) -> Result<TradesResponse, SdkError>;

type TradesResponse = PagedListResponse<TradeRecord>;

struct TradeRecord {
    trade_id:   String,    // empty string for TAKER rows and MAKER rows before on-chain assignment
    order_id:   String,
    market_id:  String,
    token_id:   String,
    side:       OrderSide, // Buy / Sell / Unknown
    size:       String,    // filled token amount
    amount:     String,    // filled amount
    price:      String,
    fee:        String,
    role:       Role,      // Maker / Taker / Unknown
    tx_hash:    String,
    created_at: String,    // Unix ms (string)
}
```

`trade_id` is `None` for TAKER rows, and for historical MAKER rows from before the on-chain trade-id assignment mechanism.

## 9. Conditional Tokens

Module: `okx_outcomes_sdk::models::position::*` (shared with positions). The API is in `okx_outcomes_sdk::api::positions`.

All three are write operations that require an EIP-712 signature. Each request body has the same outer shape: `{ action, nonce, signature }`, differing only in `action`. Each returns `TxHashResponse { tx_hash: String }`.

### 9.1 `split`

Split an equal amount of xp in a market into equal amounts of YES + NO tokens (the inverse of `merge`).

- **Endpoint:** `POST /api/v5/predictions/positions/split`
- **Auth:** Required + EIP-712 signature

```rust
pub async fn split(&self, req: &SplitRequest) -> Result<TxHashResponse, SdkError>;

struct SplitRequest { action: SplitAction, nonce: i64, signature: SignatureWrapper }
struct SplitAction {
    action_type: String, // "predictionSplit"
    market_id:   String,
    size:        String, // amount in minimal units
}
```

### 9.2 `merge`

Merge equal amounts of YES + NO tokens (back into xp).

- **Endpoint:** `POST /api/v5/predictions/positions/merge`
- **Auth:** Required + EIP-712 signature

```rust
pub async fn merge(&self, req: &MergeRequest) -> Result<TxHashResponse, SdkError>;

struct MergeRequest { action: MergeAction, nonce: i64, signature: SignatureWrapper }
struct MergeAction {
    action_type: String, // "predictionMerge"
    market_id:   String,
    size:        String,
}
```

### 9.3 `redeem`

After a market settles, redeem the caller's entire winning token balance. There is no `size` field; the server redeems the full amount the caller holds.

- **Endpoint:** `POST /api/v5/predictions/positions/redeem`
- **Auth:** Required + EIP-712 signature

```rust
pub async fn redeem(&self, req: &RedeemRequest) -> Result<TxHashResponse, SdkError>;

struct RedeemRequest { action: RedeemAction, nonce: i64, signature: SignatureWrapper }
struct RedeemAction {
    action_type: String, // "predictionRedeem"
    market_id:   String,
}
```

Returns `Api { code: 51020, ... }` if the market has not been resolved yet.

## 10. Market Data

Module: `okx_outcomes_sdk::models::price::*`. The API is in `okx_outcomes_sdk::api::prices`.

These calls hit OKX's market data API at `https://www.okx.com/api/v5/market/*` — the same host as the prediction market API but with a different path prefix and response envelope. The market data envelope wraps `code` as a JSON string, so the `code` in `SdkError::Api { code }` is the parsed integer value.

### 10.1 `get_ticker`

Latest ticker for a single instrument. `inst_id` is the market's `yes_outcome.asset_id`.

- **Endpoint:** `GET /api/v5/market/ticker`
- **Auth:** Required

```rust
pub async fn get_ticker(&self, inst_id: &str) -> Result<Ticker, SdkError>;

pub struct Ticker {
    inst_type:  String,
    inst_id:    String,
    last:       String, // latest trade price
    last_sz:    String, // latest trade size
    ask_px:     String, // best ask price
    ask_sz:     String, // size at the best ask
    bid_px:     String, // best bid price
    bid_sz:     String, // size at the best bid
    open24h:    String, // 24h open price
    high24h:    String, // 24h high price
    low24h:     String, // 24h low price
    vol24h:     String, // 24h volume (base currency)
    vol_ccy24h: String, // 24h volume (quote currency)
    sod_utc0:   String, // open price at UTC 0
    sod_utc8:   String, // open price at UTC+8
    ts:         String, // update timestamp (Unix ms decimal string)
}
```

The server returns a 1-element array; the SDK unwraps it. Returns `Api { code: -1, message: "ticker not found" }` when the inst ID is unknown.

### 10.2 `get_candles`

Candlestick history.

- **Endpoint:** `GET /api/v5/market/candles`
- **Auth:** Required

```rust
pub async fn get_candles(
    &self,
    inst_id: &str,
    bar:     Option<&str>,   // "1m" / "5m" / "15m" / "30m" / "1H" / "4H" / "1D" / ... ; default "1m"
    after:   Option<&str>,   // return candles with timestamp **before** this value (ms)
    before:  Option<&str>,   // return candles with timestamp **after** this value (ms)
    limit:   Option<i32>,    // max 300, default 100
) -> Result<Vec<Candle>, SdkError>;

pub struct Candle(pub Vec<String>);

impl Candle {
    pub fn ts(&self)        -> &str;   // index 0: open time (Unix ms string)
    pub fn open(&self)      -> &str;   // index 1: open price
    pub fn high(&self)      -> &str;   // index 2: high price
    pub fn low(&self)       -> &str;   // index 3: low price
    pub fn close(&self)     -> &str;   // index 4: close price
    pub fn vol(&self)       -> &str;   // index 5: volume (contracts)
    // index 6: volume in the base currency (no helper)
    // index 7: volume in the quote currency (no helper)
    pub fn confirmed(&self) -> bool;   // index 8: returns true when "1" (candle closed)
}
```

### 10.3 `get_pm_books`

Prediction market order book depth snapshot.

- **Endpoint:** `GET /api/v5/market/pm-books`
- **Auth:** Required
- **Rate limit:** 40 times / 2s

```rust
pub async fn get_pm_books(
    &self,
    inst_id: &str,             // YES outcome asset ID
    sz:      Option<i32>,      // number of depth levels per side; max 400 (up to 800 across both sides).
                               // Defaults to 1 (BBO only) when omitted.
) -> Result<PmBookDepth, SdkError>;

pub struct PmBookDepth {
    asks:   Vec<Vec<String>>,  // ask levels, ascending by price. Each entry is [price, size, order_count].
    bids:   Vec<Vec<String>>,  // bid levels, descending by price. Each entry is [price, size, order_count].
    ts:     String,            // snapshot timestamp (Unix ms decimal string)
    seq_id: i64,               // order book version sequence; opaque to most callers,
                               // exposed only to align with the API response
}
```

The server returns a 1-element `data` array; the SDK unwraps it. Returns `Api { code: -1, message: "pm-books snapshot not found" }` when the response is empty.

## 11. WebSocket

Module: `okx_outcomes_sdk::ws::*`. Requires the `websocket` Cargo feature.

**Connection model**

The Open API uses the same endpoint for public and private channels: `wss://<host>/ws/v5/business`. Public channels can be used anonymously. Private channels require a one-time `op: "login"` after the WS handshake.

Hosts:

```rust
pub mod ws::endpoints {
    pub const DEFAULT_WS_HOST: &str = "wss://ws.okx.com:8443";
    pub const EU_WS_HOST:      &str = "wss://wseea.okx.com";
    pub const US_WS_HOST:      &str = "wss://wsus.okx.com";
    pub const BUSINESS_PATH:   &str = "/ws/v5/business";
}
```

`PredictionsWsClient` uses `DEFAULT_WS_HOST` by default; override it with `PredictionsWsClient::with_host(...)` or the `PREDICTIONS_WS_HOST` environment variable.

Lifecycle and resilience:

- 25-second heartbeat keepalive (OKX requires < 30 seconds).
- Automatic reconnect with exponential backoff (3s -> 6s -> 12s -> capped at 30s).
- On reconnect, if credentials are stored, the client replays login and re-subscribes to every channel that was active when the connection dropped.
- Every state change triggers `connection_state_callback("public" | "private", connected: bool)`.

**Public API**

```rust
pub struct PredictionsWsClient { /* ... */ }

impl PredictionsWsClient {
    pub fn new() -> Self;
    pub fn with_host(host: &str) -> Self;

    pub async fn connect(&self, path: &str) -> Result<(), SdkError>;
    pub async fn login(&self, creds: &ApiCredentials) -> Result<(), SdkError>;
    pub async fn subscribe(&self, channel: &str, params: Vec<HashMap<String, String>>) -> Result<(), SdkError>;
    pub async fn unsubscribe(&self, channel: &str, params: Vec<HashMap<String, String>>) -> Result<(), SdkError>;
    pub async fn disconnect(&self);

    pub fn set_on_data(&self, callback: WsDataCallback);
    pub fn set_on_connection_state(&self, callback: WsConnectionStateCallback);
}

pub type WsDataCallback             = Arc<dyn Fn(&WsMessage) + Send + Sync>;
pub type WsConnectionStateCallback  = Arc<dyn Fn(&str, bool) + Send + Sync>;
```

`subscribe` is idempotent: calling it twice with the same `(channel, params)` pair does not create a duplicate subscription, nor a duplicate entry in the replay list.

Login signature (handled internally by `login`): the SDK computes `sign = Base64(HMAC-SHA256(secret_key, timestamp + "GET" + "/users/self/verify"))` and sends:

```json
{"op": "login", "args": [{"apiKey": "...", "passphrase": "...", "timestamp": "...", "sign": "..."}]}
```

The future returned by `login` resolves only after the server responds:

- `{"event":"login","code":"0"}` -> `Ok(())`.
- `{"event":"error","code":"600xx",...}` -> `Err(SdkError::WebSocket { message: "Login rejected: [60xxx] ..." })`.
- No response within 30 seconds -> `Err(SdkError::WebSocket { message: "Login timed out (30s)" })`.

**Message dispatch**

Each incoming JSON frame is parsed once into the `WsMessage` enum and handed to `on_data`. Consumers never see the raw JSON.

```rust
pub enum WsMessage {
    Event {
        event:   String,
        channel: Option<String>,
        inst_id: Option<String>,
        msg:     Option<String>,
    },
    Prices(Vec<WsPriceTick>),
    Books { data: Vec<WsPmBookData>, action: String }, // action = "snapshot" | "update"
    Trades(Vec<WsPmTrade>),
    Tickers(Vec<WsPmTicker>),
    Game(Vec<WsGameStatus>),
    EventStatus(Vec<WsEventStatus>),
    Candle(Vec<Candle>),                   // typed wrapper, 9-column OHLCV array
    Orders(Vec<WsOrder>),
    Positions(Vec<WsPosition>),
    UserTrades(Vec<WsUserTrade>),
    Balance(Vec<WsBalance>),
    Pnl(Vec<WsPnl>),
    Unknown { channel: String, raw: serde_json::Value },
}
```

`WsMessage::Event` carries acknowledgement messages such as `event:"subscribe"|"unsubscribe"|"login"|"error"`, unrelated to any data channel.

**Public channels**

### 11.1 `prediction-market-prices`

Per-market price ticks.

- **Subscribe params:** `[{"instId": "<asset_id>"}]` (one per market).
- **Message variant:** `WsMessage::Prices(Vec<WsPriceTick>)`.

```rust
struct WsPriceTick {
    yes_asset_id:     String,
    last_trade_price: Option<String>, // None before the first trade
    best_bid:         Option<String>, // None if no bids
    best_ask:         Option<String>, // None if no asks
    timestamp:        String,         // Unix ms decimal string
    probability:      String,         // basis points * 100, e.g. "6500" = 65.00%
    market_volume:    String,
    event_volume:     String,
    event_id:         String,
}
```

### 11.2 `pm-books`

Order book snapshots and incremental updates.

- **Subscribe params:** `[{"instId": "<asset_id>"}]`.
- **Message variant:** `WsMessage::Books { data, action }`, where `action` is `"snapshot"` (full snapshot on subscribe / reconnect) or `"update"` (incremental delta).

```rust
struct WsPmBookData {
    asks:        Vec<Vec<String>>, // [[price, size, ...], ...]
    bids:        Vec<Vec<String>>, // [[price, size, ...], ...]
    ts:          String,
    checksum:    Option<i64>,      // CRC32 integrity check over the normalized book
    seq_id:      Option<i64>,      // monotonic sequence; a gap = loss, reset required
    prev_seq_id: Option<i64>,      // -1 for the first snapshot
}
```

When `prev_seq_id` does not match the previous frame's `seq_id`, discard the local book and wait for the next `snapshot`.

### 11.3 `pm-trades`

Public trade feed.

- **Subscribe params:** `[{"instId": "<asset_id>"}]`.
- **Message variant:** `WsMessage::Trades(Vec<WsPmTrade>)`.

```rust
struct WsPmTrade {
    inst_id:  String,
    trade_id: Option<String>, // single push: Some; aggregated push: None
    f_id:     Option<String>, // aggregated push: first trade id; single: None
    l_id:     Option<String>, // aggregated push: last trade id; single: None
    px:       String,         // price
    sz:       String,         // size
    side:     String,         // "buy" / "sell" (taker side)
    ts:       String,
}
```

Distinguish single pushes from windowed aggregated pushes by which of `trade_id` and (`f_id`, `l_id`) is `Some`.

### 11.4 `pm-tickers`

OKX-style per-instrument ticker push.

- **Subscribe params:** `[{"instId": "<asset_id>"}]`.
- **Message variant:** `WsMessage::Tickers(Vec<WsPmTicker>)`.

```rust
struct WsPmTicker {
    inst_type: String, inst_id: String,
    last:       String, last_sz: String,
    ask_px:     String, ask_sz:  String,
    bid_px:     String, bid_sz:  String,
    open24h:    String, high24h: String, low24h: String,
    vol24h:     String, vol_ccy24h: String,
    sod_utc0:   String, sod_utc8: String,
    ts:         String,
}
```

### 11.5 `pm-event-status`

Event settlement push.

- **Subscribe params:** `[{"instId": "event-<event_id>"}]`. The SDK does **not** automatically add the `event-` prefix on the WS path; pass it explicitly.
- **Message variant:** `WsMessage::EventStatus(Vec<WsEventStatus>)`.

```rust
struct WsEventStatus {
    event_id:       String,
    status:         String,  // e.g. "resolved"
    market_id:      String,  // winning market ID
    outcome_option: String,  // "yes" / "no" / "others" / team name / "draw"
    timestamp:      String,
}
```

### 11.6 `pm-candle*`

Candlestick stream. The channel name encodes the bar: `pm-candle1m`, `pm-candle5m`, `pm-candle1H`, `pm-candle1D`, etc.

- **Subscribe params:** `[{"instId": "<asset_id>"}]`.
- **Message variant:** `WsMessage::Candle(Vec<Candle>)`, where each `Candle` is a typed wrapper over a 9-column OHLCV array. Accessors: `ts()`, `open()`, `high()`, `low()`, `close()`, `vol()`, `vol_ccy()`, `vol_ccy_quote()`, `confirmed()`.

**Private channels (require `login`)**

Subscribe with empty params (the server scopes the subscription to the logged-in account).

### 11.7 `pm-order`

Order status changes.

- **Subscribe params:** `[]`.
- **Message variant:** `WsMessage::Orders(Vec<WsOrder>)`.

```rust
struct WsOrder {
    order_id:        String,
    market_id:       String,
    status:          OrderStatus,       // Active / Filled / PartiallyFilled / PlaceFailed /
                                        // CancelFailed / Cancelled / Expired / Unknown
    side:            OrderSide,         // Buy / Sell / Unknown
    // All fields below depend on `status` — see the status -> required-fields table in the spec.
    // Modeled as Option; a missing key deserializes to None.
    client_order_id: Option<String>,
    asset_id:        Option<String>,    // assetId of YES or NO
    direction:       Option<Direction>, // Yes / No / Unknown — the outcome side this order takes
    filled_size:     Option<String>,
    order_size:      Option<String>,    // serde alias = "size"
    avg_price:       Option<String>,
    amount:          Option<String>,    // BUY = spent, SELL = received (xp)
    limit_price:     Option<String>,    // serde alias = "price"
    fail_message:    Option<String>,    // present only for PLACE_FAILED / CANCEL_FAILED
    odds_type:       Option<OddsType>,
    tx_hash:         Option<String>,    // serde rename = "txHash"
    trade_id:        Option<String>,
}
```

### 11.8 `pm-position`

Position updates.

- **Subscribe params:** `[]`.
- **Message variant:** `WsMessage::Positions(Vec<WsPosition>)`.

This channel has two payload variants; `WsPosition` represents both with a single flat struct, with the variant-specific fields wrapped in `Option`. Branch on `status` (via `PositionStatus::is_position_snapshot()` / `is_failed()`) to decide which fields are meaningful.

```rust
struct WsPosition {
    // Common to both variants
    market_id: String,
    status:    PositionStatus,    // Fill / FillFailed / Redeem / RedeemFailed /
                                  // Split / SplitFailed / Merge / MergeFailed /
                                  // Deposit / DepositFailed / Withdraw / WithdrawFailed / Unknown
    amount:    String,            // variant 1: position `remain` ("0" for REDEEM)
                                  // variant 2: split/merge/deposit/withdraw amount
    odds_type: Option<OddsType>,

    // Variant 1 (FILL / REDEEM / *_FAILED) — full position snapshot
    id:                          Option<String>,
    token_id:                    Option<String>,
    asset_id:                    Option<String>,
    timestamp:                   Option<String>,
    un_realized_pnl:             Option<String>,
    un_realized_pnl_percentage:  Option<String>,
    value:                       Option<String>,
    avg_price:                   Option<String>,
    trade_id:                    Option<String>,

    // Variant 2 (SPLIT / MERGE / DEPOSIT / WITHDRAW / *_FAILED)
    tx_hash: Option<String>,           // serde rename = "txHash"
    ext:     Option<WsPositionExt>,    // populated only for DEPOSIT
}

struct WsPositionExt {
    to_tx_hash: String,                // serde rename = "toTxHash"
}
```

### 11.9 `pm-user-trade`

The user's own trade stream.

- **Subscribe params:** `[]`.
- **Message variant:** `WsMessage::UserTrades(Vec<WsUserTrade>)`.

```rust
struct WsUserTrade {
    order_id:        String,
    client_order_id: String,    // defaults to empty string for resilience; usually present
    market_id:       String,
    token_id:        String,
    asset_id:        String,    // yesAssetId or noAssetId
    side:            OrderSide, // Buy / Sell / Unknown
    size:            String,
    price:           String,
    txhash:          String,
    timestamp:       String,
    trade_id:        String,    // trade ID
}
```

### 11.10 `pm-balance`

Balance changes.

- **Subscribe params:** `[]`.
- **Message variant:** `WsMessage::Balance(Vec<WsBalance>)`.

```rust
struct WsBalance {
    wallet_address: String,
    available:      String,
    total:          String,
    frozen:         String,
    token_id:       String,                  // on-chain Point token id
    change_type:    BalanceChangeType,       // Place / Cancel / Fill / Split / Merge /
                                             // Redeem / Deposit / Withdraw / Unknown
    change_amount:  Option<String>,          // spec: may be null
    update_time:    String,
    odds_type:      Option<OddsType>,
}
```

### 11.11 `pm-pnl`

Unrealized PnL stream — pushes **two payloads**; modeled with a serde `untagged` enum that picks the right variant based on the fields present.

- **Subscribe params:** `[]`.
- **Message variant:** `WsMessage::Pnl(Vec<WsPnl>)`.

```rust
enum WsPnl {
    Overview(WsPnlOverview),     // portfolioValue + per-period summaries
    Timeseries(WsPnlTimeseries), // chart points with high/low/current
}

struct WsPnlOverview {
    portfolio_value: String,                 // xp balance + position market value
    periods:         Vec<WsPnlPeriodSummary>,
}

struct WsPnlPeriodSummary {
    period:      String, // "1D" / "1W" / "1M" / "6M" / "1Y"
    period_pnl:  String,
    pnl_percent: String,
}

struct WsPnlTimeseries {
    period:      String,         // "0"=1D / "1"=1W / "2"=1M / "3"=6M / "4"=1Y
    interval:    String,         // ms: 600000 / 1800000 / 3600000 / 86400000
    points:      Vec<WsPnlPoint>,
    current_pnl: String,
    high:        String,
    low:         String,
}
```

**WS Error Codes**

WS-layer errors are exposed via `WsMessage::Event { event: "error", msg, .. }`; login-related errors are exposed via the `Err` returned by `login()`. Common error codes:

| Error code | Meaning |
| --- | --- |
| `60004` | Invalid login timestamp (clock drift, expired). |
| `60005` | Invalid API key. |
| `60006` | Timestamp expired (30-second window). |
| `60007` | Invalid signature. |
| `60009` | Login failed (generic). |
| `60011` | This private channel requires login. |
| `60012` | Invalid `op` value. |
| `60018` | Subscription failed (wrong channel name or params). |

## 12. Signing

Module: `okx_outcomes_sdk::signing::*`. Requires the `signing` Cargo feature.

The full pipeline for any write operation: build a typed `Action`, sign it with your `k256::ecdsa::SigningKey` via `sign_to_wrapper`, then put the resulting `SignatureWrapper` into the request body.

```rust
pub fn parse_private_key(hex_key: &str) -> Result<SigningKey, String>;
pub fn now_millis() -> u64;

pub fn sign_to_wrapper(
    action:        &Action,
    nonce:         u64,
    expires_after: Option<u64>,
    key:           &SigningKey,
) -> Result<SignatureWrapper, String>;
```

Action constructors:

```rust
pub fn action_place_order(orders: Vec<OrderRequest>) -> Action;
pub fn action_cancel(cancels: Vec<CancelRequest>) -> Action;
pub fn action_cancel_all(asset_ids: Vec<String>, market_type: &str) -> Action;
pub fn action_prediction_split (market_id: &str, size: &str) -> Action;
pub fn action_prediction_merge (market_id: &str, size: &str) -> Action;
pub fn action_prediction_redeem(market_id: &str) -> Action;
```

Typed inputs:

```rust
struct OrderRequest {
    asset_id:        String,
    side:            SigningOrderSide,  // Buy / Sell (wire side lowercase)
    market_type:     String,            // "prediction"
    client_order_id: Option<String>,    // 34-char client order ID
    price:           String,
    reduce_only:     bool,
    size:            String,
    size_type:       SizeType,          // Base (default) / Quote
    order_type:      OrderType,
}

enum OrderType {
    Limit(LimitTif),
}
enum LimitTif {
    Plain(String),                                  // "gtc" | "ioc" | "fok" | "alo"
    Gtd { expiry_time: u64 },                       // good-til-date, ms
}

struct CancelRequest {
    asset_id:    String,
    market_type: String,                            // "prediction"
    target:      CancelTarget,
}
enum CancelTarget { Oid(String), ClientOrderId(String) }
```

The wire-side counterparts (`OrderItem`, `CancelItem`) implement `TryFrom<&OrderRequest>` and `From<&CancelRequest>` respectively, so the JSON body and the signed bytes are both constructed from the same source struct.

Client order ID:

```rust
pub fn generate_client_order_id_default() -> Result<String, String>;
pub fn generate_client_order_id(region: Region, env: Env) -> Result<String, String>;
pub fn validate_client_order_id(s: &str) -> bool;
pub fn parse_client_order_id_prefix(client_order_id: Option<&str>) -> ClientOrderIdPrefix;
pub fn register_client_order_id_context(region: Region, env: Env);
```

The client order ID is a 34-char hex string of the form `0x{region}{env}{30 hex random chars}`. `generate_client_order_id_default()` reads the registered global context (HK / PROD by default). To use a different context, register it once at startup to override.

Low-level helpers:

```rust
pub fn signer_address(key: &SigningKey) -> String;
pub fn ecrecover(signing_hash: &str, signature: &str) -> Result<String, String>;
pub fn sign_action(...) -> Result<String, String>;        // returns "0x..." hex
pub fn sign_action_full(...) -> Result<(String, String, String, u8), String>; // (txhash, r, s, v)
pub fn sign_action_debug(...) -> Result<SigningDebug, String>; // returns all intermediate hashes
```

For the normal flow, use `sign_to_wrapper`. The low-level functions are only for debugging, or for callers that need access to the txhash (e.g. to display a "view in explorer" link).

## 13. Common Types

Module: `okx_outcomes_sdk::models::common::*`.

```rust
struct Pagination {
    next_cursor: Option<String>, // None on the last page
    has_more:    bool,
    page_size:   i32,            // item count on the current page
}

struct EcdsaSignature {
    r: String, // hex, with 0x prefix
    s: String, // hex, with 0x prefix
    v: u8,     // recovery id: 0 or 1
}

struct SignatureWrapper {
    // serialized as { "Ecdsa": { r, s, v } }
    ecdsa: EcdsaSignature,
}
```

The SDK transparently wraps two API envelopes:

- Prediction market REST: `{ "code": <int>, "message": "...", "data": <T> }`, where `code == 0` means success.
- OKX market data: `{ "code": "<int>", "msg": "...", "data": <T> }` (note `code` is a string). `code == "0"` means success.
