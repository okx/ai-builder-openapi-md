---
site: us
lang: zh
name: POST / 批量下单
method: POST
path: /api/v5/trade/batch-orders
module: trade
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### POST / 批量下单 [order-book-trading-trade-post-place-multiple-orders]

每次最多可以批量提交20个新订单。请求参数应该按数组格式传递，会依次委托订单。<br><br>

#### 限速：300个/2s 
#### 限速规则：User ID + Instrument ID

<aside class="notice">
与其他限速按接口调用次数不同，该接口限速按订单的总个数限速。如果单次批量请求中只有一个元素，则算在单个`下单`限速中。
</aside>

#### HTTP请求

`POST /api/v5/trade/batch-orders`

> 请求示例

```shell
#币币批量下单
 POST /api/v5/trade/batch-orders
 body
 [
    {
        "instId":"BTC-USDT",
        "tdMode":"cash",
        "clOrdId":"b15",
        "side":"buy",
        "ordType":"limit",
        "px":"2.15",
        "sz":"2"
    },
    {
        "instId":"BTC-USDT",
        "tdMode":"cash",
        "clOrdId":"b16",
        "side":"buy",
        "ordType":"limit",
        "px":"2.15",
        "sz":"2"
    }
]
```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# 批量下单
place_orders_without_clOrdId = [
    {"instId": "BTC-USDT", "tdMode": "cash", "clOrdId": "b15", "side": "buy", "ordType": "limit", "px": "2.15", "sz": "2"},
    {"instId": "BTC-USDT", "tdMode": "cash", "clOrdId": "b16", "side": "buy", "ordType": "limit", "px": "2.15", "sz": "2"}
]

result = tradeAPI.place_multiple_orders(place_orders_without_clOrdId)
print(result)
```

#### 请求参数

| 参数名                    | 类型    | 是否必须 | 描述                                                                                                                                                                                                                                                   |
|:-----------------------| :------ | :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId                 | String          | 是        | 产品ID，如 `BTC-USDT`                                                                                                                                                                                                                                    |
| tdMode                 | String          | 是        | 交易模式<br>`cash`：非保证金                                                                                                                                                                                                                                  |
| clOrdId                | String          | 否        | 客户自定义订单ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                                                                                                                                                                                 |
| tag                    | String          | 否        | 订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间。                                                                                                                                                                                                      |
| side                   | String          | 是        | 订单方向<br>`buy`：买<br>`sell`：卖                                                                                                                                                                                                                          |
| ordType                | String          | 是        | 订单类型<br>`market`：市价单<br>`limit`：限价单 <br>`post_only`：只做maker单 <br>`fok`：全部成交或立即取消 <br>`ioc`：立即成交并取消剩余 <br>`rpi`：Retail Price Improvement 订单<br>`elp`：流动性增强计划订单（已弃用，请使用 `rpi`。2026年10月31日前仍可使用。）                                                                                                                                                 |
| sz                     | String          | 是        | 委托数量                                                                                                                                                                                                                                                 |
| px                     | String          | 可选       | 委托价格，仅适用于`limit`、`post_only`、`fok`、`ioc`类型的订单                                                                                                                                                                                                        |
| tgtCcy                 | String          | 否        | 市价单委托数量`sz`的单位，仅适用于`币币`市价订单<br>`base_ccy`: 交易货币 ；`quote_ccy`：计价货币<br>买单默认`quote_ccy`， 卖单默认`base_ccy`                                                                                                                                                 |
| banAmend               | Boolean         | 否        | 是否禁止币币市价改单，true 或 false，默认false <br>为true时，余额不足时，系统不会改单，下单会失败，仅适用于币币市价单                                                                                                                                                                              |
| stpId                  | String          | 否        | ~~自成交保护ID。来自同一个母账户配着同一个ID的订单不能自成交<br>用户自定义1<=x<=999999999的整数~~（已弃用）                                                                                                                                                                                           |
| stpMode                | String          | 否        | 自成交保护模式<br>默认为 cancel maker <br>`cancel_maker`,`cancel_taker`,`cancel_both`<br>Cancel both不支持FOK                                                                                                                                     |
| tradeQuoteCcy    | String | 否       | 用于交易的计价币种。仅适用于`币币`。<br>默认值为 `instId` 的计价币种，比如：对于 `BTC-USD`，默认取 `USD`。 |
| rpiTakerAccess | Boolean | 否 | 默认值为 `false`。<br>设为 `true` 时，订单可使用 RPI 流动性，适用于 `limit`、`market`、`fok`、`ioc` 订单。<br>当 `rpiTakerAccess` 为 `true` 时，减速带机制在下单和改单时均适用于所有 `ordType`，包括 `post_only`。<br>改单时不会从原始订单继承，必须每次显式指定（省略则该次改单视为 `false`）。<br>`isElpTakerAccess` 在 2026年10月31日前作为别名继续被接受。 |
| rpiPxRound | Boolean | 否 | 默认值为 `false`。仅对 RPI 做市商订单（`ordType: rpi`）生效。设为 `true` 时，违反 RPI 做市商间距规则的价格将自动向外取整至最近的合规价位，而非被拒绝。对非 RPI 订单及 `OPTION`/`EVENTS` 无效。 |
| attachAlgoOrds         | Array of objects | 否        | 附带止盈止损或移动止盈止损订单信息                                                                                                                                                                                                                                           |
| > attachAlgoClOrdId    | String          | 否        | 下单附带止盈止损或移动止盈止损时，客户自定义的策略订单ID <br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。<br>订单完全成交，下附带策略委托单时，该值会传给`algoClOrdId`                                                                                                                                            |
| > tpTriggerPx          | String          | 可选       | 止盈触发价，如果填写此参数，必须填写 止盈委托价                                                                                                                                                                                                                             |
| > tpOrdPx              | String          | 可选       | 止盈委托价，如果填写此参数，必须填写 止盈触发价<br>委托价格为-1时，执行市价止盈                                                                                                                                                                                                          |
| > slTriggerPx          | String          | 可选       | 止损触发价，如果填写此参数，必须填写 止损委托价                                                                                                                                                                                                                             |
| > slOrdPx              | String          | 可选       | 止损委托价，如果填写此参数，必须填写 止损触发价<br>委托价格为-1时，执行市价止损                                                                                                                                                                                                          |
| > tpTriggerPxType      | String          | 否        | 止盈触发价类型<br>`last`：最新价格<br>默认为`last`                                                                                                                                                                                                                  |
| > slTriggerPxType      | String          | 否        | 止损触发价类型<br>`last`：最新价格<br>默认为`last`                                                                                                                                                                                                                  |
| > sz                   | String          | 可选       | 数量。仅适用于“多笔止盈”的止盈订单，且对于“多笔止盈”的止盈订单必填                                                                                                                                                                                                                  |
| > amendPxOnTriggerType | String          | 否        | 是否启用开仓价止损，仅适用于分批止盈的止损订单，第一笔止盈触发时，止损触发价格是否移动到开仓均价止损<br>`0`：不开启，默认值 <br>`1`：开启，且止损触发价不能为空                                                                                                                                                              |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "clOrdId":"oktswap6",
            "ordId":"12345689",
            "tag":"",
            "sCode":"0",
            "sMsg":"",
            "subCode":""
        },
        {
            "clOrdId":"oktswap7",
            "ordId":"12344",
            "tag":"",
            "sCode":"0",
            "sMsg":"",
            "subCode":""
        }
    ],
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                      |
| :--------- | :------- | :---------------------------- |
| code       | String   | 结果代码，`0`表示成功 |
| msg        | String   | 错误信息，代码为0时，该字段为空  |
| data       | Array of objects   | 包含结果的对象数组              |
| > ordId      | String   | 订单ID                        |
| > clOrdId    | String   | 客户自定义订单ID              |
| > tag        | String   | 订单标签                      |
| > sCode      | String   | 事件执行结果的code，0代表成功 |
| > sMsg       | String   | 事件执行失败或成功时的msg           |
| > subCode    | String   | sCode 的子码。<br>当 sCode 为 0（请求成功）时，返回 `""`。<br>当 sCode 不为 0（请求失败）且存在子码时，返回对应的子码；若无子码，则返回 `""`。|
| inTime | String | REST网关接收请求时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123` <br>返回的时间是请求验证后的时间。|
| outTime | String | REST网关发送响应时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`|

<aside class="notice">
clOrdId<br>
clOrdId是用户自定义的唯一ID用来识别订单。如果在请求参数中传入了，那它一定会在返回参数内，并且可以用于查询订单，撤销订单，修改订单等接口。
clOrdId不能与当前所有挂单和当前请求中的clOrdId重复。
</aside>
