---
site: us
lang: zh
name: WS / K线频道
type: guide
---

### WS / K线频道 [order-book-trading-market-data-ws-candlesticks-channel]

获取K线数据，推送频率最快是间隔1秒推送一次数据。

#### 服务地址

/ws/v5/business

> 请求示例

```shell
{
  "id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "candle1D",
		"instId": "BTC-USDT"
	}]
}
```
#### 请求参数

| 参数     | 类型   | 是否必须 | 描述                                                                                                                                                                                                                                                                                                                                                                                                                       |
| :------- | :----- | :------- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op       | String | 是       | 操作<br>`subscribe`<br>`unsubscribe`                                                                                                                                                                                                                                                                                                                                                                                        |
| args   | Array of objects  | 是       | 请求订阅的频道列表                                                                                                                                                                                                                                                                                                                                                                                                                |
| > channel | String | 是       | 频道名<br>`candle3M`<br>`candle1M`<br>`candle1W`<br>`candle1D`<br>`candle2D`<br>`candle3D`<br>`candle5D`<br>`candle12H`<br>`candle6H`<br>`candle4H`<br>`candle2H`<br>`candle1H`<br>`candle30m`<br>`candle15m`<br>`candle5m`<br>`candle3m`<br>`candle1m`<br>`candle1s`<br>`candle3Mutc`<br>`candle1Mutc`<br>`candle1Wutc`<br>`candle1Dutc`<br>`candle2Dutc`<br>`candle3Dutc`<br>`candle5Dutc`<br>`candle12Hutc`<br>`candle6Hutc` |
| > instId  | String | 是       | 产品ID                                                                                                                                                                                                                                                                                                                                                                                                                     |

> 成功返回示例

```json
{
  "id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "candle1D",
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"candle1D\", \"instId\" : \"BTC-USD-191227\"}]}",
  "connId": "a4d3ae55"
}
```
#### 返回参数

| 参数        | 类型       | 是否必须 | 描述                                                        |
|:----------|:---------| :------- |:----------------------------------------------------------|
| id     | String | 否       | 消息的唯一标识 |
| event     | String   | 是       | 事件<br>`subscribe`<br>`unsubscribe`<br>`error`             |
| arg       | Object   | 否       | 订阅的频道                                                     |
| > channel | String   | 是       | 频道名                                                       |
| > instId  | String   | 是       | 产品ID                                                      |
| code      | String   | 否       | 错误码                                                       |
| msg       | String   | 否       | 错误消息                                                      |
| connId    | String   | 是  | WebSocket连接ID                                             |

> 推送示例

```json
{
  "arg": {
    "channel": "candle1D",
    "instId": "BTC-USDT"
  },
  "data": [
    [
      "1629993600000",
      "42500",
      "48199.9",
      "41006.1",
      "41006.1",
      "3587.41204591",
      "166741046.22583129",
      "166741046.22583129",
      "0"
    ]
  ]
}
```
#### 推送数据参数

| **参数名**       | **类型** | **描述**                                 |
|:--------------| :------- |:---------------------------------------|
| arg           | Object   | 订阅成功的频道                                |
| > channel     | String   | 频道名                                    |
| > instId      | String   | 产品ID                                   |
| data          | Array of objects    | 订阅的数据                                  |
| > ts          | String   | 开始时间，Unix时间戳的毫秒数格式，如 `1597026383085`   |
| > o           | String   | 开盘价格                                   |
| > h           | String   | 最高价格                                   |
| > l           | String   | 最低价格                                   |
| > c           | String   | 收盘价格                                   |
| > vol         | String   | 交易量<br>如果是`币币`，数值为交易货币的数量。             |
| > volCcy      | String   | 交易量<br>如果是`币币`，数值为计价货币的数量。             |
| > volCcyQuote | String  | 交易量，以计价货币为单位<br>如 `BTC-USDT`单位是`USDT`。 |
| > confirm     | String  | K线状态<br>`0`：K线未完结<br>`1`：K线已完结         |
