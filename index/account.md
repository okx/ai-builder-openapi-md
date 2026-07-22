# account — OKX V5 API endpoints

Supported per site (✓ = available, ✗ = not on that site). The `doc` column is a REFERENCE path (valid for most sites) — the exact per-site/per-lang path can differ, so read `index.json` `sites.<site>.doc.<lang>` for the authoritative path, then open `docs/<site>/<lang>/<that>`. Frontmatter carries method/path/rest/ws.

| method | path | name | doc (ref) | okex | us | hongkong | my | turkey |
|---|---|---|---|:-:|:-:|:-:|:-:|:-:|
| GET | /api/v5/account/account-position-risk | Get account and position risk | api/rest/account/accountPositionRisk.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/account/adjust-leverage-info | Get leverage estimated info | api/rest/account/adjustLeverageInfo.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/account/balance | Get balance | api/rest/account/balance.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/account/bills | Get bills details (last 7 days) | api/rest/account/bill7.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/account/bills-archive | Get bills details (last 3 months) | api/rest/account/bill90.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/account/bills-history-archive | Get bills details (since 2021) | api/rest/account/getBillSince2021.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/account/collateral-assets | Get collateral assets | api/rest/account/collateralAssets.md | ✓ | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/account/config | Get account configuration | api/rest/account/config.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/account/greeks | Get Greeks | api/rest/account/greeks.md | ✓ | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/account/instruments | Get instruments | api/rest/account/instruments.md | ✓ | ✓ | ✗ | ✓ | ✓ |
| GET | /api/v5/account/interest-accrued | Get interest accrued data | api/rest/account/interestAccrued.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/account/interest-limits | Get borrow interest and limit | api/rest/account/interestAndLimit.md | ✓ | ✗ | ✗ | ✓ | ✗ |
| GET | /api/v5/account/interest-rate | Get interest rate | api/rest/account/interestRate.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/account/interest-rate-loan-quota | Get interest rate and loan quota (private) | api/rest/public/interestRateLoanQuotaPriv.md | ✗ | ✗ | ✗ | ✓ | ✗ |
| GET | /api/v5/account/leverage-info | Get leverage | api/rest/account/leverageInfo.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/account/max-avail-size | Get maximum available balance/equity | api/rest/account/maxAvailSize.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/account/max-loan | Get the maximum loan of instrument | api/rest/account/maxLoan.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/account/max-size | Get maximum order quantity | api/rest/account/maxSize.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/account/max-withdrawal | Get maximum withdrawals | api/rest/account/maxWithdrawal.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/account/mmp-config | GET MMP Config | api/rest/account/mmpConfigInfo.md | ✓ | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/account/move-positions-history | Get move positions history | api/rest/account/movePositionsHistory.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/account/position-tiers | Get PM position limitation | api/rest/account/tier.md | ✓ | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/account/positions | Get positions | api/rest/account/positions.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/account/positions-history | Get positions history | api/rest/account/positionsHistory.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/account/precheck-set-delta-neutral | Precheck set delta neutral | api/rest/account/precheckSetDeltaNeutral.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/account/risk-state | Get account risk state | api/rest/account/riskState.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/account/set-account-switch-precheck | Precheck account mode switch | api/rest/account/preCheckAccountLevel.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| GET | /api/v5/account/spot-borrow-repay-history | Get borrow/repay history | api/rest/spotModeBorrow/spotBorrowHistory.md | ✓ | ✗ | ✗ | ✓ | ✗ |
| GET | /api/v5/account/subaccount/balances | Get sub-account trading balance | api/rest/subAccount/balances.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/account/subaccount/max-withdrawal | Get sub-account maximum withdrawals | api/rest/subAccount/maxWithdrawal.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/account/subtypes | Get bill types | api/rest/account/getBillTypes.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/account/trade-fee | Get fee rates | api/rest/account/tradeFee.md | ✓ | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/account/account-level-switch-preset | Preset account mode switch | api/rest/account/preSetAccountLevel.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/account/activate-option | Activate option | api/rest/account/activateOption.md | ✓ | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/account/bills-history-archive | Apply bills details (since 2021) | api/rest/account/billSince2021.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| POST | /api/v5/account/demo-adjust-balance | Adjust demo account balance | api/rest/account/demoAdjustBalance.md | ✓ | ✓ | ✗ | ✓ | ✓ |
| POST | /api/v5/account/mmp-config | Set MMP | api/rest/account/mmpConfig.md | ✓ | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/account/mmp-reset | Reset MMP Status | api/rest/account/resetMmp.md | ✓ | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/account/move-positions | Move positions | api/rest/account/movePositions.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/account/position-builder | Position builder (new) | api/rest/account/positionBuilderNew.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/account/position-builder-graph | Position builder trend graph | api/rest/account/positionBuilderTrend.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/account/position/margin-balance | Increase/decrease margin | api/rest/account/marginBalance.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| POST | /api/v5/account/set-account-level | Set account mode | api/rest/account/setAccountLevel.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/account/set-auto-earn | Set auto earn | api/rest/account/setAutoEarn.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/account/set-auto-loan | Set auto loan | api/rest/account/setAutoLoan.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/account/set-auto-repay | Set auto repay | api/rest/spotModeBorrow/setAutoRepay.md | ✓ | ✗ | ✗ | ✓ | ✗ |
| POST | /api/v5/account/set-collateral-assets | Set collateral assets | api/rest/account/setCollateralAssets.md | ✓ | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/account/set-fee-type | Set fee type | api/rest/account/setFeeType.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/account/set-greeks | Set greeks (PA/BS) | api/rest/account/setGreeks.md | ✓ | ✓ | ✗ | ✗ | ✗ |
| POST | /api/v5/account/set-isolated-mode | Isolated margin trading settings | api/rest/account/setIsolatedMode.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/account/set-leverage | Set leverage | api/rest/account/lever.md | ✓ | ✗ | ✗ | ✓ | ✗ |
| POST | /api/v5/account/set-position-mode | Set position mode | api/rest/account/posMode.md | ✓ | ✓ | ✗ | ✓ | ✗ |
| POST | /api/v5/account/set-riskOffset-amt | Set risk offset amount | api/rest/account/riskOffsetAmt.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/account/set-settle-currency | Set settle currency | api/rest/account/setSettleCurrency.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/account/set-trading-config | Set trading config | api/rest/account/setTradingConfig.md | ✓ | ✗ | ✗ | ✗ | ✗ |
| POST | /api/v5/account/spot-manual-borrow-repay | Manual borrow / repay | api/rest/spotModeBorrow/manualBorrowAndRepay.md | ✓ | ✗ | ✗ | ✓ | ✗ |
