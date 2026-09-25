---
site: us
lang: en
name: Funds transfer
method: POST
path: /api/v5/asset/transfer
module: asset
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### Funds transfer

Only API keys with `Transfer` privilege can call this endpoint.

This endpoint supports the transfer of funds between your funding account and trading account, and from the master account to sub-accounts.

Sub-account can transfer out to master account by default. Need to call [Set permission of transfer out](/docs-v5/en/#sub-account-rest-api-set-permission-of-transfer-out) to grant privilege first if you want sub-account transferring to another sub-account (sub-accounts need to belong to same master account.)

<aside class="notice">
The success or failure of the request does not necessarily reflect the actual transfer result. Recommend checking the transfer status by calling "Get funds transfer state" to confirm the final result.
</aside>

#### Rate Limit: 2 requests per second
#### Rate limit rule: User ID + Currency

#### HTTP Request

`POST /api/v5/asset/transfer`

> Request Example

```shell
# Transfer 1.5 USDT from funding account to Trading account when current account is master-account
POST /api/v5/asset/transfer
body
{
    "ccy":"USDT",
    "amt":"1.5",
    "from":"6",
    "to":"18"
}

# Transfer 1.5 USDT from funding account to subAccount when current account is master-account
POST /api/v5/asset/transfer
body
{
    "ccy":"USDT",
    "type":"1",
    "amt":"1.5",
    "from":"6",
    "to":"6",
    "subAcct":"mini"
}

# Transfer 1.5 USDT from funding account to subAccount when current account is sub-account
POST /api/v5/asset/transfer
body 
{
    "ccy":"USDT",
    "type":"4",
    "amt":"1.5",
    "from":"6",
    "to":"6",
    "subAcct":"mini"
}
```

```python
import okx.Funding as Funding

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading: 0, Demo trading: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# Funds transfer
result = fundingAPI.funds_transfer(
    ccy="USDT",
    amt="1.5",
    from_="6",
    to="18"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| :-------- | :----- | :------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| type      | String | No       | Transfer type<br>`0`: transfer within account<br>`1`: master account to sub-account (Only applicable to API Key from master account)<br>`2`: sub-account to master account (Only applicable to API Key from master account)<br>`3`: sub-account to master account (Only applicable to APIKey from sub-account)<br>`4`: sub-account to sub-account (Only applicable to APIKey from sub-account, and target account needs to be another sub-account which belongs to same master account. Sub-account directly transfer out permission is disabled by default, set permission please refer to [Set permission of transfer out](/docs-v5/en/#sub-account-rest-api-set-permission-of-transfer-out))<br>The default is `0`.<br>If you want to make transfer between sub-accounts by master account API key, refer to [Master accounts manage the transfers between sub-accounts](/docs-v5/en/#sub-account-rest-api-master-accounts-manage-the-transfers-between-sub-accounts) |
| ccy       | String | Yes      | Transfer currency, e.g. `USDT`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| amt       | String | Yes      | Amount to be transferred                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| from      | String | Yes      | The remitting account<br>`6`: Funding account<br>`18`: Trading account                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| to        | String | Yes      | The beneficiary account<br>`6`: Funding account<br>`18`: Trading account                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| subAcct   | String | Conditional | Name of the sub-account<br>When `type` is `1`/`2`/`4`, this parameter is required.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| loanTrans  | Boolean | No    | Whether or not borrowed coins can be transferred out under `Spot mode`/`Multi-currency margin`/`Portfolio margin`<br>`true`: borrowed coins can be transferred out<br>`false`: borrowed coins cannot be transferred out<br>the default is `false`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| omitPosRisk  | String | No    | Ignore position risk<br>Default is `false`<br>Applicable to `Portfolio margin`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| clientId  | String | No    | Client-supplied ID<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "transId": "754147",
      "ccy": "USDT",
      "clientId": "",
      "from": "6",
      "amt": "0.1",
      "to": "18"
    }
  ]
}
```

#### Response Parameters

> Response Example

| Parameter | Type   | Description             |
| :-------- | :----- | :---------------------- |
| transId   | String | Transfer ID             |
| clientId  | String | Client-supplied ID      |
| ccy       | String | Currency                |
| from      | String | The remitting account   |
| amt       | String | Transfer amount         |
| to        | String | The beneficiary account |
