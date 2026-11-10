---
site: okex
lang: zh
name: POST / 修改策略委托订单
method: POST
path: /api/v5/trade/amend-algos
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 修改策略委托订单 [order-book-trading-algo-trading-post-amend-algo-order]

修改策略委托订单（仅支持止盈止损和计划委托订单，不包含、冰山委托、时间加权、移动止盈止损等订单）<br>

#### 限速：20次/2s
#### 限速规则：User ID + Instrument ID

#### HTTP请求

`POST /api/v5/trade/amend-algos`

> 请求示例

```shell
POST /api/v5/trade/amend-algos
body
{
    "algoId":"2510789768709120",
    "newSz":"2",
    "instId":"BTC-USDT"
}

```

#### 请求参数

| 参数名 | 类型   | 是否必须  | 描述                                                              |
| :----- | :----- |:------|:----------------------------------------------------------------|
| instId                | String    | 是     | 产品ID                                                            |
| algoId                | String    | 可选    | 策略委托单ID<br>`algoId`和`algoClOrdId`必须传一个，若传两个，以`algoId`为主         |
| algoClOrdId           | String    | 可选    | 客户自定义策略订单ID<br>`algoId`和`algoClOrdId`必须传一个，若传两个，以`algoId`为主     |
| cxlOnFail             | Boolean   | 否     | 当订单修改失败时，该订单是否需要自动撤销。默认为`false`<br>`false`：不自动撤单<br>`true`：自动撤单 |
| reqId                 | String    | 否     | 用户自定义修改事件ID，字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间              |
| newSz                 | String    | 可选    | 修改的新数量，必须大于0。                                                   |

<b>止盈止损</b>

| 参数名 | 类型   | 是否必须  | 描述                                                              |
| :----- | :----- |:------|:----------------------------------------------------------------|
| newTpTriggerPx        | String    | 可选    | 止盈触发价<br>如果止盈触发价或者委托价为0，那代表删除止盈                                 |
| newTpOrdPx            | String    | 可选    | 止盈委托价<br>委托价格为-1时，执行市价止盈                                        |
| newSlTriggerPx        | String    | 可选    | 止损触发价<br>如果止损触发价或者委托价为0，那代表删除止损                                 |
| newSlOrdPx            | String    | 可选    | 止损委托价<br>委托价格为-1时，执行市价止损                                        |
| newTpTriggerPxType    | String    | 可选    | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格           |
| newSlTriggerPxType    | String    | 可选    | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格           |

<b>计划委托</b>

| 参数名                 | 类型              | 是否必须  | 描述                                                                                                   |
|:--------------------|:----------------|:------|:-----------------------------------------------------------------------------------------------------|
| newTriggerPx           | String          | 是     | 修改后的触发价格                                                                                             |
| newOrdPx             | String          | 是     | 修改后的委托价格 <br>委托价格为`-1`时，执行市价委托                                                                           |
| newTriggerPxType       | String          | 否     | 修改后的计划委托触发价格类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为`last`                                |
| attachAlgoOrds      | Array of objects | 否     | 修改附带止盈止损或移动止盈止损订单信息<br>适用于`合约模式/跨币种保证金模式/组合保证金模式`                                                           |
| > newTpTriggerPx       | String          | 否     | 止盈触发价，如果填写此参数，必须填写`止盈委托价`                                                                            |
| > newTpTriggerRatio    | String          | 否     | 止盈触发比例，0.3 代表 30% <br> 仅适用于`交割`/`永续`合约<br> `newTpTriggerPx` 和 `newTpTriggerRatio` 只能传入其中一个<br> 如果主单为买入订单，必须大于 0，如果主单为卖出订单，必须处于 -1 和 0 之间。0 代表删除止盈。                                                                           |
| > newTpTriggerPxType   | String          | 否     | 修改后的止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为`last`                                   |
| > newTpOrdPx           | String          | 否     | 止盈委托价，如果填写此参数，必须填写`止盈触发价`<br>委托价格为`-1`时，执行市价止盈                                                       |
| > newSlTriggerPx       | String          | 否     | 止损触发价，如果填写此参数，必须填写`止损委托价`                                                                            |
| > newSlTriggerRatio    | String          | 否     | 止损触发比例，0.3 代表 30% <br> 仅适用于`交割`/`永续`合约<br> `newSlTriggerPx` 和 `newSlTriggerRatio` 只能传入其中一个<br> 如果主单为买入订单，必须处于 0 和 1 之间，如果主单为卖出订单，必须大于 0。0 代表删除止损。                                                                           |
| > newSlTriggerPxType   | String          | 否     | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为`last`                                   |
| > newSlOrdPx           | String          | 否     | 止损委托价，如果填写此参数，必须填写`止损触发价`<br>委托价格为`-1`时，执行市价止损                                                       |
| > newCallbackRatio    | String | 可选 | 新的回调幅度比例，如 `0.05` 代表 5%。<br>`newCallbackRatio` 和 `newCallbackSpread` 只能传入其中一个。<br>仅适用于 `ordType` = `move_order_stop` |
| > newCallbackSpread   | String | 可选 | 新的回调幅度价距。<br>`newCallbackRatio` 和 `newCallbackSpread` 只能传入其中一个。<br>仅适用于 `ordType` = `move_order_stop` |
| > newActivePx         | String | 否          | 新的激活价格。<br>仅适用于 `ordType` = `move_order_stop` |
| advChaseParams      | Array of objects | 条件必填 | 待修改的追逐参数。仅适用于 `advanceOrdType` 为 `chase` 的挂单中计划委托。 |
| > newChaseVal       | String | 条件必填 | 新的追逐值。非负数，按订单已有（不可修改）的 `chaseType` 解释。不可越过原 `chaseVal` 的 `0` ↔ 非 `0` 边界——直接跟随买一价/卖一价（`0`）与设置距离（大于 `0`）两种模式不可互换。 |
| > newMaxChaseVal    | String | 条件必填 | 新的最大追逐距离值。须为正数，按已有（不可修改）的 `maxChaseType` 解释。仅在已启用最大追逐距离时适用。 |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "algoClOrdId":"algo_01",
            "algoId":"2510789768709120",
            "reqId":"po103ux",
            "sCode":"0",
            "sMsg":""
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                      |
| :--------- | :------- | :---------------------------- |
| algoId     | String   | 订单ID                                                                                  |
| algoClOrdId| String   | 客户自定义策略订单ID                                                                      |
| reqId      | String   | 用户自定义修改事件ID，字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间        |
| sCode      | String   | 事件执行结果的code，0代表成功                                                              |
| sMsg       | String   | 事件执行失败时的msg                                                                       |
