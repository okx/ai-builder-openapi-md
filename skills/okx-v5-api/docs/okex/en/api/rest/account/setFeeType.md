---
site: okex
lang: en
name: Set fee type
method: POST
path: /api/v5/account/set-fee-type
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Set fee type

Set the fee type.
<aside class="notice">
fee type selection is only effective for Spot.
</aside>

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/set-fee-type`

> Request Example

```shell
POST /api/v5/account/set-fee-type 
body
{
    "feeType":"0"
}

```

#### Request Parameters

| Parameter     | Type   | Required | Description                                           |
| :--------- | :----- | :------- | :--------------------------------------------- |
| feeType | String | Yes       | Fee type<br>`0`: fee is charged in the currency you receive from the trade (default)<br>`1`: fee is always charged in the quote currency of the trading pair (only effective for Spot) |

> Response Example

```json
{
	"code": "0",
	"msg": "",
	"data": [
		{
			"feeType": "0"
		}
	]
}
```

#### Response Parameters

| Parameter     | Type   | Description                 |
| :--------- | :----- | :------------------- |
| feeType | String | Fee type<br>`0`: fee is charged in the currency you receive from the trade<br>`1`: fee is always charged in the quote currency of the trading pair |
