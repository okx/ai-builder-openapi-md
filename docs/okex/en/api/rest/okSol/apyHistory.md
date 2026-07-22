---
site: okex
lang: en
name: GET / APY history (Public)
method: GET
path: /api/v5/finance/staking-defi/sol/apy-history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / APY history (Public)

Public endpoints don't need authorization.

#### Rate Limit: 6 requests per second
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/finance/staking-defi/sol/apy-history`

> Request Example

```shell
GET /api/v5/finance/staking-defi/sol/apy-history?days=2

```

```python
import okx.Finance.SolStaking as SolStaking

flag = "0"  # Production trading:0 , demo trading:1

StackingAPI = SolStaking.SolStakingAPI(flag=flag)

result = StackingAPI.sol_apy_history(days="7")
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
            "rate": "0.11280000",
            "ts": "1734192000000"
        },
        {
            "rate": "0.11270000",
            "ts": "1734105600000"
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
