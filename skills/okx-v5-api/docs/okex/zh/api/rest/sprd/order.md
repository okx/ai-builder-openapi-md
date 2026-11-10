---
site: okex
lang: zh
name: 下单
method: POST
path: /api/v5/sprd/order
module: sprd
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 下单 [spread-trading-rest-api-place-order]

下单

#### 限速:：20次/ 2s

#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/sprd/order`

> 请求示例
> 

```shell
# 下价差订单
POST /api/v5/sprd/order
body
{
  "sprdId":"BTC-USDT_BTC-USDT-SWAP",
  "clOrdId":"b15",
  "side":"buy",
  "ordType":"limit",
  "px":"2.15",
  "sz":"2"
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

# 下单
result = spreadAPI.place_order(sprdId='BTC-USDT_BTC-USDT-SWAP',
                               clOrdId='b16',side='buy',ordType='limit',
                               px='2',sz='2')
print(result)
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
| --- | --- | --- | --- |
| sprdId | String | 是 | spread ID，如 BTC-USDT_BTC-USDT-SWAP |
| clOrdId | String | 否 | 客户自定义订单ID字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。 |
| tag | String | 否 | 订单标签字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间。 |
| side | String | 是 | 订单方向<br>`buy`：买，`sell`：卖 |
| ordType | String | 是 | 订单类型<br>`market`：市价单 <br>`limit`：限价单 <br>`post_only`：只做maker单 <br>`ioc`：立即成交并取消剩余|
| sz | String | 是 | 委托数量。反向价差的数量单位为USD，正向及混合价差为其对应`baseCcy`|
| px | String | 是 | 委托价格，仅适用于`limit`, `post_only`, `ioc`类型的订单 |

> 返回示例
> 

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "clOrdId": "b15",
      "ordId": "312269865356374016",
      "tag": "",
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
| tag | String | 订单标签 |
| sCode | String | 事件执行结果的code，0代表成功 |
| sMsg | String | 事件执行失败或成功时的msg |

<aside class="notice">
clOrdId <br>
clOrdId是用户自定义的唯一ID用来识别订单。如果在请求参数中传入了，那它一定会在返回参数内，并且可以用于查询订单，撤销订单，修改订单等接口。
clOrdId不能与当前所有的挂单的clOrdId重复
</aside>
<aside class="notice">
ordType<br>
订单类型，创建新订单时必须指定，您指定的订单类型将影响需要哪些订单参数和撮合系统如何执行您的订单，以下是有效的ordType：<br>
limit：限价单，要求指定sz 和 px <br>
post_only：限价委托，在下单那一刻只做maker，如果该笔订单的任何部分会吃掉当前挂单深度，则该订单将被全部撤销。 <br>
ioc：立即成交并取消剩余
</aside>
<aside class="notice">
sz <br>
反向价差(inverse spread)的数量单位是USD，与OKX订单簿相反.
</aside>
</aside>
