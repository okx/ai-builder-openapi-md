---
site: okex
lang: zh
name: 公共大宗交易单腿交易频道
type: guide
---

### 公共大宗交易单腿交易频道 [block-trading-websocket-public-channel-public-block-trades-channel]

获取欧易的最新大宗交易单腿交易信息。大宗交易中的每条腿都在单独的更新中推送。数据将在大宗交易执行15分钟后被推送。

#### URL Path

/ws/v5/business

> 请求示例

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "public-block-trades",
      "instId": "BTC-USDT-SWAP"
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
          "channel": "public-block-trades",
          "instId": "BTC-USDT-SWAP"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

#### 请求参数

| 参数名    | 类型   | 是否必填 | 描述                                 |
| :-------- | :----- | :------- | ------------------------------------ |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是       | 操作<br>`subscribe`<br>`unsubscribe`<br>   |
| args      | Array of objects  | 是       | 请求订阅的频道列表                   |
| > channel | String | 是       | 频道名<br>`public-block-trades`        |
| > instId  | String | 是       | 产品 ID, 如 `BTC-USDT-SWAP`         |

> 成功返回示例

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "public-block-trades",
    "instId": "BTC-USDT-SWAP"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"args\":[{ \"channel\" : \"public-block-trades\""}]}",
  "connId": "a4d3ae55"
}
```

#### 返回参数

| 参数名    | 类型   | 是否必需 | 描述                                         |
| :-------- | :----- | :------- | -------------------------------------------- |
| id     | String | 否       | 消息的唯一标识 |
| event     | String | 是       | 操作<br>`subscribe`<br>`unsubscribe`<br>`error`      |
| arg       | Object | 否       | 订阅的频道                                   |
| > channel | String | 是       | 频道名<br>`public-block-trades`                |
| > instId  | String | 是       | 产品 ID, 如 `BTC-USDT-SWAP`                |
| code      | String | 否       | 错误码                                       |
| msg       | String | 否       | 错误消息                                     |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例

```json
{
   "arg":{
      "channel":"public-block-trades",
      "instId":"BTC-USD-231020-5000-P"
   },
   "data":[
      {
         "fillVol":"5",
         "fwdPx":"26808.16",
         "groupId":"",
         "idxPx":"27222.5",
         "instId":"BTC-USD-231020-5000-P",
         "markPx":"0.0022406326071111",
         "px":"0.0048",
         "side":"buy",
         "sz":"1",
         "tradeId":"633971452580106242",
         "ts":"1697422572972"
      }
   ]
}
```

#### 推送数据参数

| **参数名**   | **类型** | **描述**                                                                                                          |
|-----------| -------- | ----------------------------------------------------------------------------------------------------------------- |
| arg       | Object   | 订阅成功的频道                                                                                                    |
| > channel | String   | 频道名                                                                                                            |
| > instId  | String   | 产品 ID, 如 `BTC-USDT-SWAP`                                                                                     |
| data      | Array of objects    | 公共大宗交易单腿交易信息                                                                                          |
| > instId  | String   | 产品 ID, 如 `BTC-USDT-SWAP`                                                                                     |
| > tradeId | String   | 交易 ID, 由柜台提供.                                                                                              |
| > px      | String   | 该单腿交易价格.                                                                                                   |
| > sz      | String   | 成交数量<br>对于币币交易，成交数量的单位为交易货币<br/>对于交割、永续以及期权，单位为张。 |
| > side    | String   | 交易方向, buy, sell, 从taker角度看.                                                                               |
| > fillVol | String | 成交时的隐含波动率 <br>仅适用于 `期权` | 
| > fwdPx   | String | 成交时的远期价格 <br>仅适用于 `期权` | 
| > idxPx   | String | 成交时的指数价格  <br>适用于 `交割`, `永续`, `期权` | 
| > markPx  | String | 成交时的标记价格 <br>适用于 `交割`, `永续`, `期权` |  
| > groupId | String | 组合询价单ID<br>只适用于组合询价单，普通询价单返回 "" |
| > ts      | String   | 成交时间, 时间戳格式，以毫秒为单位. 如 1597026383085.                                                           |

<aside class="notice">
组合询价单介绍<br><br>

1. 新增返回参数 groupId，协助用户将子账户执行映射到组合询价单。仅适用于组合询价单，对普通询价单返回 ""。<br>
2. 该接口返回的交易数据应为子级询价单，但拆分为单腿，tradeId 有值。<br>
</aside>
