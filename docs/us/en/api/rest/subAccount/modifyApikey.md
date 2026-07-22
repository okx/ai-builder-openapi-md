---
site: us
lang: en
name: Reset the API Key of a sub-account
method: POST
path: /api/v5/users/subaccount/modify-apikey
module: users
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### Reset the API Key of a sub-account

Applies to master accounts only and master accounts API Key must be linked to IP addresses.

#### Rate limit：1 request per second
#### Rate limit rule: User ID
#### HTTP request

`POST /api/v5/users/subaccount/modify-apikey`

> Request sample

```shell
POST /api/v5/users/subaccount/modify-apikey
body
{
    "subAcct":"yongxu",
    "apiKey":"******"
    "ip":"1.1.1.1"
}
```

```python
import okx.SubAccount as SubAccount

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

subAccountAPI = SubAccount.SubAccountAPI(apikey, secretkey, passphrase, False, flag)

# Reset the API Key of a sub-account
result = subAccountAPI.reset_subaccount_apikey(
    subAcct="hahawang1",
    apiKey="",
    ip=""
)
print(result)
```

#### Request Parameters

| Parameter name  | Type   | Required | Description                                                                                                                                                                                                                              |
| :------ | :----- | :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| subAcct | String | Yes       | Sub-account name                                                                                                                                                                                                                         |
| apiKey  | String | Yes      | Sub-account APIKey                                                                                                                                                                                                                       |
| label   | String | No     | Sub-account API Key label. The label will be reset if this is passed through.                                                                                                                                                            |
| perm    | String | No     | Sub-account API Key permissions<br>`read_only`: Read<br>`trade`: Trade<br>Separate with commas if more than one. <br>The permission will be reset if this is passed through.                                                            |
| ip      | String | No      | Sub-account API Key linked IP addresses, separate with commas if more than one. Support up to 20 IP addresses.<br>The IP will be reset if this is passed through.<br>If `ip` is set to "", then no IP addresses is linked to the APIKey. |

> Returned results

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"subAcct": "yongxu",
		"label": "v5",
        "apiKey": "******",
		"perm": "read,trade",
		"ip": "1.1.1.1",
		"ts": "1597026383085"
	}]
}
```

#### Response parameters

| **Parameter name** | **Type** | **Description**                                                        |
| :--------- | :------- |:-----------------------------------------------------------------------|
| subAcct | String | Sub-account name                                                       |
| apiKey   | String | Sub-accountAPI public key                                              |
| label   | String | Sub-account API Key label                                              |
| perm    | String | Sub-account API Key permissions<br>`read_only`: Read<br>`trade`: Trade |
| ip      | String | Sub-account API Key IP addresses that linked with API Key              |
| ts      | String | Creation time                                                          |
