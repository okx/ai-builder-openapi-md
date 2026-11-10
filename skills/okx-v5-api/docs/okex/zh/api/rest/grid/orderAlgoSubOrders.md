---
site: okex
lang: zh
name: GET / 获取网格策略委托子订单信息
method: GET
path: /api/v5/tradingBot/grid/sub-orders
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取网格策略委托子订单信息 [order-book-trading-grid-trading-get-grid-algo-sub-orders]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/tradingBot/grid/sub-orders`

> 请求示例

```shell
GET /api/v5/tradingBot/grid/sub-orders?algoId=123456&type=live&algoOrdType=grid
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                                      |
| :-------- | :----- | :------- |:------------------------------------------------------------------------|
| algoId    | String | 是       | 策略订单ID                                                                  |
| algoOrdType | String | 是       | 策略订单类型<br>`grid`：现货网格委托<br>`contract_grid`：合约网格委托 |
| type | String | 是       | 子订单状态<br>`live`：未成交<br>`filled`：已成交                                     |
| groupId | String | 否       | 组ID                                                                     |
| after    | String | 否       | 请求此ID之前（更旧的数据）的分页内容，传的值为对应接口的`ordId`                                    |
| before    | String | 否       | 请求此ID之后（更新的数据）的分页内容，传的值为对应接口的`ordId`                                    |
| limit    | String | 否       | 返回结果的数量，最大为100，默认100条                                                   |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "accFillSz": "0",
            "algoClOrdId": "",
            "algoId": "448965992920907776",
            "algoOrdType": "grid",
            "avgPx": "0",
            "cTime": "1653347949771",
            "ccy": "",
            "ctVal": "",
            "fee": "0",
            "feeCcy": "USDC",
            "groupId": "3",
            "instId": "BTC-USDC",
            "instType": "SPOT",
            "lever": "0",
            "ordId": "449109084439187456",
            "ordType": "limit",
            "pnl": "0",
            "posSide": "net",
            "px": "30404.3",
            "rebate": "0",
            "rebateCcy": "USDT",
            "side": "sell",
            "state": "live",    
            "sz": "0.00059213",
            "tag": "",
            "tdMode": "cash",
            "uTime": "1653347949831"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**       | **类型**     | **描述**                                                                                                                  |
|:--------------|:-----------|:------------------------------------------------------------------------------------------------------------------------|
| algoId        | String     | 策略订单ID                                                                                                                  |
| algoClOrdId   | String     | 用户自定义策略ID                                                                                                               |
| instType      | String     | 产品类型                                                                                                                    |
| instId        | String     | 产品ID                                                                                                                    |
| algoOrdType   | String     | 策略订单类型<br>`grid`：现货网格委托<br>`contract_grid`：合约网格委托                                                 |
| groupId       | String     | 组ID                                                                                                                     |
| ordId         | String     | 子订单ID                                                                                                                   |
| cTime         | String     | 子订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                 |
| uTime         | String     | 子订单更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                 |
| tdMode        | String     | 子订单交易模式<br>`cross`：全仓<br>`isolated`：逐仓<br>`cash`：非保证金                                                                   |
| ccy           | String     | 保证金币种<br>仅适用于`合约模式`模式下的`全仓杠杆`订单                                                                                       |
| ordType       | String     | 子订单类型<br>`market`：市价单<br>`limit`：限价单<br>`ioc`：立即成交并取消剩余                                                                                    |
| sz            | String     | 子订单委托数量                                                                                                                 |
| state         | String     | 子订单状态<br>`canceled`：撤单成功<br>`live`：等待成交<br>`partially_filled`：部分成交<br>`filled`：完全成交<br>`cancelling`：撤单中                 |
| side          | String     | 子订单订单方向<br>`buy`：买<br>`sell`：卖                                                                                              |
| px            | String     | 子订单委托价格                                                                                                                 |
| fee           | String     | 子订单手续费数量                                                                                                                |
| feeCcy        | String     | 子订单手续费币种                                                                                                                |
| rebate        | String     | 子订单返佣数量                                                                                                                 |
| rebateCcy     | String     | 子订单返佣币种                                                                                                                 |
| avgPx         | String     | 子订单平均成交价格                                                                                                               |
| accFillSz     | String     | 子订单累计成交数量                                                                                                               |
| posSide       | String     | 子订单持仓方向<br>`net`：买卖模式                                                              |
| pnl           | String     | 子订单收益                                                                                                                   |
| ctVal         | String     | 合约面值<br>仅支持`FUTURES/SWAP`                                                                                               |
| lever         | String     | 杠杆倍数                                                                                                                    |
| tag           | String     | 订单标签                                                                                                                    |
