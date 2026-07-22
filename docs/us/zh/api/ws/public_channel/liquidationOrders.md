---
site: us
lang: zh
name: 平台公共爆仓单频道
type: guide
---

### 平台公共爆仓单频道[public-data-websocket-liquidation-orders-channel]

获取爆仓单信息。显示的强平数据并不准确代表欧易的总强平量，亦不应被当做总强平量使用。

#### URL Path

/ws/v5/public

> 请求示例

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "liquidation-orders",
      "instType": "SWAP"
    }
  ]
}
```
```python
import asyncio
from okx.websocket.WsPublicAsync import WsPublicAsync

def callbackFunc(message):
    print(message)

async def main():
    ws = WsPublicAsync(url="wss://wspap.okx.com:8443/ws/v5/public")
    await ws.start()
    args = [
        {
          "channel": "liquidation-orders",
          "instType": "SWAP"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

#### 请求参数

| 参数      | 类型   | 是否必须 | 描述                                                |
| :-------- | :----- | :------- | :-------------------------------------------------- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是       | 操作<br>`subscribe`<br>`unsubscribe` |
| args    | Array of objects  | 是       | 请求订阅的频道 |
| > channel | String | 是       | 频道名<br>`liquidation-orders`|
| > instType  | String | 是       | 产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权 |

> 返回结果

```json
{
  "id": "1512",
    "arg": {
        "channel": "liquidation-orders",
        "instType": "SWAP"
    },
    "data": [
        {
            "details": [
                {
                    "bkLoss": "0",
                    "bkPx": "0.007831",
                    "ccy": "",
                    "posSide": "short",
                    "side": "buy",
                    "sz": "13",
                    "ts": "1692266434010"
                }
            ],
            "instFamily": "IOST-USDT",
            "instId": "IOST-USDT-SWAP",
            "instType": "SWAP",
            "uly": "IOST-USDT"
        }
    ]
}
```

#### 返回参数

| **参数名**    | **类型**           | **描述**                                                              |
|:-----------|:-----------------|:--------------------------------------------------------------------|
| id         | String           | 消息的唯一标识 |
| arg        | Object           | 订阅成功的频道                                                             |
| > channel  | String           | 频道名                                                                 |
| > instType | String           | 产品类型                                                                |
| data       | Array of objects | 订阅的数据                                                               |
| > instType | String           | 产品类型                                                                |
| > instId   | String           | 产品ID，如 `BTC-USD-SWAP`                                               |
| > uly      | String           | 标的指数<br>适用于`交割`/`永续`/`期权`                                           |
| > details  | Array of objects | 详细内容                                                                |
| >> side    | String           | 订单方向<br>`buy`：买<br>`sell`：卖<br>仅适用于`交割`/`永续`                        |
| >> posSide | String           | 持仓模式方向<br>`long`：开平仓模式开多<br>`short`：开平仓模式开空<br>`net`：买卖模式           |
| >> bkPx    | String           | 强平标记价格，与系统爆仓账号委托成交的价格，仅适用于`交割/永续`                                     |
| >> sz      | String           | 强平数量<br>适用于`杠杆`/`交割`/`永续`<br/>对于`杠杆`，单位为交易货币。<br/>对于`交割/永续`，单位为张。   |
| >> bkLoss  | String           | 穿仓亏损数量                                                              |
| >> ccy     | String           | 强平币种<br>适用于`币币杠杆`                                                       |
| >> ts      | String           | 强平发生的时间，Unix时间戳的毫秒数格式，如 `1597026383085`    /                        |

<aside class="notice">

爆仓数据来自不同的数据源，因此推送的数据在时间上不一定是顺序的。

</aside>
