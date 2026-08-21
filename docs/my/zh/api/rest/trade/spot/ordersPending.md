---
site: my
lang: zh
name: GET / 获取未成交订单列表
method: GET
path: /api/v5/trade/orders-pending
module: trade
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### GET / 获取未成交订单列表 [order-book-trading-trade-get-order-list]

获取当前账户下所有未成交订单信息

#### 限速：60次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/trade/orders-pending`

> 请求示例

```shell
GET /api/v5/trade/orders-pending?ordType=post_only,fok,ioc&instType=SPOT

```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# 查询所有未成交订单
result = tradeAPI.get_order_list(
    instType="SPOT",
    ordType="post_only,fok,ioc"
)
print(result)
```

#### 请求参数

| 参数名            | 类型   | 是否必须 | 描述                                                                                                |
|:---------------| :----- | :------- |:--------------------------------------------------------------------------------------------------|
| instType       | String | 否       | 产品类型<br>`SPOT`：币币                                                                                 |
| instId         | String | 否       | 产品ID，如 `BTC-USDT`                                                                                 |
| ordType        | String | 否       | 订单类型<br>`market`：市价单<br>`limit`：限价单<br>`post_only`：只做maker单<br>`fok`：全部成交或立即取消<br>`ioc`：立即成交并取消剩余<br>`rpi`：Retail Price Improvement 订单<br>`elp`：流动性增强计划订单（已弃用，请使用 `rpi`。2026年10月31日前仍可使用。） |
| state          | String | 否       | 订单状态<br>`live`：等待成交<br>`partially_filled`：部分成交                                                    |
| after          | String | 否       | 请求此ID之前（更旧的数据）的分页内容，传的值为对应接口的`ordId`                                                              |
| before         | String | 否       | 请求此ID之后（更新的数据）的分页内容，传的值为对应接口的`ordId`                                                              |
| limit          | String | 否       | 返回结果的数量，最大为100，默认100条                                                                             |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "accFillSz": "0",
            "algoClOrdId": "",
            "algoId": "",
            "attachAlgoClOrdId": "",
            "attachAlgoOrds": [],
            "avgPx": "",
            "cTime": "1724733617998",
            "cancelSource": "",
            "cancelSourceReason": "",
            "category": "normal",
            "ccy": "",
            "clOrdId": "",
            "fee": "0",
            "feeCcy": "BTC",
            "fillPx": "",
            "fillSz": "0",
            "fillTime": "",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "isTpLimit": "false",
            "lever": "",
            "linkedAlgoOrd": {
                "algoId": ""
            },
            "ordId": "1752588852617379840",
            "ordType": "post_only",
            "pnl": "0",
            "posSide": "net",
            "px": "13013.5",
            "pxType": "",
            "pxUsd": "",
            "pxVol": "",
            "quickMgnType": "",
            "rebate": "0",
            "rebateCcy": "USDT",
            "reduceOnly": "false",
            "side": "buy",
            "slOrdPx": "",
            "slTriggerPx": "",
            "slTriggerPxType": "",
            "source": "",
            "state": "live",
            "stpId": "",
            "stpMode": "cancel_maker",
            "sz": "0.001",
            "tag": "",
            "tdMode": "cash",
            "tgtCcy": "",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "tpTriggerPxType": "",
            "tradeId": "",
            "uTime": "1724733617998"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**                | **类型**          | **描述**                                                                                                                                                                                                                                                                           |
|:-----------------------|:----------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType               | String          | 产品类型                                                                                                                                                                                                                                                                             |
| instId                 | String          | 产品ID                                                                                                                                                                                                                                                                             |
| tgtCcy                 | String          | 币币市价单委托数量`sz`的单位<br>`base_ccy`：交易货币<br>`quote_ccy`：计价货币<br>仅适用于`币币`市价订单<br>默认买单为`quote_ccy`，卖单为`base_ccy`                                                                                                                                                                        |
| ccy                    | String          | 保证金币种，适用于`逐仓杠杆`及`合约模式`下的`全仓杠杆`订单以及交割、永续和期权合约订单。                                                                                                                                                                                                                                                   |
| ordId                  | String          | 订单ID                                                                                                                                                                                                                                                                             |
| clOrdId                | String          | 客户自定义订单ID                                                                                                                                                                                                                                                                        |
| tag                    | String          | 订单标签                                                                                                                                                                                                                                                                             |
| px                     | String          | 委托价格，对于期权，以币(如BTC, ETH)为单位                                                                                                                                                                                                                                                       |
| pxUsd                  | String          | 期权价格，以USD为单位 <br>仅适用于期权，其他业务线返回空字符串""                                                                                                                                                                                                                                            |
| pxVol                  | String          | 期权订单的隐含波动率 <br>仅适用于期权，其他业务线返回空字符串""                                                                                                                                                                                                                                              |
| pxType                 | String          | 期权的价格类型 <br>`px`：代表按价格下单，单位为币 (请求参数 px 的数值单位是BTC或ETH) <br>`pxVol`：代表按pxVol下单 <br>`pxUsd`：代表按照pxUsd下单，单位为USD (请求参数px 的数值单位是USD)                                                                                                                                                   |
| sz                     | String          | 委托数量                                                                                                                                                                                                                                                                             |
| pnl                    | String          | 收益，适用于有成交的平仓订单，其他情况均为0                                                                                                                                                                                                                                                           |
| ordType                | String          | 订单类型<br>`market`：市价单<br>`limit`：限价单 <br>`post_only`：只做maker单 <br>`fok`：全部成交或立即取消 <br>`ioc`：立即成交并取消剩余 <br>`optimal_limit_ioc`：市价委托立即成交并取消剩余（仅适用交割、永续）<br>`mmp`：做市商保护(仅适用于组合保证金账户模式下的期权订单)<br>`mmp_and_post_only`：做市商保护且只做maker单(仅适用于组合保证金账户模式下的期权订单) <br>`op_fok`：期权简选（全部成交或立即取消）<br>`rpi`：Retail Price Improvement 订单<br>`elp`：流动性增强计划订单（已弃用，请使用 `rpi`。2026年10月31日前仍可使用。） |
| side                   | String          | 订单方向                                                                                                                                                                                                                                                                             |
| posSide                | String          | 持仓方向                                                                                                                                                                                                                                                                             |
| tdMode                 | String          | 交易模式                                                                                                                                                                                                                                                                             |
| accFillSz              | String          | 累计成交数量                                                                                                                                                                                                                                                                           |
| fillPx                 | String          | 最新成交价格。如果还没成交，系统返回""。                                                                                                                                                                                                                                                            |
| tradeId                | String          | 最新成交ID                                                                                                                                                                                                                                                                           |
| fillSz                 | String          | 最新成交数量                                                                                                                                                                                                                                                                           |
| fillTime               | String          | 最新成交时间                                                                                                                                                                                                                                                                           |
| avgPx                  | String          | 成交均价。如果还没成交，系统返回`0`。                                                                                                                                                                                                                                                             |
| state                  | String          | 订单状态<br>`live`：等待成交 <br>`partially_filled`：部分成交<br>                                                                                                                                                                                                                              |
| lever                  | String          | 杠杆倍数，0.01到125之间的数值，仅适用于 `币币杠杆/交割/永续`                                                                                                                                                                                                                                             |
| attachAlgoClOrdId      | String          | 下单附带止盈止损或移动止盈止损时，客户自定义的策略订单ID                                                                                                                                                                                                                                                           |
| tpTriggerPx            | String          | 止盈触发价                                                                                                                                                                                                                                                                            |
| tpTriggerPxType        | String          | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                                                                                                                                                                                            || tpOrdPx     | String   | 止盈委托价                                                   |
| slTriggerPx            | String          | 止损触发价                                                                                                                                                                                                                                                                            |
| slTriggerPxType        | String          | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                                                                                                                                                                                            |
| slOrdPx                | String          | 止损委托价                                                                                                                                                                                                                                                                            |
| tpOrdPx                | String          | 止盈委托价                                                                                                                                                                                                                                                                            |
| attachAlgoOrds         | Array of objects | 附带止盈止损或移动止盈止损订单信息                                                                                                                                                                                                                                                                       |
| > attachAlgoId         | String          | 附带止盈止损或移动止盈止损的订单ID，改单时，可用来标识该笔附带止盈止损订单。下附带策略委托单时，该值不会传给 algoId                                                                                                                                                                                                                                      |
| > attachAlgoClOrdId    | String          | 下单附带止盈止损或移动止盈止损时，客户自定义的策略订单ID                                                                                                                                                                                                                                                           |
| > tpTriggerPx          | String          | 止盈触发价                                                                                                                                                                                                                                                                            |
| > tpTriggerPxType      | String          | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                                                                                                                                                                                            |
| > tpOrdPx              | String          | 止盈委托价                                                                                                                                                                                                                                                                            |
| > slTriggerPx          | String          | 止损触发价                                                                                                                                                                                                                                                                            |
| > slTriggerPxType          | String          | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                                                                                                                                                                                            |
| > slOrdPx              | String          | 止损委托价                                                                                                                                                                                                                                                                            |
| > sz                   | String          | 张数。仅适用于“多笔止盈”的止盈订单                                                                                                                                                                                                                                                               |
| > amendPxOnTriggerType | String          | 是否启用开仓价止损，仅适用于分批止盈的止损订单<br>`0`：不开启，默认值 <br>`1`：开启                                                                                                                                                                                                                                |
| stpId                  | String          | ~~自成交保护ID<br>如果自成交保护不适用则返回""~~（已弃用）                                                                                                                                                                                                                                              |
| stpMode                | String          | 自成交保护模式                                                                                                                                                                                                                                                                          |
| feeCcy                 | String          | 交易手续费币种                                                                                                                                                                                                                                                                          |
| fee                    | String          | 手续费与返佣<br>对于币币和杠杆，为订单交易累计的手续费，平台向用户收取的交易手续费，为负数。如： -0.01<br>对于交割、永续和期权，为订单交易累计的手续费和返佣                                                                                                                                                                                            |
| rebateCcy              | String          | 返佣金币种                                                                                                                                                                                                                                                                            |
| source                 | String          | 订单来源<br>`6`：计划委托策略触发后的生成的普通单<br>`7`：止盈止损策略触发后的生成的普通单<br>`13`：策略委托单触发后的生成的普通单<br>`25`：移动止盈止损策略触发后的生成的普通单                                                                                                                                                                          |
| rebate                 | String          | 返佣金额，仅适用于币币和杠杆，平台向达到指定lv交易等级的用户支付的挂单奖励（返佣），如果没有返佣金，该字段为“”。手续费返佣为`正数`，如：`0.01`                                                                                                                                                                                                    |
| category               | String          | 订单种类<br>`normal`：普通委托                                                                                                                                                                                                                                                            |
| reduceOnly             | String          | 是否只减仓，`true` 或 `false`                                                                                                                                                                                                                                                           |
| quickMgnType           | String          | ~~一键借币类型，仅适用于杠杆逐仓的一键借币模式<br>`manual`：手动，`auto_borrow`：自动借币，`auto_repay`：自动还币~~（已弃用）                                                                                                                                                                                               |
| algoClOrdId            | String          | 客户自定义策略订单ID。策略订单触发，且策略单有`algoClOrdId`是有值，否则为"",                                                                                                                                                                                                                                  |
| algoId                 | String          | 策略委托单ID，策略订单触发时有值，否则为""                                                                                                                                                                                                                                                          |
| uTime                  | String          | 订单状态更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                                                                                                                                         |
| cTime                  | String          | 订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                                                                                                                                           |
| cancelSource           | String          | 订单取消来源的原因枚举值代码                                                                                                                                                                                                                                                                     |
| cancelSourceReason     | String          | 订单取消来源的对应具体原因                                                                                                                                                                                                                                                                      |
