---
site: okex
lang: zh
name: WebSocket 私有频道
type: guide
---

## WebSocket 私有频道 [block-trading-websocket-private-channel]

### 询价频道 [block-trading-websocket-private-channel-rfqs-channel]

获取用户自身发送或接收的询价信息。每当用户自身发送或接收询价时，数据都将被推送。

#### 服务地址

/ws/v5/business (需要登录)

> 请求示例

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "rfqs"
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
          "channel": "rfqs"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

#### 请求参数

| 参数名 | 类型   | 是否必填 | 描述                         |
| :-------- | :----- | :------- | ----------------------------------- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是      | 操作<br>`subscribe`<br>`unsubscribe`<br>|
| args      | Array of objects  | 是      | 请求订阅的频道列表          |
| > channel | String | 是      | 频道名<br>`rfqs`              |

> 成功返回示例 

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "rfqs"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"rfqs\"}]}",
  "connId": "a4d3ae55"
}
```

#### 返回参数

| 参数名 | 类型   | 是否必填 | 描述                                  |
| :-------- | :----- | :------- | -------------------------------------------- |
| id     | String | 否       | 消息的唯一标识 |
| event     | String | 是       | 操作<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object | 否       | 订阅的频道                          |
| > channel | String | 是       | 频道名<br>`rfqs`                         |
| code      | String | 否       | 错误码                                 |
| msg       | String | 否       | 错误消息                                |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例

```json
{
    "arg":{
        "channel":"rfqs",
        "uid": "77982378738415879"
    },
    "data":[
        {
            "cTime":"1611033737572",
            "uTime":"1611033737572",
            "traderCode":"DSK2",
            "rfqId":"22534",
            "clRfqId":"",
            "tag":"123456",
            "state":"active",
            "flowType": "",
            "validUntil":"1611033857557",
            "allowPartialExecution": false,
            "counterparties":[
                "DSK4",
                "DSK5"
            ],
            "legs":[
                {
                    "instId":"BTCUSD-211208-36000-C",
                    "tdMode":"cross",
                    "ccy":"USDT",
                    "sz":"25.0",
                    "side":"buy",
                    "posSide": "long",
                    "tgtCcy":""
                },
                {
                    "instId":"ETHUSD-211208-45000-C",
                    "tdMode":"cross",
                    "ccy":"USDT",
                    "sz":"25.0",
                    "side":"sell",
                    "posSide": "long",
                    "tgtCcy":""
                }
            ]
        }
    ]
}
```

#### 推送数据参数

| **参数名** | **类型** | **描述**                                                                                                                                                                     |
| -------------- | --------- |----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg           | Object   | 订阅成功的频道                                                                                                                                                                    |
| > channel     | String   | 频道名                                                                                                                                                                        |
| > uid         | String   | 用户标识                                                                                                                                                                       |
| data          | Array of objects    | 订阅的数据                                                                                                                                                                      |
| > cTime	      | String   | 询价单创建时间，Unix时间戳的毫秒数格式。                                                                                                                                                     |
| > uTime	      | String	 | 询价单状态更新时间，Unix时间戳的毫秒数格式。                                                                                                                                                   |
| > state	      | String	 | 询价单的状态<br>有效值有 `active` `canceled` `filled` `expired` `traded_away` `failed`<br/>`filled` 表示询价单已成功按照做市商的报价成交。<br/>`traded_away` 仅适用于报价方。同一笔询价单可能对一个报价方显示为 `filled`，而对另一个报价方显示为 `traded_away`。<br/>示例：询价方创建询价单 → 做市商A报价 pxA，做市商B报价 pxB → pxA 优于 pxB → 询价方执行做市商A的报价 → 做市商A看到 `filled`，做市商B看到 `traded_away`。                                                                    |
| > counterparties	|Array of Strings| 报价方列表                                                                                                                                                                      |
| > validUntil	| String   | 询价单的过期时间，Unix时间戳的毫秒数格式。                                                                                                                                                    |
| > clRfqId	    | String	 | 询价单自定义ID，为客户敏感信息，不会公开，对报价方返回""。                                                                                                                                            |
| > tag         | String   | 询价单标签，与此询价单关联的大宗交易将有相同的标签。                                                                                                                                                 |
| > flowType | String | 识别询价单的类型。 <br>仅适用于报价方，返回""给询价方。                                                                                                                                            |
| > traderCode	| String	 | 询价方唯一标识代码，询价时 Anonymous 设置为 `True` 时不可见                                                                                                                                    |
| > rfqId	      | String	 | 询价单ID                                                                                                                                                                      |
| > allowPartialExecution	      | Boolean	 | RFQ是否可以被部分执行，如果腿的比例和原RFQ一致。>有效值为`true`或`false`。                                                                                                                            |
| > legs	      | Array of objects| 	组合交易                                                                                                                                                                      |
| >> instId	    | String	 | 产品ID                                                                                                                                                                       |
| >> tdMode	    |String	   | 交易模式 <br>保证金模式：`cross`全仓 `isolated`逐仓 <br>非保证金模式：`cash`非保证金. <br>如未提供，tdMode 将继承系统设置的默认值： <br>合约模式 & 现货: `cash` <br>合约模式和跨币种保证金模式下买入期权： `isolated` <br>其他情况: `cross` |
| >> ccy   	    |String	   | 保证金币种，仅适用于`合约模式`下的`全仓杠杆`订单 <br>在其他情况下该参数将被忽略。                                                                                                                           |
| >> sz	        | String	 | 委托数量                                                                                                                                                                       |
| >> side	      | String	 | 询价单方向                                                                                                                                                                      |
| >> posSide    | String   | 持仓方向 <br>买卖模式下默认为`net`。如未指定，则返回""，相当于`net`。 <br>在开平仓模式下仅可选择`long`或`short`。 如未指定，则返回""，对应于为交易开新仓位的方向（买入=>`long`，卖出=>`short`）。<br>仅适用交割、永续。                                  |
| >> tgtCcy	    | String	 | 委托数量的类型 <br>定义`sz`属性的单位。仅适用于 instType=`SPOT`。有效值为`base_ccy`和`quote_ccy`。未指定时，默认为`base_ccy`。                                                                                |
| >> tradeQuoteCcy	        | String           | 	交易使用的计价币种。仅适用于 SPOT。<br>默认值为 instId 的报价币种，例如：对于 `BTC-USD`，默认值为 `USD`.                                                                                                                          |
| > groupId | String | 组合询价单ID<br> 只适用于组合询价单，普通询价单返回 "" |
| > acctAlloc | Array of objects | 组合询价单的账户分配<br> 只适用于 Taker |
| >> acct | String | 账户名 |
| >> legs | Array of objects | 组合交易 |
| >>> instId | String | 产品ID |
| >>> sz | String | 委托数量 |
| >>> tdMode | String | 交易模式 |
| >>> ccy | String | 保证金币种 |
| >>> posSide | String | 持仓方向 |

<aside class="notice">
state: pending_fill 是一个瞬间状态，该频道不会推送。
</aside>

<aside class="notice">
组合询价单介绍<br><br>

1. allowPartialExecution 字段始终为 true，适用于 Taker 和 Maker 的组合询价单。<br>
2. 新增返回参数 acctAlloc ，包含所有账户分配信息，但仅适用于 Taker。<br>
3. 新增返回参数 groupId，适用于 Taker 和 Maker。<br>
4. 对于组合询价单状态<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. 如果任何分配账户处于待执行状态，则状态为 pending_fill<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. 否则，<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. 如果任何分配账户已成交，则状态为 filled<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. 如果所有分配账户均失败，则状态为 failed<br>
</aside>
