---
site: okex
lang: zh
name: WS / 现货网格策略委托订单频道
type: guide
---

### WS / 现货网格策略委托订单频道 [order-book-trading-grid-trading-ws-spot-grid-algo-orders-channel]

支持现货网格策略订单的定时推送和事件推送

#### 服务地址

/ws/v5/business (需要登录)

> 请求示例

```shell
{
  	"id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "grid-orders-spot",
		"instType": "SPOT"
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
        url = "wss://ws.okx.com/ws/v5/business",
        useServerTime=False
    )
    await ws.start()
    args = [{
		"channel": "grid-orders-spot",
		"instType": "SPOT"
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
| > channel  | String | 是       | 频道名<br>`grid-orders-spot`                                        |
| > instType | String | 是       | 产品类型<br>`SPOT`：币币<br>`ANY`：全部 |
| > instId   | String | 否       | 产品ID                                                       |
| > algoId   | String | 否       | 策略ID                                                       |

> 成功返回示例

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "grid-orders-spot",
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"grid-orders-spot\", \"instType\" : \"FUTURES\"}]}",
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
| > instType | String | 是       | 产品类型                                                      |
| > instId   | String | 否       | 产品ID                                                       |
| > algoId   | String | 否       | 策略ID                                                       |
| code       | String | 否       | 错误码                                                       |
| msg        | String | 否       | 错误消息                                                     |
| connId   | String   | 是  | WebSocket连接ID |
> 推送示例

```json
{
	"arg": {
		"channel": "grid-orders-spot",
		"instType": "ANY",
		"uid": "4470****9584"
	},
	"data": [{
        "algoClOrdId": "",
		"algoId": "568028283477164032",
        "activeOrdNum":"10",
		"algoOrdType": "grid",
		"annualizedRate": "0",
		"arbitrageNum": "0",
		"baseSz": "0",
		"cTime": "1681700496249",
		"cancelType": "0",
		"curBaseSz": "0",
		"curQuoteSz": "25",
		"floatProfit": "0",
		"gridNum": "10",
		"gridProfit": "0",
		"instId": "BTC-USDT",
		"instType": "SPOT",
		"investment": "25",
		"maxPx": "5000",
		"minPx": "400",
		"pTime": "1682416738467",
		"perMaxProfitRate": "1.14570215",
		"perMinProfitRate": "0.0991200440528634356837",
		"pnlRatio": "0",
		"profit": "0",
		"quoteSz": "25",
		"rebateTrans": [{
			"rebate": "0",
			"rebateCcy": "BTC"
		}, {
			"rebate": "0",
			"rebateCcy": "USDT"
		}],
		"runPx": "30031.7",
		"runType": "1",
		"triggerParams": [{
			"triggerAction": "start",
			"triggerStrategy": "instant",
			"delaySeconds": "0",
			"triggerType": "auto",
			"triggerTime": ""
		}, {
			"triggerAction": "stop",
			"triggerStrategy": "instant",
			"delaySeconds": "0",
			"stopType": "1",
			"triggerType": "manual",
			"triggerTime": ""
		}],
		"singleAmt": "0.00101214",
		"slTriggerPx": "",
		"state": "running",
		"stopResult": "0",
		"stopType": "2",
		"tag": "",
		"totalAnnualizedRate": "0",
		"totalPnl": "0",
		"tpTriggerPx": "",
		"tradeNum": "0",
		"uTime": "1682406665527",
        "profitSharingRatio": "",
        "copyType": "0",
		"tradeQuoteCcy": "USDT"
	}]
}
```

#### 推送数据参数

| **参数名**               | **类型**          | **描述**                                                                                                                 |
|:----------------------|:----------------|:-----------------------------------------------------------------------------------------------------------------------|
| arg                   | Object          | 订阅成功的频道                                                                                                                |
| > channel             | String          | 频道名                                                                                                                    |
| > instType            | String          | 产品类型                                                                                                                   |
| > uid                 | String          | 用户ID                                                                                                                   |
| data                  | Array of objects           | 订阅的数据                                                                                                                  |
| > algoId              | String          | 策略订单ID                                                                                                                 |
| > algoClOrdId         | String          | 用户自定义策略ID                        |
| > instType            | String          | 产品类型                                                                                                                   |
| > instId              | String          | 产品ID                                                                                                                   |
| > cTime               | String          | 策略订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                               |
| > uTime               | String          | 策略订单更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                               |
| > algoOrdType         | String          | 策略订单类型<br>`grid`：现货网格                                                                                                  |
| > state               | String          | 订单状态<br>`starting`：启动中<br>`running`：运行中<br>`stopping`：终止中<br>`stopped`：已停止                                             |
| > rebateTrans         | Array of objects | 返佣划转信息                                                                                                                 |
| >> rebate             | String          | 返佣数量                                                                                                                   |
| >> rebateCcy          | String          | 返佣币种                                                                                                                   |
| > triggerParams       | Array of objects | 信号触发参数                                                                                                                 |
| >> triggerAction      | String          | 触发行为<br>`start`：网格启动<br>`stop`：网格停止                                                                                    |
| >> triggerStrategy    | String          | 触发策略<br>`instant`：立即触发<br>`price`：价格触发<br>`rsi`：rsi指标触发                                                                |
| >> delaySeconds       | String          | 延迟触发时间，单位为秒                                                                                                            |
| >> triggerTime        | String          | triggerAction实际触发时间，Unix时间戳的毫秒数格式, 如 `1597026383085`                                                                   |
| >> triggerType        | String          | triggerAction的实际触发类型<br>`manual`：手动触发<br>`auto`: 自动触发                                                                  |
| >> timeframe          | String          | K线种类<br>`3M`, `5M`, `15M`, `30M` (`M`代表分钟)<br>`1H`, `4H` (`H`代表小时)<br>`1D` (`D`代表天)<br>该字段只在`triggerStrategy`为`rsi`时有效 |
| >> thold              | String          | 阈值<br>取值[1,100]的整数<br>该字段只在`triggerStrategy`为`rsi`时有效                                                                  |
| >> triggerCond        | String          | 触发条件<br>`cross_up`：上穿<br>`cross_down`：下穿<br>`above`：上方<br>`below`：下方<br>`cross`：交叉<br>该字段只在`triggerStrategy`为`rsi`时有效  |
| >> timePeriod         | String          | 周期<br>`14`<br>该字段只在`triggerStrategy`为`rsi`下有效                                                                          |
| >> triggerPx          | String          | 触发价格<br>该字段只在`triggerStrategy`为`price`下有效                                                                              |
| >> stopType           | String          | 策略停止类型<br>现货 `1`：卖出交易币，`2`：不卖出交易币<br>合约网格 `1`：停止平仓，`2`：停止不平仓 <br>该字段只在`triggerAction`为`stop`时有效                   |
| > maxPx               | String          | 区间最高价格                                                                                                                 |
| > minPx               | String          | 区间最低价格                                                                                                                 |
| > gridNum             | String          | 网格数量                                                                                                                   |
| > runType             | String          | 网格类型<br>`1`：等差，`2`：等比                                                                                                  |
| > tpTriggerPx         | String          | 止盈触发价                                                                                                                  |
| > slTriggerPx         | String          | 止损触发价                                                                                                                  |
| > tradeNum            | String          | 挂单成交次数                                                                                                                 |
| > arbitrageNum        | String          | 网格套利次数                                                                                                                 |
| > singleAmt           | String          | 单网格买卖量                                                                                                                 |
| > perMinProfitRate    | String          | 预期单网格最低利润率                                                                                                             |
| > perMaxProfitRate    | String          | 预期单网格最高利润率                                                                                                             |
| > runPx               | String          | 启动时价格                                                                                                                  |
| > totalPnl            | String          | 总收益                                                                                                                    |
| > pnlRatio            | String          | 收益率                                                                                                                    |
| > investment          | String          | 投入金额<br>现货网格如果投入了交易币则折算为计价币                                                                                            |
| > gridProfit          | String          | 网格利润                                                                                                                   |
| > floatProfit         | String          | 浮动盈亏                                                                                                                   |
| > totalAnnualizedRate | String          | 总年化                                                                                                                    |
| > annualizedRate      | String          | 网格年化                                                                                                                   |
| > cancelType          | String          | 网格策略停止原因<br>`0`：无<br>`1`：手动停止<br>`2`：止盈停止<br>`3`：止损停止<br>`4`：风控停止<br>`5`：交割停止<br>`6`: 信号停止                                          |
| > stopType            | String          | 网格策略停止类型<br>现货网格 `1`：卖出交易币，`2`：不卖出交易币<br>合约网格 `1`：市价全平，`2`：停止不平仓                                                       |
| > quoteSz             | String          | 计价币投入数量<br>仅适用于`现货网格`                                                                                                  |
| > baseSz              | String          | 交易币投入数量<br>仅适用于`现货网格`                                                                                                  |
| > curQuoteSz          | String          | 当前持有的计价币资产<br>仅适用于`现货网格`                                                                                               |
| > curBaseSz           | String          | 当前持有的交易币资产<br>仅适用于`现货网格`                                                                                               |
| > profit              | String          | 当前可提取利润,单位是计价币<br>仅适用于`现货网格`                                                                                           |
| > stopResult          | String          | 现货网格策略停止结果<br>`0`：默认，`1`：市价卖币成功 `-1`：市价卖币失败<br>仅适用于`现货网格`                                                              |
| > activeOrdNum        | String          | 子订单挂单数量                                                                                                                   |
| > tag                 | String          | 订单标签                                                                                                                   |
| > profitSharingRatio  | String          | 分润比例<br>取值范围[0,0.3]<br>如果是普通订单（既不是带单也不是跟单），该字段返回""                                                                     |
| > copyType            | String          | 分润订单类型<br>`0`：普通订单<br>`1`：普通跟单<br>`2`：分润跟单<br>`3`：带单                                                                   |
| > pTime               | String          | 网格策略的推送时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                              |
| > tradeQuoteCcy       | String          | 用于交易的计价币种。 |
