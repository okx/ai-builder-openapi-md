---
site: okex
lang: en
name: Precheck set delta neutral
method: GET
path: /api/v5/account/precheck-set-delta-neutral
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Precheck set delta neutral

#### Rate limit: 1 request per 2 seconds

#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/precheck-set-delta-neutral`

> Request example

```shell
GET /api/v5/account/precheck-set-delta-neutral?stgyType=1

```

#### Request parameters

| **Parameter** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| stgyType | String | Yes | Strategy type<br>`0`: general strategy<br>`1`: delta neutral strategy |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "unmatchedInfoCheck": [
                {
                    "posList": [],
                    "ordList": [],
                    "deltaLever": "",
                    "type": "spot_mode"
                },
               {
                    "posList": ["123","123","123"],
                    "ordList": [],
                    "deltaLever": "",
                    "type": "isolated_margin"
                }
            ]
        }
    ],
    "msg": ""
}

```

#### Response parameters

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| unmatchedInfoCheck | Array of objects | Unmatched information list |
| > type | String | Unmatched information type<br>`spot_mode`: DNA is not supported under spot mode<br>`futures_mode`: DNA is not supported under futures mode<br>`isolated_margin`: Isolated margin position is not supported in DNA<br>`isolated_contract`: Isolated contract position is not supported in DNA<br>`positions_options`: Options are not supported in DNA<br>`isolated_pending_orders`: Isolated pending orders are not supported in DNA<br>`pending_orders_options`: Pending options orders are not supported in DNA<br>`trading_bot`: Trading bot is not supported in DNA<br>`repay_borrowings`: borrowing in the targeted strategy will exceed the main account borrowing limit after the switch. Repay liabilities and try again.<br>`loan`: Flexible loan and DNA cannot be used at the same time <br>`delta_risk`: delta risk check failed, lower delta and try again<br>`collateral_all`: all coins must be set as collateral in DNA<br>`risk_unit_type`: The account is part of a delta neutral risk unit and cannot be switched to general mode. Remove it from the risk unit before switching strategies. |
| > deltaLever | String | Delta leverage<br>Applicable when type is `delta_risk` |
| > ordList | Array of strings | Unmatched order list, order ID<br>Applicable when type is `isolated_pending_orders`/`pending_orders_options` |
| > posList | Array of strings | Unmatched position list, position ID<br>Applicable when type is `isolated_margin`/`isolated_contract`/`positions_options` |
