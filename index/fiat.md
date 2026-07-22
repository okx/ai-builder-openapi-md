# fiat — OKX V5 API endpoints

Supported per site (✓ = available, ✗ = not on that site). The `doc` column is a REFERENCE path (valid for most sites) — the exact per-site/per-lang path can differ, so read `index.json` `sites.<site>.doc.<lang>` for the authoritative path, then open `docs/<site>/<lang>/<that>`. Frontmatter carries method/path/rest/ws.

| method | path | name | doc (ref) | okex | us | hongkong | my | turkey |
|---|---|---|---|:-:|:-:|:-:|:-:|:-:|
| GET | /api/v5/fiat/buy-sell/currencies | Get buy/sell currencies | api/rest/cashier/currencies.md | ✓ | ✗ | ✗ | ✓ | ✗ |
| GET | /api/v5/fiat/buy-sell/currency-pair | Get buy/sell currency pair | api/rest/cashier/currencyPair.md | ✓ | ✗ | ✗ | ✓ | ✗ |
| GET | /api/v5/fiat/buy-sell/history | Get buy/sell trade history | api/rest/cashier/tradeHistory.md | ✓ | ✗ | ✗ | ✓ | ✗ |
| GET | /api/v5/fiat/deposit | Get deposit order detail | api/rest/fundings/depositOrderDetail.md | ✓ | ✓ | ✗ | ✓ | ✓ |
| GET | /api/v5/fiat/deposit-order-history | Get deposit order history | api/rest/fundings/depositOrderHistory.md | ✓ | ✓ | ✗ | ✓ | ✓ |
| GET | /api/v5/fiat/deposit-payment-methods | Get deposit payment methods | api/rest/fundings/depositPaymentMethods.md | ✓ | ✓ | ✗ | ✓ | ✓ |
| GET | /api/v5/fiat/withdrawal | Get withdrawal order detail | api/rest/fundings/withdrawalOrderDetail.md | ✓ | ✓ | ✗ | ✓ | ✓ |
| GET | /api/v5/fiat/withdrawal-order-history | Get withdrawal order history | api/rest/fundings/withdrawalOrderHistory.md | ✓ | ✓ | ✗ | ✓ | ✓ |
| GET | /api/v5/fiat/withdrawal-payment-methods | Get withdrawal payment methods | api/rest/fundings/withdrawalPaymentMethods.md | ✓ | ✓ | ✗ | ✓ | ✓ |
| POST | /api/v5/fiat/buy-sell/quote | Get buy/sell quote | api/rest/cashier/quote.md | ✓ | ✗ | ✗ | ✓ | ✗ |
| POST | /api/v5/fiat/buy-sell/trade | Buy/sell trade | api/rest/cashier/trade.md | ✓ | ✗ | ✗ | ✓ | ✗ |
| POST | /api/v5/fiat/cancel-withdrawal | Cancel withdrawal order | api/rest/fundings/cancelWithdrawalOrder.md | ✓ | ✓ | ✗ | ✓ | ✓ |
| POST | /api/v5/fiat/create-withdrawal | Create withdrawal order | api/rest/fundings/createWithdrawalOrder.md | ✓ | ✓ | ✗ | ✓ | ✓ |
