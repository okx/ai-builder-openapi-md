---
site: okex
lang: zh
name: WebSocket 公共频道
type: guide
---

## WebSocket 公共频道 [block-trading-websocket-public-channel]

### 公共大宗交易频道 [block-trading-websocket-public-channel-public-structure-block-trades-channel]

获取欧易的最新大宗交易信息。同一大宗交易中的所有腿都包含在同一更新中。数据将在大宗交易执行15分钟后被推送。

#### URL Path

/ws/v5/business

> 请求示例

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "public-struc-block-trades"
    }
  ]
}
```

```python
import asyncio
from okx.websocket.WsPublicAsync import WsPublicAsync

def callbackFunc(message):
    print(message)

async def main():
    ws = WsPublicAsync(url="wss://wspap.okx.com:8443/ws/v5/business")
    await ws.start()
    args = [
        {
          "channel": "public-struc-block-trades"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

#### 请求参数

| 参数名 | 类型   | 是否必填 | 描述                                  |
| :-------- | :----- | :------- | -------------------------------------------- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是      | 操作<br>`subscribe`<br>`unsubscribe`<br>|
| args      | Array of objects  | 是      | 请求订阅的频道列表          |
| > channel | String | 是      | 频道名<br>`public-struc-block-trades`   |

> 成功返回示例

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "public-struc-block-trades"
  },
  "connId": "a4d3ae55"
}
```

> 失败返回示例

```json
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"public-struc-block-trades\""}]}",
  "connId": "a4d3ae55"
}
```

#### 返回参数

| 参数名 | 类型   | 是否必填 | 描述                                  |
| :-------- | :----- | :------- | -------------------------------------------- |
| id     | String | 否       | 消息的唯一标识 |
| event     | String | 是       | 操作<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object | 否       | 订阅的频道                           |
| > channel | String | 是       | 频道名<br>`public-struc-block-trades`           |
| code      | String | 否       | 错误码                                  |
| msg       | String | 否       | 错误消息                                |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例

```json
{
    "arg":{
        "channel":"public-struc-block-trades"
    },
    "data":[
        {

            "cTime":"1608267227834",
            "blockTdId":"1802896",
            "groupId":"",
            "legs":[
                {
                    "px":"0.323",
                    "sz":"25.0",
                    "instId":"BTC-USD-20220114-13250-C",
                    "side":"sell",
                    "tradeId":"15102"
                },
                {
                    "px":"0.666",
                    "sz":"25",
                    "instId":"BTC-USD-20220114-21125-C",
                    "side":"buy",
                    "tradeId":"15103"
                }
            ]
        }
    ]
}
```

#### 推送数据参数

| **参数名** | **类型** | **描述**                      |
| -------------- | --------- |-----------------------------|
| arg           | Object   | 订阅成功的频道                     |
| > channel     | String   | 频道名                         |
| data          | Array of objects    | 订阅的数据                       |
| > cTime	      | String	 | 执行创建的时间戳，Unix 时间戳格式，以毫秒为单位。 |
| > blockTdId	  | String   | 大宗交易ID                          |
| > groupId | String | 组合询价单ID<br>只适用于组合询价单，普通询价单返回 "" |
| > legs	      | Array of objects| 组合交易                        |
| >> instId	    | String	 | 产品名Id                       |
| >> px	        | String	 | 成交价格                        |
| >> sz         | String   | 成交数量<br>对于币币交易，成交数量的单位为交易货币<br/>对于交割、永续以及期权，单位为张。 |
| >> side	      | String	 | 询价单方向，从 Taker的视角看       |
| >> tradeId	  | String	 | 最新成交Id                      |

<aside class="notice">
组合询价单介绍<br><br>

1. 新增返回参数 groupId，协助用户将子账户执行映射到组合询价单。仅适用于组合询价单，对普通询价单返回 ""。<br>
2. 该接口返回的交易数据应为父级询价单，而不是子级询价单，与子账户分配无关，blockTdId 及 tradeId 为空
</aside>

<aside class="notice">
blockTdId 与 rfqId 的对应关系<br><br>

对于普通询价单，每个 <code>blockTdId</code> 与一个 <code>rfqId</code> 一一对应。对于组合询价单，一个 <code>rfqId</code> 可能对应多个 <code>blockTdId</code>。<br><br>
本频道不直接返回 <code>rfqId</code>。作为交易对手方（询价方或成交的报价方）的用户，可订阅私有<a href="/docs-v5/zh/#block-trading-websocket-private-channel-structure-block-trades-channel">大宗交易频道</a>，该频道同时包含 <code>rfqId</code> 和 <code>blockTdId</code>，可用于两个频道之间的关联查询。
</aside>
