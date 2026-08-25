---
site: okex
lang: zh
name: 获取订单信息
method: GET
path: /api/v5/sprd/order
module: sprd
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取订单信息 [spread-trading-rest-api-get-order-details]

查订单信息

#### 限速：20次/2s

#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/sprd/order`

> 请求示例
> 

```shell
GET /api/v5/sprd/order?ordId=2510789768709120

```
```python
import okx.SpreadTrading as SpreadTrading

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

spreadAPI = SpreadTrading.SpreadTradingAPI(apikey, secretkey, passphrase, False, flag)

# 获取订单详情
result = spreadAPI.get_order_details(ordId='1905309079888199680')
print(result)
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
| --- | --- | --- | --- |
| ordId | String | 可选 | 订单ID，`ordId`和`clOrdId`必须传一个，若传两个，以`ordId`为主 |
| clOrdId | String | 可选 | 用户自定义ID，如果`clOrdId`关联了多个订单，只会返回最近的那笔订单 |

> 返回示例
> 

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "sprdId": "BTC-USD-SWAP_BTC-USD-200329",
      "ordId": "312269865356374016",
      "clOrdId": "b1",
      "tag": "",
      "px": "999",
      "sz": "3",
      "ordType": "limit",
      "side": "buy",
      "fillSz": "0",
      "fillPx": "",
      "tradeId": "",
      "accFillSz": "0",
      "pendingFillSz": "2",
      "pendingSettleSz": "1",
      "canceledSz": "1",
      "state": "live",
      "avgPx": "0",
      "cancelSource": "",
      "uTime": "1597026383085",
      "cTime": "1597026383085"
    }
  ]
}

```

#### 返回参数

| 参数名             | 类型 | 描述                                                                                                                                                                                                                                |
|-----------------| --- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| sprdId          | String | Spread ID                                                                                                                                                                                                                         |
| ordId           | String | 订单ID                                                                                                                                                                                                                              |
| clOrdId         | String | 客户自定义订单ID                                                                                                                                                                                                                         |
| tag             | String | 订单标签                                                                                                                                                                                                                              |
| px              | String | 委托价格                                                                                                                                                                                                                              |
| sz              | String | 委托数量                                                                                                                                                                                                                              |
| ordType         | String | 订单类型<br>`market`：市价单 <br>`limit`：限价单 <br>`post_only`：只做maker单 <br>`ioc`：立即成交并取消剩余                                                                                                                                                 |
| side            | String | 订单方向                                                                                                                                                                                                                              |
| fillSz          | String | 最新成交数量                                                                                                                                                                                                                            |
| fillPx          | String | 最新成交价格                                                                                                                                                                                                                            |
| tradeId         | String | 最近成交ID                                                                                                                                                                                                                            |
| accFillSz       | String | 累计成交数量                                                                                                                                                                                                                            |
| pendingFillSz   | String | 待成交数量（包括待结算数量）                                                                                                                                                                                                                    |
| pendingSettleSz | String | 待结算数量                                                                                                                                                                                                                             |
| canceledSz      | String | 被取消数量                                                                                                                                                                                                                             |
| avgPx           | String | 成交均价，如果成交数量为0，该字段为"0"                                                                                                                                                                                                             |
| state           | String | 订单状态<br>`canceled`：撤单成功<br>`live`：等待成交<br>`partially_filled`：部分成交<br>`filled`：完全成交                                                                                                                                                |
| cancelSource    | String | 撤单原因<br>`0`: 系统撤单<br>`1`: 用户撤单 <br>`14`: 已撤单：IOC 委托订单未完全成交，仅部分成交，导致部分挂单被撤回<br>`15`: 已撤单：该订单委托价不在限价范围内<br> `20`: 系统倒计时撤单 <br>`31`: 当前只挂单订单 (Post only) 将会吃掉挂单深度<br> `32`: 自成交保护<br>`34`: 订单结算失败因为保证金不足 <br>`35`: 撤单因为其他订单保证金不足<br>`44`：由于该币种的可用余额不足，无法在触发自动换币后进行兑换，您的订单已撤销，撤销订单后恢复的余额将用于自动换币。当该币种的总抵押借贷量达到平台抵押借贷风控上限时，则会触发自动换币。 |
| uTime           | String | 订单状态更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                                                                                          |
| cTime           | String | 订单创建时间，Unix时间戳的毫秒数格式， 如 `1597026383085`                                                                                                                                                                                           |

<aside class="notice">
订单数量等式: pendingFillSz + canceledSz + accFillSz = sz
</aside>
