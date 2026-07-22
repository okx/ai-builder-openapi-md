---
site: hongkong
lang: zh
name: WS / Status 频道
type: guide
---

## WS / Status 频道 [status-ws-status-channel]

获取系统维护的状态，当系统维护状态改变，改期，以及修改结束时间时推送。首次订阅：”推送最新一条的变化数据“；后续每次有状态变化，推送变化的内容。

由计划系统维护引起的短暂不可用（<5秒）和WebSocket闪断连接（用户可以立即重连）将不会公布。此类维护只会在市场波动性低的时期进行。

#### URL Path

/ws/v5/public

> 请求示例

```shell
{
    "id": "1512",
    "op": "subscribe",
    "args": [{
        "channel": "status"
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
        "channel": "status"
    }]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```
#### 请求参数

| 参数          | 类型   | 是否必须 | 描述                                                         |
|:------------| :----- | :------- | :----------------------------------------------------------- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op          | String | 是       | 操作<br>`subscribe`<br>`unsubscribe`          |
| args        | Array of objects  | 是       | 请求订阅的频道列表                                           |
| > channel   | String | 是       | 频道名<br>`status` |

> 成功返回示例

```json
{
    "id": "1512",
    "event": "subscribe",
    "arg": {
        "channel": "status"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"statuss\"}]}",
    "connId": "a4d3ae55"
}
```
#### 返回参数

| 参数      | 类型   | 是否必须 | 描述                                                         |
| :-------- | :----- | :------- | :----------------------------------------------------------- |
| id     | String | 否       | 消息的唯一标识 |
| event     | String | 是       | 事件<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg     | Object | 否       | 订阅的频道                                                   |
| > channel | String | 是       | 频道名<br>`status`                                                       |
| code      | String | 否       | 错误码                                                       |
| msg       | String | 否       | 错误消息                                                     |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例

```json
{
    "arg": {
        "channel": "status"
    },
    "data": [
        {
            "begin": "1672823400000",
            "end": "1672825980000",
            "href": "",
            "preOpenBegin": "",
            "scheDesc": "",
            "serviceType": "0",
            "state": "completed",
            "system": "unified",
            "maintType": "1",
            "env": "1",
            "title": "Trading account WebSocket system upgrade",
            "ts": "1672826038470"
        }
    ]
}
```
#### 推送数据参数

| 参数名            | 类型   | 描述                                                                                                                         |
|:---------------| :----- |----------------------------------------------------------------------------------------------------------------------------|
| arg            | Object | 订阅成功的频道                                                                                                                    |
| > channel      | String | 频道名<br>`status`                                                                                                                        |
| data           | Array of objects  | 订阅的数据                                                                                                                      |
| > title        | String | 系统维护说明的标题                                                                                                                  |
| > state        | String | 系统的状态，`scheduled`:等待中 ; `ongoing`:进行中 ; `pre_open`:预开放；`completed`:已完成 `canceled`:  已取消 <br>当维护时间过长，会存在预开放时间，一般持续10分钟左右。 |
| > begin        | String | 系统维护的开始时间，Unix时间戳的毫秒数格式 如：`1617788463867`                                                                                  |
| > end          | String | 交易全面开放的时间，Unix时间戳的毫秒数格式 如：`1617788463867` <br>在维护完成前，是预期结束时间；维护完成后，会变更为实际结束时间。                                            |
| > preOpenBegin | String | 预开放开始的时间，开放撤单、Post Only 下单和资金转入功能的时间                                                                                       |
| > href         | String | 系统维护详情的超级链接,若无返回值，默认值为空，如：“”                                                                                               |
| > serviceType  | String | 服务类型， `0`：WebSocket ; `5`：交易服务；`6`：大宗交易；`7`：策略交易；`8`：交易服务 (按账户分批次)；`9`：交易服务 (按产品分批次)；`10`：价差交易；`11`：跟单交易；`99`：其他（如：停止部分产品交易）                   |
| > system       | String | 系统，`unified`：交易账户                                                                                                          |
| > scheDesc     | String | 改期进度说明，如： `由 2021-01-26T16:30:00.000Z 改期到 2021-01-28T16:30:00.000Z`                                                        |
| > maintType    | String | 维护类型。<br>`1`：计划维护；`2`：临时维护；`3`：系统故障                                                                                       |
| > env          | String | 环境。<br>`1`：实盘，`2`：模拟盘                                                                                                     |
| > ts           | String | 事件变更的推送时间，Unix时间戳的毫秒数格式，如：`1617788463867`                                                                                  |
