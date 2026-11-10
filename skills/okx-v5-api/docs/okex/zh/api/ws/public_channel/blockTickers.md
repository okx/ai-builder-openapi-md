---
site: okex
lang: zh
name: 大宗交易行情频道
type: guide
---

### 大宗交易行情频道 [block-trading-websocket-public-channel-block-tickers-channel]

获取最近24小时大宗交易量<br>

当发生成交事件时触发推送，此外，也会根据订阅维度每隔5分钟推送一次

#### 服务地址

/ws/v5/business

> 请求示例

```shell
{
    "id": "1512",
	"op": "subscribe",
	"args": [{
        "channel": "block-tickers",
        "instId": "BTC-USDT"
	}]
}
```

```python
import asyncio
from okx.websocket.WsPublicAsync import WsPublicAsync

def callbackFunc(message):
    print(message)

async def main():
    ws = WsPublicAsync(url="wss://wspap.okx.com/ws/v5/business")
    await ws.start()
    args = [{
        "channel": "block-tickers",
        "instId": "BTC-USDT"
	}]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```
#### 请求参数

| 参数          | 类型               | 是否必须 | 描述                             |
|:------------|:-----------------| :------- | :------------------------------- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op          | String           | 是       | 操作<br>`subscribe`<br>`unsubscribe` |
| args        | Array of objects | 是       | 请求订阅的频道列表               |
| > channel   | String           | 是       | 频道名<br>`block-tickers`                |
| > instId    | String           | 是       | 产品ID                           |

> 成功返回示例

```json
{
    "id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "block-tickers",
		"instId": "LTC-USD-200327"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"block-tickers\", \"instId\" : \"LTC-USD-200327\"}]}",
    "connId": "a4d3ae55"
}
```
#### 返回参数

| 参数      | 类型   | 是否必须 | 描述                                      |
| :-------- | :----- | :------- | :---------------------------------------- |
| id     | String | 否       | 消息的唯一标识 |
| event     | String | 是       | 事件<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg     | Object | 否       | 订阅的频道                                |
| > channel | String | 是       | 频道名 `block-tickers`                                   |
| > instId  | String | 是       | 产品ID                                    |
| code      | String | 否       | 错误码                                    |
| msg       | String | 否       | 错误消息                                  |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例

```json
{
    "arg": {
        "channel": "block-tickers"
    },
    "data": [
        {
            "instType": "SWAP",
            "instId": "LTC-USD-SWAP",
            "volCcy24h": "0",
            "vol24h": "0",
            "ts": "1597026383085"
        }
    ]
}
```
#### 推送数据参数

| **参数名** | **类型** | **描述**                                                     |
| ---------- | -------- | ------------------------------------------------------------ |
| arg      | Object   | 订阅成功的频道                                               |
| > channel  | String   | 频道名                                                       |
| > instId   | String   | 产品ID                                                       |
| data     | Array of objects    | 订阅的数据                                                   |
| > instId     | String   | 产品ID                                                       |
| > instType   | String   | 产品类型                                                     |
| > volCcy24h  | String   | 24小时成交量，以`币`为单位<br>如果是`衍生品`合约，数值为交易货币的数量。<br>如果是`币币/币币杠杆`，数值为计价货币的数量。          |
| > vol24h     | String   | 24小时成交量，以`张`为单位<br>如果是`衍生品`合约，数值为合约的张数。<br>如果是`币币/币币杠杆`，数值为交易货币的数量。          |
| > ts         | String   | 数据产生时间，Unix时间戳的毫秒数格式，如 `1597026383085`     |
