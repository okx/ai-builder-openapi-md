---
site: hongkong
lang: en
name: Master accounts manage the transfers between sub-accounts
method: POST
path: /api/v5/asset/subaccount/transfer
module: asset
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Master accounts manage the transfers between sub-accounts

Applies to master accounts only. 

Only API keys with `Trade` privilege can call this endpoint.

#### Rate limit：1 request per second
#### Rate limit rule: User ID
#### HTTP request

`POST /api/v5/asset/subaccount/transfer`

> Request sample

```shell
POST /api/v5/asset/subaccount/transfer
body
{
    "ccy":"USDT",
    "amt":"1.5",
    "from":"6",
    "to":"6",
    "fromSubAccount":"test-1",
    "toSubAccount":"test-2"
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

# Master accounts manage the transfers between sub-accounts
result = subAccountAPI.subAccount_transfer(
    ccy="USDT",
    amt="10",
    froms="6",
    to="6",
    fromSubAccount="test-1",
    toSubAccount="test-2"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                      |
| :------ | :----- | :------- |:---------------------------------------------------------------------------------------------------------------------------------|
| ccy  | String | Yes      | Currency                                                                                                                         |
| amt | String | Yes       | Transfer amount                                                                                                                  |
| from   | String | Yes       | Account type of transfer from sub-account<br>`6`: Funding Account<br>`18`: Trading account                                         |
| to  | String | Yes       | Account type of transfer to sub-account<br>`6`: Funding Account<br>`18`: Trading account                                           |
| fromSubAccount   | String | Yes       | Sub-account name of the account that transfers funds out.                                                                        |
| toSubAccount   | String | Yes       | Sub-account name of the account that transfers funds in.                                                                         |
| loanTrans  | Boolean | No    | Whether or not borrowed coins can be transferred out under `Multi-currency margin`/`Portfolio margin`<br>The default is `false` |
| omitPosRisk  | String | No    | Ignore position risk<br>Default is `false`<br>Applicable to `Portfolio margin`                                                   |

> Returned results

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "transId":"12345",
        }
    ]
}
```

#### Response parameters

| **Parameter name** | **Type** | **Description**                                              |
| :--------- | :------- | :---------------------------------------------------- |
| transId     | String   | Transfer ID                 |
