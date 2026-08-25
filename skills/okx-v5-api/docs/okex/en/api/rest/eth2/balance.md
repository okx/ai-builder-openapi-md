---
site: okex
lang: en
name: GET / Balance
method: GET
path: /api/v5/finance/staking-defi/eth/balance
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Balance

The balance represents the real-time total BETH holdings across the entire account, including assets in the trading account, funding account, and those currently in the redeeming process.

#### Rate Limit: 6 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/staking-defi/eth/balance`

> Request Example

```shell
GET /api/v5/finance/staking-defi/eth/balance
```

```python
import okx.Finance.EthStaking as EthStaking

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading:0 , demo trading:1

StackingAPI = EthStaking.EthStakingAPI(apikey, secretkey, passphrase, False, flag)

result = StackingAPI.eth_balance()
print(result)
```

#### Request Parameters

None

> Response Example

```json
{
    "code": "0",
    "data": [
      {
        "amt": "0.63926191",
        "ccy": "BETH",
        "latestInterestAccrual": "0.00006549",
        "totalInterestAccrual": "0.01490596",
        "ts": "1699257600000"
      }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter             | Type   | Description                                                                                            |
|-----------------------|--------|--------------------------------------------------------------------------------------------------------|
| ccy                   | String | Currency, e.g. `BETH`                                                                                  |
| amt                   | String | Currency amount                                                                                        |
| latestInterestAccrual | String | Latest interest accrual |
| totalInterestAccrual  | String | Total interest accrual                                                                                 |
| ts                    | String | Query data time, Unix timestamp format in milliseconds, e.g. `1597026383085`                           |
