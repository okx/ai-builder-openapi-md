---
site: us
lang: zh
name: WS / 行情频道
type: guide
---

### WS / 行情频道 [order-book-trading-market-data-ws-tickers-channel]

获取产品的最新成交价、买一价、卖一价和24小时交易量等信息。在提前挂单阶段，best ask的价格有机会低于best bid。<br>
最快100ms推送一次，没有触发事件时不推送，触发推送的事件有：成交、买一卖一发生变动。

#### 服务地址

/ws/v5/public

> 请求示例

```shell
{
  	"id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "tickers",
		"instId": "BTC-USDT"
	}]
}
```
#### 请求参数

| 参数        | 类型   | 是否必须 | 描述                                 |
|:----------| :----- | :------- |:-----------------------------------|
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是       | 操作<br>`subscribe`<br>`unsubscribe` |
| args      | Array of objects  | 是       | 请求订阅的频道列表                          |
| > channel | String | 是       | 频道名<br>`tickers`                   |
| > instId  | String | 是       | 产品ID                               |

> 成功返回示例

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "tickers",
		"instId": "BTC-USDT"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"tickers\", \"instId\" : \"LTC-USD-200327\"}]}",
	"connId": "a4d3ae55"
}
```
#### 返回参数

| 参数        | 类型       | 是否必须 | 描述                                            |
|:----------|:---------|:-----|:----------------------------------------------|
| id     | String | 否       | 消息的唯一标识 |
| event     | String   | 是    | 事件<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object   | 否    | 订阅的频道                                         |
| > channel | String   | 是    | 频道名                                           |
| > instId  | String   | 是    | 产品ID                                          |
| code      | String   | 否    | 错误码                                           |
| msg       | String   | 否    | 错误消息                                          |
| connId    | String   | 是    | WebSocket连接ID                                 |

> 推送示例

```json
{
	"arg": {
		"channel": "tickers",
		"instId": "BTC-USDT"
	},
	"data": [{
		"instType": "SPOT",
		"instId": "BTC-USDT",
		"last": "9999.99",
		"lastSz": "0.1",
		"askPx": "9999.99",
		"askSz": "11",
		"bidPx": "8888.88",
		"bidSz": "5",
		"open24h": "9000",
		"high24h": "10000",
		"low24h": "8888.88",
		"volCcy24h": "2222",
		"vol24h": "2222",
		"sodUtc0": "2222",
		"sodUtc8": "2222",
		"ts": "1597026383085"
	}]
}
```
#### 推送数据参数

| **参数名**    | **类型**     | **描述**                                                       |
|------------|------------|--------------------------------------------------------------|
| arg        | Object     | 订阅成功的频道                                                      |
| > channel  | String     | 频道名                                                          |
| > instId   | String     | 产品ID                                                         |
| data       | Array of objects      | 订阅的数据                                                        |
| > instType   | String     | 产品类型                                                         |
| > instId     | String     | 产品ID                                                         |
| > last       | String     | 最新成交价                                                        |
| > lastSz     | String     | 最新成交的数量                                                      |
| > askPx      | String     | 卖一价                                                          |
| > askSz      | String     | 卖一价对应的量                                                      |
| > bidPx      | String     | 买一价                                                          |
| > bidSz      | String     | 买一价对应的数量                                                     |
| > open24h    | String     | 24小时开盘价                                                      |
| > high24h    | String     | 24小时最高价                                                      |
| > low24h     | String     | 24小时最低价                                                      |
| > volCcy24h  | String     | 24小时成交量<br>如果是`币币`，数值为计价货币的数量。 |
| > vol24h     | String     | 24小时成交量<br>如果是`币币`，数值为交易货币的数量。  |
| > sodUtc0    | String     | UTC+0 时开盘价                                                   |
| > sodUtc8    | String     | UTC+8 时开盘价                                                   |
| > ts         | String     | 数据产生时间，Unix时间戳的毫秒数格式，如 `1597026383085`                       |
