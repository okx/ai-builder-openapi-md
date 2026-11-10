---
site: okex
lang: en
name: Set position mode
method: POST
path: /api/v5/account/set-position-mode
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Set position mode

Futures mode and Multi-currency mode: `FUTURES` and `SWAP` support both `long/short` mode and `net` mode. In `net` mode, users can only have positions in one direction; In `long/short` mode, users can hold positions in long and short directions.<br>
Portfolio margin mode: `FUTURES` and `SWAP` only support `net` mode

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/set-position-mode`

> Request Example

```shell
POST /api/v5/account/set-position-mode
body 
{
    "posMode":"long_short_mode"
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

# Set position mode
result = accountAPI.set_position_mode(
    posMode="long_short_mode"
)
print(result)
```

#### Request Parameters

| Parameter  | Type | Required | Description                                                       |
| :------ | :------- | :------- | :--------------------------------------------------------- |
| posMode | String   | Yes       | Position mode<br>`long_short_mode`: long/short, only applicable to `FUTURES`/`SWAP`<br>`net_mode`: net |

> Response Example

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"posMode": "long_short_mode"
	}]
}
```

#### Response Parameters

| Parameter  | Type   | Description     |
| :------ | :----- | :------- |
| posMode | String | Position mode |

<aside class="notice">
<p>
 Portfolio margin account only supports net mode
</p>
</aside>
