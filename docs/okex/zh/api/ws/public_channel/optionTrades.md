---
site: okex
lang: zh
name: WS / 期权公共成交频道
type: guide
---

### WS / 期权公共成交频道 [order-book-trading-market-data-ws-option-trades-channel]

获取最近的期权成交数据，有成交数据就推送，每次推送仅包含一条成交数据。

#### URL Path

/ws/v5/public

> 请求示例

```shell
{
    "id": "1512",
    "op": "subscribe",
    "args": [{
        "channel": "option-trades",
        "instType": "OPTION",
        "instFamily": "BTC-USD"
    }]
}
```

```python

import asyncio

from okx.websocket.WsPublicAsync import WsPublicAsync

def callbackFunc(message):
    print(message)

async def main():
    ws = WsPublicAsync(url="wss://wspap.okx.com:8443/ws/v5/public")
    await ws.start()
    args = [{
        "channel": "option-trades",
        "instType": "OPTION",
        "instFamily": "BTC-USD"
    }]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

#### 请求参数

| 参数           | 类型   | 是否必须 | 描述                                                         |
|:-------------| :----- | :------- | :----------------------------------------------------------- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op           | String | 是       | 操作<br>`subscribe`<br>`unsubscribe`          |
| args         | Array of objects  | 是       | 请求订阅的频道列表                                           |
| > channel    | String | 是       | 频道名<br>`option-trades` |
| > instType   | String | 是       | 产品类型，`OPTION`：期权 |
| > instId     | String | 可选   | 产品ID，如 BTC-USD-221230-4000-C，`instId` 和 `instFamily` 必须传一个，若传两个，以 `instId` 为主                                                 |
| > instFamily | String | 可选   | 交易品种，如 BTC-USD                                                                                         |

> 成功返回示例

```json
{
    "id": "1512",
    "event": "subscribe",
    "arg": {
        "channel": "option-trades",
        "instType": "OPTION",
        "instFamily": "BTC-USD"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"option-trades\"}]}",
    "connId": "a4d3ae55"
}
```
#### 返回参数

| 参数      | 类型   | 是否必须 | 描述                                                         |
| :-------- | :----- | :------- | :----------------------------------------------------------- |
| id     | String | 否       | 消息的唯一标识 |
| event     | String | 是       | 事件<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg     | Object | 否       | 订阅的频道                                                   |
| > channel | String | 是       | 频道名                                                       |
| code      | String | 否       | 错误码                                                       |
| msg       | String | 否       | 错误消息                                                     |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例

```json
{
    "arg": {
        "channel": "option-trades",
        "instType": "OPTION",
        "instFamily": "BTC-USD"
    },
    "data": [
        {
            "fillVol": "0.5066007836914062",
            "fwdPx": "16469.69928595038",
            "idxPx": "16537.2",
            "instFamily": "BTC-USD",
            "instId": "BTC-USD-230224-18000-C",
            "markPx": "0.04690107010619562",
            "optType": "C",
            "px": "0.045",
            "side": "sell",
            "sz": "2",
            "tradeId": "38",
            "ts": "1672286551080"
        }
    ]
}
```
#### 推送数据参数

| 参数名          | 类型   | 描述                                                                                                                         |
|:-------------| :----- |----------------------------------------------------------------------------------------------------------------------------|
| arg          | Object | 订阅成功的频道                                                                                                                    |
| > channel    | String | 频道名                                                                                                                        |
| data         | Array of objects  | 订阅的数据                                                                                                                      |
| > instId     | String   | 产品ID                                 |
| > instFamily | String   | 交易品种                                 |
| > tradeId    | String   | 成交ID                                 |
| > px         | String   | 成交价格                                 |
| > sz         | String   | 成交数量，单位为张。                         |
| > side       | String   | 成交方向 <br>`buy`：买  <br>`sell`：卖       |
| > optType    | String   | 期权类型，C：看涨期权 P：看跌期权 ，仅适用于期权           |
| > fillVol    | String   | 成交时的隐含波动率（对应成交价格）                    |
| > fwdPx      | String   | 成交时的远期价格       |
| > idxPx      | String   | 成交时的指数价格       |
| > markPx     | String   | 成交时的标记价格       |
| > ts         | String   | 成交时间，Unix时间戳的毫秒数格式， 如`1597026383085` |

<aside class="notice">
该频道订阅成功后的首条数据可能为最近一笔成交的缓存数据，请忽略。
</aside>
