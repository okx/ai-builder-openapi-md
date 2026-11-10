---
site: okex
lang: zh
name: WS / 合约网格策略委托订单频道
type: guide
---

### WS / 合约网格策略委托订单频道 [order-book-trading-grid-trading-ws-contract-grid-algo-orders-channel]

支持合约网格策略订单的定时推送和事件推送

#### 服务地址

/ws/v5/business (需要登录)

> 请求示例

```shell
{
  	"id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "grid-orders-contract",
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
        url = "wss://ws.okx.com/ws/v5/business",
        useServerTime=False
    )
    await ws.start()
    args = [{
		"channel": "grid-orders-contract",
		"instType": "SWAP"
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
| > channel  | String | 是       | 频道名<br>`grid-orders-contract`                                        |
| > instType | String | 是       | 产品类型<br>`SWAP`：永续<br>`FUTURE`：交割<br>`ANY`：全部 |
| > instId   | String | 否       | 产品ID                                                       |
| > algoId   | String | 否       | 策略ID                                                       |

> 成功返回示例

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "grid-orders-contract",
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"grid-orders-contract\", \"instType\" : \"FUTURES\"}]}",
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
		"channel": "grid-orders-contract",
		"instType": "ANY",
		"uid": "4470****9584"
	},
	"data": [{
		"actualLever": "2.3481494635276649",
        "activeOrdNum": "10",
        "algoClOrdId": "",
		"algoId": "571039869070475264",
		"algoOrdType": "contract_grid",
		"annualizedRate": "0",
		"arbitrageNum": "0",
		"availEq": "52.3015392887089673",
		"basePos": true,
		"cTime": "1682418514204",
		"cancelType": "0",
		"direction": "long",
		"eq": "108.7945652387089673",
		"floatProfit": "8.7945652387089673",
		"gridNum": "10",
		"gridProfit": "0",
		"instId": "BTC-USDT-SWAP",
		"instType": "SWAP",
		"investment": "100",
		"lever": "5",
		"liqPx": "16370.482143120824",
		"maxPx": "36437.3",
		"minPx": "26931.9",
		"ordFrozen": "5.38638",
		"pTime": "1682492574068",
		"perMaxProfitRate": "0.1687494513302446",
		"perMinProfitRate": "0.1263869357706788",
		"pnlRatio": "0.0879456523870897",
		"rebateTrans": [{
			"rebate": "0",
			"rebateCcy": "USDT"
		}],
		"runPx": "27306.9",
		"runType": "1",
		"singleAmt": "1",
		"slTriggerPx": "",
		"state": "running",
		"stopType": "0",
		"sz": "100",
		"tag": "",
		"totalAnnualizedRate": "38.52019574554529",
		"totalPnl": "8.7945652387089673",
		"tpTriggerPx": "",
		"tradeNum": "9",
		"triggerParams": [{
			"triggerAction": "start",
			"delaySeconds": "0",
			"triggerStrategy": "price",
			"triggerPx": "1",
			"triggerType": "manual",
			"triggerTime": "1682418561497"
		}, {
			"triggerAction": "stop",
			"delaySeconds": "0",
			"triggerStrategy": "instant",
			"stopType": "1",
			"triggerType": "manual",
			"triggerTime": "0"
		}],
		"uTime": "1682492552257",
        "profitSharingRatio": "",
        "copyType": "0",
        "tpRatio": "",
        "slRatio": "",
        "fee": "",
        "fundingFee": ""
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
| > algoClOrdId         | String          | 用户自定义策略ID                                                                                                              |
| > instType            | String          | 产品类型                                                                                                                   |
| > instId              | String          | 产品ID                                                                                                                   |
| > cTime               | String          | 策略订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                               |
| > uTime               | String          | 策略订单更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                               |
| > algoOrdType         | String          | 策略订单类型<br>`contract_grid`：合约网格                                                                                         |
| > state               | String          | 订单状态<br>`starting`：启动中<br>`running`：运行中<br>`stopping`：终止中<br>`no_close_position`：已停止未平仓（仅适用于合约网格）<br>`stopped`：已停止     |
| > rebateTrans         | Array of objects | 返佣划转信息                                                                                                                 |
| >> rebate             | String          | 返佣数量                                                                                                                   |
| >> rebateCcy          | String          | 返佣币种                                                                                                                   |
| > triggerParams       | Array of objects | 信号触发参数                                                                                                                 |
| >> triggerAction      | String          | 触发行为<br>`start`：网格启动<br>`stop`：网格停止                                                                                    |
| >> triggerStrategy    | String          | 触发策略<br>`instant`：立即触发<br>`price`：价格触发<br>`rsi`：rsi指标触发                                                                |
| >> delaySeconds       | String          | 延迟触发时间，单位为秒                                                                                                            |
| >> triggerTime        | String          | triggerAction实际触发时间，Unix时间戳的毫秒数格式, 如 `1597026383085`                                                                   |
| >> triggerType        | String          | triggerAction的实际触发类型<br>`manual`：手动触发<br>`auto`: 自动触发                                                                  |
| >> timeframe          | String          | K线种类<br>`3m`, `5m`, `15m`, `30m` (`m`代表分钟)<br>`1H`, `4H` (`H`代表小时)<br>`1D` (`D`代表天)<br>该字段只在`triggerStrategy`为`rsi`时有效 |
| >> thold              | String          | 阈值<br>取值[1,100]的整数<br>该字段只在`triggerStrategy`为`rsi`时有效                                                                  |
| >> triggerCond        | String          | 触发条件<br>`cross_up`：上穿<br>`cross_down`：下穿<br>`above`：上方<br>`below`：下方<br>`cross`：交叉<br>该字段只在`triggerStrategy`为`rsi`时有效  |
| >> timePeriod         | String          | 周期<br>`14`<br>该字段只在`triggerStrategy`为`rsi`下有效                                                                          |
| >> triggerPx          | String          | 触发价格<br>该字段只在`triggerStrategy`为`price`下有效                                                                              |
| >> stopType           | String          | 策略停止类型<br>现货网格 `1`：卖出交易币，`2`：不卖出交易币<br>合约网格 `1`：停止平仓，`2`：停止不平仓 <br>该字段只在`triggerAction`为`stop`时有效                      |
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
| > investment          | String          | 累计投入金额<br>现货网格如果投入了交易币则折算为计价币                                                                                          |
| > gridProfit          | String          | 网格利润                                                                                                                   |
| > floatProfit         | String          | 浮动盈亏                                                                                                                   |
| > totalAnnualizedRate | String          | 总年化                                                                                                                    |
| > annualizedRate      | String          | 网格年化                                                                                                                   |
| > cancelType          | String          | 网格策略停止原因<br>`0`：无<br>`1`：手动停止<br>`2`：止盈停止<br>`3`：止损停止<br>`4`：风控停止<br>`5`：交割停止<br>`6`: 信号停止                             |
| > stopType            | String          | 网格策略停止类型<br>现货网格 `1`：卖出交易币，`2`：不卖出交易币<br>合约网格 `1`：市价全平，`2`：停止不平仓                                                       |
| > direction           | String          | 合约网格类型<br>`long`：做多，`short`：做空，`neutral`：中性<br>仅适用于`合约网格`                                                              |
| > basePos             | Boolean         | 是否开底仓<br>仅适用于`合约网格`                                                                                                    |
| > sz                  | String          | 投入保证金，单位为`USDT`<br>仅适用于`合约网格`                                                                                          |
| > lever               | String          | 杠杆倍数<br>仅适用于`合约网格`                                                                                                     |
| > actualLever         | String          | 实际杠杆倍数<br>仅适用于`合约网格`                                                                                                   |
| > liqPx               | String          | 预估强平价格<br>仅适用于`合约网格`                                                                                                   |
| > eq                  | String          | 策略账户总权益<br>仅适用于`合约网格`                                                                                                  |
| > ordFrozen           | String          | 挂单占用<br>适用于`合约网格`                                                                                                      |
| > availEq             | String          | 可用保证金<br>适用于`合约网格`                                                                                                     |
| > activeOrdNum        | String          | 子订单挂单数量                                                                                                                |
| > tag                 | String          | 订单标签                                                                                                                   |
| > profitSharingRatio  | String          | 分润比例<br>取值范围[0,0.3]<br>如果是普通订单（既不是带单也不是跟单），该字段返回""                                                                     |
| > copyType            | String          | 分润订单类型<br>`0`：普通订单<br>`1`：普通跟单<br>`2`：分润跟单<br>`3`：带单                                                                   |
| > tpRatio             | String          | 止盈比率，0.1 代表 10%                                                                                                        |
| > slRatio             | String   | 止损比率，0.1 代表 10%                                                                                                        |
| > fee                 | String          | 累计手续费金额，仅适用于合约网格，其他网格策略为""                                                                                             |
| > fundingFee          | String   | 累计资金费用，仅适用于合约网格，其他网格策略为""                                                                                              |
| > pTime               | String          | 网格策略的推送时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                              |
