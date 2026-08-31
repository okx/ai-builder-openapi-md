# public — OKX V5 API endpoints

Supported per site (✓ = available, ✗ = not on that site). The `doc` column is a REFERENCE path (valid for most sites) — the exact per-site/per-lang path can differ, so read `index.json` `sites.<site>.doc.<lang>` for the authoritative path, then open `docs/<site>/<lang>/<that>`. Frontmatter carries method/path/rest/ws.

| method | path | name | doc (ref) | okex | us | my | turkey |
|---|---|---|---|:-:|:-:|:-:|:-:|
| GET | /api/v5/public/block-trades | Get public single-leg transactions of block trades | api/rest/market/blockTrades_new.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/public/convert-contract-coin | Unit convert | api/rest/public/unitConvert.md | ✓ | ✓ | ✗ | ✗ |
| GET | /api/v5/public/delivery-exercise-history | Get delivery/exercise history | api/rest/public/deliveryExerciseHistory.md | ✓ | ✓ | ✓ | ✗ |
| GET | /api/v5/public/delta-hedge-currencies | Get Delta hedge currencies | api/rest/public/deltaHedgeCurrencies.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/public/discount-rate-interest-free-quota | Get discount rate and interest-free quota | api/rest/public/discountRateFreeInterest.md | ✓ | ✓ | ✗ | ✗ |
| GET | /api/v5/public/economic-calendar | Get economic calendar data | api/rest/public/economicCalendar.md | ✓ | ✗ | ✗ | ✓ |
| GET | /api/v5/public/estimated-price | Get estimated delivery/exercise price | api/rest/public/estimatedPrice.md | ✓ | ✓ | ✓ | ✗ |
| GET | /api/v5/public/estimated-settlement-info | Get estimated future settlement price | api/rest/public/estimatedFutureSettlementPrice.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/public/event-contract/events | Get events | api/rest/public/events.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/public/event-contract/markets | Get markets | api/rest/public/markets.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/public/event-contract/series | Get series | api/rest/public/series.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/public/funding-rate | Get funding rate | api/rest/public/fundingRate.md | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/public/funding-rate-history | Get funding rate history | api/rest/public/fundingRateHistory.md | ✓ | ✓ | ✓ | ✗ |
| GET | /api/v5/public/instrument-tick-bands | Get instrument tick bands | api/rest/public/instrumentTickBands.md | ✓ | ✓ | ✗ | ✗ |
| GET | /api/v5/public/instruments | Get instruments | api/rest/public/instruments.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/public/insurance-fund | Get security fund | api/rest/public/insuranceFund.md | ✓ | ✓ | ✓ | ✗ |
| GET | /api/v5/public/interest-rate-loan-quota | Get interest rate and loan quota | api/rest/public/interestRateLoanQuota.md | ✓ | ✓ | ✗ | ✗ |
| GET | /api/v5/public/mark-price | Get mark price | api/rest/public/markPrice.md | ✓ | ✓ | ✓ | ✗ |
| GET | /api/v5/public/market-data-history | Get historical market data | api/rest/public/marketDataHistory.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/public/mm-instrument-types | Get MM instrument types | api/rest/public/getMmInstrumentTypes.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/public/open-interest | Get open interest | api/rest/public/openInterest.md | ✓ | ✓ | ✓ | ✗ |
| GET | /api/v5/public/opt-summary | Get option market data | api/rest/public/optSummary.md | ✓ | ✓ | ✗ | ✗ |
| GET | /api/v5/public/option-trades | GET / Option trades | api/rest/public/optionTrades.md | ✓ | ✓ | ✗ | ✗ |
| GET | /api/v5/public/position-tiers | Get position tiers | api/rest/public/tier.md | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/public/premium-history | Get premium history | api/rest/public/premiumIndex.md | ✓ | ✓ | ✗ | ✗ |
| GET | /api/v5/public/price-limit | Get limit price | api/rest/public/limitPrice.md | ✓ | ✓ | ✓ | ✗ |
| GET | /api/v5/public/settlement-history | Get futures settlement history | api/rest/public/futureSettlementHistory.md | ✓ | ✓ | ✗ | ✗ |
| GET | /api/v5/public/time | Get system time | api/rest/public/time.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/public/underlying | Get underlying | api/rest/public/underlying.md | ✓ | ✓ | ✓ | ✗ |
