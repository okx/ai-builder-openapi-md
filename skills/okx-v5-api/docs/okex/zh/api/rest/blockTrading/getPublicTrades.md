---
site: okex
lang: zh
name: 获取大宗交易公共多腿成交数据
method: GET
path: /api/v5/rfq/public-trades
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取大宗交易公共多腿成交数据 [block-trading-rest-api-get-public-multi-leg-transactions-of-block-trades]

获取已经执行的大宗交易。数据将在大宗交易执行15分钟后更新。

#### 限速: 5次/2s
#### 限速规则：IP
#### HTTP Requests

`GET /api/v5/rfq/public-trades`

> 请求示例

```shell
GET /api/v5/rfq/public-trades
```

```python
import okx.BlockTrading as BlockTrading

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘：1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# 获取大宗交易公共成交数据
result = blockTradingAPI.get_public_trades()
print(result)
```

#### 请求参数
| 参数名  | 类型  | 是否必须    | 描述                                                                                     |
| :------ | :----- | :----- |:---------------------------------------------------------------------------------------|
| beginId |String |否 | 请求的起始大宗交易ID，请求此ID之后（更新的数据）的分页内容，不包括 beginId                                           |
| endId	  |String |否 | 请求的结束大宗交易ID，请求此ID之前（更旧的数据）的分页内容，不包括 endId                                                  |
| limit	  |String |否 | 返回结果的数量，最大为100，默认100条                                                              |

> 返回示例

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "blockTdId": "439161457415012352",
            "groupId": "",
            "legs": [
                {
                    "instId": "BTC-USD-210826",
                    "side": "sell",
                    "sz": "100",
                    "px": "11000",
                    "tradeId": "439161457415012354"
                },
                {
                    "instId": "BTC-USD-SWAP",
                    "side": "sell",
                    "sz": "100",
                    "px": "50",
                    "tradeId": "439161457415012355"
                },
                {
                    "instId": "BTC-USDT",
                    "side": "buy",
                    "sz": "0.1", //for public feed, spot "sz" is in baseccy
                    "px": "10.1",
                    "tradeId": "439161457415012356"
                },
                {
                    "instId": "BTC-USD-210326-60000-C",
                    "side": "buy",
                    "sz": "200",
                    "px": "0.008",
                    "tradeId": "439161457415012357"
                },
                {
                    "instId": "BTC-USD-220930-5000-P",
                    "side": "sell",
                    "sz": "200",
                    "px": "0.008",
                    "tradeId": "439161457415012360"
                },
                {
                    "instId": "BTC-USD-220930-10000-C",
                    "side": "sell",
                    "sz": "200",
                    "px": "0.008",
                    "tradeId": "439161457415012361"
                },
                {
                    "instId": "BTC-USD-220930-10000-P",
                    "side": "sell",
                    "sz": "200",
                    "px": "0.008",
                    "tradeId": "439161457415012362"
                },
                {
                    "instId": "ETH-USD-220624-100100-C",
                    "side": "sell",
                    "sz": "100",
                    "px": "0.008",
                    "tradeId": "439161457415012363"
                }
            ],
            "strategy":"CALL_CALENDAR_SPREAD",
            "cTime": "1650976251241"
        }
    ]
}

```

#### 返回参数

| 参数名          | 类型   | 描述                      |
|:-------------| :----- |:------------------------|
| code	        | String| 结果代码，0 表示成功。                      |
| msg	         | String| 错误信息，如果代码不为 0，则不为空。          |
| data	        | Array of objects| 	包含结果的对象数组.           |
| > strategy   | String | 期权策略, 如 `CALL_CALENDAR_SPREAD` |
| > cTime	     | String| 	成交时间，Unix时间戳的毫秒数格式。     |
| > blockTdId	 | String| 	大宗交易ID                              |
| > groupId | String | 组合询价单ID<br>只适用于组合询价单，普通询价单返回 "" |
| > legs	      | Array of objects| 	组合交易                    |
| >> instId	   | String| 	产品ID                                 |
| >> px	       | String| 	成交价格                                |
| >> sz        | String   | 成交数量<br>对于币币交易，成交数量的单位为交易货币<br/>对于交割、永续以及期权，单位为张。 |
| >> side	     | String| 	询价单方向，从 Taker的视角看              |
| >> tradeId	  | String| 	最新成交ID                              |

<aside class="notice">
组合询价单介绍<br><br>

1. 新增返回参数 groupId，协助用户将子账户执行映射到组合询价单。仅适用于组合询价单，对普通询价单返回 ""。<br>
2. 该接口返回的交易数据应为父级询价单，而不是子级询价单，与子账户分配无关。blockTdId 及 tradeId 为空。<br>
</aside>
