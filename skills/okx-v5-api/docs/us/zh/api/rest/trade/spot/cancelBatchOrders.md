---
site: us
lang: zh
name: POST / 批量撤单
method: POST
path: /api/v5/trade/cancel-batch-orders
module: trade
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### POST / 批量撤单 [order-book-trading-trade-post-cancel-multiple-orders]

撤销未完成的订单，每次最多可以撤销20个订单。请求参数应该按数组格式传递。

#### 限速：300个/2s
#### 限速规则：User ID + Instrument ID

<aside class="notice">
与其他限速按接口调用次数不同，该接口限速按订单的总个数限速。如果单次批量请求中只有一个元素，则算在单个`撤单`限速中。
</aside>

#### HTTP请求

`POST /api/v5/trade/cancel-batch-orders`

> 请求示例

```shell
POST /api/v5/trade/cancel-batch-orders
body
[
    {
        "instId":"BTC-USDT",
        "ordId":"12312"
    },
    {
        "instId":"BTC-USDT",
        "ordId":"1212"
    }
]

```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                         |
| :------ | :----- | :------- | :----------------------------------------------------------- |
| instId  | String | 是       | 产品ID，如 `BTC-USDT`                              |
| ordId   | String | 可选     | 订单ID， `ordId`和`clOrdId`必须传一个，若传两个，以`ordId`为主 |
| clOrdId | String | 可选     | 用户自定义ID                                                 |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "clOrdId":"okt6",
            "ordId":"12345689",
            "sCode":"0",
            "sMsg":""
        },
        {
            "clOrdId":"okt7",
            "ordId":"12344",
            "sCode":"0",
            "sMsg":""
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                      |
| :--------- | :------- | :---------------------------- |
| ordId      | String   | 订单ID                        |
| clOrdId    | String   | 客户自定义订单ID              |
| sCode      | String   | 事件执行结果的code，0代表成功 |
| sMsg       | String   | 事件执行失败时的msg           |
