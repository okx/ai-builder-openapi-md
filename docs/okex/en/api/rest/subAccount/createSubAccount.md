---
site: okex
lang: en
name: Create sub-account
method: POST
path: /api/v5/users/subaccount/create-subaccount
module: users
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Create sub-account

Applies to master accounts only and master accounts API Key must be linked to IP addresses.

#### Rate limit：1 request per second
#### Rate limit rule: User ID
#### HTTP request

`POST /api/v5/users/subaccount/create-subaccount`

> Request sample

```shell
POST /api/v5/users/subaccount/create-subaccount
body
{
    "subAcct": "subAccount002",
    "type": "1",
    "label": "123456"
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

| Parameter name | Type   | Required    | Description                                                                                                                                                                                                                                        |
|:---------------| :----- |:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| subAcct        | String | Yes         | Sub-account name                                                                                                                                                                                                                                   |
| type           | String | Yes         | Sub-account type <br>`1`: Standard sub-account <br>`5`: Custody trading sub-account - Copper<br> `12`: Custody trading sub-account - Komainu                                                                                                       |
| label          | String | No          | Sub-account notes. 6-32 letters (case sensitive), numbers or special characters like *.                                                                                                                                                            |
| pwd           | String | Conditional | Sub-account login password, it is required for KYB users only.<br/> Your password must contain:<br/>8 - 32 characters long.<br/>1 lowercase character (a-z).<br/>1 uppercase character (A-Z).<br/>1 number.<br/>1 special character e.g. ! @ # $ % |

> Returned results

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "label": "123456 ",
            "subAcct": "subAccount002",
            "ts": "1744875304520",
            "uid": "698827017768230914"
        }
    ]
}
```

#### Response parameters

| **Parameter name** | **Type** | **Description**   |
|:-------------------| :------- |:------------------|
| subAcct            | String | Sub-account name  |
| label              | String | Sub-account notes |
| uid                | String | Sub-account ID    |
| ts                 | String | Creation time     |
