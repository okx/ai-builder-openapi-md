---
site: okex
lang: en
name: Isolated margin trading settings
method: POST
path: /api/v5/account/set-isolated-mode
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Isolated margin trading settings

You can set the currency margin and futures/perpetual Isolated margin trading mode

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/set-isolated-mode`

> Request Example

```shell
POST /api/v5/account/set-isolated-mode
body
{
    "isoMode":"automatic",
    "type":"MARGIN"
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

# Isolated margin trading settings
result = accountAPI.set_isolated_mode(
    isoMode="automatic",
    type="MARGIN"
)
print(result)
```

#### Request Parameters

| Parameter     | Type   | Required | Description                                                                                                                       |
| :--------- | :----- | :------- |:----------------------------------------------------------------------------------------------------------------------------------|
| isoMode  | String | Yes     | Isolated margin trading settings<br> `auto_transfers_ccy`: New auto transfers, enabling both base and quote currency as the margin for isolated margin trading. Only applicable to `MARGIN`.<br>`automatic`: Auto transfers |
| type     | String | Yes    | Instrument type<br>`MARGIN`<br>`CONTRACTS`                                                                                        |

<aside class="notice">

When there are positions and pending orders in the current account, the margin transfer mode from position to position cannot be adjusted.
</aside>

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "isoMode": "automatic"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter     | Type   | Description                                                                                          |
| :--------- | :----- |:-----------------------------------------------------------------------------------------------------|
| isoMode   | String | Isolated margin trading settings<br>`automatic`: Auto transfers |

<aside class="notice">
CONTRACTS<br>
Auto transfers: Automatically occupy and release the margin when opening and closing positions
</aside>

<aside class="notice">
MARGIN<br>
Auto transfers: Automatically borrow and return coins when opening and closing positions
</aside>
