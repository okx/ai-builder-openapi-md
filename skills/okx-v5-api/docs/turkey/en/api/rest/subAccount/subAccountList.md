---
site: turkey
lang: en
name: Get sub-account list
method: GET
path: /api/v5/users/subaccount/list
module: users
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

## REST API
### Get sub-account list

Applies to master accounts only

#### Rate limit：20 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP request

`GET /api/v5/users/subaccount/list`

> Request sample

```shell
GET /api/v5/users/subaccount/list

```

```python
import okx.SubAccount as SubAccount

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

subAccountAPI = SubAccount.SubAccountAPI(apikey, secretkey, passphrase, False, flag)

# Get sub-account list
result = subAccountAPI.get_subaccount_list()
print(result)
```

#### Request Parameters

| Parameter | Type | Required | Description                                                                                                                                                 |
| :------ | :----- |:---------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| enable  | String | No       | Sub-account status <br>`true`: Normal  `false`: Frozen                                                                                                      |
| subAcct | String | No       | Sub-account name                                                                                                                                            |
| after   | String | No       | Query the data earlier than the requested subaccount creation timestamp, the value should be a Unix timestamp in millisecond format. e.g. `1597026383085`                    |
| before  | String | No       | Query the data newer than the requested subaccount creation timestamp, the value should be a Unix timestamp in millisecond format. e.g. `1597026383085`                       |
| limit   | String | No       | Number of results per request. The maximum is 100. The default is 100.                                                                                      |

> Returned results

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "canTransOut": false,
            "enable": true,
            "frozenFunc": [
            ],
            "gAuth": false,
            "label": "D456DDDLx",
            "mobile": "",
            "subAcct": "D456DDDL",
            "ts": "1659334756000",
            "type": "1",
            "uid": "3400***********7413",
            "subAcctLv": "1",
            "firstLvSubAcct": "D456DDDL",
            "ifDma": false
        }
    ]
}
```

#### Response parameters

| **Parameter name** | **Type**        | **Description**                                                                                                                                                                                                                |
|:-------------------|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| type               | String          | Sub-account type <br>`1`: Standard sub-account <br>`2`: Managed trading sub-account <br>`5`: Custody trading sub-account - Copper<br>`9`: Managed trading sub-account - Copper<br> `12`: Custody trading sub-account - Komainu |
| enable             | Boolean         | Sub-account status<br>`true`: Normal<br>`false`: Frozen (global)                                                                                                                                                               |
| subAcct            | String          | Sub-account name                                                                                                                                                                                                               |
| uid                | String          | Sub-account uid                                                                                                                                                                                                                |
| label              | String          | Sub-account note                                                                                                                                                                                                               |
| mobile             | String          | Mobile number that linked with the sub-account.                                                                                                                                                                                |
| gAuth              | Boolean         | If the sub-account switches on the Google Authenticator for login authentication. <br>`true`: On  `false`: Off                                                                                                                 |
| frozenFunc         | Array of strings | Frozen functions<br>`trading`<br>`convert`<br>`transfer`<br>`withdrawal`<br>`deposit`<br>`flexible_loan`                                                                                                                       |
| canTransOut        | Boolean         | Whether the sub-account has the right to transfer out. <br>`true`: can transfer out <br>`false`: cannot transfer out                                                                                                           |
| ts                 | String          | Sub-account creation time, Unix timestamp in millisecond format. e.g. `1597026383085`                                                                                                                                          |
| subAcctLv      | String  | Sub-account level <br>`1`: First level sub-account<br/>`2`: Second level sub-account.                                                                                                                                          |
| firstLvSubAcct | String  | The first level sub-account. <br>For subAcctLv: 1, firstLvSubAcct is equal to subAcct<br/>For subAcctLv: 2, subAcct belongs to firstLvSubAcct.                                                                                 |
| ifDma          | Boolean | Whether it is dma broker sub-account. <br>`true`: Dma broker sub-account<br/> `false`: It is not dma broker sub-account.                                                                                                       |
