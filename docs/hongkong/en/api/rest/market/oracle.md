---
site: hongkong
lang: en
name: Get oracle
method: GET
path: /api/v5/market/open-oracle
module: market
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get oracle

Get the crypto price of signing using Open Oracle smart contract.

#### Rate Limit: 1 request per 5 seconds
#### Rate limit rule: IP
#### HTTP Request

`GET /api/v5/market/open-oracle`

> Request Example

```shell
GET /api/v5/market/open-oracle
```

```python
import okx.MarketData as MarketData

flag = "0"  # Production trading:0 , demo trading:1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# Get the crypto price of signing using Open Oracle smart contract
result = marketDataAPI.get_oracle()
print(result)
```

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "messages":[
                "0x000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000616d3b1400000000000000000000000000000000000000000000000000000000000000c00000000000000000000000000000000000000000000000000000000e70528b800000000000000000000000000000000000000000000000000000000000000006707269636573000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000034254430000000000000000000000000000000000000000000000000000000000"
            ],
            "prices":{
                "BTC":"62014"
            },
            "signatures":[
                "0xf18330e59eaf42373c2c40f1f9e24113ba21d4ed734dd3ed3bc1d12290fa74ba5623fca1113c5d245a1202dc065e333615b90f810f12132ce4a1ecacb8c6b24a000000000000000000000000000000000000000000000000000000000000001b"
            ],
            "timestamp":"1634548500"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                     |
| :--------- | :------- | :--------------------------------------------------- |
| messages       | String   | ABI-encoded values [kind, timestamp, key, value], where kind equals 'prices', timestamp is the time when price was obtained, key is the asset ticker (e.g. btc) and value is the asset price.    |
| prices         | String   | Readable asset prices                                          |
| signatures     | String   | Ethereum-compatible ECDSA signatures for each message         |
| timestamp      | String   | Time of latest datapoint, Unix timestamp, e.g. `1597026387`    |

<aside class="notice">
OKX Oracle public key is 85615b076615317c80f14cbad6501eec031cd51c<br>
</aside>
