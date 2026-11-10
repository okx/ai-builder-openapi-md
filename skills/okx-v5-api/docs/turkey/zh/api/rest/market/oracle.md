---
site: turkey
lang: zh
name: Oracle  上链交易数据
method: GET
path: /api/v5/market/open-oracle
module: market
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### Oracle  上链交易数据 [public-data-rest-api-get-oracle]

获取使用 Open Oracle 智能合约签名后加密资产价格。

#### 限速：1次/5s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/market/open-oracle`

> 请求示例

```shell
GET /api/v5/market/open-oracle
```

```python
import okx.MarketData as MarketData

flag = "0"  # 实盘:0 , 模拟盘：1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# Oracle 上链交易数据
result = marketDataAPI.get_oracle(
)
print(result)
```

> 返回结果

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

#### 返回参数

| **参数名** | **类型** | **描述**                                                                                                      |
| :--------- | :------- |:------------------------------------------------------------------------------------------------------------|
| messages       | String   | 数组包含对[ kind，timestamp，key，value]进行ABI编码的值，其中kind恒等于`prices`，timestamp是获取价格的时间戳，key是加密资产（如 `ETH`），value是资产价格 |
| prices         | String   | 价格                                                                                                          |
| signatures     | String   | 每个消息的以太坊兼容ECDSA签名的数组                                                                                        |
| timestamp      | String   | 获取最新数据点的时间，Unix时间戳,如 `1597026387`                                                                           |

<aside class="notice">
欧易 Oracle公钥是 85615b076615317c80f14cbad6501eec031cd51c
</aside>
