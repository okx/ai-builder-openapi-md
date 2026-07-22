---
site: okex
lang: en
name: Create an API Key for a sub-account
method: POST
path: /api/v5/users/subaccount/apikey
module: users
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Create an API Key for a sub-account

Applies to master accounts only and master accounts API Key must be linked to IP addresses.

#### Rate limit：1 request per second
#### Rate limit rule: User ID
#### HTTP request

`POST /api/v5/users/subaccount/apikey`

> Request sample

```shell
POST /api/v5/users/subaccount/apikey
body
{
    "subAcct":"panpanBroker2",
    "label":"broker3",
    "passphrase": "******",
    "perm":"trade"
}
```

#### Request Parameters

| Parameter  | Type  | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                           |
| :------ | :----- | :------- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| subAcct | String | Yes     | Sub-account name, supports 6 to 20 characters that include numbers and letters (case sensitive, space symbol is not supported).                                                                                                                                                                                                                                                                                       |
| label   | String | Yes      | API Key note                                                                                                                                                                                                                                                                                                                                                                                                          |
| passphrase  | String | Yes      | API Key password, supports 8 to 32 alphanumeric characters containing at least 1 number, 1 uppercase letter, 1 lowercase letter and 1 special character.                                                                                                                                                                                                                                                              |
| perm    | String | No      | API Key permissions <br>`read_only`: Read only <br>`trade`: Trade                                                                                                                                                                                                                                                                                                                                                     |
| ip      | String | No      | Link IP addresses, separate with commas if more than one. Support up to 20 addresses.<br><font color="red"><b>For security reasons, it is recommended to bind IP addresses.</b></font><br><font color="red"><b>API keys with trading or withdrawal permissions that are not bound to IPs will expire after 14 days of inactivity. (API keys in demo trading will not be deleted.)</b></font>                          |

> Returned result

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"subAcct": "test-1",
		"label": "v5",
        "apiKey": "******",
		"secretKey": "******",
        "passphrase": "******",
		"perm": "read_only,trade",
		"ip": "1.1.1.1,2.2.2.2",
		"ts": "1597026383085"
	}]
}
```

#### Response parameters

| **Parameter name** | **Type** | **Description**                                              |
| :--------- | :------- | :---------------------------------------------------- |
| subAcct | String |  Sub-account name   |
| label   | String | API Key note|
| apiKey   | String | API public key|
| secretKey   | String | API private key|
| passphrase  | String | API Key password|
| perm    | String | API Key access <br>`read_only` : Read only  `trade` : Trade |
| ip      | String | IP address that linked with API Key |
| ts      | String | Creation time |
