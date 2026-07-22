---
site: okex
lang: zh
name: 获取历史订单记录（近三月)
method: GET
path: /api/v5/sprd/orders-history-archive
module: sprd
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取历史订单记录（近三月) [spread-trading-rest-api-get-orders-history-last-3-months]

获取最近三个月挂单，且完全成交的订单数据，包括三个月以前挂单，但近三个月才成交的订单数据。按照订单创建时间倒序排序。

#### 限速：20次/2s

#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/sprd/orders-history-archive`

> 请求示例
> 

```shell
GET /api/v5/sprd/orders-history-archive

```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述                                                                               |
| --- | --- | --- |----------------------------------------------------------------------------------|
| sprdId | String | 否 | spread ID，如BTC-USDT_BTC-USDT-SWAP                                                |
| ordType | String | 否 | 订单类型<br>`market`：市价单 <br>`limit`：限价单 <br>`post_only`：只做maker单 <br>`ioc`：立即成交并取消剩余 |
| state | String | 否 | 订单状态<br>`canceled`：撤单成功<br>`filled`：完全成交                                         |
| instType  | String | 否       | 产品类型<br>`SPOT`：币币<br>`FUTURES`:交割合约<br>`SWAP`：永续合约 <br>订单任意一条腿的spread包含相应产品类型，则返回 |
| instFamily| String | 否       | 交易品种，如 `BTC-USDT` <br>订单任意一条腿的spread包含相应交易品种，则返回                                   |
| beginId | String | 否 | 请求的起始订单ID，请求此ID之后（更新的数据）的分页内容，不包括 beginId                                        |
| endId | String | 否 | 请求的结束订单ID，请求此ID之前（更旧的数据）的分页内容，不包括 endId                                          |
| begin | String | 否 | 筛选的开始时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085`                                        |
| end | String | 否 | 筛选的结束时间戳，Unix 时间戳为毫秒数格式，如 `1597027383085`                                        |
| limit | String | 否 | 返回结果的数量，最大为100，默认100条                                                            |

> 返回示例
> 

```json
{
  "code": "0",
  "msg": "",
  "data": [
     {
      "sprdId": "BTC-USDT_BTC-UST-SWAP",
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
      "state": "cancelled",
      "avgPx": "0",
      "cancelSource": "",
      "uTime": "1597026383085",
      "cTime": "1597026383085"
    }
  ]
}
```

#### 返回参数

| 参数名 | 类型 | 描述 |
| --- | --- | --- |
| sprdId | String | spread ID，如BTC-USDT_BTC-USDT-SWAP |
| ordId | String | 订单ID |
| clOrdId | String | 客户自定义订单ID |
| tag | String | 订单标签 |
| px | String | 委托价格 |
| sz | String | 委托数量 |
| ordType | String | 订单类型<br>`market`：市价单 <br>`limit`：限价单 <br>`post_only`：只做maker单 <br>`ioc`：立即成交并取消剩余 |
| side | String | 订单方向 |
| fillSz | String | 最新成交数量 |
| fillPx | String | 最新成交价格 |
| tradeId | String | 最近成交ID |
| accFillSz | String | 累计成交数量 |
| pendingFillSz | String | 待成交数量（包括待结算数量） |
| pendingSettleSz | String | 待结算数量 |
| canceledSz | String | 被取消数量 |
| avgPx | String | 成交均价，如果成交数量为0，该字段为"0" |
| state | String | 订单状态<br>`canceled`：撤单成功<br>`filled`：完全成交 |
| cancelSource | String | 撤单原因<br>`0`: 系统撤单<br>`1`: 用户撤单 <br>`14`: 已撤单：IOC 委托订单未完全成交，仅部分成交，导致部分挂单被撤回<br>`15`: 已撤单：该订单委托价不在限价范围内<br> `20`: 系统倒计时撤单 <br>`31`: 当前只挂单订单 (Post only) 将会吃掉挂单深度<br> `32`: 自成交保护<br>`34`: 订单结算失败因为保证金不足 <br>`35`: 撤单因为其他订单保证金不足|
| uTime | String | 订单状态更新时间，Unix时间戳的毫秒数格式，如：`1597026383085` |
| cTime | String | 订单创建时间，Unix时间戳的毫秒数格式， 如 ： `1597026383085` |
