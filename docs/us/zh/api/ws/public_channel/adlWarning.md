---
site: us
lang: zh
name: 自动减仓预警频道
type: guide
---

### 自动减仓预警频道 [public-data-websocket-adl-warning-channel]

自动减仓预警。

仅在 `warning` 或 `adl` 状态下推送数据，每1秒推送一次，展示风险保证金余额及相关风险信息。`normal` 状态下不再推送数据。

更多自动减仓细节，请见[自动减仓机制介绍] (https://www.okx.com/cn/help/iv-introduction-to-auto-deleveraging-adl)

#### 服务地址

/ws/v5/public

> 请求示例

```shell
{
   "id": "1512",
    "op": "subscribe",
    "args": [{
        "channel": "adl-warning",
        "instType": "FUTURES",
        "instFamily": "BTC-USDT"
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
        "channel": "adl-warning",
        "instType": "FUTURES",
        "instFamily": "BTC-USDT"
    }]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

#### 请求参数

| 参数           | 类型               | 是否必须 | 描述                                                   |
|--------------|------------------| --- |------------------------------------------------------|
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op           | String           | 是 | 操作<br>`subscribe`<br>`unsubscribe`                   |
| args         | Array of objects | 是 | 请求订阅的频道列表                                            |
| > channel    | String           | 是 | 频道名<br>`adl-warning`                                 |
| > instType   | String           | 是 | 产品类型<br>`FUTURES`：交割合约<br>`SWAP`：永续合约<br>`OPTION`：期权 |
| > instFamily | String           | 否 | 交易品种                                                 |

> 成功返回示例

```json
{
   "id": "1512",
   "event":"subscribe",
   "arg":{
      "channel":"adl-warning",
      "instType":"FUTURES",
      "instFamily":"BTC-USDT"
   },
   "connId":"48d8960a"
}

```

> 失败返回示例

```json
{
   "id": "1512",
   "event":"error",
   "msg":"Illegal request: { \"event\": \"subscribe\", \"arg\": { \"channel\": \"adl-warning\", \"instType\": \"FUTURES\", \"instFamily\": \"BTC-USDT\" } }",
   "code":"60012",
   "connId":"48d8960a"
}

```

#### 返回参数

| 参数           | 类型       | 是否必须 | 描述 |
|--------------|----------|------| --- |
| id     | String | 否       | 消息的唯一标识 |
| event        | String   | 是    | 事件<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg          | Object   | 否    | 订阅的频道 |
| > channel    | String   | 是    | 频道名<br>`adl-warning` |
| > instType   | String   | 是    | 产品类型 |
| > instFamily | String   | 否    | 交易品种 |
| code         | String   | 否    | 错误码 |
| msg          | String   | 否    | 错误消息 |
| connId       | String   | 是    | WebSocket连接ID |

> 推送示例

```json
{
   "arg":{
      "channel":"adl-warning",
      "instType":"FUTURES",
      "instFamily":"BTC-USDT"
   },
   "data":[
      {
         "instType":"FUTURES",
         "instFamily":"BTC-USDT",
         "state":"warning",
         "bal":"280784384.9564228289548144",
         "ccy":"",
         "maxBal":"",
         "maxBalTs":"",
         "adlType":"",
         "adlBal":"",
         "adlRecBal":"",
         "ts":"1700210763001",
         "decRate":"",
         "adlRate":"",
         "adlRecRate":""
      }
   ]
}

```

#### 推送数据参数

| 参数名          | 类型               | 描述                                                                                                                                                                         |
|--------------|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg          | Object           | 请求订阅的频道                                                                                                                                                                    |
| > channel    | String           | 频道名<br>`adl-warning`                                                                                                                                                       |
| > instType   | String           | 产品类型                                                                                                                                                                       |
| > instFamily | String           | 交易品种                                                                                                                                                                       |
| data         | Array of objects | 订阅的数据                                                                                                                                                                      |
| > instType   | String           | 产品类型                                                                                                                                                                       |
| > instFamily | String           | 交易品种                                                                                                                                                                       |
| > state      | String           | 状态 <br>`warning`：预警状态 <br>`adl`：已开启自动减仓                                                                                                                  |
| > bal        | String           | 实时风险保证金余额                                                                                                                                                                  |
| > ccy        | String           | ~~风险保证金余额对应币种~~（已弃用，返回 `""`。将在后续更新中删除）                                                                                                                                |
| > maxBal     | String           | ~~过去八小时内的风险保证金余额最大值 <br>仅在状态为`warning`及`adl`时推送，状态为`normal`时推送空字符串""~~（已弃用，返回 `""`。将在后续更新中删除）                                                                                                         |
| > maxBalTs   | String           | ~~过去八小时内风险保证金余额最大值对应的时间戳，Unix时间戳的毫秒数格式，如 `1597026383085`~~（已弃用，返回 `""`。将在后续更新中删除）                                                                                                                     |
| > adlType    | String           | ~~关于自动减仓的事件<br>`rate_adl_start`：由于风险保证金下降率过高造成的自动减仓开始 <br>`bal_adl_start`：由于风险保证金余额下降过高造成的自动减仓开始 <br>`pos_adl_start`：由于强平单的规模积累到一定程度的自动减仓开始（仅适用于盘前交易市场） <br>`adl_end`：自动减仓结束~~（已弃用，返回 `""`。将在后续更新中删除） |
| > adlBal     | String           | ~~触发自动减仓的风险保证金余额~~（已弃用，返回 `""`。将在后续更新中删除）                                                                                                                                             |
| > adlRecBal  | String           | ~~自动减仓结束的风险保证金余额~~（已弃用，返回 `""`。将在后续更新中删除）                                                                                                                                             |
| > ts         | String           | 数据更新时间，Unix时间戳的毫秒数格式，如 1597026383085                                                                                                                                       |
| > decRate    | String           | ~~风险保证金实时下降率（bal与maxBal相比较） <br>仅在状态为`warning`及`adl`时推送，状态为`normal`时推送空字符串""~~（已弃用）                                                                                        |
| > adlRate    | String           | ~~触发自动减仓的风险保证金下降率~~（已弃用）                                                                                                                                                   |
| > adlRecRate | String           | ~~自动减仓结束的风险保证金下降率~~（已弃用）                                                                                                                                                   |
