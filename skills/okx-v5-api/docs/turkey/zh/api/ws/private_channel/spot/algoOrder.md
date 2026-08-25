---
site: turkey
lang: zh
name: WS / 策略委托订单频道
type: guide
---

### WS / 策略委托订单频道 [order-book-trading-algo-trading-ws-algo-orders-channel]

获取策略委托订单，首次订阅不推送，只有当下单、撤单等事件触发时，推送数据

#### 服务地址

/ws/v5/business (需要登录)

> 请求示例：单个

```shell
{
	"id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "orders-algo",
		"instType": "SPOT"
	}]
}
```

> 请求示例

```shell
{
  	"id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "orders-algo",
		"instType": "SPOT"
	}]
}
```
#### 请求参数

| 参数               | 类型     | 是否必须 | 描述                                                                                              |
|:-----------------|:-------|------|:------------------------------------------------------------------------------------------------|
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op               | String | 是    | 操作<br>`subscribe`<br>`unsubscribe`                                                               |
| args             | Array of objects  | 是    | 请求订阅的频道列表                                                                                       |
| > channel        | String | 是    | 频道名<br>`orders-algo`                                                                               |
| > instType       | String | 是    | 产品类型<br>`SPOT`：币币<br>`ANY`：全部 |
| > instId         | String | 否    | 产品ID                                                                                            |

> 成功返回示例：单个

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "orders-algo",
		"instType": "SPOT"
	},
	"connId": "a4d3ae55"
}
```

> 成功返回示例

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "orders-algo",
		"instType": "SPOT"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"orders-algo\", \"instType\" : \"FUTURES\"}]}",
	"connId": "a4d3ae55"
}
```
#### 返回参数

| 参数         | 类型     | 是否必须 | 描述                                                                               |
|:-----------|:-------|------|:---------------------------------------------------------------------------------|
| id     | String | 否       | 消息的唯一标识 																	|	
| event      | String | 是    | 事件<br>`subscribe`<br>`unsubscribe`<br>`error`                                  |
| arg        | Object | 否    | 订阅的频道                                                                            |
| > channel  | String | 是    | 频道名                                                                              |
| > instType | String | 是    | 产品类型<br>`SPOT`：币币<br>`ANY`：全部 |
| > instId   | String | 否    | 产品ID                                                                             |
| code       | String | 否    | 错误码                                                                              |
| msg        | String | 否    | 错误消息                                                                             |
| connId     | String   | 是  | WebSocket连接ID                                                                    |

> 推送示例：单个

```json
{
	"arg": {
		"channel": "orders-algo",
        "uid": "77982378738415879",
		"instType": "SPOT"
	},
	"data": [{
		"actualPx": "0",
		"actualSide": "",
		"actualSz": "0",
		"algoClOrdId": "",
		"algoId": "581878926302093312",
        "attachAlgoOrds": [],
		"amendResult": "",
		"cTime": "1685002746818",
		"ccy": "",
		"clOrdId": "",
		"closeFraction": "",
		"failCode": "",
		"instId": "BTC-USDC",
		"instType": "SPOT",
		"last": "26174.8",
		"lever": "0",
		"notionalUsd": "11.0",
		"ordId": "",
		"ordIdList": [],
		"ordPx": "",
		"ordType": "conditional",
		"posSide": "",
		"quickMgnType": "",
		"reduceOnly": "false",
		"reqId": "",
		"side": "buy",
		"slOrdPx": "",
		"slTriggerPx": "",
		"slTriggerPxType": "",
		"state": "live",
		"sz": "11",
		"tag": "",
		"tdMode": "cross",
		"tgtCcy": "quote_ccy",
		"tpOrdPx": "-1",
		"tpTriggerPx": "1",
		"tpTriggerPxType": "last",
		"triggerPx": "",
		"triggerTime": "",
		"amendPxOnTriggerType": "0"
	}]
}
```

#### 推送数据参数

| **参数名**                | **类型**            | **描述**                                                                                                          |
|:-----------------------|:------------------|:----------------------------------------------------------------------------------------------------------------|
| arg                    | Object            | 订阅成功的频道                                                                                                         |
| > channel              | String            | 频道名                                                                                                             |
| > uid                  | String            | 用户标识                                                                                                            |
| > instType             | String            | 产品类型                                                                                                            |
| > instFamily           | String            | 交易品种<br>适用于`交割`/`永续`/`期权`                                                                                       |
| > instId               | String            | 产品ID                                                                                                            |
| data                   | Array of objects             | 订阅的数据                                                                                                           |
| > instType             | String            | 产品类型                                                                                                            |
| > instId               | String            | 产品ID                                                                                                            |
| > ccy                  | String            | 保证金币种，适用于`逐仓杠杆`及`合约模式`下的`全仓杠杆`订单                                                                                |
| > ordId                | String            | 最新一笔订单ID，与策略委托订单关联的订单ID                                                                                         |
| > ordIdList            | Array of strings            | 订单ID列表，当止盈止损存在市价拆单时，会有多个。                                                                                       |
| > algoId               | String            | 策略委托单ID                                                                                                         |
| > clOrdId              | String            | 客户自定义订单ID                                                                                                       |
| > sz                   | String            | 委托数量<br>`币币`/`币币杠杆`以币为单位<br>`交割`/`永续`/`期权`以张为单位                                                                 |
| > ordType              | String            | 订单类型<br>`conditional`：单向止盈止损 <br>`oco`：双向止盈止损<br>`trigger`：计划委托                                                 |
| > side                 | String            | 订单方向<br>`buy`<br>`sell`                                                                                         |
| > posSide              | String            | 持仓方向<br>`long`：开平仓模式开多<br>`short`：开平仓模式开空<br>`net`：买卖模式                                                         |
| > tdMode               | String            | 交易模式<br>保证金模式      `cross`：全仓  `isolated`：逐仓 <br>非保证金模式  `cash`：现金                                              |
| > tgtCcy               | String            | 币币市价单委托数量`sz`的单位<br>`base_ccy`：交易货币<br>`quote_ccy`：计价货币<br>仅适用于`币币`市价订单<br>默认买单为`quote_ccy`，卖单为`base_ccy`      |
| > lever                | String            | 杠杆倍数，0.01到125之间的数值，仅适用于 `币币杠杆/交割/永续`                                                                            |
| > state                | String            | 订单状态<br>`live`：待生效<br>`effective`：已生效<br>`canceled`：已撤销<br>`order_failed`：委托失败<br>`partially_failed`：部分委托失败<br>`partially_effective`: 部分生效      |
| > tpTriggerPx          | String            | 止盈触发价                                                                                                           |
| > tpTriggerPxType      | String            | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                           |
| > tpOrdPx              | String            | 止盈委托价，委托价格为`-1`时，执行市价止盈                                                                                         |
| > slTriggerPx          | String            | 止损触发价                                                                                                           |
| > slTriggerPxType      | String            | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                           |
| > slOrdPx              | String            | 止损委托价委托价格为`-1`时，执行市价止损                                                                                          |
| > triggerPx            | String            | 计划委托单的触发价格                                                                                                      |
| > triggerPxType        | String            | 计划委托单的触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                       |
| > ordPx                | String            | 计划委托单的委托价格                                                                                                      |
| > last                 | String            | 下单时的最新成交价                                                                                                       |
| > actualSz             | String            | 实际委托量                                                                                                           |
| > actualPx             | String            | 实际委价                                                                                                            |
| > tag                  | String            | 订单标签                                                                                                            |
| > notionalUsd          | String            | 委托单预估美元价值                                                                                                       |
| > actualSide           | String            | 实际触发方向<br>`sl`：止损<br>`tp`：止盈<br/>仅适用于`单向止盈止损委托`和`双向止盈止损委托`                                                                                    |
| > triggerTime          | String            | 策略委托触发时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                        |
| > reduceOnly           | String            | 是否只减仓<br>`true` 或 `false`                                                                                           |
| > failCode             | String            | 代表策略触发失败的原因，已撤销和已生效时为""，委托失败时有值，如 51008；<br>仅适用于单向止盈止损委托、双向止盈止损委托、移动止盈止损委托、计划委托。                                |
| > algoClOrdId          | String            | 客户自定义策略订单ID                                                                                                     |
| > cTime                | String            | 订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                          |
| > reqId                | String            | 修改订单时使用的request ID，如果没有修改，该字段为""                                                                                |
| > amendResult          | String            | 修改订单的结果<br>`-1`：失败<br>`0`：成功																		                                                                  |
| > amendPxOnTriggerType | String            | 是否启用开仓价止损，仅适用于分批止盈的止损订单<br>`0`：不开启，默认值 <br>`1`：开启                                                               |
| > attachAlgoOrds       | Array of objects   | 附带止盈止损或移动止盈止损订单信息<br>适用于`合约模式/跨币种保证金模式/组合保证金模式`                                                                      |
| >> attachAlgoClOrdId   | String            | 下单附带止盈止损或移动止盈止损时，客户自定义的策略订单ID，字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。<br>订单完全成交，下附带策略委托单时，该值会传给algoClOrdId。            |
| >> tpTriggerPx         | String            | 止盈触发价，如果填写此参数，必须填写`止盈委托价`                                                                                       |
| >> tpTriggerPxType     | String            | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为`last`                                              |
| >> tpOrdPx             | String            | 止盈委托价，如果填写此参数，必须填写`止盈触发价`<br>委托价格为`-1`时，执行市价止盈                                                                  |
| >> slTriggerPx         | String            | 止损触发价，如果填写此参数，必须填写`止损委托价`                                                                                       |
| >> slTriggerPxType     | String            | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为`last`                                              |
| >> slOrdPx             | String            | 止损委托价，如果填写此参数，必须填写`止损触发价`<br>委托价格为`-1`时，执行市价止损                                                                  |
