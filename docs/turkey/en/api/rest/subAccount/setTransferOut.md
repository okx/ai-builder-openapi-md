---
site: turkey
lang: en
name: Set permission of transfer out
method: POST
path: /api/v5/users/subaccount/set-transfer-out
module: users
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Set permission of transfer out

Set permission of transfer out for sub-account (only applicable to master account API key). Sub-account can transfer out to master account by default.

#### Rate Limit: 1 request per second
#### Rate limit rule: User ID
#### HTTP Request

`POST /api/v5/users/subaccount/set-transfer-out`

> Request Example

```shell
POST /api/v5/users/subaccount/set-transfer-out
body
{
    "subAcct": "Test001,Test002",
    "canTransOut": true
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

# Set permission of transfer out for sub-account
result = subAccountAPI.set_permission_transfer_out(
    subAcct="hahawang1",
    canTransOut=False
)
print(result)
```

#### Request Parameters

| Parameter  | Type  | Required | Description                                                                                                                                |
| :------ | :----- |:---------|:-------------------------------------------------------------------------------------------------------------------------------------------|
| subAcct  | String | Yes      | Name of the sub-account. Single sub-account or multiple sub-account (no more than 20) separated with comma.                                |
| canTransOut | Boolean | No       | Whether the sub-account has the right to transfer out. The default is `true`.<br>`false`: cannot transfer out <br>`true`: can transfer out |

> Returned result

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "subAcct": "Test001",
            "canTransOut": true
        },
        {
            "subAcct": "Test002",
            "canTransOut": true
        }
    ]
}

```

#### Response parameters

| **Parameter**  | **Type** | **Description**                                                                     |
|:---------------| :------- |:------------------------------------------------------------------------------------|
| subAcct        | String   | Name of the sub-account                                                             |
| canTransOut    | Boolean   | Whether the sub-account has the right to transfer out. <br>`false`: cannot transfer out <br>`true`: can transfer out                  |
