---
site: okex
lang: en
name: Get custody trading sub-account list
method: GET
path: /api/v5/users/entrust-subaccount-list
module: users
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get custody trading sub-account list

The trading team uses this interface to view the list of sub-accounts currently under escrow

#### Rate limit：1 request per second
#### Rate limit rule: User ID
#### HTTP request

`GET /api/v5/users/entrust-subaccount-list`

> Request sample

```shell
GET /api/v5/users/entrust-subaccount-list

```

```python
import okx.SubAccount as SubAccount

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

subAccountAPI = SubAccount.SubAccountAPI(apikey, secretkey, passphrase, False, flag)

# Get custody trading sub-account list
result = subAccountAPI.get_entrust_subaccount_list()
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                 |
| :------ | :----- | :------- | :----------------------------------------------------- |
| subAcct | String | No    | Sub-account name |

> Returned results

```json
{
    "code":"0",
    "msg":"",
    "data":[
       {
          "subAcct":"test-1"
       },
       {
          "subAcct":"test-2"
       }
    ]
}
```

#### Response parameters

| **Parameter name** | **Type** | **Description**                                              |
| :--------- | :------- | :---------------------------------------------------- |
| subAcct    | String   | Sub-account name                                                |
