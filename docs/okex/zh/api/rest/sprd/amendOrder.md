---
site: okex
lang: zh
name: 修改订单
method: POST
path: /api/v5/sprd/amend-order
module: sprd
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 修改订单 [spread-trading-rest-api-amend-order]

修改当前未成交的挂单<br>

#### 限速：20次/2s

#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/sprd/amend-order`

> 请求示例

```shell
POST /api/v5/sprd/amend-order
body
{
    "ordId":"2510789768709120",
    "newSz":"2"
}

```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                                                              |
| --- | --- | --- | --- |
| ordId | String | 可选 | 订单ID， `ordId`和`clOrdId`必须传一个，若传两个，以`ordId`为主 |
| clOrdId | String | 可选 | 用户自定义order ID |
| reqId | String | 否 | 用户自定义修改事件ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。 |
| newSz | String | 可选 | 修改的新数量，对于部分成交订单，该数量应包含已成交数量。 <br>`newSz` 和 `newPx`不可同时为空。 |
| newPx | String | 可选 | 修改后的新价格。<br>`newSz` 和 `newPx`不可同时为空。 |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
         "clOrdId":"",
         "ordId":"12344",
         "reqId":"b12344",
         "sCode":"0",
         "sMsg":""
        }
    ]
}

```

#### 返回参数

| 参数名 | 类型 | 描述                      |
| --- | --- | --- |
| ordId | String | 订单ID |
| clOrdId | String | 用户自定义order ID |
| reqId | String | 用户自定义修改事件ID |
| sCode | String | 事件执行结果的code，0代表成功 |
| sMsg | String | 事件执行失败或成功时的msg |

<aside class="notice">
newSz<br>
若修改订单时，订单修改后的新数量小于或等于 (accFillSz + canceledSz + pendingSettleSz)，在 pendingSettleSz 结算后，订单状态会根据 canceledSz 的不同而不同。当 canceledSz = 0，订单状态将被改为 filled；当 canceledSz > 0，订单状态将被改为 canceled。
</aside>

<aside class="notice">
修改订单返回sCode等于0不能严格认为该订单已经被修改，只表示您的修改订单请求被系统服务器所接受，改单结果以订单频道推送的状态或者查询订单状态为准
</aside>
