---
site: turkey
lang: en
name: Get locked Travel Rule deposits
method: GET
path: /api/v5/asset/travel-rule/get-locked
module: asset
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
multi_endpoint: true
---

## Endpoint sections

This generated file documents multiple REST endpoints. For a single-endpoint lookup, use this table or `rg -n -F "<search key>" <this-file>` to locate the target section, then read only that section instead of loading the whole file.

| Method | Path | Search key | Section |
| --- | --- | --- | --- |
| GET | `/api/v5/asset/travel-rule/get-locked` | `GET /api/v5/asset/travel-rule/get-locked` | Get locked Travel Rule deposits |
| POST | `/api/v5/asset/travel-rule/unlock` | `POST /api/v5/asset/travel-rule/unlock` | Unlock Travel Rule deposit |

### Get locked Travel Rule deposits

Retrieve the list of deposits currently locked under the Travel Rule. Returns an empty array if the user has no locked deposits. The maximum number of locked deposits is limited to 50.

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID + IP

#### HTTP Request

`GET /api/v5/asset/travel-rule/get-locked`

> Request Example

```shell
GET /api/v5/asset/travel-rule/get-locked
```

```python
import okx.Funding as Funding

apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading: 0, Demo trading: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

result = fundingAPI.get_travel_rule_locked_deposits()
print(result)
```

#### Request Parameters

None. User identity is resolved from API key authentication.

> Response Example

```json
{
  "code": "0",
  "data": [
    {
      "depId": "98765",
      "txId": "0xabc123def456789...",
      "addr": "0xdef456abc789012...",
      "ccy": "USDT",
      "chain": "ERC20",
      "amt": "100.00000000",
      "ts": "1714564800000"
    }
  ],
  "msg": ""
}
```

#### Response Parameters

| Parameter | Type   | Description                                                                    |
| :-------- | :----- | :----------------------------------------------------------------------------- |
| depId     | String | Deposit ID                                                                     |
| txId      | String | On-chain transaction hash                                                      |
| addr      | String | Receiving deposit address                                                      |
| ccy       | String | Currency, e.g. `USDT`. Empty string if currency lookup fails.                  |
| chain     | String | Network/chain name, e.g. `ERC20`. Empty string if chain lookup fails.          |
| amt       | String | Deposit amount                                                                 |
| ts        | String | Data generation time. Unix timestamp in milliseconds, e.g. `1714564800000`    |

---

### Unlock Travel Rule deposit

Submit originator identity information to unlock a deposit locked under the Travel Rule. If the deposit has already been unlocked, returns success without re-processing.

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID + IP

#### HTTP Request

`POST /api/v5/asset/travel-rule/unlock`

> Request Example

```shell
POST /api/v5/asset/travel-rule/unlock
body
{
  "depId": "12345",
  "personType": "2",
  "trReason": "2",
  "walletType": "exchange",
  "exchName": "okx",
  "origFirstName": "John",
  "origLastName": "Doe",
  "origCountry": "TR",
  "origStreetName": "123 Main St",
  "origTownName": "Istanbul",
  "origCountrySubDivision": "Istanbul",
  "origPostCode": "34000"
}
```

```python
import okx.Funding as Funding

apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading: 0, Demo trading: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

result = fundingAPI.unlock_travel_rule_deposit(
    depId="12345",
    personType="2",
    trReason="2",
    walletType="exchange",
    exchName="okx",
    origFirstName="John",
    origLastName="Doe",
    origCountry="TR",
    origStreetName="123 Main St",
    origTownName="Istanbul",
    origCountrySubDivision="Istanbul",
    origPostCode="34000"
)
print(result)
```

#### Request Parameters

| Parameter              | Type   | Required    | Description                                                                                                                                  |
| :--------------------- | :----- | :---------- | :------------------------------------------------------------------------------------------------------------------------------------------- |
| depId                  | String | Yes         | Deposit ID identifying which locked deposit to unlock                                                                                        |
| personType             | String | Yes         | Originator type<br>`1`: self (the depositor is the account holder)<br>`2`: third party                                                       |
| trReason               | String | Yes         | Transfer reason<br>`1`: Private wallet storage<br>`2`: Personal investment<br>`3`: Exchange transfer<br>`4`: Family support                  |
| walletType             | String | Yes         | Source wallet type<br>`exchange`: exchange wallet<br>`private`: private wallet                                                               |
| exchName               | String | Conditional | Source exchange name, e.g. `okx`. Required when `walletType` = `exchange`                                                                    |
| origFirstName          | String | Conditional | Originator first name. Required if `origCompanyName` is not provided                                                                         |
| origLastName           | String | Conditional | Originator last name. Required if `origCompanyName` is not provided                                                                          |
| origCompanyName        | String | Conditional | Originator company name. Required if `origFirstName` and `origLastName` are not provided                                                     |
| origCountry            | String | Yes         | Originator country of residence. ISO 3166-1 alpha-2 code, e.g. `TR`                                                                         |
| origStreetName         | String | Yes         | Originator street address, e.g. `123 Main St`                                                                                               |
| origTownName           | String | Yes         | Town/city where the originator is located                                                                                                    |
| origCountrySubDivision | String | Yes         | State/province of the originator, e.g. `Istanbul`                                                                                           |
| origPostCode           | String | Conditional | Originator postal code                                                                                                                       |

> Response Example

```json
{
  "code": "0",
  "data": [{"result": true}],
  "msg": ""
}
```

#### Response Parameters

| Parameter | Type             | Description                                                                                                                      |
| :-------- | :--------------- | :------------------------------------------------------------------------------------------------------------------------------- |
| result    | Boolean          | `true`: unlock accepted — deposit released or was already unlocked<br>Empty `data` array: unlock failed — deposit remains locked |
