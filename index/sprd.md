# sprd — OKX V5 API endpoints

Supported per site (✓ = available, ✗ = not on that site). The `doc` column is a REFERENCE path (valid for most sites) — the exact per-site/per-lang path can differ, so read `index.json` `sites.<site>.doc.<lang>` for the authoritative path, then open `docs/<site>/<lang>/<that>`. Frontmatter carries method/path/rest/ws.

| method | path | name | doc (ref) | okex | us | my | turkey |
|---|---|---|---|:-:|:-:|:-:|:-:|
| GET | /api/v5/sprd/books | Get order book (Public) | api/rest/sprd/getBooks.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/sprd/order | Get order details | api/rest/sprd/getOrder.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/sprd/orders-history | Get orders (last 21 days) | api/rest/sprd/getOrdersHistory.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/sprd/orders-history-archive | Get orders history (last 3 months) | api/rest/sprd/getOrdersHistoryArchive.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/sprd/orders-pending | Get active orders | api/rest/sprd/getOrdersPending.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/sprd/public-trades | Get public trades (Public) | api/rest/sprd/getPublicTrades.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/sprd/spreads | Get Spreads (Public) | api/rest/sprd/getSpreads.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/sprd/trades | Get trades (last 7 days) | api/rest/sprd/getTrades.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/sprd/amend-order | Amend order | api/rest/sprd/amendOrder.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/sprd/cancel-all-after | Cancel All After | api/rest/sprd/cancelAllAfter.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/sprd/cancel-order | Cancel order | api/rest/sprd/cancelOrder.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/sprd/mass-cancel | Cancel All orders | api/rest/sprd/cancelAll.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/sprd/order | Place order | api/rest/sprd/order.md | ✓ | ✗ | ✗ | ✗ |
