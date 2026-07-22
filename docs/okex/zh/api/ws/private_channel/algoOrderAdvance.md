---
site: okex
lang: zh
name: WS / 高级策略委托订单频道
type: guide
---

### WS / 高级策略委托订单频道 [order-book-trading-algo-trading-ws-advance-algo-orders-channel]

获取高级策略委托订单（冰山、时间加权、移动止盈止损），首次订阅推送，当下单、撤单等事件触发时，推送数据

#### 服务地址

/ws/v5/business (需要登录)

> 请求示例：单个

```shell
{
    "id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "algo-advance",
		"instType": "SPOT",
		"instId": "BTC-USDT"
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
        url = "wss://ws.okx.com:8443/ws/v5/business",
        useServerTime=False
    )
    await ws.start()
    args = [
        {
          "channel": "algo-advance",
          "instType": "SPOT",
          "instId": "BTC-USDT"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

> 请求示例

```shell
{
    "id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "algo-advance",
		"instType": "SPOT",
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
        url = "wss://ws.okx.com:8443/ws/v5/business",
        useServerTime=False
    )
    await ws.start()
    
    args = [{
		"channel": "algo-advance",
		"instType": "SPOT",
	}]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### 请求参数

| 参数       | 类型   | 是否必须 | 描述                                                         |
| :--------- | :----- | -------- | :----------------------------------------------------------- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op         | String | 是       | 操作<br>`subscribe`<br>`unsubscribe`          |
| args     | Array of objects  | 是       | 请求订阅的频道列表                                           |
| > channel  | String | 是       | 频道名<br>`algo-advance`                                        |
| > instType | String | 是       | 产品类型<br>`SPOT`：币币<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`ANY`：全部 |
| > instId   | String | 否       | 产品ID                                                       |
| > algoId   | String | 否       | 策略ID                                                       |

> 成功返回示例：单个

```json
{
	"event": "subscribe",
	"arg": {
		"channel": "algo-advance",
		"instType": "SPOT",
		"instId": "BTC-USDT"
	},
    "connId": "a4d3ae55"
}
```

> 成功返回示例

```json
{
	"event": "subscribe",
	"arg": {
		"channel": "algo-advance",
		"instType": "SPOT"
	},
    "connId": "a4d3ae55"
}
```

> 失败返回示例

```json
{
	"event": "error",
	"code": "60012",
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"algo-advance\", \"instType\" : \"FUTURES\"}]}",
    "connId": "a4d3ae55"
}
```
#### 返回参数

| 参数       | 类型   | 是否必须 | 描述                                                         |
| :--------- | :----- | -------- | :----------------------------------------------------------- |
| id     | String | 否       | 消息的唯一标识 |
| event      | String | 是       | 事件<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg      | Object | 否       | 订阅的频道                                                   |
| > channel  | String | 是       | 频道名                                                       |
| > instType | String | 是       | 产品类型<br>`SPOT`：币币<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`ANY`：全部 |
| > instId   | String | 否       | 产品ID                                                       |
| > algoId   | String | 否       | 策略ID                                                       |
| code       | String | 否       | 错误码                                                       |
| msg        | String | 否       | 错误消息                                                     |
| connId   | String   | 是  | WebSocket连接ID |
> 推送示例：单个

```json
{
    "arg":{
        "channel":"algo-advance",
        "uid": "77982378738415879",
        "instType":"SPOT",
        "instId":"BTC-USDT"
    },
    "data":[
        {
            "actualPx":"",
            "actualSide":"",
            "actualSz":"0",
            "algoId":"355056228680335360",
            "cTime":"1630924001545",
            "ccy":"",
            "clOrdId": "",
            "count":"1",
            "instId":"BTC-USDT",
            "instType":"SPOT",
            "lever":"0",
            "notionalUsd":"",
            "ordPx":"",
            "ordType":"iceberg",
            "pTime":"1630924295204",
            "posSide":"net",
            "pxLimit":"10",
            "pxSpread":"1",
            "pxVar":"",
            "side":"buy",
            "slOrdPx":"",
            "slTriggerPx":"",
            "state":"pause",
            "sz":"0.1",
            "szLimit":"0.1",
            "tag": "adadadadad",
            "tdMode":"cash",
            "timeInterval":"",
            "tpOrdPx":"",
            "tpTriggerPx":"",
            "triggerPx":"",
            "triggerTime":"",
            "tradeQuoteCcy": "USDT",
            "callbackRatio":"",
            "callbackSpread":"",
            "activePx":"",
            "moveTriggerPx":"",
          	"failCode": "",
		    "algoClOrdId": "",
            "reduceOnly": "",
            "isTradeBorrowMode": true
        }
    ]
}
```

#### 推送数据参数

| **参数名**            | **类型**   | **描述**                                                                                                                                          |
|:-------------------|:---------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| arg                | Object   | 订阅成功的频道                                                                                                                                         |
| > channel          | String   | 频道名                                                                                                                                             |
| > uid              | String   | 用户标识                                                                                                                                            |
| > instType         | String   | 产品类型                                                                                                                                            |
| > instId           | String   | 产品ID                                                                                                                                            |
| > algoId           | String   | 策略ID                                                                                                                                            |
| data               | Array of objects    | 订阅的数据                                                                                                                                           |
| > instType         | String   | 产品类型                                                                                                                                            |
| > instId           | String   | 产品ID                                                                                                                                            |
| > ccy              | String   | 保证金币种，适用于`逐仓杠杆`及`合约模式`下的`全仓杠杆`订单                                                                                                               |
| > ordId            | String   | 订单ID，与策略委托订单关联的订单ID                                                                                                                             |
| > algoId           | String   | 策略委托单ID                                                                                                                                         |
| > clOrdId          | String   | 客户自定义订单ID                                                                                                                                       |
| > sz               | String   | 委托数量，`币币/币币杠杆` 以币为单位；`交割`/`永续`/`期权` 以张为单位                                                                                                           |
| > side             | String   | 订单方向，`buy`  `sell`                                                                                                                              |
| > posSide          | String   | 持仓方向 <br>`long`：开平仓模式开多<br>`short`：开平仓模式开空<br>`net`：买卖模式                                                                                        |
| > tdMode           | String   | 交易模式<br>保证金模式      `cross`：全仓  `isolated`：逐仓 <br>非保证金模式  `cash`：现金                                                                              |
| > tgtCcy           | String   | 币币市价单委托数量`sz`的单位<br>`base_ccy`: 交易货币 ；`quote_ccy`：计价货币<br>仅适用于`币币`市价订单<br>默认买单为`quote_ccy`，卖单为`base_ccy`                                        |
| > lever            | String   | 杠杆倍数，0.01到125之间的数值，仅适用于 `币币杠杆/交割/永续`                                                                                                            |
| > state            | String   | 订单状态<br>`live`：待生效<br>`effective`：已生效<br>`partially_effective`：部分生效<br>`canceled `：已撤销<br>`order_failed`：委托失败<br>`pause`: 暂停生效                  |
| > tpTriggerPx      | String   | 止盈触发价                                                                                                                                           |
| > tpOrdPx          | String   | 止盈委托价，委托价格为`-1`时，执行市价止盈                                                                                                                         |
| > slTriggerPx      | String   | 止损触发价                                                                                                                                           |
| > slOrdPx          | String   | 止损委托价委托价格为`-1`时，执行市价止损                                                                                                                          |
| > triggerPx        | String   | 计划委托单的触发价格                                                                                                                                      |
| > ordPx            | String   | 计划委托单的委托价格                                                                                                                                      |
| > actualSz         | String   | 实际委托量                                                                                                                                           |
| > actualPx         | String   | 实际委价                                                                                                                                            |
| > tag              | String   | 订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间。                                                                                                 |
| > notionalUsd      | String   | 委托单预估美元价值                                                                                                                                       |
| > actualSide       | String   | 实际触发方向，`sl`：止损  `tp`：止盈                                                                                                                         |
| > triggerTime      | String   | 策略委托触发时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                        |
| > cTime            | String   | 订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                          |
| > pxVar            | String   | 价格比例<br>仅适用于`冰山委托`和`时间加权委托`                                                                                                                     |
| > pxSpread         | String   | 价距<br>仅适用于`冰山委托`和`时间加权委托`                                                                                                                       |
| > szLimit          | String   | 单笔数量<br>仅适用于`冰山委托`和`时间加权委托`                                                                                                                     |
| > pxLimit          | String   | 挂单限制价<br>仅适用于`冰山委托`和`时间加权委托`                                                                                                                    |
| > timeInterval     | String   | 下单间隔<br>仅适用于`时间加权委托`                                                                                                                            |
| > count            | String   | 策略订单计数<br>仅适用于`冰山委托`和`时间加权委托`                                                                                                                   |
| > callbackRatio    | String   | 回调幅度的比例<br>仅适用于`移动止盈止损`                                                                                                                         |
| > callbackSpread   | String   | 回调幅度的价距<br>仅适用于`移动止盈止损`                                                                                                                         |
| > activePx         | String   | 移动止盈止损激活价格<br>仅适用于`移动止盈止损`                                                                                                                      |
| > failCode         | String   | 代表策略触发失败的原因，已撤销和已生效时为""，委托失败时有值，如 51008；<br>仅适用于单向止盈止损委托、双向止盈止损委托、移动止盈止损委托、计划委托。                                                                |
| > algoClOrdId      | String   | 客户自定义策略订单ID                                                                                                                                     |
| > moveTriggerPx    | String   | 移动止盈止损触发价格<br>仅适用于`移动止盈止损`                                                                                                                      |
| > reduceOnly       | String   | 是否只减仓，`true` 或 `false`                                                                                                                          |
| > pTime            | String   | 订单信息的推送时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                       || isTradeBorrowMode            | String            | 是否自动借币<br> true：自动借币<br> false：不自动借币<br>仅适用于计划委托、移动止盈止损和 时间加权策略                                                                                  |
| > isTradeBorrowMode            | Boolean            | 是否自动借币<br> true：自动借币<br> false：不自动借币<br>仅适用于计划委托、移动止盈止损和 时间加权策略                                                                                 |
| > tradeQuoteCcy | String	 | 用于交易的计价币种。 |
