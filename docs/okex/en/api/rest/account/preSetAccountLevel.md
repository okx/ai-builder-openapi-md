---
site: okex
lang: en
name: Preset account mode switch
method: POST
path: /api/v5/account/account-level-switch-preset
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Preset account mode switch

Pre-set the required information for account mode switching. When switching from `Portfolio margin mode` back to `Futures mode` / `Multi-currency margin mode`, and if there are existing cross-margin contract positions, it is mandatory to pre-set leverage.

If the user does not follow the required settings, they will receive an error message during the pre-check or when setting the account mode.

#### Rate limit: 5 requests per 2 seconds

#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/account-level-switch-preset`

> Request example

```shell
# 1. Futures mode -> Multi-currency margin mode
POST /api/v5/account/account-level-switch-preset
{
    "acctLv": "3"
}

# 2. Multi-currency margin mode -> Futures mode
POST /api/v5/account/account-level-switch-preset
{
    "acctLv": "2"
}

# 3. Portfolio margin mode -> Futures mode/Multi-currency margin mode, the user have cross-margin contract position and lever is required
POST /api/v5/account/account-level-switch-preset
{
    "acctLv": "2",
    "lever": "10"
}

# 4. Portfolio margin mode -> Futures mode/Multi-currency margin mode, the user doesn't have cross-margin contract position and lever is not required
POST /api/v5/account/account-level-switch-preset
{
    "acctLv": "3"
}

```

#### Request parameters

| **Parameter** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| acctLv | String | Yes | Account mode<br> `2`: Futures mode<br> `3`: Multi-currency margin code<br> `4`: Portfolio margin mode |
| lever | String | Optional | Leverage<br> Required when switching from Portfolio margin mode to `Futures mode` or `Multi-currency margin mode`, and the user holds cross-margin positions. |
| riskOffsetType | String | Optional | ~~Risk offset type<br> `1`: Spot-derivatives (USDT) risk offset<br> `2`: Spot-derivatives (Crypto) risk offset<br> `3`: Derivatives only mode<br> `4`: Spot-derivatives (USDC) risk offset<br> Applicable when switching from Futures mode or Multi-currency margin mode to Portfolio margin mode.~~(Deprecated) |

> Response example 1. Futures mode -> Multi-currency margin mode

```json
{
    "acctLv": "3",
    "curAcctLv": "2",
    "lever": "",
    "riskOffsetType": ""
}
```

> Response example 2. Multi-currency margin mode -> Futures mode

```json
{
    "acctLv": "2",
    "curAcctLv": "3",
    "lever": "",
    "riskOffsetType": ""
}
```

> Response example 3. Portfolio margin mode -> Futures mode/Multi-currency margin mode

```json
{
    "acctLv": "2",
    "curAcctLv": "4",
    "lever": "10",
    "riskOffsetType": ""
}
```

> Response example 4. Portfolio margin mode -> Futures mode/Multi-currency margin mode

```json
{
    "acctLv": "3",
    "curAcctLv": "4",
    "lever": "",
    "riskOffsetType": ""
}
```

#### Response parameters

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| curAcctLv | String | Current account mode |
| acctLv | String | Account mode after switch |
| lever | String | The leverage user preset for cross-margin positions |
| riskOffsetType | String | ~~The risk offset type user preset~~(Deprecated) |

<aside class="notice">
<br>
lever: When switching from Portfolio margin mode to Futures mode or Multi-currency margin mode, if the user holds cross-margin positions, this parameter must be provided; otherwise, error code 50014 will occur. The maximum allowable value for this parameter is determined by the smallest maximum leverage based on current position sizes under the target mode. For example, if a user in PM mode holds three cross-margin positions, with maximum allowable leverage of 20x, 50x, and 100x respectively, the maximum leverage it can set is 20x.
</aside>
