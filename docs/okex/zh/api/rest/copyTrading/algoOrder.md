---
site: okex
lang: zh
name: POST / 带单或跟单仓位止盈止损
method: POST
path: /api/v5/copytrading/algo-order
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 带单或跟单仓位止盈止损 [order-book-trading-copy-trading-post-place-lead-stop-order]

为当前未平仓的带单仓位设置止盈止损。

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/copytrading/algo-order`

> 请求示例

```shell
POST /api/v5/copytrading/algo-order
body
{
    "subPosId": "518541406042591232",
    "tpTriggerPx": "10000"
}
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                                   |
| :------ | :----- |:-----|:---------------------------------------------------------------------|
| instType     | String | 否       | 产品类型<br>SPOT：币币<br>SWAP：永续合约，默认值                                   |
| subPosId  | String | 是    | 带单或者跟单仓位ID                                                           |
| tpTriggerPx  | String | 可选    | 止盈触发价，tpTriggerPx 和 slTriggerPx 至少需要填写一个<br>如果止盈触发价为0，那代表删除止盈。                  |
| slTriggerPx  | String | 可选    | 止损触发价，<br>如果止损触发价为0，那代表删除止损                                                     |
| tpOrdPx     | String | 否       | 止盈委托价<br>委托价格为-1时，执行市价止盈，默认为市价止盈<br>仅适用于现货交易员                                   |
| slOrdPx     | String | 否       | 止损委托价<br>委托价格为-1时，执行市价止损，默认为市价止损<br>仅适用于现货交易员                                        |
| tpTriggerPxType  | String | 否    | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为last |
| slTriggerPxType  | String | 否    | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为last |
| tag     | String | 否       | 订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间。                     |
| subPosType     | String | 否       | 数据的类型<br>`lead`: 带单，默认值<br>`copy`: 跟单                             |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "subPosId": "518560559046594560",
            "tag":""
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**  | **类型** | **描述**                                                                                                                                                                                       |
| :---------- | :------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| subPosId      | String   | 带单或者跟单仓位ID                                                                                                                                                                                         |
| tag      | String   | 订单标签                                                                                                                                                                                       |
