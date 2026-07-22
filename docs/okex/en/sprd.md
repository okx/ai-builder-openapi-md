---
site: okex
lang: en
name: Spread Trading
type: guide
---

# Spread Trading

<aside>
👉 The Spread Orderbook product enables users to <b>post or consume liquidity</b> on spreads for <b>large sizes</b> that are guaranteed <b>atomic execution</b>. Benefits include simplified futures rolls, funding arbitrage, yield enhancement, and speculation on basis and term structures.

</aside>

## Introduction
### Basic Concepts

1. **Spread -** Entering a trade where the trader is long one instrument and short an offsetting quantity of a related instrument, forming a trade with two risk offsetting legs.
2. **Order-book -** A collection of offers to trade an instrument or basket. Each offer contains a defined instrument or group of instruments, relevant quantity, and the price at which the offerer is willing to transact. Takers can then immediately consume these offers up to the full amount of quantity listed at the offered price. The pending order limit of spread trading is 500 across all spreads.

### High Level Workflow 

Nitro Spreads is centered around the familiar concept of a Central Limit Order Book (**CLOB**).

- Spreads consist of instruments sourced from OKX where they are cleared and settled.
- Anyone can act as a "Taker," who consumes an existing resting order, or a "Maker," whose order is consumed.
- Trades take place when orders are crossed. Trades are then sent for clearing and settlement on OKX.

At a high level, the Nitro Spreads workflow is as follows:

1. *Maker* rests a Limit Order upon a Spread's Order Book.
2. *Taker* consumes a resting Order via a Limit Order.
3. The crossed orders are sent for clearing and settlement.
4. The *Taker* and *Maker* receive confirmation of the success or rejection of the Trade.
5. All users are notified of successfully settled & cleared Trades, minus the counterparties or sides (`buy` / `sell`) involved.

Key aspects of Nitro Spreads:

* All Spreads have **publicly accessible** Central Limit Order Books **(CLOB)**.
* The availability of trading Spreads is determined by OKX. Typically, these Spreads encompass all possible combinations of delta one derivatives (Expiry Futures and Perpetual Futures) and SPOT within a specific instrument family (e.g. "BTC/USDT" or "ETH/USDC").
* **Partial fills** and multiple orders can be consumed as part of a single trade.
* **Counterparties** are **NOT** selected. All Spread Order Books can be engaged by anyone, effectively trading against the broader market.
* Anonymity is maintained throughout the process, with all orders and trades conducted on an **anonymous basis**.
* Users have the flexibility to place multiple orders on both the bid and ask sides of the Order Book, allowing for a **ladder-style** configuration.

## Comprehensive API Workflow

<aside class="notice">
Notifications regarding Orders and Trades will be received by both the Taker and the Maker through the WebSocket Notification channels.
</aside>

A user assumes the role of a *Maker* when their Order is executed upon by another Order. A user becomes a *Taker* when they submit an Order that crosses an existing Order in the Order Book.

### Obtaining Available Spreads
To retrieve all available Spreads for trading on OKX, make a request to the `GET /api/v5/sprd/spreads` endpoint.

### Retrieving Your Orders
To retrieve orders on OKX, make a request to the `GET /api/v5/sprd/order` endpoint.

### Retrieving Your Trades
To retrieve trades on OKX, make a request to the `GET /api/v5/sprd/trades` endpoint.

### Submitting an Order
To submit an order to a Spread's Order Book, make a request to the `POST /api/v5/sprd/order` endpoint.

### Spread States

There are three different states during a Spread's life cycle: `live`, `suspend`, and `expired` as detailed below:

1. `live`: Spreads that are actively traded on Nitro Spreads
2. `suspend`: Spreads in which at least one of the legs is suspended and the other one is active or suspended on the OKX orderbook exchange; or spreads in which the underlying instruments are still live on the OKX orderbook exchange, but removed from Nitro Spreads
3. `expired`: Spreads in which at least one of the underlying instruments is expired on the OKX orderbook exchange

Please refer to the following table for all possible scenarios given the state of the underlying instruments and the resulting state of the spread on Nitro Spreads (except for the case that the spread is delisted on Nitro Spreads):

| Instrument A | Instrument B | Spread State |
| --- | --- | --- |
| Live | Live | Live |
| Suspend | Live | Suspend |
| Live | Suspend | Suspend |
| Suspend | Suspend | Suspend |
| Expired | Live | Expired |
| Live | Expired | Expired |
| Suspend | Expired | Expired |
| Expired | Suspend | Expired |
| Expired | Expired | Expired |

### Trade Lifecycle

In order for a trade to take place, two orders must be crossed within a Spread's Order Book.

Obtain information about the state of an Order and determine if it has reached its final state by monitoring the `sprd-orders`WebSocket channel. The `state` key in the channel indicates the current state of the Order. If the state is `live` or `partially_filled`, it means that the Order still has available size (`sz`) that the creator or another user can take action on. On the other hand, if the state is `canceled` or `filled`, the Order no longer has any available actions that the creator or any other user can take action on.

It is important to closely track the values of the following attributes: `sz`(size),`pendingFillSz` (pending fill size), `canceledSz` (canceled size), and `accFillSz`(accumulated fill size). These attributes provide crucial information regarding the status and progression of the Order.

### Order State

Track the state of an order by subscribing to the `sprd-orders` WebSocket channel.

1. Upon submitting an order, whether as a Maker or Taker, an order update message is sent via the orders WebSocket channel. The message will indicate the order's `state` == `live`.
2. Order matching and trade settlement are asynchronous processes. When the order is matched but not settled, system pushes `pendingSettleSz` > 0 and `fillSz` == ""
3. If the order is partially filled, an order update message is sent with `state` == `partially_filled`.
4. In the event that the order is completely filled, an order update message is sent with the `state` == `filled`.
5. If the order is not fully filled but has reached its final state, an order update message is sent with the `state` == `canceled`.
6. If a certain part of an order is rejected, an order update message is sent with updated `canceledSz` and `pendingFillSz`, and `code` and `msg` corresponding to the error.

### Trade State

Track the state of a trade by subscribing to the `sprd-trades`WebSocket channel.

1. After an executed trade undergoes clearing and settlement on OKX, it reaches finality.
2. For successfully cleared trades, a WebSocket message is sent with the `state`denoted as `filled`.
3. In the case of an unsuccessful trade clearing, a trade update message is sent with the `state` reflected as `rejected`.
4. If the trade state is `rejected`, the trade update message will also include the error `code` and a corresponding error message (`msg`) that explains the reason for the rejection.

### All Trades

All users have the ability to receive updates on all trades that take place through the OKX Nitro Spreads product.

It's important to note that OKX Nitro Spreads does not disclose information about the counterparties involved in the trades or the individual `side` (`buy` or `sell`) of the composite legs that were traded.

1. By subscribing to the `sprd-public-trades`WebSocket channel, WebSocket messages are sent exclusively for trades that have been successfully cleared and settled.
