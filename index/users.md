# users — OKX V5 API endpoints

Supported per site (✓ = available, ✗ = not on that site). The `doc` column is a REFERENCE path (valid for most sites) — the exact per-site/per-lang path can differ, so read `index.json` `sites.<site>.doc.<lang>` for the authoritative path, then open `docs/<site>/<lang>/<that>`. Frontmatter carries method/path/rest/ws.

| method | path | name | doc (ref) | okex | us | my | turkey |
|---|---|---|---|:-:|:-:|:-:|:-:|
| GET | /api/v5/users/entrust-subaccount-list | Get custody trading sub-account list | api/rest/subAccount/getEntrustSubAccountList.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/users/glp/historicalperformance | GET / Get GLP historical performance | api/rest/account/getGlpHistoricalPerformance.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/users/glp/todayperformance | GET / Get GLP today performance | api/rest/account/getGlpTodayPerformance.md | ✓ | ✗ | ✗ | ✗ |
| GET | /api/v5/users/partner/if-rebate | Get the user's affiliate rebate information | api/rest/affiliate/ifRebate.md | ✗ | ✗ | ✗ | ✓ |
| GET | /api/v5/users/subaccount/apikey | Query the API Key of a sub-account | api/rest/subAccount/getApikey.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/users/subaccount/list | Get sub-account list | api/rest/subAccount/subAccountList.md | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/users/subaccount/apikey | Create an API Key for a sub-account | api/rest/subAccount/placeApikey.md | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/users/subaccount/create-subaccount | Create sub-account | api/rest/subAccount/createSubAccount.md | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/users/subaccount/delete-apikey | Delete the API Key of sub-accounts | api/rest/subAccount/deleteApikey.md | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/users/subaccount/modify-apikey | Reset the API Key of a sub-account | api/rest/subAccount/modifyApikey.md | ✓ | ✓ | ✓ | ✓ |
| POST | /api/v5/users/subaccount/set-transfer-out | Set permission of transfer out | api/rest/subAccount/setTransferOut.md | ✓ | ✓ | ✓ | ✓ |
