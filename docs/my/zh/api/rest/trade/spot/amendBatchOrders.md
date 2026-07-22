---
site: my
lang: zh
name: POST / 批量修改订单
method: POST
path: /api/v5/trade/amend-batch-orders
module: trade
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### POST / 批量修改订单 [order-book-trading-trade-post-amend-multiple-orders]

修改未完成的订单，一次最多可批量修改20个订单。请求参数应该按数组格式传递。

#### 限速：300个/2s
#### 限速规则：User ID + Instrument ID

<aside class="notice">
与其他限速按接口调用次数不同，该接口限速按订单的总个数限速。如果单次批量请求中只有一个元素，则算在单个`修改订单`限速中。
</aside>

#### HTTP请求

`POST /api/v5/trade/amend-batch-orders`

> 请求示例

```shell
POST /api/v5/trade/amend-batch-orders
body
[
    {
        "ordId":"590909308792049444",
        "newSz":"2",
        "instId":"BTC-USDT"
    },
    {
        "ordId":"590909308792049555",
        "newSz":"2",
        "instId":"BTC-USDT"
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

# 按ordId修改未完成的订单
amend_orders_with_orderId = [
    {"instId": "BTC-USDT", "ordId": "590909308792049444","newSz":"2"},
    {"instId": "BTC-USDT", "ordId": "590909308792049555","newSz":"2"}
]

result = tradeAPI.amend_multiple_orders(amend_orders_with_orderId)
print(result)
```

#### 请求参数

| 参数名                      | 类型              | 是否必须   | 描述                                                                                            |
|:-------------------------|:----------------|:-------|:----------------------------------------------------------------------------------------------|
| instId                   | String          | 是      | 产品ID                                                                                          |
| cxlOnFail                | Boolean         | 否      | 当订单修改失败时，该订单是否需要自动撤销。默认为`false`<br>`false`：不自动撤单<br>`true`：自动撤单                               |
| ordId                    | String          | 可选     | 订单ID， `ordId`和`clOrdId`必须传一个，若传两个，以`ordId`为主                                                  |
| clOrdId                  | String          | 可选     | 用户自定义order ID                                                                                 |
| reqId                    | String          | 否      | 用户自定义修改事件ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                        |
| newSz                    | String          | 可选     | 修改的新数量，对于部分成交订单，该数量应包含已成交数量。                                                                  |
| newPx                    | String          | 可选     | 修改后的新价格                                                                                       |
| attachAlgoOrds           | Array of objects | 否      | 附带止盈止损或移动止盈止损订单信息                                                                                    |
| > attachAlgoId           | String          | 可选     | 附带止盈止损或移动止盈止损的订单ID，由系统生成，改单时必填，用来标识该笔附带止盈止损订单。下附带策略委托单时，该值不会传给 algoId                                |
| > attachAlgoClOrdId      | String          | 可选     | 下单附带止盈止损或移动止盈止损时，客户自定义的策略订单ID                                                                        |
| > newTpTriggerPx         | String          | 可选     | 止盈触发价<br>如果止盈触发价或者委托价为0，那代表删除止盈。只适用于交割和永续合约。                                                  |
| > newTpOrdPx             | String          | 可选     | 止盈委托价<br>委托价格为-1时，执行市价止盈。只适用于交割和永续合约。                                                         |
| > newSlTriggerPx         | String          | 可选     | 止损触发价<br>如果止损触发价或者委托价为0，那代表删除止损。只适用于交割和永续合约。                                                  |
| > newSlOrdPx             | String          | 可选     | 止损委托价<br>委托价格为-1时，执行市价止损。 只适用于交割和永续合约。                                                        |
| > newTpTriggerPxType     | String          | 可选     | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>只适用于`交割`/`永续`<br>如果要新增止盈，该参数必填       |
| > newSlTriggerPxType     | String          | 可选     | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>只适用于`交割`/`永续`<br>如果要新增止损，该参数必填       |
| > sz                     | String          | 可选     | 新的张数。仅适用于“多笔止盈”的止盈订单且必填                                                                       |
| > amendPxOnTriggerType   | String          | 否      | 是否启用开仓价止损，仅适用于分批止盈的止损订单<br>`0`：不开启，默认值<br>`1`：开启“开仓价止损”                                       |

<aside class="notice">
newSz<br>
修改的数量<=该笔订单已成交数量时，该订单的状态会修改为完全成交状态。<br>
</aside>

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "clOrdId":"oktswap6",
            "ordId":"12345689",
            "reqId":"b12344",
            "sCode":"0",
            "sMsg":"",
            "subCode":""
        },
        {
            "clOrdId":"oktswap7",
            "ordId":"12344",
            "reqId":"b12344",
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
| > clOrdId    | String   | 用户自定义ID                  |
| > reqId      | String   | 用户自定义修改事件ID          |
| > sCode      | String   | 事件执行结果的code，0代表成功 |
| > sMsg       | String   | 事件执行失败时的msg           |
| > subCode    | String   | sCode 的子码。<br>当 sCode 为 0（请求成功）时，返回 `""`。<br>当 sCode 不为 0（请求失败）且存在子码时，返回对应的子码；若无子码，则返回 `""`。|
| inTime | String | REST网关接收请求时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123` <br>返回的时间是请求验证后的时间。|
| outTime | String | REST网关发送响应时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`|
