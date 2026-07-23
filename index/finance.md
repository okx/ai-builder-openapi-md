# finance — OKX V5 API endpoints

Supported per site (✓ = available, ✗ = not on that site). The `doc` column is a REFERENCE path (valid for most sites) — the exact per-site/per-lang path can differ, so read `index.json` `sites.<site>.doc.<lang>` for the authoritative path, then open `docs/<site>/<lang>/<that>`. Frontmatter carries method/path/rest/ws.

| method | path | name | doc (ref) | okex | us | my | turkey |
|---|---|---|---|:-:|:-:|:-:|:-:|
| GET | /api/v5/finance/flexible-loan/borrow-currencies | GET / Borrowable currencies | api/rest/flexibleLoan/getBorrowableCurrencies.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/flexible-loan/collateral-assets | GET / Collateral assets | api/rest/flexibleLoan/getColAssets.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/flexible-loan/interest-accrued | GET / Accrued interest | api/rest/flexibleLoan/getInterestHistory.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/flexible-loan/loan-history | GET / Loan history | api/rest/flexibleLoan/getLoanHistory.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/flexible-loan/loan-info | GET / Loan info | api/rest/flexibleLoan/getLoanInfo.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/flexible-loan/max-collateral-redeem-amount | GET / Maximum collateral redeem amount | api/rest/flexibleLoan/getMaxRedeemAmt.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/okusd/account | GET / Get OKUSD Account | api/rest/okusd/account.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/okusd/limits | GET / OKUSD limits | api/rest/okusd/limits.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/okusd/rate/history | GET / Get APR History | api/rest/okusd/rateHistory.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/okusd/redeem/history | GET / Get Redemption History | api/rest/okusd/redeemHistory.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/okusd/rewards/history | GET / Get Rewards History | api/rest/okusd/rewardsHistory.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/okusd/subscribe/history | GET / Get Subscription History | api/rest/okusd/subscribeHistory.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/savings/balance | GET / Saving balance | api/rest/savings/piggyBankBalance.md | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/finance/savings/lending-history | GET / Lending history | api/rest/savings/lendingHistory.md | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/finance/savings/lending-rate-history | GET / Public borrow history (public) | api/rest/savings/publicBorrowHistory.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/savings/lending-rate-history-priv | GET / Public borrow history (private) | api/rest/savings/publicBorrowHistoryPriv.md | ✗ | ✗ | ✓ | ✗ |
| GET | /api/v5/finance/savings/lending-rate-summary | GET / Public borrow info (public) | api/rest/savings/publicBorrowInfo.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/savings/lending-rate-summary-priv | GET / Public borrow info (private) | api/rest/savings/publicBorrowInfoPriv.md | ✗ | ✗ | ✓ | ✗ |
| GET | /api/v5/finance/sfp/dcd/currency-pair | GET / Currency pairs | api/rest/dcd/currencyPairs.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/sfp/dcd/order-history | GET / Order history | api/rest/dcd/orderHistory.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/sfp/dcd/order-status | GET / Order state | api/rest/dcd/orderState.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/sfp/dcd/products | GET / Product info | api/rest/dcd/productInfo.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/stable-rewards/apy-history | GET / APY history | api/rest/stableRewards/apyHistory.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/stable-rewards/balance | GET / Balance | api/rest/stableRewards/balance.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/stable-rewards/product-info | GET / Product info | api/rest/stableRewards/productInfo.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/staking-defi/eth/apy-history | GET / APY history (Public) | api/rest/eth2/apyHistory.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/staking-defi/eth/balance | GET / Balance | api/rest/eth2/balance.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/staking-defi/eth/product-info | GET / Product info | api/rest/eth2/productInfo.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/staking-defi/eth/purchase-redeem-history | GET / Purchase&Redeem history | api/rest/eth2/P&RHistory.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/staking-defi/offers | GET / Offers | api/rest/earning/offers.md | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/finance/staking-defi/orders-active | GET / Active orders | api/rest/earning/ordersActive.md | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/finance/staking-defi/orders-history | GET / Order history | api/rest/earning/ordersHistory.md | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/finance/staking-defi/sol/apy-history | GET / APY history (Public) | api/rest/okSol/apyHistory.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/staking-defi/sol/balance | GET / Balance | api/rest/okSol/balance.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/staking-defi/sol/product-info | GET / Product info | api/rest/okSol/productInfo.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/finance/staking-defi/sol/purchase-redeem-history | GET / Purchase&Redeem history | api/rest/okSol/P&RHistory.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/finance/flexible-loan/adjust-collateral | POST / Adjust collateral | api/rest/flexibleLoan/adjustCol.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/finance/flexible-loan/max-loan | POST / Maximum loan amount | api/rest/flexibleLoan/getMaxLoan.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/finance/okusd/redeem | POST / Redeem OKUSD | api/rest/okusd/redeem.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/finance/okusd/subscribe | POST / Subscribe OKUSD | api/rest/okusd/subscribe.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/finance/savings/purchase-redempt | POST / Savings purchase/redemption | api/rest/savings/purchaseRedempt.md | ✓ | ✗ | ✓ | ✗ |
| POST | /api/v5/finance/savings/set-lending-rate | POST / Set lending rate | api/rest/savings/setLendingRate.md | ✓ | ✗ | ✓ | ✗ |
| POST | /api/v5/finance/sfp/dcd/quote | POST / Request for quote | api/rest/dcd/quote.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/finance/sfp/dcd/redeem | POST / Redeem | api/rest/dcd/redeem.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/finance/sfp/dcd/redeem-quote | POST / Request for redeem quote | api/rest/dcd/redeemQuote.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/finance/sfp/dcd/trade | POST / Trade | api/rest/dcd/trade.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/finance/staking-defi/cancel | POST / Cancel purchases/redemptions | api/rest/earning/cancel.md | ✓ | ✗ | ✓ | ✗ |
| POST | /api/v5/finance/staking-defi/eth/cancel-redeem | POST / Cancel redeem | api/rest/eth2/cancelRedeem.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/finance/staking-defi/eth/purchase | POST / Purchase | api/rest/eth2/purchase.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/finance/staking-defi/eth/redeem | POST / Redeem | api/rest/eth2/redeem.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/finance/staking-defi/purchase | POST / Purchase | api/rest/earning/purchase.md | ✓ | ✗ | ✓ | ✗ |
| POST | /api/v5/finance/staking-defi/redeem | POST / Redeem | api/rest/earning/redeem.md | ✓ | ✗ | ✓ | ✗ |
| POST | /api/v5/finance/staking-defi/sol/purchase | POST / Purchase | api/rest/okSol/purchase.md | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/finance/staking-defi/sol/redeem | POST / Redeem | api/rest/okSol/redeem.md | ✓ | ✗ | ✗ | ✗ |
