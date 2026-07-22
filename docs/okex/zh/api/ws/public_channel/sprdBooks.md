---
site: okex
lang: zh
name: WebSocket公共频道
type: guide
---

## WebSocket公共频道 [spread-trading-websocket-public-channel]

- 实盘地址: `wss://ws.okx.com:8443/ws/v5/business`
- 模拟盘地址: `wss://wspap.okx.com:8443/ws/v5/business` 

### 深度频道 [spread-trading-websocket-public-channel-order-book-channel]

获取Spread深度数据。可用频道有：

* `sprd-bbo-tbt`: 首次推1档快照数据，以后定量推送，每10毫秒当1档快照数据有变化推送一次1档数据
* `sprd-books5`: 首次推5档快照数据，以后定量推送，每100毫秒当5档快照数据有变化推送一次5档数据
* `sprd-books-l2-tbt`: 首次推400档快照数据，以后增量推送，每10毫秒推送一次变化的数据
* 单个连接、交易产品维度，深度频道的推送顺序固定为：sprd-bbo-tbt -> sprd-books-l2-tbt -> sprd-books5

#### URL Path

/ws/v5/business

> 请求示例：sprd-books5

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "sprd-books5",
      "sprdId": "BTC-USDT_BTC-USDT-SWAP"
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
          "channel": "sprd-books5",
          "sprdId": "BTC-USDT_BTC-USDT-SWAP"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

> 请求示例：sprd-books-l2-tbt

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "sprd-books-l2-tbt",
      "sprdId": "BTC-USDT_BTC-USDT-SWAP"
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
          "channel": "sprd-books-l2-tbt",
          "sprdId": "BTC-USDT_BTC-USDT-SWAP"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### 请求参数

| 参数        | 类型 | 是否必须 | 描述                                               |
|-----------| --- | --- |--------------------------------------------------|
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是 | 操作<br>`subscribe`<br>`unsubscribe`               |
| args      | Array of objects | 是 | 请求订阅的频道列表<br>`sprd-bbo-tbt`<br>`sprd-books5`<br>`sprd-books-l2-tbt` |
| > channel | String | 是 | 频道名                                              |
| > sprdId  | String | 是 | spread ID                                        |

> 返回示例：sprd-books5
 

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "sprd-books5",
    "sprdId": "BTC-USDT_BTC-USDT-SWAP"
  },
  "connId": "a4d3ae55"
}

```

> 返回示例：sprd-books-l2-tbt
 
```json
{
  "id": "1512",
   "event":"subscribe",
   "arg":{
      "channel":"sprd-books-l2-tbt",
      "sprdId":"BTC-USDT_BTC-USDT-SWAP"
   },
   "connId":"214fdd24"
}
```

> 失败示例
 

```json
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"args\":[{ \"channel\" : \"sprd-books5\", \"sprdId\" : \"BTC-USD_BTC-USD-191227\"}]}",
  "connId": "a4d3ae55"
}

```

#### 返回参数

| 参数        | 类型       | 是否必须 | 描述                                                              |
|-----------|----------|------|-----------------------------------------------------------------|
| id     | String | 否       | 消息的唯一标识 |
| event     | String   | 是    | 事件<br>`subscribe`<br>`unsubscribe`<br>`error`                   |
| arg       | Object   | 否    | 订阅的频道<br>`sprd-bbo-tbt`<br>`sprd-books5`<br>`sprd-books-l2-tbt` |
| > channel | String   | 是    | 频道名                                                             |
| > sprdId  | String   | 是    | spread ID                                                       |
| msg       | String   | 否    | 错误消息                                                            |
| code      | String   | 否    | 错误码                                                             |
| connId    | String   | 是    | WebSocket连接ID                                                   |

> 推送示例：sprd-books5

```json
{
  "arg": {
    "channel": "sprd-books5",
    "sprdId": "BTC-USDT_BTC-USDT-SWAP"
  },
  "data": [
    {
      "asks": [
        ["111.06","55154","2"],
        ["111.07","53276","2"],
        ["111.08","72435","2"],
        ["111.09","70312","2"],
        ["111.1","67272","2"]],
      "bids": [
        ["111.05","57745","2"],
        ["111.04","57109","2"],
        ["111.03","69563","2"],
        ["111.02","71248","2"],
        ["111.01","65090","2"]],
      "ts": "1670324386802",
      "seqId":1724294007352168320
    }
  ]
}

```
> 推送示例：sprd-books-l2-tbt

```json
{
   "arg":{
      "channel":"sprd-books-l2-tbt",
      "sprdId":"BTC-USDT_BTC-USDT-SWAP"
   },
   "action":"snapshot",
   "data":[
      {
         "asks":[
            ["1.9","1.1","3"],
            ["2.5","0.9","1"],
            ["3.2","4.921","1"],
            ["4.8","0.165","1"],
            ["5.2","4.921","1"]
          ......
         ],
         "bids":[
            ["1.8","0.165","1"],
            ["0.6","0.2","2"],
            ["0","23.49","1"],
            ["-0.1","1","1"],
            ["-0.6","1","1"],
            ["-3.9","4.921","1"]
            ......
         ],
         "ts":"1724391380926",
         "checksum":-1285595583,
         "prevSeqId":-1,
         "seqId":1724294007352168320
      }
   ]
}
```

#### 推送数据参数

| 参数名 | 类型 | 描述 |
| --- | --- | --- |
| arg | Object | 订阅成功的频道 |
| > channel | String | 频道名 |
| > sprdId | String | Spread ID |
| action | String | 推送数据动作，增量推送数据还是全量推送数据<br>`snapshot`：全量<br>`update`：增量 |
| data | Array of objects | 订阅的数据 |
| > asks | Array of strings | 卖方深度 |
| > bids | Array of strings | 买方深度 |
| > ts | String | 数据更新时间戳，Unix时间戳的毫秒数格式，如 1597026383085 |
| > checksum | Integer | 检验和 （下方注解）。仅适用 `sprd-books-l2-tbt` |
| > prevSeqId | Integer | 上一个推送的序列号。仅适用 `sprd-books-l2-tbt` |
| > seqId | Integer | 推送的序列号 （下方注解） |

<aside class="notice">
asks和bids值数组举例说明： ["411.8", "10", "4"] <br>
- 411.8为深度价格 <br>
- 10为此价格的数量 （单位为szCcy) <br>
- 4为此价格的订单数量 <br>
</aside>

#### 序列号

`seqId`是交易所行情的一个序号。如果用户通过多个websocket连接同一频道，收到的序列号会是相同的。每个`sprdId`对应一套。用户可以使用在增量推送频道的`prevSeqId`和`seqId`来构建消息序列。这将允许用户检测数据包丢失和消息的排序。正常场景下`seqId`的值大于`prevSeqId`。新消息中的`prevSeqId`与上一条消息的`seqId`匹配。最小序列号值为0，除了快照消息的`prevSeqId`为-1。<br>

异常情况：<br>
1. 如果一段时间内没有深度更新，OKX将发一条消息`'asks': [], 'bids': []`以通知用户连接是正常的。推送的`seqId`跟上一条信息的一样，`prevSeqId`等于`seqId`。
2. 序列号可能由于维护而重置，在这种情况下，用户将收到一条`seqId`小于`prevSeqId`的增量消息。随后的消息将遵循常规的排序规则。

##### 示例
1. 快照推送：`prevSeqId = -1`，`seqId = 10`
2. 增量推送1（正常更新）：`prevSeqId = 10`，`seqId = 15`
3. 增量推送2（无更新）：`prevSeqId = 15`，`seqId = 15`
4. 增量推送3（序列重置）：`prevSeqId = 15`，`seqId = 3`
5. 增量推送4（正常更新）：`prevSeqId = 3`，`seqId = 5`

#### Checksum机制

此机制可以帮助用户校验深度数据的准确性。

##### 深度合并

用户订阅增量推送深度频道成功后，首先获取初始全量深度数据，当获取到增量推送数据后，更新本地全量深度数据。 

  1. 如果有相同价格，则比较数量；数量为0删除此深度，数量有变化则替换此数据。
  2. 如果没有相同价格，则按照价格优劣排序（bid为价格降序，ask为价格升序），将深度信息插入到全量数据中

##### 计算校验和
先用深度合并后前25档bids和asks组成一个字符串（其中ask和bid中的价格和数量以冒号连接），再计算其crc32值（32位有符号整型）。
