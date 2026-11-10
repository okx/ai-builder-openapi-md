---
site: okex
lang: en
name: Set auto loan
method: POST
path: /api/v5/account/set-auto-loan
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Set auto loan

Only applicable to `Multi-currency margin` and `Portfolio margin`

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/set-auto-loan`

> Request Example

```shell
POST /api/v5/account/set-auto-loan
body
{
    "autoLoan":true,
}
```

#### Request Parameters

| Parameter | Type   | Required    | Description                                                                                                                                                      |
| :-------- | :----- |:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| autoLoan    | Boolean | No          | Whether to automatically make loans <br>Valid values are `true`, `false` <br>The default is `true`                                                                                                       |

> Response Example

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"autoLoan": true
	}]
}
```

#### Response Parameters

| Parameter | Type   | Description                       |
| :-------- | :----- | :-------------------------------- |
| autoLoan     | Boolean | Whether to automatically make loans                          |
