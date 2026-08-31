# asset — OKX V5 API endpoints

Supported per site (✓ = available, ✗ = not on that site). The `doc` column is a REFERENCE path (valid for most sites) — the exact per-site/per-lang path can differ, so read `index.json` `sites.<site>.doc.<lang>` for the authoritative path, then open `docs/<site>/<lang>/<that>`. Frontmatter carries method/path/rest/ws.

| method | path | name | doc (ref) | okex | us | my | turkey |
|---|---|---|---|:-:|:-:|:-:|:-:|
| GET | /api/v5/asset/asset-valuation | Get account asset valuation | api/rest/fundings/assetValuation.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/asset/balances | Get balance | api/rest/fundings/balances.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/asset/bills | Asset bills details | api/rest/fundings/fundingBill.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/asset/bills-history | Asset bills history | api/rest/fundings/fundingBillHistory.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/asset/convert/currencies | Get convert currencies | api/rest/convert/currencies.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/asset/convert/currency-pair | Get convert currency pair | api/rest/convert/currencyPair.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/asset/convert/history | Get convert history | api/rest/convert/tradeHistory.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/asset/currencies | Get currencies | api/rest/fundings/currencies.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/asset/deposit-address | Get deposit address | api/rest/fundings/depositAddress.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/asset/deposit-history | Get deposit history | api/rest/fundings/depositHistory.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/asset/deposit-withdraw-status | Get deposit withdraw status | api/rest/fundings/depositWithdrawStatus.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/asset/exchange-list | Get exchange list (public) | api/rest/fundings/exchangeList.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/asset/monthly-statement | Get monthly statement (last year) | api/rest/fundings/getMonthlyStatement.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/asset/non-tradable-assets | Get non-tradable assets | api/rest/fundings/nonTradableAssets.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/asset/subaccount/balances | Get sub-account funding balance | api/rest/subAccount/fundingBalances.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/asset/subaccount/bills | Get history of sub-account transfer | api/rest/subAccount/bills.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/asset/subaccount/managed-subaccount-bills | Get history of managed sub-account transfer | api/rest/subAccount/managedSubAcctBills.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/asset/transfer-state | Get funds transfer state | api/rest/fundings/transferState.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/asset/travel-rule/get-locked | Get locked Travel Rule deposits | api/rest/fundings/travelRule.md | ✗ | ✗ | ✗ | ✓ |
| GET | /api/v5/asset/withdrawal-history | Get withdrawal history | api/rest/fundings/withdrawalHistory.md | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/asset/cancel-withdrawal | Cancel withdrawal | api/rest/fundings/cancelWithdrawal.md | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/asset/convert/estimate-quote | Estimate quote | api/rest/convert/quote.md | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/asset/convert/trade | Convert trade | api/rest/convert/trade.md | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/asset/monthly-statement | Apply for monthly statement (last year) | api/rest/fundings/monthlyStatement.md | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/asset/subaccount/transfer | Master accounts manage the transfers between sub-accounts | api/rest/subAccount/transfer.md | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/asset/transfer | Funds transfer | api/rest/fundings/transfer.md | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/asset/travel-rule/unlock | Unlock Travel Rule deposit | api/rest/fundings/travelRule.md | ✗ | ✗ | ✗ | ✓ |
| POST | /api/v5/asset/withdrawal | Withdrawal | api/rest/fundings/withdrawal.md | ✓ | ✓ | ✓ | ✓ |
