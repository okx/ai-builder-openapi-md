---
site: us
lang: zh
name: 爆仓风险预警推送频道
type: guide
---

### 爆仓风险预警推送频道 [trading-account-websocket-position-risk-warning]

此推送频道仅作为风险提示，不建议作为策略交易的风险判断。<br>
在行情剧烈波动的情况下，可能会出现此消息推送的同时仓位已经被强平的可能性。<br>
预警会在某一个逐仓仓位有风险时推送。预警会在所有全仓仓位有风险时推送。<br>
该频道的并发连接受到如下规则限制：[WebSocket 连接限制] (/docs-v5/zh/#overview-websocket-connection-count-limit)

#### 服务地址

/ws/v5/private (需要登录)

> 请求示例

```shell
{
    "id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "liquidation-warning",
		"instType": "ANY"
	}]
}
```

```python
import asyncio

from okx.websocket.WsPrivateAsync import WsPrivateAsync

def callbackFunc(message):
    print(message)

async def main():

    ws = WsPrivateAsync(
        apiKey = "YOUR_API_KEY",
        passphrase = "YOUR_PASSPHRASE",
        secretKey = "YOUR_SECRET_KEY",
        url = "wss://ws.okx.com/ws/v5/private",
        useServerTime=False
    )
    await ws.start()
    args = [
        {
          "channel": "liquidation-warning",
          "instType": "ANY"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### 请求参数

| 参数       | 类型   | 是否必须 | 描述                                                                                                        |
| :--------- | :----- | -------- |:----------------------------------------------------------------------------------------------------------|
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op         | String | 是       | 操作<br>`subscribe`<br>`unsubscribe`                                                                         |
| args     | Array of objects  | 是       | 请求订阅的频道列表                                                                                                 |
| > channel  | String | 是       | 频道名<br>`liquidation-warning`                                                                                 |
| > instType | String | 是       | 产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`ANY `：全部              |
| > instFamily      | String | 否       | 交易品种<br>适用于`交割`/`永续`/`期权`                                                                                     |
| > instId   | String | 否       | 产品ID                                                                                                      |

> 成功返回示例

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "liquidation-warning",
    "instType": "ANY"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"liquidation-warning\", \"instType\" : \"FUTURES\"}]}",
    "connId": "a4d3ae55"
}
```
#### 返回参数

| 参数               | 类型   | 是否必须 | 描述                                                                                                  |
|:-----------------| :----- | -------- |:----------------------------------------------------------------------------------------------------|
| id     | String | 否       | 消息的唯一标识 |
| event            | String | 是       | 事件<br>`subscribe`<br>`unsubscribe`<br>`error`                                                              |
| arg              | Object | 否       | 订阅的频道                                                                                               |
| > channel        | String | 是       | 频道名 ，`liquidation-warning`                                                                          |
| > instType       | String | 是       | 产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权 <br>`ANY`：全部 |
| > instFamily     | String | 否       | 交易品种<br>适用于`交割`/`永续`/`期权`                                                                                            |
| > instId         | String | 否       | 产品ID                                                                                                |
| code             | String | 否       | 错误码                                                                                                 |
| msg              | String | 否       | 错误消息                                                                                                |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例：单个

```json
{
    "arg":{
        "channel":"liquidation-warning",
        "uid": "77982378738415879",
        "instType":"FUTURES"
    },
    "data":[
        {
            "cTime":"1619507758793",
            "ccy":"ETH",
            "instId":"ETH-USD-210430",
            "instType":"FUTURES",
            "lever":"10",
            "markPx":"2353.849",
            "mgnMode":"isolated",
            "mgnRatio":"11.731726509588816",
            "pTime":"1619507761462",
            "pos":"1",
            "posCcy":"",
            "posId":"307173036051017730",
            "posSide":"long",
            "uTime":"1619507761462",
        }
    ]
}
```

#### 推送数据参数

| 参数名                 | 类型 | 描述                                                         |
|:--------------------| :------- | :----------------------------------------------------------- |
| arg                 | Object   | 订阅成功的频道                                               |
| > channel           | String   | 频道名                                                       |
| > uid               | String   | 用户标识                                                  |
| > instType          | String   | 产品类型                                                     |
| > instFamily        | String   | 交易品种                                                  |
| > instId            | String   | 产品ID                                                   |
| data                | Array of objects    | 订阅的数据                                                   |
| > instType          | String | 产品类型 |
| > mgnMode           | String | 保证金模式， `cross`：全仓  `isolated`：逐仓                   |
| > posId             | String | 持仓ID                                                    |
| > posSide           | String | 持仓方向<br>`long`：开平仓模式开多<br>`short`：开平仓模式开空<br>`net`：买卖模式（`交割`/`永续`/`期权`：`pos`为正代表开多，`pos`为负代表开空。`币币杠杆`：`posCcy`为交易货币时，代表开多；`posCcy`为计价货币时，代表开空。） |
| > pos               | String | 持仓数量 |
| > posCcy            | String | 持仓数量币种，仅适用于`币币杠杆` |
| > instId            | String | 产品ID，如 `BTC-USDT-SWAP` |
| > lever             | String | 杠杆倍数，不适用于`期权卖方` |
| > markPx            | String   | 标记价格                                                     |
| > mgnRatio          | String | 维持保证金率 |
| > ccy               | String | 占用保证金的币种 |
| > cTime             | String   | 持仓创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`   |
| > uTime             | String | 最近一次持仓更新时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| > pTime             | String | 持仓信息的推送时间，Unix时间戳的毫秒数格式，如 `1597026383085` |

<aside class="notice"> 
触发推送逻辑：爆仓预警和爆仓短信的触发逻辑一致
</aside>
