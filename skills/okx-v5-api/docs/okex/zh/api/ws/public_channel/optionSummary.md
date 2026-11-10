---
site: okex
lang: zh
name: 期权定价频道
type: guide
---

### 期权定价频道 [public-data-websocket-option-summary-channel]

获取所有期权合约详细定价信息，一次性推送所有

#### URL Path

/ws/v5/public

> 请求示例

```shell
{
    "id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "opt-summary",
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
    ws = WsPublicAsync(url="wss://wspap.okx.com/ws/v5/public")
    await ws.start()
    args = [
        {
          "channel": "opt-summary",
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

| 参数              | 类型   | 是否必须 | 描述                                |
|:----------------| :----- | :------- |:----------------------------------|
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op              | String | 是       | 操作<br>`subscribe`<br>`unsubscribe` |
| args            | Array of objects  | 是       | 请求订阅的频道列表                         |
| > channel       | String | 是       | 频道名<br>`opt-summary`                 |
| > instFamily    | String | 是       | 交易品种                              |

> 返回示例

```json
{
    "id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "opt-summary",
		"instFamily": "BTC-USD"
	},
	"connId": "a4d3ae55"
}
```

> 失败示例

```json
{
    "id": "1512",
	"event": "error",
	"code": "60012",
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"opt-summary\", \"instFamily\" : \"BTC-USD\"}]}",
	"connId": "a4d3ae55"
}
```
#### 返回参数

| 参数              | 类型   | 是否必须 | 描述                                                         |
|:----------------| :----- | :------- | :----------------------------------------------------------- |
| id     | String | 否       | 消息的唯一标识 |
| event           | String | 是       | 事件<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg             | Object | 否       | 订阅的频道                                                   |
| > channel       | String | 是       | 频道名                                                       |
| > instFamily    | String | 是       | 交易品种                                            |
| code            | String | 否       | 错误码                                                       |
| msg             | String | 否       | 错误消息                                                     |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例

```json
{
    "arg": {
        "channel": "opt-summary",
        "instFamily": "BTC-USD"
    },
    "data": [
        {
            "instType": "OPTION",
            "instId": "BTC-USD-241013-70000-P",
            "uly": "BTC-USD",
            "delta": "-1.1180902625",
            "gamma": "2.2361957091",
            "vega": "0.0000000001",
            "theta": "0.0000032334",
            "lever": "8.465747567",
            "markVol": "0.3675503331",
            "bidVol": "0",
            "askVol": "1.1669998535",
            "realVol": "",
            "deltaBS": "-0.9999672034",
            "gammaBS": "0.0000000002",
            "thetaBS": "28.2649858387",
            "vegaBS": "0.0000114332",
            "ts": "1728703155650",
            "fwdPx": "62604.6993093463",
            "volLv": "0.2044711229"
        }
    ]
}
```

#### 推送数据参数

| **参数名**      | **类型** | **描述**                                                  |
|:-------------| :------- | :-------------------------------------------------------- |
| arg          | Object   | 订阅成功的频道                                            |
| > channel    | String   | 频道名                                                    |
| > instFamily | String   | 交易品种                                                  |
| data         | Array of objects    | 订阅的数据                                                |
| > instType   | String   | 产品类型， `OPTION`                                       |
| > instId     | String   | 产品ID                                                    |
| > uly        | String   | 标的指数                                                  |
| > delta      | String   | 期权价格对`uly`价格的敏感度                                 |
| > gamma      | String   | delta对`uly`价格的敏感度                                    |
| > vega       | String   | 期权价格对隐含波动率的敏感度                              |
| > theta      | String   | 期权价格对剩余期限的敏感度                                |
| > deltaBS    | String   | BS模式下期权价格对`uly`价格的敏感度                                 |
| > gammaBS    | String   | BS模式下delta对`uly`价格的敏感度                                    |
| > vegaBS     | String   | BS模式下期权价格对隐含波动率的敏感度                              |
| > thetaBS    | String   | BS模式下期权价格对剩余期限的敏感度                                |
| > lever      | String   | 杠杆倍数                                                  |
| > markVol    | String   | 标记波动率                                                |
| > bidVol     | String   | bid波动率                                                 |
| > askVol     | String   | ask波动率                                                 |
| > realVol    | String   | 已实现波动率，目前该字段暂未启用                          |
| > volLv      | String | 平价期权的隐含波动率 |
| > fwdPx      | String   | 远期价格                       |
| > ts         | String   | 数据更新时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
