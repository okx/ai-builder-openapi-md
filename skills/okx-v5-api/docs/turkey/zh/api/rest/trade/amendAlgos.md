---
site: turkey
lang: zh
name: POST / 修改策略委托订单
method: POST
path: /api/v5/trade/amend-algos
module: trade
rest: https://tr.okx.com
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
