---
site: okex
lang: zh
name: WS / 定投策略委托订单频道
type: guide
---

### WS / 定投策略委托订单频道 [order-book-trading-recurring-buy-ws-recurring-buy-orders-channel]

支持定投策略订单的定时推送和事件推送

#### 服务地址

/ws/v5/business (需要登录)

> 请求示例

```shell
{
  	"id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "algo-recurring-buy",
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
        url = "wss://ws.okx.com:8443/ws/v5/business",
        useServerTime=False
    )
    await ws.start()
    args = [{
		"channel": "algo-recurring-buy",
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
| > channel  | String | 是       | 频道名<br>`algo-recurring-buy`                                        |
| > instType | String | 是       | 产品类型<br>`SPOT`：币币<br>`ANY`：全部 |
| > algoId   | String | 否       | 策略ID                                                       |

> 成功返回示例

```json
{
	"id": "1512",
	"event": "subscribe",
	"arg": {
			"channel": "algo-recurring-buy",
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"algo-recurring-buy\", \"instType\" : \"FUTURES\"}]}",
        "connId": "a4d3ae55"
}
```
#### 返回参数

| 参数       | 类型   | 是否必须 | 描述                                   |
| :--------- | :----- | -------- |:-------------------------------------|
| id     | String | 否       | 消息的唯一标识 |
| event      | String | 是       | 事件<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg      | Object | 否       | 订阅的频道                                |
| > channel  | String | 是       | 频道名                                  |
| > instType | String | 是       | 产品类型                                 |
| > algoId   | String | 否       | 策略ID                                 |
| code       | String | 否       | 错误码                                  |
| msg        | String | 否       | 错误消息                                 |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例

```json
{
	"arg": {
		"channel": "algo-recurring-buy",
		"instType": "SPOT",
		"uid": "447*******584"
	},
	"data": [{
		"algoClOrdId": "",
		"algoId": "644497312047435776",
		"algoOrdType": "recurring",
		"amt": "100",
		"cTime": "1699932133373",
		"cycles": "0",
		"instType": "SPOT",
		"investmentAmt": "0",
		"investmentCcy": "USDC",
		"mktCap": "0",
		"nextInvestTime": "1699934415300",
		"pTime": "1699933314691",
		"period": "hourly",
		"pnlRatio": "0",
		"recurringDay": "",
		"recurringHour": "1",
		"recurringList": [{
			"avgPx": "0",
			"ccy": "BTC",
			"profit": "0",
			"px": "36482",
			"ratio": "0.2",
            "minPx": "30000",
            "maxPx": "50000",
			"totalAmt": "0"
		}, {
			"avgPx": "0",
			"ccy": "ETH",
			"profit": "0",
			"px": "2057.54",
			"ratio": "0.8",
            "minPx": "",
            "maxPx": "",
			"totalAmt": "0"
		}],
		"recurringTime": "12",
        "recurringTimeType": "1",
        "recurringTimeMinutes": "",
        "source": ["1"],
		"state": "running",
		"stgyName": "stg1",
		"tag": "",
		"timeZone": "8",
		"totalAnnRate": "0",
		"totalPnl": "0",
		"uTime": "1699932136249",
		"tradeQuoteCcy": "USDT"
	}]
}
```

#### 推送数据参数

| **参数名**          | **类型**          | **描述**                                                                                             |
|:-----------------|:----------------|:---------------------------------------------------------------------------------------------------|
| arg              | Object          | 订阅成功的频道                                                                                            |
| > channel        | String          | 频道名                                                                                                |
| > instType       | String          | 产品类型                                                                                               |
| > algoId         | String          | 策略ID                                                                                               |
| > uid            | String          | 用户ID                                                                                               |
| data             | Array of objects           | 订阅的数据                                                                                              |
| > algoId         | String          | 策略订单ID                                                                                             |
| > algoClOrdId    | String          | 客户自定义订单ID                                                                                          |
| > instType       | String          | 产品类型<br>`SPOT`：现货                                                                                  |
| > cTime          | String          | 策略订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                           |
| > uTime          | String          | 策略订单更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                           |
| > algoOrdType    | String          | 策略订单类型<br>`recurring`：定投                                                                           |
| > state          | String          | 订单状态<br>`running`：运行中<br>`stopping`：终止中<br>`stopped`：已停止<br/>`pause`: 已暂停                                           |
| > stgyName       | String          | 策略自定义名称，不超过40个字符                                                                                   |
| > recurringList  | Array of objects | 定投信息                                                                                               |
| >> ccy           | String          | 定投币种，如 `BTC`                                                                                       |
| >> ratio         | String          | 定投币种资产占比，如 "0.2"代表占比20%                                                                            |
| >> minPx | String | 价格区间最低价，`""` 代表没有限制 |
| >> maxPx | String | 价格区间最高价，`""` 代表没有限制 |
| >> totalAmt      | String          | 累计购入定投币种的数量                                                                                        |
| >> profit        | String          | 定投收益，单位为`investmentCcy`                                                                            |
| >> avgPx         | String          | 定投均价，计价单位为`investmentCcy`                                                                          |
| >> px            | String          | 当前价格，计价单位为`investmentCcy`                                                                          |
| > period         | String          | 周期类型<br><br>`monthly`：月<br>`weekly`：周<br>`daily`：日<br>`hourly`：小时            |
| > recurringDay   | String          | 投资日<br>当周期类型为`monthly`，则取值范围是 [1,28] 的整数<br>当周期类型为`weekly`，则取值范围是 [1,7] 的整数 |
| > recurringHour  | String          | 小时级别定投的间隔<br>`1`/`4`/`8`/`12`<br>如：`1`代表每隔`1`个小时定投                                               |
| > recurringTime  | String          | 投资时间，取值范围是 [0,23] 的整数                                                                              |
| > timeZone       | String          | 时区（UTC），取值范围是 [-12,14] 的整数<br>如 `8`表示UTC+8（东8区），北京时间                                               |
| > amt            | String          | 每期投入数量                                                                                             |
| > investmentAmt  | String          | 累计投入数量                                                                                             |
| > investmentCcy  | String          | 投入数量单位，只能是`USDT`/`USDC`                                                                            |
| > nextInvestTime   | String          | 下一次定投发生的时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                 |
| > totalPnl       | String          | 总收益                                                                                                |
| > totalAnnRate   | String          | 总年化                                                                                                |
| > pnlRatio       | String          | 收益率                                                                                                |
| > mktCap         | String          | 当前总市值，单位为`USDT`                                                                                    |
| > cycles         | String          | 定投累计轮数                                                                                             |
| > tag            | String          | 订单标签                                                                                               |
| > pTime          | String          | 策略订单的推送时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                          |
| > tradeQuoteCcy   | String   | 用于交易的计价币种。 |
| > recurringTimeType | String | 定投时间类型 |
| > recurringTimeMinutes | String | 自定义定投分钟数 |
| > source | Array | 定投来源 |
