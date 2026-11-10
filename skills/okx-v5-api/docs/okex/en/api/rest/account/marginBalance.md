---
site: okex
lang: en
name: Increase/decrease margin
method: POST
path: /api/v5/account/position/margin-balance
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Increase/decrease margin

Increase or decrease the margin of the isolated position. Margin reduction may result in the change of the actual leverage.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/position/margin-balance`

> Request Example

```shell
POST /api/v5/account/position/margin-balance 
body
{
    "instId":"BTC-USDT-200626",
    "posSide":"short",
    "type":"add",
    "amt":"1"
}

```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Increase margin
result = accountAPI.adjustment_margin(
    instId="BTC-USDT-SWAP",
    posSide="short",
    type= "add",
    amt="1"
)
print(result)
```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                         |
| :------ | :----- | :------- | :----------------------------------------------------------- |
| instId  | String | Yes       | Instrument ID                                                       |
| posSide | String | Yes       | Position side, the default is `net`<br>`long` <br>`short` <br>`net` |
| type    | String | Yes       | `add`: add margin <br>`reduce`: reduce margin                 |
| amt     | String | Yes       | Amount to be increased or decreased.   |
| ccy     | String | Conditional        | Currency <br>Applicable to `isolated` `MARGIN` orders                                                                                |

> Response Example

```json
{
	"code": "0",
	"msg": "",
	"data": [{
            "amt": "0.3",
            "ccy": "BTC",
            "instId": "BTC-USDT",
            "leverage": "",
            "posSide": "net",
            "type": "add"
        }]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                |
| :--------- | :------- | :-------------------------------------- |
| instId     | String   | Instrument ID                              |
| posSide    | String   | Position side, `long`  `short` |
| amt        | String   | Amount to be increase or decrease |
| type       | String   | `add`: add margin<br>`reduce`: reduce margin                      |
| leverage   | String   | Real leverage after the margin adjustment |
| ccy        | String   | Currency                         |

<aside class="notice">
Manual transfer mode<br>
The value of the margin initially assigned to the isolated position must be greater than or equal to 10,000 USDT, and a position will be created on the account.
</aside>
