# copytrading — OKX V5 API endpoints

Supported per site (✓ = available, ✗ = not on that site). The `doc` column is a REFERENCE path (valid for most sites) — the exact per-site/per-lang path can differ, so read `index.json` `sites.<site>.doc.<lang>` for the authoritative path, then open `docs/<site>/<lang>/<that>`. Frontmatter carries method/path/rest/ws.

| method | path | name | doc (ref) | okex | us | hongkong | my | turkey |
|---|---|---|---|:-:|:-:|:-:|:-:|:-:|
| GET | /api/v5/copytrading/config | GET / Account configuration | api/rest/copyTrading/config.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/copy-settings | GET / Copy settings | api/rest/copyTrading/copySettings.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/current-lead-traders | GET / My lead traders | api/rest/copyTrading/currentLeadTraders.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/current-subpositions | GET / Existing lead positions | api/rest/copyTrading/currentSubpositions.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/instruments | GET / Leading instruments | api/rest/copyTrading/instruments.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/profit-sharing-details | GET / Profit sharing details | api/rest/copyTrading/profitSharingDetails.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/public-config | GET / Copy trading configuration | api/rest/copyTrading/publicConfig.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/public-copy-traders | GET / Copy traders | api/rest/copyTrading/publicCopyTraders.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/public-current-subpositions | GET / Lead trader current lead positions | api/rest/copyTrading/publicCurrentSubpositions.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/public-lead-traders | GET / Lead trader ranks | api/rest/copyTrading/publicLeadTraders.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/public-pnl | GET / Lead trader daily pnl | api/rest/copyTrading/publicPnl.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/public-preference-currency | GET / Lead trader currency preferences | api/rest/copyTrading/publicPreferenceCurrency.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/public-stats | GET / Lead trader stats | api/rest/copyTrading/publicStats.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/public-subpositions-history | GET / Lead trader lead position history | api/rest/copyTrading/publicSubpositionsHistory.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/public-weekly-pnl | GET / Lead trader weekly pnl | api/rest/copyTrading/publicWeeklyPnl.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/subpositions-history | GET / Lead position history | api/rest/copyTrading/subpositionsHistory.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/total-profit-sharing | GET / Total profit sharing | api/rest/copyTrading/totalProfitSharing.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/total-unrealized-profit-sharing | GET / Total unrealized profit sharing | api/rest/copyTrading/totalUnrealizedProfitSharing.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/copytrading/unrealized-profit-sharing-details | GET / Unrealized profit sharing details | api/rest/copyTrading/unrealizedProfitSharingDetails.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/copytrading/algo-order | POST / Place lead stop order | api/rest/copyTrading/algoOrder.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/copytrading/amend-copy-settings | POST / Amend copy settings | api/rest/copyTrading/amendCopySettings.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/copytrading/amend-profit-sharing-ratio | POST / Amend profit sharing ratio | api/rest/copyTrading/amendProfitSharingRatio.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/copytrading/close-subposition | POST / Close lead position | api/rest/copyTrading/closeSubposition.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/copytrading/first-copy-settings | POST / First copy settings | api/rest/copyTrading/firstCopySettings.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/copytrading/set-instruments | POST / Amend leading instruments | api/rest/copyTrading/setInstruments.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/copytrading/stop-copy-trading | POST / Stop copying | api/rest/copyTrading/stopCopyTrading.md | ✓ | ✗ | ✗ | ✗ | ✗ |
