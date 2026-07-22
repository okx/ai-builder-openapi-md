# predictions — OKX V5 API endpoints

Supported per site (✓ = available, ✗ = not on that site). The `doc` column is a REFERENCE path (valid for most sites) — the exact per-site/per-lang path can differ, so read `index.json` `sites.<site>.doc.<lang>` for the authoritative path, then open `docs/<site>/<lang>/<that>`. Frontmatter carries method/path/rest/ws.

| method | path | name | doc (ref) | okex | us | hongkong | my | turkey |
|---|---|---|---|:-:|:-:|:-:|:-:|:-:|
| GET | /api/v5/predictions/balance | 7.1 Query account balance | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/predictions/events | 1.1 Get events | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/predictions/events/search | 1.5 Search events | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/predictions/events/{eventId} | 1.2 Get a single event | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/predictions/events/{eventId}/markets | 1.3 Get markets | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/predictions/markets/{marketId} | 1.4 Get a single market | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/predictions/orders | 3.4 Get user orders | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/predictions/orders/{orderId} | 3.3 Get a single order | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/predictions/positions | 6.1 Query current positions | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/predictions/trades | 5.1 Query trades | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/predictions/heartbeat | 3.6 Send heartbeat | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/predictions/orders | 3.1 Place order | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/predictions/orders/cancel | 3.2 Cancel a single order | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/predictions/orders/cancel-all | 3.5 Cancel all / market orders | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/predictions/positions/merge | 4.2 Merge (YES + NO → xp) | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/predictions/positions/redeem | 4.3 Redeem (redeem xp after settlement) | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/predictions/positions/split | 4.1 Split (xp → YES + NO) | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
