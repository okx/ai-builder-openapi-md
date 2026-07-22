# trade — OKX V5 API endpoints

Supported per site (✓ = available, ✗ = not on that site). The `doc` column is a REFERENCE path (valid for most sites) — the exact per-site/per-lang path can differ, so read `index.json` `sites.<site>.doc.<lang>` for the authoritative path, then open `docs/<site>/<lang>/<that>`. Frontmatter carries method/path/rest/ws.

| method | path | name | doc (ref) | okex | us | hongkong | my | turkey |
|---|---|---|---|:-:|:-:|:-:|:-:|:-:|
| GET | /api/v5/trade/account-rate-limit | GET / Account rate limit | api/rest/trade/accountRateLimit.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/trade/easy-convert-currency-list | GET / Easy convert currency list | api/rest/trade/easyConvertList.md | ✓ | ✓ | ✓ | ✗ | ✓ |
| GET | /api/v5/trade/easy-convert-history | GET / Easy convert history | api/rest/trade/easyConvertHistory.md | ✓ | ✓ | ✓ | ✗ | ✓ |
| GET | /api/v5/trade/fills | GET / Transaction details (last 3 days) | api/rest/trade/fills.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/trade/fills-archive | GET / Transaction details (in the past 2 years) | api/rest/trade/getFillsHistoryArchive.md | ✗ | ✗ | ✓ | ✗ | ✓ |
| GET | /api/v5/trade/fills-history | GET / Transaction details (last 3 months) | api/rest/trade/fillsHistory.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/trade/one-click-repay-currency-list | GET / One-click repay currency list | api/rest/trade/oneClickRepayList.md | ✓ | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/trade/one-click-repay-currency-list-v2 | GET / One-click repay currency list (New) | api/rest/trade/oneClickRepayListV2.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/trade/one-click-repay-history | GET / One-click repay history | api/rest/trade/oneClickRepayHistory.md | ✓ | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/trade/one-click-repay-history-v2 | GET / One-click repay history (New) | api/rest/trade/oneClickRepayHistoryV2.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/trade/order | GET / Order details | api/rest/trade/getOrder.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/trade/order-algo | GET / Algo order details | api/rest/trade/getOrderAlgo.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/trade/orders-algo-history | GET / Algo order history | api/rest/trade/ordersAlgoHistory.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/trade/orders-algo-pending | GET / Algo order list | api/rest/trade/ordersAlgoPending.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/trade/orders-history | GET / Order history (last 7 days) | api/rest/trade/ordersHistory.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/trade/orders-history-archive | GET / Order history (last 3 months) | api/rest/trade/ordersHistoryArchive.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/trade/orders-pending | GET / Order List | api/rest/trade/ordersPending.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/trade/amend-algos | POST / Amend algo order | api/rest/trade/amendAlgos.md | ✓ | ✓ | ✗ | ✓ | ✓ |
| POST | /api/v5/trade/amend-batch-orders | POST / Amend multiple orders | api/rest/trade/amendBatchOrders.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/trade/amend-order | POST / Amend order | api/rest/trade/amendOrder.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/trade/batch-orders | POST / Place multiple orders | api/rest/trade/batchOrders.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/trade/cancel-algos | POST / Cancel algo order | api/rest/trade/cancelAlgos.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/trade/cancel-all-after | POST / Cancel All After | api/rest/trade/cancelAllAfter.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/trade/cancel-batch-orders | POST / Cancel multiple orders | api/rest/trade/cancelBatchOrders.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/trade/cancel-order | POST / Cancel order | api/rest/trade/cancelOrder.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/trade/close-position | POST / Close positions | api/rest/trade/closePosition.md | ✓ | ✗ | ✗ | ✓ | ✗ |
| POST | /api/v5/trade/easy-convert | POST / Place easy convert | api/rest/trade/easyConvertOrder.md | ✓ | ✓ | ✓ | ✗ | ✓ |
| POST | /api/v5/trade/fills-archive | POST / Transaction details (in the past 2 years) | api/rest/trade/fillsHistoryArchive.md | ✗ | ✗ | ✓ | ✗ | ✓ |
| POST | /api/v5/trade/mass-cancel | POST / Mass cancel order | api/rest/trade/massCancel.md | ✓ | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/trade/one-click-repay | POST / Trade one-click repay | api/rest/trade/oneClickRepayOrder.md | ✓ | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/trade/one-click-repay-v2 | POST / Trade one-click repay (New) | api/rest/trade/oneClickRepayOrderV2.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| POST | /api/v5/trade/order | POST / Place order | api/rest/trade/placeOrder.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/trade/order-algo | POST / Place algo order | api/rest/trade/orderAlgo.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/trade/order-precheck | POST / Order precheck | api/rest/trade/orderPrecheck.md | ✓ | ✗ | ✗ | ✗ | ✗ |
