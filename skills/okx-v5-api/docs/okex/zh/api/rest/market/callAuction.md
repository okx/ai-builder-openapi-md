---
site: okex
lang: zh
name: GET / 集合竞价信息
method: GET
path: /api/v5/market/call-auction-details
module: market
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 集合竞价信息 [order-book-trading-market-data-get-call-auction-details]

获取集合竞价相关信息

#### 限速：20次/2s
#### 限速规则：IP
#### HTTP请求

`GET /api/v5/market/call-auction-details`

> 请求示例

```shell
GET /api/v5/market/call-auction-details?instId=ONDO-USDC

```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
| --- | --- | --- | --- |
| instId | String | 是 | 产品ID，如 BTC-USDT |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "instId": "ONDO-USDC",
            "unmatchedSz": "9988764",
            "eqPx": "0.6",
            "matchedSz": "44978",
            "state": "continuous_trading",
            "auctionEndTime": "1726542000000",
            "ts": "1726542000007"
        }
    ]
}
```

#### 返回参数

| 参数名 | 类型 | 描述 |
| --- | --- | --- |
| instId | String | 产品ID |
| eqPx | String | 均衡价格 |
| matchedSz | String | 买卖双边的匹配数量，单位为交易货币 |
| unmatchedSz | String | 未匹配数量 |
| auctionEndTime | String | 集合竞价结束时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| state | String | 交易状态<br>`call_auction`：集合竞价<br>`continuous_trading`：连续交易 |
| ts | String | 数据产生时间，Unix时间戳的毫秒数格式，如 `1597026383085` |

<aside class="notice">
在集合竞价期间，用户可以获取均衡价格、匹配数量、未匹配数量和集合竞价结束时间的更新。数据大约每秒更新一次。当集合竞价结束时，该接口将返回实际开盘价、匹配数量和未匹配数量。
<br>
对于从未进入集合竞价的交易产品，该接口也会返回结果，但交易状态字段state始终为`continuous_trading`，其他字段为0或空。
</aside>
