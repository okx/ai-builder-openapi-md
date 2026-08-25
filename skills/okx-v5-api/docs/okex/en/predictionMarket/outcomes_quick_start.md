---
site: okex
lang: en
name: Quick Start
type: guide
---

# Quick Start

This guide walks you through the minimal integration flow of the Outcomes API and submitting your first order.

## Prerequisites

Before you start, make sure you have the following:

| Requirement | Description |
| ---------- | -------------------------------- |
| OKX account | Prediction market capability enabled |
| Available balance | For submitting orders |
| OKX App | For creating an API Key and authorizing an Agent |
| Dev environment | Examples in this doc use the Rust SDK |
| Agent private key | For signing write operations such as place / cancel order |

<aside class="notice">
The API Key and the Agent private key are two different credentials. The API Key authenticates REST requests; the Agent private key signs trading write operations.
</aside>

## Step 1: Set Up an API Key

Before signing any request, you must first create an API Key via the [OKX API management page](https://www.okx.com/account/my-api) or the App. After creation, you receive the following 3 credentials, which must be kept safe:

| Credential | Description |
| ---------- | ------------------------ |
| APIKey | Public identifier |
| SecretKey | Used to sign requests |
| Passphrase | An extra security credential you set yourself |

APIKey and SecretKey are randomly generated and provided by the platform; the Passphrase is set by you to strengthen the security of API access.

### APIKey Permissions

For the prediction market, select all of the following permissions:

| Permission | Description |
| ------------- | ------------------------------------ |
| Read | Read-only operations such as querying bills and history |
| Trade | Write operations such as place order, cancel, transfer, and config changes |

### Creation Steps

| Step 1: Tap "View" on the App prediction market page | Step 2: Tap the top-right menu |
| :---: | :---: |
| <img src="images/outcomes-quickstart-en-1.jpeg" alt="Tap View on the App prediction market page" style="width: 100%; max-width: 300px;" /> | <img src="images/outcomes-quickstart-en-2.jpeg" alt="Tap the top-right menu" style="width: 100%; max-width: 300px;" /> |

| Step 3: Open the API Key generation page | Step 4: Generate the API Key |
| :---: | :---: |
| <img src="images/outcomes-quickstart-en-3.jpeg" alt="Open the API Key generation page" style="width: 100%; max-width: 300px;" /> | <img src="images/outcomes-quickstart-en-4.jpeg" alt="Generate the API Key" style="width: 100%; max-width: 300px;" /> |

| Step 5: View details after generation | Step 6: Details page |
| :---: | :---: |
| <img src="images/outcomes-quickstart-en-5.jpeg" alt="View details after generation" style="width: 100%; max-width: 300px;" /> | <img src="images/outcomes-quickstart-en-6.jpeg" alt="Details page" style="width: 100%; max-width: 300px;" /> |

All REST endpoints require OKX API credentials. Construct the client with `with_credentials` (see Step 5).

## Step 2: Set Up an Agent

Keep your main key safe — create an Agent key that signs trades on your behalf.

1. Generate a new Ethereum key pair (any key-generation tool). Save the **Agent private key** and the **Agent address**.
2. Authorize the Agent in the **OKX App** — Agent authorization must be done via the App, not the API.

### App Agent Authorization Steps

| Step 1: Enter the Agent address to authorize | Step 2: Authorization succeeded |
| :---: | :---: |
| <img src="images/outcomes-quickstart-en-7.jpeg" alt="Enter the Agent address to authorize" style="width: 100%; max-width: 300px;" /> | <img src="images/outcomes-quickstart-en-8.jpeg" alt="Authorization succeeded" style="width: 100%; max-width: 300px;" /> |

After authorization, all subsequent write API calls use the Agent private key; read API calls do not require the private key.

## Step 3: Add the SDK

**Installation**

Add this SDK and the Tokio runtime to your `Cargo.toml`:

> Cargo.toml dependencies

```toml
[dependencies]
okx-outcomes-sdk = { git = "https://github.com/okx/outcomes-sdk.git", features = ["signing", "websocket"] }
tokio = { version = "1", features = ["full"] }
```

That is the complete dependency list. `tokio-tungstenite`, `k256`, `sha3`, `futures-util`, etc. are pulled in transitively via the SDK's features. **Do not** add these dependencies yourself, or you may end up with versions inconsistent with what the SDK was built against.

**Feature Flags**

| Feature | What it enables | When to enable |
| ----------- | ---------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| _default_ | REST client only (events, markets, orders, positions, balance, trades, prices, sports data). | Read-only integration. |
| `signing` | EIP-712 + ECDSA action signing helpers. | Any write operation: `place_order`, `cancel_order`, `cancel_all`, `split`, `merge`, `redeem`, `heartbeat`. |
| `websocket` | The tokio-tungstenite-based `PredictionsWsClient` and the typed `WsMessage` parser. | Real-time prices, trades, order book, and user order / position / balance streams. |

## Step 4: Configure Environment Variables

We recommend storing credentials in environment variables:

> Environment variable configuration

```shell
export PREDICTIONS_API_KEY="your-api-key"
export PREDICTIONS_API_SECRET="your-secret-key"
export PREDICTIONS_API_PASSPHRASE="your-passphrase"
export PREDICTIONS_AGENT_PRIVATE_KEY="0x..."
```

<aside class="warning">
In production, use a secrets management service and do not commit the <code>.env</code> file to your code repository.
</aside>

## Step 5: Initialize the Client

Initialize the SDK Client with the API Key, Secret Key, and Passphrase:

> Client initialization example

```rust
use okx_outcomes_sdk::{ApiCredentials, OutcomesSdkClient};

let creds = ApiCredentials {
    api_key:    std::env::var("PREDICTIONS_API_KEY")?,
    secret_key: std::env::var("PREDICTIONS_API_SECRET")?,
    passphrase: std::env::var("PREDICTIONS_API_PASSPHRASE")?,
};

let client = OutcomesSdkClient::with_credentials(creds);
```

The SDK signs REST requests locally and automatically writes the required authentication headers.

<aside class="notice">
<code>secret_key</code> is never sent over the network. The SDK signs each request locally with HMAC-SHA256 per OKX's REST authentication spec: <code>OK-ACCESS-SIGN = Base64(HMAC-SHA256(secret_key, timestamp + METHOD + path + body))</code>.
</aside>

<aside class="notice">
Placing / cancelling orders and split / merge / redeem also require an <b>on-chain private key</b> for EIP-712 signing (i.e. the Agent private key). This key is independent of your REST API Key.
</aside>

## Step 6: Query Tradable Markets

`get_events` returns a paginated list of events. All filter parameters are optional:

> Query events example

```rust
let page = client
    .get_events(
        Some("active"),      // status: "active" | "resolved"
        None,                // tag
        None,                // league_id
        Some("volume_24h"),  // sort: "volume" | "volume_24h" | "ending_soon" | "newest"
        None,                // cursor (from the previous page)
        Some(20),            // page_size (max 50)
    )
    .await?;

for event in &page.events {
    println!("{} - {} markets ({} points volume)",
        event.event_title, event.total_markets_count, event.volume);
}

// Pagination: pass `page.pagination.next_cursor` back as `cursor` on the next call.
```

**Other read methods**

| Method | Purpose |
| ---------------------------------------------------------------------- | -------------------------------- |
| `client.search(keyword, cursor, page_size)` | Search events and markets by keyword. |
| `client.get_event(event_id)` | A single event and its full market list. |
| `client.get_event_markets(event_id)` | All markets under a single event (no pagination). |
| `client.get_market(market_id)` | Get a single market by market ID. |
| `client.get_ticker(inst_id)` | Latest ticker for a single market instrument. |
| `client.get_candles(...)` | Candlestick history. |
| `client.get_trades(...)` | Recent public trade history. |

Find a market with `status: "active"` and record:

- `yesOutcome.assetId` (e.g. `"100049000"`) — used for placing orders
- `marketId` (e.g. `"1"`) — used for position queries

<aside class="warning">
A newly created market may have <code>assetId: null</code> (not yet on-chain). Skip these — wait until <code>assetId</code> is populated before operating.
</aside>

## Step 7: Place a Limit Order

Buy 10 YES tokens at 0.55 xp on market `100049000`. This requires **two layers of authentication**: the API Key headers + the SDK-signed request body.

> Full limit order example

```rust
use okx_outcomes_sdk::{ApiCredentials, OutcomesSdkClient};
use okx_outcomes_sdk::models::order::{
    LimitOrderType, OrderItem, PlaceOrderAction, PlaceOrderRequest, SigningOrderSide, SizeType,
};
use okx_outcomes_sdk::signing::{
    action_place_order, generate_client_order_id_default, now_millis, parse_private_key,
    sign_to_wrapper, LimitTif, OrderRequest, OrderType,
};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let creds = ApiCredentials {
        api_key:    std::env::var("PREDICTIONS_API_KEY")?,
        secret_key: std::env::var("PREDICTIONS_API_SECRET")?,
        passphrase: std::env::var("PREDICTIONS_API_PASSPHRASE")?,
    };
    let client = OutcomesSdkClient::with_credentials(creds);

    // 1. Load the on-chain signing key (hex, with or without 0x prefix).
    let key = parse_private_key(&std::env::var("PREDICTIONS_AGENT_PRIVATE_KEY")?)?;

    // 2. Build the typed order. `asset_id` is the outcome asset ID from Step 6.
    let order_request = OrderRequest {
        asset_id:         "100049000".into(),
        side:             SigningOrderSide::Buy,
        market_type:      "prediction".into(),
        client_order_id:  Some(generate_client_order_id_default()?),
        price:            "0.55".into(),
        reduce_only:      false,
        size:             "10".into(),
        size_type:        SizeType::Base, // omitted on the wire (default)
        order_type:       OrderType::Limit(LimitOrderType { tif: LimitTif::Gtc }),
    };

    // 3. Derive the wire-format OrderItem from the same OrderRequest so the
    //    JSON body cannot drift from the signed msgpack bytes.
    let order_item = OrderItem::try_from(&order_request)?;
    let action = action_place_order(vec![order_request]);

    // 4. Sign and assemble the SignatureWrapper expected by the wire format.
    let nonce = now_millis();
    let signature = sign_to_wrapper(&action, nonce, None, &key)?;

    // 5. Submit.
    let req = PlaceOrderRequest {
        action: PlaceOrderAction {
            action_type: "placeOrder".into(),
            grouping:    "na".into(),
            orders:      vec![order_item],
        },
        nonce: nonce as i64,
        signature,
    };
    let resp = client.place_order(&req).await?;
    println!("tx_hash: {}", resp.tx_hash);
    Ok(())
}
```

> Place order response example

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "txHash": "0x7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6f7a8b"
  }
}
```

## Step 8: View Orders

When `status = Some("open")`, `list_orders` returns all non-terminal orders on the account, page by page. Pass the `next_cursor` from the previous response back via the `cursor` parameter until `has_next` is `false`:

> Query order list example

```rust
use okx_outcomes_sdk::models::order::OrderRecord;

let mut cursor: Option<String> = None;
let mut all_open: Vec<OrderRecord> = Vec::new();

loop {
    let page = client
        .list_orders(
            None,             // market_id: None = all markets, Some(123) = a specific market
            Some("open"),     // "open" = PENDING_PLACE / ACTIVE / PENDING_CANCEL; "closed" = FILLED / PARTIALLY_FILLED / CANCELLED / EXPIRED / FAILED
            cursor.as_deref(),
            Some(50),         // max items per page
        )
        .await?;
    all_open.extend(page.list);
    if !page.has_next {
        break;
    }
    cursor = page.next_cursor;
}

println!("{} open orders", all_open.len());
for o in &all_open {
    println!(
        "#{} [{}] {} {}/{} @ {}  market={} asset={} cloid={:?}",
        o.id, o.status, o.side, o.filled_size, o.size, o.price,
        o.market_id, o.asset_id, o.client_order_id,
    );
}
```

Each `OrderRecord` exposes the order parameters (`side`, `size`, `price`, `order_type`, `size_type`, `expiration`), order metadata (`market_id`, `asset_id`, `client_order_id`, `tx_hash`), and the current lifecycle status (`status`, `filled_size`, `filled_amount`).

<aside class="notice">
To see only resting orders, filter on <code>o.status == "ACTIVE"</code>; this excludes orders still being processed by the matching engine and orders in the process of being cancelled.
</aside>

Tips:

- `market_id` is an `i64` in the SDK, not a string; pass it as `Some(123_456_789)`. The `marketId` returned by the REST API is a string, which the SDK converts internally.
- `client_order_id` (the cloid passed when placing the order) can be passed to `cancel_order` to cancel by client ID instead of server ID.
- Record `id` (e.g. `578840`) — you need it when cancelling.

## Step 9: Cancel an Order

The cancel flow is the same as `place_order`, also in three steps: build a typed `CancelRequest`, derive the corresponding wire-format `CancelItem` from it, sign the action, and finally submit the wire request. You can locate the target order by the order's server `oid` (the `OrderRecord.id` value) or by `cloid` (the `client_order_id` passed when placing the order).

The snippet below assumes `client` and `key` are already constructed as in the order placement section.

> Cancel order example

```rust
use okx_outcomes_sdk::models::order::{CancelItem, CancelOrderAction, CancelOrderRequest};
use okx_outcomes_sdk::signing::{
    action_cancel, now_millis, sign_to_wrapper, CancelRequest, CancelTarget,
};

// 1. CancelTarget must be one of:
//    Oid   = server-assigned order ID (OrderRecord.id), a decimal string.
//    Cloid = client-assigned order ID, a hex string with the 0x prefix.
let cancel_request = CancelRequest {
    asset_id:    "100049000".into(),
    market_type: "prediction".into(),
    target:      CancelTarget::Oid("578840".into()),
    // target:   CancelTarget::Cloid("0xabc...".into()),
};

// 2. Derive the wire item from the same request so the signed bytes do not diverge from the JSON body.
let cancel_item = CancelItem::from(&cancel_request);
let action      = action_cancel(vec![cancel_request]);

// 3. Sign and submit.
let nonce     = now_millis();
let signature = sign_to_wrapper(&action, nonce, None, &key)?;

let resp = client
    .cancel_order(&CancelOrderRequest {
        action: CancelOrderAction {
            action_type: "cancel".into(),
            cancels:     vec![cancel_item],
        },
        nonce:     nonce as i64,
        signature,
    })
    .await?;
println!("cancel tx_hash: {}", resp.tx_hash);
```

<aside class="notice">
A single <code>CancelOrderRequest</code> can carry multiple <code>CancelItem</code>s: put all the <code>CancelRequest</code>s you want to cancel into one <code>action_cancel(vec![...])</code> call, and they will share one signature and one on-chain transaction.
</aside>

<aside class="notice">
Cancellation is asynchronous: <code>cancel_order</code> returns once the transaction is accepted, and the order moves from <code>PENDING_CANCEL</code> to <code>CANCELLED</code> as the chain confirms. To wait for the terminal state, poll <code>get_order(id)</code> or subscribe to the private <code>pm-order</code> WebSocket channel.
</aside>

## Step 10: View Balance, Orders, and Positions

> Query balance and positions example

```rust
// Balance
let entries = client.get_balance().await?; // Vec<BalanceEntry>
for entry in &entries {
    println!("[{}] available={} total={}", entry.odds_type, entry.available, entry.balance);
}

// Positions
let positions = client.get_positions(
    Some("open"), // "open" | "closed" | "settled"
    None,         // market_id
    None,         // cursor
    Some(50),     // limit
).await?;
println!("{} positions", positions.list.len());
```

For order queries, see Step 8. `get_positions` is also paginated via `cursor` + `limit`.

## Full Flow Recap

The minimal integration flow is:

1. Create an API Key in the App / web
2. Authorize an Agent in the App
3. Install the SDK
4. Configure environment variables
5. Initialize the Client
6. Query Events and Markets
7. Get the assetId of the YES / NO outcome and submit an order with it
8. Query order status
9. Cancel the order if needed
10. Query positions and balance after the fill

## Notes

| Topic | Description |
| ---------- | ---------------------------------------------------------------------------------------------------------------- |
| Heartbeat | If you run a bot, set up a heartbeat to auto-cancel all orders when the bot disconnects. Send a pre-signed cancel-all every < 5 minutes; see the SDK docs for details. |
| Price tick | See the table below. |
| Nonce | Use the current millisecond timestamp. Each nonce can be used only once. |
| Timestamp skew | `OK-ACCESS-TIMESTAMP` must be within 30 seconds of the server time, otherwise error code 50102 is returned. |

**Price Tick Rules**

| Price range | Min precision | Example |
| ---------------- | -------- | ---------------- |
| 0.04 - 0.96 | 0.01 | 0.04, 0.55, 0.96 |
| < 0.04 or > 0.96 | 0.001 | 0.039, 0.962 |

## FAQ

| Issue | Possible cause | Resolution |
| --- | --- | --- |
| `assetId` is `null` | The market is not yet registered | Wait until `assetId` returns a valid value before placing orders |
| Order fails | Market not tradable, insufficient balance, signature error, or invalid parameters | Check the error code and the order failure reason |
| Signature fails | The Agent private key is incorrect, or the signed content does not match the request body | Confirm the Agent is authorized and use the SDK to build the signature |
| Order not found | Wrong order ID, or the order has already ended | Use the order list endpoint to confirm the order status |
| Order still not cancelled after cancel | The cancellation is being processed asynchronously | Wait for the status to update from `PENDING_CANCEL` to `CANCELLED` |
