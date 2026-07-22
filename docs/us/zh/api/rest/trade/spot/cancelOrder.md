---
site: us
lang: zh
name: POST / 撤单
method: POST
path: /api/v5/trade/cancel-order
module: trade
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### POST / 撤单 [order-book-trading-trade-post-cancel-order]

撤销之前下的未完成订单。

#### 限速：60次/2s
#### 限速规则：User ID + Instrument ID

#### HTTP请求

`POST /api/v5/trade/cancel-order`

> 请求示例

```shell
POST /api/v5/trade/cancel-order
body
{
    "ordId":"590908157585625111",
    "instId":"BTC-USDT"
}

```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# 撤单
result = tradeAPI.cancel_order(instId="BTC-USDT", ordId = "590908157585625111")
print(result)
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                           |
| :------ | :----- | :------- |:---------------------------------------------|
| instId  | String | 是       | 产品ID，如 `BTC-USDT`                            |
| ordId   | String | 可选     | 订单ID， `ordId`和`clOrdId`必须传一个，若传两个，以`ordId`为主 |
| clOrdId | String | 可选     | 用户自定义ID                                      |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "clOrdId":"oktswap6",
            "ordId":"12345689",
            "sCode":"0",
            "sMsg":""
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
| > sCode      | String   | 事件执行结果的code，0代表成功 |
| > sMsg       | String   | 事件执行失败时的msg           |
| inTime | String | REST网关接收请求时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123` <br>返回的时间是请求验证后的时间。|
| outTime | String | REST网关发送响应时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`|

<aside class="notice">
撤单返回sCode等于0不能严格认为该订单已经被撤销，只表示您的撤单请求被系统服务器所接受，撤单结果以订单频道推送的状态或者查询订单状态为准<br>
</aside>
