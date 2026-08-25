---
site: okex
lang: zh
name: WS / 改单
type: guide
---

### WS / 改单 [spread-trading-websocket-trade-api-ws-amend-order]

修改当前未成交的订单

#### 服务地址

/ws/v5/business (需要登录)

#### 限速：20次/2s
#### 限速规则：User ID

<aside class="notice">
同Nitro Spread`改单` REST API 共享限速
</aside>

> 请求示例

```shell
{
   "id":"1512",
   "op":"sprd-amend-order",
   "args":[
      {
         "ordId":"2510789768709120",
         "newSz":"2"
      }
   ]
}

```

#### 请求参数

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| id             | String | 是       | 消息的唯一标识 <br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。                                                                                                                                                                  |
| op             | String | 是       | 支持的业务操作，`sprd-amend-order`                                                                                                                                                                                                                                      |
| args           | Array of objects  | 是       | 请求参数                                                                                                                                                                                                                                                    |
| > ordId | String | 可选 | 订单ID <br>ordId 和 clOrdId必须传一个，若传两个，以 ordId 为主 |
| > clOrdId | String | 可选 | 由用户设置的订单ID |
| > reqId | String | 否 | 用户自定义修改事件ID <br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。 |
| > newSz | String | 可选 | 修改的新数量，对于部分成交订单，该数量应包含已成交数量。<br>`newSz` 或 `newPx`至少传一个。 |
| > newPx | String | 可选 | 修改后的新价格 |

> 成功返回示例

```json
{
  "id": "1512",
  "op": "sprd-amend-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
      "reqId": "b12344",
      "sCode": "0",
      "sMsg": ""
    }
  ],
  "code": "0",
  "msg": ""
}

```

> 失败返回示例

```json
{
  "id": "1512",
  "op": "sprd-amend-order",
  "data": [
    {
      "clOrdId": "",
      "ordId": "2510789768709120",
      "reqId": "b12344",
      "sCode": "5XXXX",
      "sMsg": "order not exist"
    }
  ],
  "code": "1",
  "msg": ""
}

```

> 格式错误返回示例

```json
{
  "id": "1512",
  "op": "sprd-amend-order",
  "data": [],
  "code": "60013",
  "msg": "Invalid args"
}

```

#### 返回参数

| 参数 | 类型 | 描述 |
| --- | --- | --- |
| id | String | 消息的唯一标识 |
| op | String | 操作 |
| code | String | 代码 |
| msg | String | 消息 |
| data | Array of objects | 请求成功后返回的数据 |
| > ordId | String | 订单ID |
| > clOrdId | String | 由用户设置的订单ID |
| > reqId | String | 用户自定义修改事件ID |
| > sCode | String | 订单状态码，0 代表成功 |
| > sMsg | String | 订单状态消息 |

<aside class="notice">
newSz<br>
若修改订单时，订单修改后的新数量小于或等于 (accFillSz + canceledSz + pendingSettleSz)，在 pendingSettleSz 结算后，订单状态会根据 canceledSz 的不同而不同。当 canceledSz = 0，订单状态将被改为 filled；当 canceledSz > 0，订单状态将被改为 canceled。
</aside>

<aside class="notice">
修改订单返回sCode等于0不能严格认为该订单已经被修改，只表示您的修改订单请求被系统服务器所接受，改单结果以订单频道推送的状态或者查询订单状态为准
</aside>
