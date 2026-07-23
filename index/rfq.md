# rfq — OKX V5 API endpoints

Supported per site (✓ = available, ✗ = not on that site). The `doc` column is a REFERENCE path (valid for most sites) — the exact per-site/per-lang path can differ, so read `index.json` `sites.<site>.doc.<lang>` for the authoritative path, then open `docs/<site>/<lang>/<that>`. Frontmatter carries method/path/rest/ws.

| method | path | name | doc (ref) | okex | us | my | turkey |
|---|---|---|---|:-:|:-:|:-:|:-:|
| GET | /api/v5/rfq/counterparties | Get Counterparties | api/rest/blockTrading/counterparties.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/rfq/maker-instrument-settings | Get Quote products | api/rest/blockTrading/getMakerInstrumentSettings.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/rfq/mmp-config | Get MMP Config | api/rest/blockTrading/mmpConfigInfo.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/rfq/public-trades | Get public multi-leg transactions of block trades | api/rest/blockTrading/getPublicTrades.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/rfq/quotes | Get quotes | api/rest/blockTrading/getQuotes.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/rfq/rfqs | Get rfqs | api/rest/blockTrading/getRFQs.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/rfq/trades | Get trades | api/rest/blockTrading/getTrades.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/rfq/cancel-all-after | Cancel All After | api/rest/blockTrading/cancelAllAfter.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/rfq/cancel-all-quotes | Cancel all Quotes | api/rest/blockTrading/cancelAllQuotes.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/rfq/cancel-all-rfqs | Cancel all RFQs | api/rest/blockTrading/cancelAllRFQs.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/rfq/cancel-batch-quotes | Cancel multiple Quotes | api/rest/blockTrading/cancelBatchQuotes.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/rfq/cancel-batch-rfqs | Cancel multiple RFQs | api/rest/blockTrading/cancelBatchRFQs.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/rfq/cancel-quote | Cancel Quote | api/rest/blockTrading/cancelQuote.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/rfq/cancel-rfq | Cancel RFQ | api/rest/blockTrading/cancelRFQ.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/rfq/create-quote | Create Quote | api/rest/blockTrading/createQuote.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/rfq/create-rfq | Create RFQ | api/rest/blockTrading/createRFQ.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/rfq/execute-quote | Execute Quote | api/rest/blockTrading/executeQuote.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/rfq/maker-instrument-settings | Set Quote products | api/rest/blockTrading/makerInstrumentSettings.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/rfq/mmp-config | Set MMP | api/rest/blockTrading/mmpConfig.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/rfq/mmp-reset | Reset MMP status | api/rest/blockTrading/mmpReset.md | ✓ | ✗ | ✗ | ✗ |
