---
site: okex
lang: zh
name: 撤单
method: POST
path: /api/v5/sprd/cancel-order
module: sprd
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 撤单 [spread-trading-rest-api-cancel-order]

撤销之前下的未完成订单。

#### 限速：20次/2s

#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/sprd/cancel-order`

> 请求示例
> 

```shell
POST /api/v5/sprd/cancel-order
body
{
    "ordId":"2510789768709120"
}

```
```python
import okx.SpreadTrading as SpreadTrading

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

spreadAPI = SpreadTrading.SpreadTradingAPI(apikey, secretkey, passphrase, False, flag)

# 撤单
result = spreadAPI.cancel_order(ordId='1905309079888199680')
print(result)
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
| --- | --- | --- | --- |
| ordId | String | 可选 | 订单ID， `ordId`和`clOrdId`必须传一个，若传两个，以`ordId`为主 |
| clOrdId | String | 可选 | 用户自定义ID |

> 返回示例
> 

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "clOrdId": "oktswap6",
            "ordId": "12345689",
            "sCode": "0",
            "sMsg": ""
        }
    ]
}

```

#### 返回参数

| 参数名 | 类型 | 描述 |
| --- | --- | --- |
| ordId | String | 订单ID |
| clOrdId | String | 客户自定义订单ID |
| sCode | String | 事件执行结果的code，0代表成功 |
| sMsg | String | 事件执行失败时的msg |

<aside class="notice">
撤单返回sCode等于0不能严格认为该订单已经被撤销，只表示您的撤单请求被系统服务器所接受，撤单结果以订单频道推送的状态或者查询订单状态为准
</aside>
