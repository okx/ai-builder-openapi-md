---
site: okex
lang: en
name: GET / APY history (Public)
method: GET
path: /api/v5/finance/staking-defi/eth/apy-history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / APY history (Public)

Public endpoints don't need authorization.

#### Rate Limit: 6 requests per second
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/finance/staking-defi/eth/apy-history`

> Request Example

```shell
GET /api/v5/finance/staking-defi/eth/apy-history?days=2

```

```python
import okx.Finance.EthStaking as EthStaking

flag = "0"  # Production trading:0 , demo trading:1

StackingAPI = EthStaking.EthStakingAPI(flag=flag)

result = StackingAPI.eth_apy_history(days="7")
print(result)
```

#### Request Parameters

| Parameter      | Type   | Required | Description                                                             |
|:---------------|:-------|:---------|:------------------------------------------------------------------------|
| days           | String | Yes      | Get the days of APY(Annual percentage yield) history record in the past<br>No more than 365 days |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "rate": "0.02690000",
            "ts": "1734195600000"
        },
        {
            "rate": "0.02840000",
            "ts": "1734109200000"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter | Type   | Description                                                            |
|-----------|--------|------------------------------------------------------------------------|
| rate      | String | APY(Annual percentage yield), e.g. `0.01` represents `1%`              |
| ts        | String | Data time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
