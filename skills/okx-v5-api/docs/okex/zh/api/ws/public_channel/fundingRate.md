---
site: okex
lang: zh
name: 资金费率频道
type: guide
---

### 资金费率频道 [public-data-websocket-funding-rate-channel]

获取合约资金费率，30秒到90秒内推送一次数据

#### URL Path

/ws/v5/public

> 请求示例

```shell
{
   "id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "funding-rate",
		"instId": "BTC-USD-SWAP"
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
          "channel": "funding-rate",
          "instId": "BTC-USD-SWAP"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### 请求参数

| 参数    | 类型   | 是否必须 | 描述                                                |
| :------ | :----- | :------- | :-------------------------------------------------- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op      | String | 是       | 操作<br>`subscribe`<br>`unsubscribe` |
| args  | Array of objects  | 是       | 请求订阅的频道列表                                  |
| > channel | String | 是       | 频道名<br>`funding-rate`                              |
| > instId  | String | 是       | 产品ID                                              |

> 成功返回示例

```json
{
   "id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "funding-rate",
		"instId": "BTC-USD-SWAP"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"funding-rate\", \"instId\" : \"BTC-USD-SWAP\"}]}",
	"connId": "a4d3ae55"
}
```
#### 返回参数

| 参数     | 类型   | 是否必须 | 描述                                                         |
| :------- | :----- | :------- | :----------------------------------------------------------- |
| id     | String | 否       | 消息的唯一标识 |
| event    | String | 是       | 事件<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg    | Object | 否       | 订阅的频道                                                   |
| > channel | String | 是       | 频道名                                                       |
| > instId  | String | 否       | 产品ID                                                       |
| code     | String | 否       | 错误码                                                       |
| msg      | String | 否       | 错误消息                                                     |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例

```json
{
   "arg":{
      "channel":"funding-rate",
      "instId":"BTC-USD-SWAP"
   },
   "data":[
      {
         "fundingRate":"0.0001875391284828",
         "fundingTime":"1700726400000",
         "instId":"BTC-USD-SWAP",
         "instType":"SWAP",
         "method": "current_period",
         "maxFundingRate":"0.00375",
         "minFundingRate":"-0.00375",
         "nextFundingRate":"",
         "nextFundingTime":"1700755200000",
         "premium": "0.0001233824646391",
         "settFundingRate":"0.0001699799259033",
         "settState":"settled",
         "ts":"1700724675402"
      }
   ]
}
```
#### 推送数据参数

| **参数名**       | **类型** | **描述**                                                     |
| :--------------- | :------- | :----------------------------------------------------------- |
| arg            | Object   | 订阅成功的频道                                               |
| > channel         | String   | 频道名                                                       |
| > instId          | String   | 产品ID                                                       |
| data           | Array of objects    | 订阅的数据                                                   |
| > instType        | String   | 产品类型<br>`SWAP`：永续合约<br>`FUTURES`：X-Perps 交割合约 |
| > instId          | String   | 产品ID，如 `BTC-USD-SWAP`                                    |
| > method | String | 资金费收取逻辑 <br>`current_period`：当期收 ~~<br>`next_period`：跨期收~~（不再支持跨期收合约） |
| > formulaType | String | 公式类型<br>`noRate`：旧资金费率计算公式<br>`withRate`：新资金费率计算公式 |
| > fundingRate     | String   | 资金费率                                                     |
| > fundingTime     | String   | 最新的到期结算的资金费时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| > nextFundingRate | String   | ~~下一期预测资金费率~~（不再支持跨期收合约）                                           |
| > nextFundingTime | String   | 下一期资金费时间，Unix时间戳的毫秒数格式，如 `1622851200000` |
| > minFundingRate | String | 下一期的预测资金费率下限 |
| > maxFundingRate | String | 下一期的预测资金费率上限 |
| > interestRate | String | 利率 |
| > impactValue | String | 深度加权金额（计价币数量） |
| > settState | String | 资金费率结算状态 <br>`processing`：结算中 <br>`settled`：已结算 |
| > settFundingRate | String | 若 settState = `processing`，该字段代表用于本轮结算的资金费率；若 settState = `settled`，该字段代表用于上轮结算的资金费率 |
| > premium | String | 溢价指数<br> 公式：[max (0，深度加权买价 - 指数价格) – max (0，指数价格 – 深度加权卖价)] / 指数价格|
| > ts | String | 数据更新时间，Unix时间戳的毫秒数格式，如 `1597026383085` |

<aside class="notice">
针对一些资金费率波动较大的小币种，OKX也将实时关注行情变化，在必要时候，将资金费率收取频率从8小时收付，改成频率较高的6小时/4小时/2小时/1小时收付。因此，用户应关注`fundingTime`及`nextFundingTime`字段以确定合约的资金费收取频率。
</aside>
