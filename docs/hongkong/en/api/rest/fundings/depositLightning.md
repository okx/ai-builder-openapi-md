---
site: hongkong
lang: en
name: Lightning deposits
method: GET
path: /api/v5/asset/deposit-lightning
module: asset
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Lightning deposits

Users can create up to 10,000 different invoices within 24 hours.

#### Rate Limit: 2 requests per second
#### Rate limit rule: User ID

#### HTTP Request
`GET /api/v5/asset/deposit-lightning`

> Request Example

```shell
GET /api/v5/asset/deposit-lightning?ccy=BTC&amt=0.01

```

```python
import okx.Funding as Funding

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading: 0, Demo trading: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# Lightning deposits
result = fundingAPI.get_deposit_lightning(
    ccy="USDT",
    amt="100"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                            |
| ------ | ------ | -------- |----------------------------------------------------------------------------------------|
| ccy  | String | Yes       | Token symbol. Currently only `BTC` is supported.                                       |
| amt  | String | Yes       | Deposit amount between 0.000001 - 0.1                                                  |
| to   | String | No        | Receiving account <br>`6`: Funding account<br>`18`: Trading account<br>Default is `6`. |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "cTime": "1631171307612",
            "invoice": "lnbc100u1psnnvhtpp5yq2x3q5hhrzsuxpwx7ptphwzc4k4wk0j3stp0099968m44cyjg9sdqqcqzpgxqzjcsp5hzzdszuv6yv6yw5svctl8kc8uv6y77szv5kma2kuculj86tk3yys9qyyssqd8urqgcensh9l4zwlwr3lxlcdqrlflvvlwldutm6ljx486h7lylqmd06kky6scas7warx69sregzrx20ffmsr4sp865x3wasrjd8ttgqrlx3tr",
            "verifiedName":"John Corner"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter | Type   | Description                                                         |
| :----- | :----- | :----------------------------------------------------------- |
| invoice       | String | Invoice text |
| cTime         | String | Invoice creation time      |
| verifiedName        | String | Verified name (for recipient)                                                                                                                                    |

<aside class="notice">
This API function service is only open to some users. If you need this function service, please send an email to `pablo.magro@okx.com` to apply
</aside>
