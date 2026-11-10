---
site: okex
lang: zh
name: 报价频道
type: guide
---

### 报价频道 [block-trading-websocket-private-channel-quotes-channel]

获取用户自身发送或接收的报价信息。每当用户自身发送或接收报价时，数据都将被推送。

#### 服务地址

/ws/v5/business (需要登录)

> 请求示例

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "quotes"
    }
  ]
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
    args = [
        {
          "channel": "quotes"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### 请求参数

| 参数名 | 类型   | 是否必填 | 描述                          |
| :-------- | :----- | :------- | ------------------------------------ |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是      | 操作<br>`subscribe`<br>`unsubscribe`<br>|
| args      | Array of objects  | 是      | 请求订阅的频道列表          |
| > channel | String | 是      | 频道名<br>`quotes`               |

> 成功返回示例

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "quotes"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"quotes\"}]}",
  "connId": "a4d3ae55"
}
```

#### 返回参数

| 参数名 | 类型       | 是否必填 | 描述                                  |
| :-------- |:---------|:-----| -------------------------------------------- |
| id     | String | 否       | 消息的唯一标识 |
| event     | String   | 是    | 操作<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object   | 否    | 订阅的频道                          |
| > channel | String   | 是    | 频道名<br>`quotes`                       |
| code      | String   | 否    | 错误码                                   |
| msg       | String   | 否    | 错误消息                                |
| connId   | String   | 是    | WebSocket连接ID |

> 推送示例

```json
{
    "arg":{
        "channel":"quotes",
        "uid": "77982378738415879"
    },
    "data":[
        {
            "validUntil":"1608997227854",
            "uTime":"1608267227834",
            "cTime":"1608267227834",
            "legs":[
                {
                    "px":"0.0023",
                    "sz":"25.0",
                    "instId":"BTC-USD-220114-25000-C",
                    "tdMode":"cross",
                    "ccy":"USDT",
                    "side":"sell",
                    "posSide": "long",
                    "tgtCcy":""

                },
                {
                    "px":"0.0045",
                    "sz":"25",
                    "instId":"BTC-USD-220114-35000-C",
                    "tdMode":"cross",
                    "ccy":"USDT",
                    "side":"buy",
                    "posSide": "long",
                    "tgtCcy":""

                }
            ],
            "quoteId":"25092",
            "rfqId":"18753",
            "tag":"123456",
            "traderCode":"SATS",
            "quoteSide":"sell",
            "state":"canceled",
            "reason":"mmp_canceled",
            "clQuoteId":""
        }
    ]
}
```

#### 推送数据参数

| **参数名**          | **类型**            | **描述**                                                                                                                                                                       |
|------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg              | Object            | 订阅成功的频道                                                                                                                                                                      |
| > channel        | String            | 频道名                                                                                                                                                                          |
| > uid            | String            | 账户ID，账户uid和app上的一致                                                                                                                                                           |
| data             | Array of objects  | 订阅的数据                                                                                                                                                                        |
| > cTime	         | String	           | 报价单创建时间，Unix时间戳的毫秒数格式。                                                                                                                                                       |
| > uTime	         | String	           | 报价单状态更新时间，Unix时间戳的毫秒数格式。                                                                                                                                                     |
| > state	         | String	           | 报价单的状态<br>`active`<br>`canceled`<br>`filled`<br>`expired`<br>`failed`                                                                                                        |
| > reason	        | String	           | 状态原因<br>`mmp_canceled`                                                                                                                                                       |
| > validUntil	    | String	           | 报价单的过期时间，Unix时间戳的毫秒数格式。                                                                                                                                                      |
| > rfqId	         | String	           | 询价单ID                                                                                                                                                                        |
| > clRfqId	       | String	           | 询价单自定义ID，为客户敏感信息，不会公开，对报价方返回""。                                                                                                                                              |
| > quoteId	       | String	           | 报价单ID                                                                                                                                                                        |
| > clQuoteId	     | String	           | 报价单自定义ID，为客户敏感信息，不会公开，对询价方返回""。                                                                                                                                              |
| > tag            | String            | 报价单标签，与此报价单关联的大宗交易将有相同的标签。                                                                                                                                                   |
| > traderCode	    | String	           | 报价方唯一标识代码，报价时 Anonymous 设置为 `True` 时不可见。                                                                                                                                     |
| > quoteSide	     | String	           | 报价单方向<br>`buy`<br>`sell`<br>当报价单方向为`buy`，对maker来说，执行方向与legs里的方向相同，对taker来说相反。反之同理。                                                                                           |
| > legs	          | Array of objects  | 	组合交易                                                                                                                                                                        |
| >> instId	       | String	           | 产品ID                                                                                                                                                                         |
| >> tdMode	       | String	           | 交易模式<br>保证金模式 `cross`：全仓 `isolated`：逐仓 <br>非保证金模式 `cash`：非保证金<br>如未提供，tdMode 将继承系统设置的默认值： <br>合约模式 & 现货模式: `cash` <br>合约模式和跨币种保证金模式下买入期权： `isolated` <br>其他情况: `cross` |
| >> ccy   	       | String	           | 保证金币种，仅适用于`合约模式`下的`全仓杠杆`订单 <br>在其他情况下该参数将被忽略。                                                                                                                             |
| >> sz	           | String	           | 委托数量                                                                                                                                                                         |
| >> px	           | String	           | 委托价格                                                                                                                                                                         |
| >> side	         | String	           | 报价单方向                                                                                                                                                                        |
| >> posSide       | String            | 持仓方向<br>买卖模式下默认为`net`。如未指定，则返回""，相当于`net`。 <br>在开平仓模式下仅可选择`long`或`short`。 如未指定，则返回""，对应于为交易开新仓位的方向（买入=>`long`，卖出=>`short`）。<br>仅适用交割、永续。                                     |
| >> tgtCcy	       | String	           | 委托数量的类型 <br>定义`sz`属性的单位。仅适用于 instType=`SPOT`。有效值为`base_ccy`和`quote_ccy`。未指定时，默认为`base_ccy`。                                                                                  |
| >> tradeQuoteCcy	        | String           | 	 交易使用的计价币种。仅适用于 SPOT。<br>默认值为 instId 的报价币种，例如：对于 `BTC-USD`，默认值为 `USD`.                                                                                                                          |
