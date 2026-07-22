# market — OKX V5 API endpoints

Supported per site (✓ = available, ✗ = not on that site). The `doc` column is a REFERENCE path (valid for most sites) — the exact per-site/per-lang path can differ, so read `index.json` `sites.<site>.doc.<lang>` for the authoritative path, then open `docs/<site>/<lang>/<that>`. Frontmatter carries method/path/rest/ws.

| method | path | name | doc (ref) | okex | us | hongkong | my | turkey |
|---|---|---|---|:-:|:-:|:-:|:-:|:-:|
| GET | /api/v5/market/block-ticker | Get block ticker | api/rest/market/blockTicker.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/market/block-tickers | Get block tickers | api/rest/market/blockTickers.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/market/books | GET / Order book | api/rest/market/books.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/market/books-full | GET / Full order book | api/rest/market/fullBooks.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/market/books-sbe | SBE Order book | api/rest/sbe/sbe.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/market/call-auction-details | GET / Call auction details | api/rest/market/callAuction.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/market/candles | GET / Candlesticks | api/rest/market/candles.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/market/exchange-rate | Get exchange rate | api/rest/market/exchangeRate.md | ✓ | ✗ | ✓ | ✗ | ✓ |
| GET | /api/v5/market/history-candles | GET / Candlesticks history | api/rest/market/historyCandles.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/market/history-index-candles | Get index candlesticks history | api/rest/market/historyIndexCandles.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/market/history-mark-price-candles | Get mark price candlesticks history | api/rest/market/historyMarkPriceCandles.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/market/history-trades | GET / Trades history | api/rest/market/historyTrades.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/market/index-candles | Get index candlesticks | api/rest/market/indexCandles.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/market/index-components | Get index components | api/rest/market/indexComponents.md | ✓ | ✗ | ✗ | ✓ | ✗ |
| GET | /api/v5/market/index-tickers | Get index tickers | api/rest/market/indexTickers.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/market/mark-price-candles | Get mark price candlesticks | api/rest/market/markPriceCandles.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/market/open-oracle | Get oracle | api/rest/market/oracle.md | ✗ | ✗ | ✓ | ✗ | ✓ |
| GET | /api/v5/market/option/instrument-family-trades | GET / Option trades by instrument family | api/rest/market/optionTrades.md | ✓ | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/market/platform-24-volume | GET / 24H total volume | api/rest/market/24Volume.md | ✓ | ✗ | ✓ | ✗ | ✓ |
| GET | /api/v5/market/pm-books | 2.3 Get order book | predictionMarket/outcomes_rest_api.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/market/sprd-candles | Get candlesticks | api/rest/sprd/getCandleSticks.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/market/sprd-history-candles | Get candlesticks history | api/rest/sprd/getCandleSticksHistory.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/market/sprd-ticker | Get ticker (Public) | api/rest/sprd/getTicker_new.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/market/ticker | GET / Ticker | api/rest/market/ticker.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/market/tickers | GET / Tickers | api/rest/market/tickers.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/market/trades | GET / Trades | api/rest/market/trades.md | ✓ | ✓ | ✓ | ✓ | ✓ |
