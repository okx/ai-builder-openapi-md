---
site: my
lang: en
name: Delete the API Key of sub-accounts
method: POST
path: /api/v5/users/subaccount/delete-apikey
module: users
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### Delete the API Key of sub-accounts

Applies to master accounts only and master accounts API Key must be linked to IP addresses.

#### Rate limit：1 request per second
#### Rate limit rule: User ID
#### HTTP request

`POST /api/v5/users/subaccount/delete-apikey`

> Request sample

```shell
POST /api/v5/users/subaccount/delete-apikey
body
{
    "subAcct":"test00001",
    "apiKey":"******"
}
```

#### Request Parameters

| Parameter  | Type|Required | Description                                                  |
| :------ | :----- | :------- | :----------------------------------------------------- |
| subAcct | String | Yes       | Sub-account name|
| apiKey   | String | Yes       | API public key |

> Returned results

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"subAcct": "test00001"
	}]
}
```

#### Response parameters

| **Parameter name** | **Type** | **Description**                                              |
| :--------- | :------- | :---------------------------------------------------- |
| subAcct | String |  Sub-account name  |
