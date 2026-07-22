# tradingBot — OKX V5 API endpoints

Supported per site (✓ = available, ✗ = not on that site). The `doc` column is a REFERENCE path (valid for most sites) — the exact per-site/per-lang path can differ, so read `index.json` `sites.<site>.doc.<lang>` for the authoritative path, then open `docs/<site>/<lang>/<that>`. Frontmatter carries method/path/rest/ws.

| method | path | name | doc (ref) | okex | us | hongkong | my | turkey |
|---|---|---|---|:-:|:-:|:-:|:-:|:-:|
| GET | /api/v5/tradingBot/dca/cycle-list | GET / DCA cycle list | api/rest/dca/orderAlgoCycleList.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/dca/history-list | GET / DCA algo order history | api/rest/dca/orderAlgoDetailsHistory.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/dca/ongoing-list | GET / DCA algo order details | api/rest/dca/orderAlgoDetails.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/dca/orders | GET / DCA sub orders | api/rest/dca/orderAlgoSubOrders.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/dca/position-details | Get / DCA algo order position details | api/rest/dca/orderAlgoPositionDetails.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/grid/ai-param | GET / Grid AI parameter (public) | api/rest/grid/aiParam.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/grid/grid-quantity | GET / Max grid quantity (public) | api/rest/grid/maxGridQuantity.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/grid/orders-algo-details | GET / Grid algo order details | api/rest/grid/orderAlgoDetails.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/grid/orders-algo-history | GET / Grid algo order history | api/rest/grid/orderAlgoHistory.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/grid/orders-algo-pending | GET / Grid algo order list | api/rest/grid/orderAlgoPending.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/grid/positions | GET / Grid algo order positions | api/rest/grid/orderAlgoPosition.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/grid/sub-orders | GET / Grid algo sub orders | api/rest/grid/orderAlgoSubOrders.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/public/rsi-back-testing | GET / RSI back testing (public) | api/rest/grid/rsiBackTesting.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/recurring/orders-algo-details | GET / Recurring buy order details | api/rest/recurring/orderAlgoDetails.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/recurring/orders-algo-history | GET / Recurring buy order history | api/rest/recurring/orderAlgoHistory.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/recurring/orders-algo-pending | GET / Recurring buy order list | api/rest/recurring/orderAlgoPending.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/recurring/sub-orders | GET / Recurring buy sub orders | api/rest/recurring/orderAlgoSubOrders.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/signal/event-history | GET / Signal bot event history | api/rest/signalBot/orderAlgoEventHistory.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/signal/orders-algo-details | GET / Signal bot order details | api/rest/signalBot/orderAlgoDetails.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/signal/orders-algo-history | GET / Signal bot history | api/rest/signalBot/orderAlgoHistory.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/signal/orders-algo-pending | GET / Active signal bot | api/rest/signalBot/orderAlgoPending.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/signal/positions | GET / Signal bot order positions | api/rest/signalBot/orderAlgoPosition.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/signal/positions-history | GET / Position history | api/rest/signalBot/orderAlgoPositionHistory.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/signal/signals | GET / Signals | api/rest/signalBot/getSignals.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/tradingBot/signal/sub-orders | GET / Signal bot sub orders | api/rest/signalBot/orderAlgoSubOrders.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/dca/amend-order-algo | POST / Amend spot dca basic param | api/rest/dca/orderAlgoAmendBasicParam.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/dca/create | POST / Place dca algo order | api/rest/dca/orderAlgoPlace.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/dca/margin/add | POST / Add dca margin | api/rest/dca/addMarginBal.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/dca/margin/reduce | POST / Reduce dca margin | api/rest/dca/reduceMarginBal.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/dca/orders/manual-buy | POST / Add investment | api/rest/dca/addInvestment.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/dca/settings/reinvestment | POST / Amend dca reinvestment | api/rest/dca/orderAlgoAmendReinvestment.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/dca/settings/take-profit | POST / Amend dca take profit settings | api/rest/dca/orderAlgoAmendTakeProfit.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/dca/stop | POST / Stop dca algo order | api/rest/dca/closePosition.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/grid/adjust-investment | POST / Add investment | api/rest/grid/addInvestment.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/grid/amend-algo-basic-param | POST / Amend grid algo order basic param | api/rest/grid/orderAlgoAmendBasicParam.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/grid/amend-order-algo | POST / Amend grid algo order | api/rest/grid/orderAlgoAmend.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/grid/cancel-close-order | POST / Cancel close position order for contract grid | api/rest/grid/orderAlgoCancelClosePosOrder.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/grid/close-position | POST / Close position for contract grid | api/rest/grid/orderAlgoClosePos.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/grid/compute-margin-balance | POST / Compute margin balance | api/rest/grid/compMarginBal.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/grid/copy-order-algo | POST / Copy grid algo order | api/rest/grid/copyOrderAlgoPlace.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/grid/margin-balance | POST / Adjust margin balance | api/rest/grid/adjMarginBal.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/grid/min-investment | POST / Compute min investment (public) | api/rest/grid/minInvestment.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/grid/order-algo | POST / Place grid algo order | api/rest/grid/orderAlgoPlace.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/grid/order-instant-trigger | POST / Instant trigger grid algo order | api/rest/grid/orderAlgoInstantTrigger.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/grid/stop-order-algo | POST / Stop grid algo order | api/rest/grid/orderAlgoCancel.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/grid/withdraw-income | POST / Spot grid withdraw income | api/rest/grid/withdrawIncome.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/recurring/add-investment | POST / Add investment | api/rest/recurring/addInvestment.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/recurring/amend-order-algo | POST / Amend recurring buy order | api/rest/recurring/orderAlgoAmend.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/recurring/amend-price-range | POST / Amend price range | api/rest/recurring/amendPriceRange.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/recurring/amend-recurring-amount | POST / Amend recurring buy amount | api/rest/recurring/amendRecurringAmount.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/recurring/amend-recurring-time | POST / Amend recurring buy time | api/rest/recurring/amendRecurringTime.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/recurring/order-algo | POST / Place recurring buy order | api/rest/recurring/orderAlgoPlace.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/recurring/pause | POST / Pause recurring buy | api/rest/recurring/orderAlgoPause.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/recurring/restart | POST / Restart recurring buy | api/rest/recurring/orderAlgoRestart.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/recurring/stop-order-algo | POST / Stop recurring buy order | api/rest/recurring/orderAlgoCancel.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/signal/amendTPSL | POST / Amend TPSL | api/rest/signalBot/amendTPSL.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/signal/cancel-sub-order | POST / Cancel sub order | api/rest/signalBot/cancelSubOrder.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/signal/close-position | POST / Close position | api/rest/signalBot/closePosition.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/signal/create-signal | POST / Create signal | api/rest/signalBot/signal.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/signal/margin-balance | POST / Adjust margin balance | api/rest/signalBot/adjMarginBal.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/signal/order-algo | POST / Create signal bot | api/rest/signalBot/orderAlgoPlace.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/signal/set-instruments | POST / Set instruments | api/rest/signalBot/setInstruments.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/signal/stop-order-algo | POST / Cancel signal bots | api/rest/signalBot/orderAlgoCancel.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/tradingBot/signal/sub-order | POST / Place sub order | api/rest/signalBot/placeSubOrder.md | ✓ | ✗ | ✗ | ✗ | ✗ |
