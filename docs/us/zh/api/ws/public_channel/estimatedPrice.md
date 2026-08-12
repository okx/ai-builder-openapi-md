---
site: us
lang: zh
name: 预估永续/交割/行权/结算价格频道
type: guide
---

### 预估永续/交割/行权/结算价格频道 [public-data-websocket-estimated-delivery-exercise-settlement-price-channel]

在永续/交割/行权/结算前30分钟内，将基于指数价格计算并推送预估价，更新频率约为每 200 毫秒一次。

#### URL Path

/ws/v5/public

> 请求示例

```shell
{
    "id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "estimated-price",
		"instType": "FUTURES",
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
    args = [
        {
          "channel": "estimated-price",
          "instType": "FUTURES",
          "instFamily": "BTC-USD"
        }
    ]
    
    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### 请求参数

| 参数               | 类型              | 是否必须 | 描述                                                                             |
|:-----------------|:----------------| :------- |:-------------------------------------------------------------------------------|
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op               | String          | 是       | 操作<br>`subscribe`<br>`unsubscribe`                                              |
| args             | Array of objects | 是       | 请求订阅的频道列表                                                                      |
| > channel        | String          | 是       | 频道名<br>`estimated-price`                                                          |
| > instType       | String          | 是       | 产品类型<br>`FUTURES`：交割<br>`OPTION`：期权<br>`SWAP`：永续<br>`EVENTS`：事件合约                                      |
| > instFamily     | String          | 可选       | 交易品种<br>`instFamily`和`instId`必须指定一个                                                   |
| > instId         | String          | 可选       | 产品ID<br>`instFamily`和`instId`必须指定一个                                                                           |

> 成功返回示例

```json
{
    "id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "estimated-price",
		"instType": "FUTURES",
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"estimated-price\", \"instId\" : \"FUTURES\",\"instFamily\" :\"BTC-USD\"}]}",
	"connId": "a4d3ae55"
}
```
#### 返回参数

| 参数           | 类型       | 是否必须 | 描述                                                       |
|:-------------|:---------|:-----|:---------------------------------------------------------|
| id     | String | 否       | 消息的唯一标识 |
| event        | String   | 是    | 事件<br>`subscribe`<br>`unsubscribe`<br>`error`            |
| arg          | Object   | 否    | 订阅的频道                                                    |
| > channel    | String   | 是    | 频道名                                                      |
| > instType   | String   | 是    | 产品类型<br>`FUTURES`：交割<br>`OPTION`：期权<br>`SWAP`：永续<br>`EVENTS`：事件合约                      |
| > instFamily | String   | 否    | 交易品种                                                     |
| > instId     | String   | 否    | 产品ID                                                     |
| code         | String   | 否    | 错误码                                                      |
| msg          | String   | 否    | 错误消息                                                     |
| connId       | String   | 是    | WebSocket连接ID                                            |

> 推送示例

```json
{
    "arg": {
        "channel": "estimated-price",
        "instType": "FUTURES",
        "instFamily": "XRP-USDT"
    },
    "data": [{
        "instId": "XRP-USDT-250307",
        "instType": "FUTURES",
        "settlePx": "2.4230631578947368",
        "settleType": "settlement",
        "ts": "1741244598708"
    }]
}
```
#### 推送数据参数

| **参数名**      | **类型**           | **描述**                                                  |
|:-------------|:-----------------|:--------------------------------------------------------|
| arg          | Object           | 订阅成功的频道                                                 |
| > channel    | String           | 频道名                                                     |
| > instType   | String           | 产品类型<br>`FUTURES`：交割<br>`OPTION`：期权<br>`SWAP`：永续<br>`EVENTS`：事件合约                  |
| > instFamily | String           | 交易品种                                                    |
| > instId     | String           | 产品ID                                                    |
| data         | Array of objects | 订阅的数据                                                   |
| > instType   | String           | 产品类型                                                    |
| > instId     | String           | 产品ID，如 `BTC-USD-170310`                                 |
| > settleType | String           | 类型<br>`settlement`：结算<br>`delivery`：交割<br>`exercise`：行权 |
| > settlePx   | String           | 预估价                                                     |
| > ts         | String           | 数据更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                  |
