---
site: my
lang: en
name: Concept Overview
type: guide
---

# Concept Overview

This section introduces the core business concepts you need to understand before integrating the Outcomes API, including xp (the base asset), events, markets, outcomes, prices, Split / Merge, the mirror order book, the order book, positions, settlement, and the key IDs.

Once you understand these concepts, you will be able to reason more clearly about:

- How to find a tradable market from an event
- How to choose the YES / NO outcome
- Why placing orders, cancelling orders, and querying market data generally use `assetId`
- The relationship between Split / Merge and the mirror order book
- The difference between a Binary Market and a NegRisk Market

<aside class="notice">
<code>xp</code> is short for OKX X-Layer Points. It is distributed through OKX campaigns and cannot be purchased directly; it is the base asset of this prediction market. Users trade each market's YES / NO outcomes with xp, and all of the order placement, balances, Split / Merge, and settlement below are denominated in xp.
</aside>

<aside class="notice">
<code>assetId</code> is one of the most important identifiers in the trading-related endpoints. What you ultimately trade is neither the Event nor the Market itself, but the assetId corresponding to a specific outcome under a Market.
</aside>

## 1. Prediction Market

A prediction market is a market for trading on the outcomes of future events.

Each market usually corresponds to a clear question, for example:

**Will Germany win a given match?**

Users trade the different outcomes of that question based on their own judgment. The most common outcomes are `YES` and `NO`.

Example:

| Object | Example |
| --- | --- |
| Market | Will Germany win? |
| YES | Germany wins |
| NO | Germany does not win |

In simple terms:

| Direction | Meaning |
| --- | --- |
| Buy YES | The user believes the outcome will happen |
| Buy NO | The user believes the outcome will not happen |

## 2. Core Object Model

The core object relationships in the Outcomes API are as follows:

- **Event**
  - **Market**
    - **YES outcome**
    - **NO outcome**

In simple terms:

| Object | Meaning |
| --- | --- |
| Event | A real-world event |
| Market | A specific tradable question under an Event |
| Outcome | A tradable result under a Market, e.g. YES / NO |
| assetId | The tradable asset ID of a specific outcome |

When integrating, you typically follow this path:

1. Query an `Event`
2. Get the `Market`s under that `Event`
3. Choose the `YES` or `NO` outcome under a `Market`
4. Trade using the `assetId` of that outcome

## 3. Event and Market

An Event is the upper-level organizational unit of the prediction market, representing a real-world event.

Example:

| Object | Example |
| --- | --- |
| Event | Germany vs. Curacao |

An Event can contain multiple Markets:

| Market | Question |
| --- | --- |
| Market 1 | Will Germany win? |
| Market 2 | Will the two teams draw? |
| Market 3 | Will Curacao win? |

What users actually trade is not the Event itself, but the YES or NO outcome under a specific Market.

<aside class="notice">
An Event is mainly used to organize and display markets; what is actually traded is the specific outcome under a Market.
</aside>

## 4. Outcome

An Outcome is the tradable object under a Market.

In a binary market, each Market usually has two outcomes:

| Outcome | Meaning |
| --- | --- |
| YES | The Market's result will happen |
| NO | The Market's result will not happen |

Each outcome has its own `assetId`. When placing an order, what you ultimately use is the `assetId` of the target outcome.

Example:

- `marketId`: `100001`
- `question`: Will Germany win?
- `yesOutcome`: `assetId` = `100049000`, `price` = `0.65`
- `noOutcome`: `assetId` = `100049001`, `price` = `0.35`

To buy YES, use `yesOutcome.assetId`.
To buy NO, use `noOutcome.assetId`.

## 5. Price and Probability

A price in the prediction market is a decimal between `0` and `1`. It can be understood as the market's real-time estimate of the probability that an outcome happens.

For example:

| Outcome | Current price | Interpretation |
| --- | --- | --- |
| YES | `0.65` | The market currently estimates the probability of YES at about 65% |
| NO | `0.35` | The market currently estimates the probability of NO at about 35% |

**Why price ≈ probability**: a winning outcome settles at `1 xp` and a losing one at `0 xp`, so the **expected value of holding an outcome = 1 xp × the probability that it occurs**. In an efficient market the price converges to this expected value, so the price can be read as the market's estimate of that outcome's probability.

The price is formed by market trading and does not represent the final result. The final result is determined by market settlement.

<aside class="warning">
The price is only a real-time expectation formed by market trading; it does not represent the platform's judgment or guarantee of the final result.
</aside>

## 6. Complementary Relationship of YES / NO

YES and NO are the two sides of the same Binary Market. When one outcome happens, the other does not.

Ideally, the sum of their prices is close to `1`.

| Outcome | Price |
| --- | --- |
| YES | `0.65` |
| NO | `0.35` |
| YES + NO | `1.00` |

This relationship can be summarized as:

**YES + NO ≈ 1**

You do not need to handle the YES / NO price conversion manually. When placing an order, simply choose the target outcome and use its `assetId`.

## 7. Split / Merge Mechanism

Split / Merge is the underlying mechanism behind the YES / NO complementary relationship.

For a Binary Market, YES and NO can be seen as a pair of conditional outcomes. The system can convert between xp (the base asset of this prediction market) and the YES / NO outcomes via Split and Merge.

### 7.1 Split

Split splits one unit of xp into equal amounts of YES and NO outcomes.

**1 xp → 1 YES + 1 NO**

This can be understood as: the user splits one complete market entitlement into the market's two complementary outcomes.

| Before | After |
| --- | --- |
| 1 xp | 1 YES + 1 NO |

### 7.2 Merge

Merge is the inverse of Split. The user can merge equal amounts of YES and NO outcomes back into xp.

**1 YES + 1 NO → 1 xp**

Merge is only possible when the YES and NO amounts are equal.

| Before | After |
| --- | --- |
| 1 YES + 1 NO | 1 xp |

This mechanism guarantees the complementary relationship of YES and NO:

**YES + NO ≈ 1**

In other words, within the same Binary Market, YES and NO are not fully independent assets, but the two sides of the same set of conditional outcomes.

## 8. Mirror Order Book

Based on the YES / NO complementary relationship, the system can use a mirror order book to handle YES and NO trading in a unified way.

For the same Binary Market:

| User action | Economically equivalent to |
| --- | --- |
| Buy YES @ `0.60` | Sell NO @ `0.40` |
| Buy NO @ `0.30` | Sell YES @ `0.70` |

The reason is:

**YES price + NO price ≈ 1**

Therefore, the system can support both YES and NO trading on a single unified order book at the underlying level.

You do not need to split or convert mirror orders manually. When placing an order, simply choose the outcome the user wants to trade and use its `assetId`. The system handles the YES / NO equivalence at the order book level.

<aside class="notice">
The mirror order book is the system's underlying matching mechanism. For API consumers, the most important thing is to choose the correct outcome and place orders with the corresponding assetId.
</aside>

## 9. Order Book

Outcomes uses a central limit order book (CLOB).

CLOB stands for Central Limit Order Book. Market prices are not set directly by the platform, but are formed by users' orders and trades.

The order book has two sides:

| Side | Meaning |
| --- | --- |
| Bid | The price a buyer is willing to buy at |
| Ask | The price a seller is willing to sell at |

Example:

| Side | Price | Size |
| --- | --- | --- |
| Ask | `0.67` | `100` |
| Ask | `0.66` | `80` |
| Bid | `0.65` | `120` |
| Bid | `0.64` | `200` |

When the bid and ask prices match, the orders are matched and traded.

## 10. Maker and Taker

When an order is filled, the user may be a Maker or a Taker.

| Role | Meaning |
| --- | --- |
| Maker | Posts an order to the order book, providing liquidity |
| Taker | Actively fills an existing order in the book, consuming liquidity |

Example:

The current best ask is `0.66`.

| User action | Result | Role |
| --- | --- | --- |
| Buy at `0.66` | Fills immediately | Taker |
| Post a buy at `0.60` | Cannot fill immediately; enters the book and waits | Maker |

Order matching generally follows:

| Priority | Description |
| --- | --- |
| Price priority | Better prices are matched first |
| Time priority | At the same price, earlier orders in the book are matched first |

## 11. Position

Open orders that are not yet filled occupy the available balance. After an order is filled, the user obtains a position in the corresponding outcome.

Example:

| Action | Result |
| --- | --- |
| User buys 10 YES | After the fill, the user holds 10 YES outcomes |

Positions change with the following operations:

| Operation | Effect on position |
| --- | --- |
| Fill | Increases or decreases the corresponding outcome position |
| Close | Decreases an existing outcome position |
| Split | Generates YES / NO outcomes |
| Merge | Merges YES / NO outcomes |
| Settlement | Updates positions and balances based on the final result |

## 12. Closing a Position

A user does not have to wait for market settlement to exit a position. If the market is still tradable, the user can close a position by selling an existing outcome.

Example:

| Step | Action |
| --- | --- |
| Step 1 | User buys YES at `0.40` |
| Step 2 | YES price later rises to `0.70` |
| Step 3 | User sells YES to close the position |
| Step 4 | The user's position and balance are updated based on the fill |

Closing a position is essentially exiting an existing position via a reverse trade.

## 13. Settlement

Once an event's result is determined, the Market enters the settlement process.

After settlement:

| Outcome | Settlement result |
| --- | --- |
| Winning outcome | Settled at `1 xp` |
| Losing outcome | Settled at `0 xp` |

Example:

Market: Will Germany win?

If Germany ultimately wins:

| Outcome | Result |
| --- | --- |
| YES | Wins |
| NO | Loses |

If Germany does not win:

| Outcome | Result |
| --- | --- |
| YES | Loses |
| NO | Wins |

After the market settles, open orders are closed, and the user's positions and balances are updated based on the final result.

<aside class="warning">
After a market enters the settling or settled state, normal trading is generally no longer possible. Check the Market status before placing an order.
</aside>

## 14. Binary Market

A Binary Market is the most common market structure, representing a yes / no question.

Example:

| Market | Will Germany win? |
| --- | --- |
| YES | Germany wins |
| NO | Germany does not win |

A Binary Market usually has only two outcomes:

- `YES`
- `NO`

You only need to choose the `assetId` of YES or NO to trade.

## 15. NegRisk Market

A NegRisk Market is used for scenarios with multiple mutually exclusive outcomes. An Event can contain multiple related Markets, but usually only one outcome ultimately wins.

Example:

Event: Who will win the championship?

| Market | Question |
| --- | --- |
| Market 1 | Will Germany win the title? |
| Market 2 | Will France win the title? |
| Market 3 | Will Brazil win the title? |
| Market 4 | Will another team win the title? |

If Germany ultimately wins the title:

| Market | YES result |
| --- | --- |
| Will Germany win the title? | Wins |
| Will France win the title? | Loses |
| Will Brazil win the title? | Loses |
| Will another team win the title? | Loses |

For developers, a NegRisk Market does not change the basic trading method. You still choose the YES or NO outcome under a specific Market and trade with its `assetId`.

| Market type | Characteristics | Developer trading method |
| --- | --- | --- |
| Binary Market | A single yes / no question | Choose the `assetId` of YES or NO |
| NegRisk Market | Multiple mutually exclusive outcomes | Still choose the `assetId` of YES or NO under a specific Market |

## 16. Market Status

A Market goes through different states during its lifecycle.

Common states:

| Status | Meaning | Generally tradable |
| --- | --- | --- |
| `active` | Market is trading normally | Yes |
| `paused` | Market trading is paused | No |
| `settling` | Market is settling | No |
| `resolved` | Market has been settled | No |

Check the Market status before placing an order. If the Market is not `active`, the order may be rejected.

<aside class="notice">
The exact status enum is determined by the API response. Consumers should not rely solely on the front-end display status to decide whether a market is tradable.
</aside>

## 17. Key IDs

When integrating the API, the various IDs are the most easily confused.

Keep these in mind:

| ID | Meaning | Common use |
| --- | --- | --- |
| `eventId` | Event ID | Query Event details and its Markets |
| `marketId` | Market ID | Identify a specific tradable question |
| `assetId` | An outcome's asset ID | Place order, cancel, market data, order book, positions, and other trading operations |
| `orderId` | Order ID | Query orders, cancel, track order status |

The most important rule is:

**Order placement, cancellation, market data, and order book operations generally use `assetId`.**

If the API returns `assetId` as `null`, the outcome is temporarily not tradable. Wait until `assetId` returns a valid value before performing trading operations.

<aside class="warning">
If assetId is null, do not place orders, cancel, or query the order book, etc.; otherwise the request may be rejected.
</aside>

## 18. Summary

You can understand the core model of Outcomes with the following chain:

**Event → Market → Outcome → assetId → Order / Position**

The mapping is:

| Stage | Description |
| --- | --- |
| Event | Find a real-world event |
| Market | Find a specific question under that event |
| Outcome | Choose YES or NO |
| assetId | Get the tradable asset ID |
| Order | Place an order using assetId |
| Position | A position is formed after the fill |

For API consumers, the most critical points are:

| Key point | Description |
| --- | --- |
| Do not trade the Event | The Event is only an organizational unit |
| Do not trade the Market directly | The Market is a specific question |
| Actually trade the Outcome | The Outcome is represented by `assetId` |
| YES / NO are complementary | `YES + NO ≈ 1` |
| The mirror order book is handled by the system | No manual conversion needed |
| Check status before ordering | The Market must be tradable |
| assetId must not be null | `assetId = null` means temporarily not tradable |
