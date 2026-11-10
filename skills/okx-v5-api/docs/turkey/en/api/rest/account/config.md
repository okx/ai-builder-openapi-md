---
site: turkey
lang: en
name: Get account configuration
method: GET
path: /api/v5/account/config
module: account
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get account configuration

Retrieve current account configuration.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/config`

> Request Example

```shell
GET /api/v5/account/config

```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Retrieve current account configuration
result = accountAPI.get_account_config()
print(result)
```

#### Request Parameters

none

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "acctLv": "2",
            "acctStpMode": "cancel_maker",
            "autoLoan": false,
            "ctIsoMode": "automatic",
            "enableSpotBorrow": false,
            "greeksType": "PA",
            "feeType": "0",
            "ip": "",
            "type": "0",
            "kycLv": "3",
            "label": "v5 test",
            "level": "Lv1",
            "levelTmp": "",
            "liquidationGear": "-1",
            "mainUid": "44705892343619584",
            "mgnIsoMode": "automatic",
            "opAuth": "1",
            "perm": "read_only,withdraw,trade",
            "posMode": "long_short_mode",
            "roleType": "0",
            "spotBorrowAutoRepay": false,
            "spotOffsetType": "",
            "spotRoleType": "0",
            "spotTraderInsts": [],
            "stgyType": "0",
            "traderInsts": [],
            "uid": "44705892343619584",
            "settleCcy": "USDC",
            "settleCcyList": ["USD", "USDC", "USDG"]
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter**       | **Type** | **Description**                                                                                                                                                                                                                                                                                                                                           |
|:--------------------|:---------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uid                 | String   | Account ID of current request.                                                                                                                                                                                                                                                                                                                            |
| mainUid             | String   | Main Account ID of current request. <br>The current request account is main account if uid = mainUid.<br>The current request account is sub-account if uid != mainUid.                                                                                                                                                                                    |
| acctLv              | String   | Account mode <br>`1`: Spot mode<br>`2`: Futures mode<br>`3`: Multi-currency margin<br>`4`: Portfolio margin                                                                                                                                                                                                                                               |
| acctStpMode         | String   | Account self-trade prevention mode <br>`cancel_maker` <br>`cancel_taker` <br>`cancel_both` <br>The default value is `cancel_maker`. Users can log in to the webpage through the master account to modify this configuration                                                                                                                                                                    |
| posMode             | String   | Position mode<br>`long_short_mode`: long/short, only applicable to `FUTURES`/`SWAP`<br>`net_mode`: net                                                                                                                                                                                                                                                    |
| autoLoan            | Boolean  | Whether to borrow coins automatically<br>`true`: borrow coins automatically<br>`false`: not borrow coins automatically                                                                                                                                                                                                                                    |
| greeksType          | String   | Current display type of Greeks<br>`PA`: Greeks in coins<br>`BS`: Black-Scholes Greeks in dollars                                                                                                                                                                                                                                                          |
| feeType             | String   | Fee type<br>`0`: fee is charged in the currency you receive from the trade<br>`1`: fee is always charged in the quote currency of the trading pair                                                                                                                                                                                                                                                                                                                    |
| level               | String   | The user level of the current real trading volume on the platform,  e.g `Lv1`, which means regular user level.                                                                                                                                                                                                                                                                            |
| levelTmp            | String   | Temporary experience user level of special users, e.g `Lv1`                                                                                                                                                                                                                                                                                               |
| ctIsoMode           | String   | Contract isolated margin trading settings<br>`automatic`: Auto transfers<br>`autonomy`: Manual transfers                                                                                                                                                                                                                                                  |
| mgnIsoMode          | String   | Margin isolated margin trading settings<br> `auto_transfers_ccy`: New auto transfers, enabling both base and quote currency as the margin for isolated margin trading<br>`automatic`: Auto transfers<br>`quick_margin`: Quick Margin Mode (For new accounts, including subaccounts, some defaults will be `automatic`, and others will be `quick_margin`) |
| spotOffsetType      | String   | <del>Risk offset type<br>`1`: Spot-Derivatives(USDT) to be offsetted<br>`2`: Spot-Derivatives(Coin) to be offsetted<br>`3`: Only derivatives to be offsetted<br>Only applicable to `Portfolio margin`</del><br>(Deprecated)                                                                                                                               |
| stgyType | String | Strategy type<br>`0`: general strategy<br>`1`: delta neutral strategy |
| roleType            | String   | Role type<br>`0`: General user<br>`1`: Leading trader<br>`2`: Copy trader                                                                                                                                                                                                                                                                                 |
| traderInsts         | Array of strings    | Leading trade instruments, only applicable to Leading trader                                                                                                                                                                                                                                                                                              |
| spotRoleType        | String   | SPOT copy trading role type.<br>`0`: General user；`1`: Leading trader；`2`: Copy trader                                                                                                                                                                                                                                                                    |
| spotTraderInsts     | Array of strings    | Spot lead trading instruments, only applicable to lead trader                                                                                                                                                                                                                                                                                             |
| opAuth              | String   | Whether the optional trading was activated<br>`0`: not activate<br>`1`: activated                                                                                                                                                                                                                                                                         |
| kycLv               | String   | Main account KYC level<br>`0`: No verification<br>`1`: level 1 completed<br>`2`: level 2 completed<br>`3`: level 3 completed<br>If the request originates from a subaccount, kycLv is the KYC level of the main account. <br>If the request originates from the main account, kycLv is the KYC level of the current account.                              |
| label               | String   | API key note  of current request API key. No more than 50 letters (case sensitive) or numbers, which can be pure letters or pure numbers.                                                                                                                                                                                                                 |
| ip                  | String   | IP addresses that linked with current API key, separate with commas if more than one, e.g. `117.37.203.58,117.37.203.57`. It is an empty string "" if there is no IP bonded.                                                                                                                                                                              |
| perm                | String   | The permission of the current requesting API key or Access token<br>`read_only`: Read<br>`trade`: Trade<br>`withdraw`: Withdraw                                                                                                                                                                                                                           |
| liquidationGear     | String   | The maintenance margin ratio level of liquidation alert<br/>  `3` and `-1` means that you will get hourly liquidation alerts on app and channel "Position risk warning" when your margin level drops to or below 300%. `-1` is the initial value which has the same effect as `-3` <br/>`0` means that there is not alert                                 |
| enableSpotBorrow    | Boolean  | Whether borrow is allowed or not in `Spot mode`<br>`true`: Enabled<br>`false`: Disabled                                                                                                                                                                                                                                                                   |
| spotBorrowAutoRepay | Boolean  | Whether auto-repay is allowed or not in `Spot mode`<br>`true`: Enabled<br>`false`: Disabled                                                                                                                                                                                                                                                               |
| type               | String   | Account type <br>`0`: Main account <br>`1`: Standard sub-account <br>`2`: Managed trading sub-account <br>`5`: Custody trading sub-account - Copper<br>`9`: Managed trading sub-account - Copper<br> `12`: Custody trading sub-account - Komainu                                                                                                          |
| settleCcy     | String    | Current account's USD-margined contract settle currency |
| settleCcyList     | String    | Current account's USD-margined contract settle currency list, like ["USD", "USDC", "USDG"]. |
