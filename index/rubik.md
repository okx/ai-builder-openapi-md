# rubik — OKX V5 API endpoints

Supported per site (✓ = available, ✗ = not on that site). The `doc` column is a REFERENCE path (valid for most sites) — the exact per-site/per-lang path can differ, so read `index.json` `sites.<site>.doc.<lang>` for the authoritative path, then open `docs/<site>/<lang>/<that>`. Frontmatter carries method/path/rest/ws.

| method | path | name | doc (ref) | okex | us | hongkong | my | turkey |
|---|---|---|---|:-:|:-:|:-:|:-:|:-:|
| GET | /api/v5/rubik/stat/contracts/long-short-account-ratio | Get long/short ratio | api/rest/tradingData/longShortAccountRatio.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/rubik/stat/contracts/long-short-account-ratio-contract | Get contract long/short ratio | api/rest/tradingData/longShortAccountRatioContract.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/rubik/stat/contracts/long-short-account-ratio-contract-top-trader | Get top traders contract long/short ratio | api/rest/tradingData/topTradersLongShortAccountRatioContract.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/rubik/stat/contracts/long-short-position-ratio-contract-top-trader | Get top traders contract long/short ratio (by position) | api/rest/tradingData/topTradersLongShortPositionRatioContract.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/rubik/stat/contracts/open-interest-history | Get contract open interest history | api/rest/tradingData/openInestVolumeHistory.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/rubik/stat/contracts/open-interest-volume | Get contracts open interest and volume | api/rest/tradingData/openInestVolume.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/rubik/stat/margin/loan-ratio | Get margin long/short ratio | api/rest/tradingData/loanRatio.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/rubik/stat/option/open-interest-volume | Get options open interest and volume | api/rest/tradingData/optOpenInestVolume.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/rubik/stat/option/open-interest-volume-expiry | Get open interest and volume (expiry) | api/rest/tradingData/optOpenInterestVolumeExpiry.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/rubik/stat/option/open-interest-volume-ratio | Get put/call ratio | api/rest/tradingData/optOpenInestVolumeRatio.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/rubik/stat/option/open-interest-volume-strike | Get open interest and volume (strike) | api/rest/tradingData/optOpenInterestVolumeStrIke.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/rubik/stat/option/taker-block-volume | Get taker flow | api/rest/tradingData/optTakerBlockVolume.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/rubik/stat/taker-volume | Get taker volume | api/rest/tradingData/takerVolume.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/rubik/stat/taker-volume-contract | Get contract taker volume | api/rest/tradingData/contractTakerVolume.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/rubik/stat/trading-data/support-coin | Get support coin | api/rest/tradingData/supportCoin.md | ✓ | ✗ | ✗ | ✗ | ✗ |
