---
site: hongkong
lang: en
name: Lightning withdrawals
method: POST
path: /api/v5/asset/withdrawal-lightning
module: asset
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Lightning withdrawals

The maximum withdrawal amount is 0.1 BTC per request, and 1 BTC in 24 hours. The minimum withdrawal amount is approximately 0.000001 BTC. Sub-account does not support withdrawal.

<aside class="notice">
This API function service is only open to some users. If you need this function service, please send an email to `pablo.magro@okx.com` to apply
</aside>

#### Rate Limit: 2 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/asset/withdrawal-lightning`

> Request Example

```shell
POST /api/v5/asset/withdrawal-lightning
body
{
    "ccy":"BTC",
    "invoice":"lnbc100u1psnnvhtpp5yq2x3q5hhrzsuxpwx7ptphwzc4k4wk0j3stp0099968m44cyjg9sdqqcqzpgxqzjcsp5hz"
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

# Lightning withdrawals
result = fundingAPI.withdrawal_lightning(
    ccy="BTC",
    invoice="lnbc100u1psnnvhtpp5yq2x3q5hhrzsuxpwx7ptphwzc4k4wk0j3stp0099968m44cyjg9sdqqcqzpgxqzjcsp5hz"
)
print(result)
```

#### Request Parameters

| Parameter                | Type   | Required | Description                                                                                                                                                                                                                                                                             |
|--------------------------| ------ | -------- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy                      | String | Yes       | Token symbol. Currently only `BTC` is supported.                                                                                                                                                                                                                                        |
| invoice                  | String | Yes    | Invoice text                                                                                                                                                                                                                                                                            |
| memo                     | String | No       | Lightning withdrawal memo                                                                                                                                                                                                                                                               |
| rcvrInfo                 | Object | Conditional  | Recipient information<br>For the specific entity users to do on-chain withdrawal/lightning withdrawal, this information is required.                                                                                                                                                    |
| > walletType             | String | Yes          | Wallet Type<br>`exchange`: Withdraw to exchange wallet<br>`private`: Withdraw to private wallet<br>If withdrawal to the exchange wallet, relevant information about the recipient must be provided.<br>Withdrawal to a private wallet does not require providing recipient information. |
| > exchId                 | String | Conditional  | Exchange ID<br>You can query supported exchanges through the endpoint of [Get exchange list (public)](/docs-v5/en/#funding-account-rest-api-get-exchange-list-public)<br>If the exchange is not in the exchange list, fill in '0' in this field. <br>Apply to walletType = `exchange`   |
| > rcvrFirstName          | String | Conditional  | Receiver's first name, e.g. `Bruce`<br>Apply to walletType = `exchange`                                                                                                                                                                                                                 |
| > rcvrLastName           | String | Conditional  | Receiver's last name, e.g. `Wayne`<br>Apply to walletType = `exchange`                                                                                                                                                                                                                  |
| > rcvrCountry            | String | Conditional  | The recipient's country, e.g. `United States`<br>Apply to walletType = `exchange`                                                                                                                                                                                                       |
| > rcvrCountrySubDivision | String | Conditional  | State/Province of the recipient, e.g. `California`<br>Apply to walletType = `exchange`                                                                                                                                                                                                  |
| > rcvrTownName           | String | Conditional  | The town/city where the recipient is located, e.g. `San Jose`<br>Apply to walletType = `exchange`                                                                                                                                                                                       |
| > rcvrStreetName         | String | Conditional  | Recipient's street address, e.g. `Clementi Avenue 1`<br>Apply to walletType = `exchange`                                                                                                                                                                                                |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [{
        "wdId": "121212",
        "cTime": "1597026383085"
    }]
}
```
#### Response Parameters

| Parameter | Type   | Description                                                         |
| :----- | :----- | :----------------------------------------------------------- |
| wdId   | String | Withdrawal ID |
| cTime  | String | Withdrawal ID creation time   |
