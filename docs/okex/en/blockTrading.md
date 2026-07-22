---
site: okex
lang: en
name: Block Trading
type: guide
---

# Block Trading 

## Block Trading Workflow

A block trade is a **large sized, privately negotiated** transaction that allows traders to execute spot, perpetuals, futures, options and a combination of instruments (multi leg) which are traded **outside the order book** and at a **mutually agreed price** between the counter-parties. Once the transaction economics have been agreed upon, it will be submitted to OKX to be seamlessly margined, cleared and executed.

**Basic Concepts**

1. **RFQs** - Request for Quote sent by the Taker to Maker(s). It captures the quantity, instrument or multi instrument strategy that a Taker wants to trade.
2. **Quotes** - Quotes are created by the *Maker* in response to a requested RFQ.
3. **Trades** - Trades occur when the *Taker* successfully *executes* upon a makers quote to an RFQ.

**High Level Workflow** 

To trade as either Taker or Maker, users need to deposit at least 100,000 USD into their trading account. 
In addition, to become a Maker, [Please complete the form to access block trading](https://share.hsforms.com/1mYdfKtJJR3CC03IyCeC6hg3a1fq).

1. Taker creates an RFQ and selects which counterparties to broadcast the RFQ to.
2. Multiple Maker(s) send a two way quote as a response to the RFQ.
3. Taker chooses to execute upon the best quote and the trade is sent to OKX for clearing & settlement.
4. Taker & Maker receive confirmation of the trade's execution. 
5. Trade economics are published to market feed. (minus counterparty info) 

**Self-trade Prevention**
Users cannot send RFQ requests to themselves.

**Taker's Perspective**

1. Taker creates an RFQ using `POST /api/v5/rfq/create-rfq`. Taker can pull available instruments via `GET /api/v5/public/instruments` and available counterparties from `GET /api/v5/rfq/counterparties`.
2. Taker can cancel an RFQ anytime until it becomes inactive with `POST /api/v5/rfq/cancel-rfq`.
3. Maker, who is a requested counterparty to the RFQ, and is notified over the `rfqs` WebSocket channel, can provide a Quote to the RFQ.
4. Taker, who will be notified of quotes from the `quotes` WebSocket channel, can execute upon the best Quote with `POST /api/v5/rfq/execute-quote`.
5. Taker will receive confirmation of the trade's successful execution on the `struc-block-trades` and `rfqs` WebSocket channel.
6. Taker will also receive confirmation of the trade being completed on the `public-struc-block-trades` WebSocket channel as well as all other block trades on OKX.

**Maker's Perspective**

1. Maker is notified about a new RFQ who they are a counterparty to, on the `rfqs` WebSocket channel.
2. Maker can create a one way or two way Quote using `POST /api/v5/rfq/create-quote`.
3. Maker can cancel an existing quote anytime until it becomes inactive with `POST /api/v5/rfq/cancel-quote`.
4. Taker chooses to execute upon an available Quote.
5. Maker will receive updates of their Quote from the `quotes` WebSocket channel.
6. Maker will receive confirmation of the successful execution of their Quote from the `struc-block-trades` and `quotes` WebSocket channel.
7. Maker will receive confirmation of the trade being completed on the `public-struc-block-trades` WebSocket channel as well as all other block trades on OKX.
