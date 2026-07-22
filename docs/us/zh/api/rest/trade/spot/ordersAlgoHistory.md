---
site: us
lang: zh
name: GET / 获取历史策略委托单列表
method: GET
path: /api/v5/trade/orders-algo-history
module: trade
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### GET / 获取历史策略委托单列表 [order-book-trading-algo-trading-get-algo-order-history]

获取最近3个月当前账户下所有策略委托单列表

#### 限速：20次/2s 
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/trade/orders-algo-history`

> 请求示例

```shell
GET /api/v5/trade/orders-algo-history?ordType=conditional&state=effective
```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# 查询 单向止盈止损 历史订单
result = tradeAPI.order_algos_history(
    state="effective",
    ordType="conditional"
)
print(result)
```

#### 请求参数

| 参数名      | 类型     | 是否必须 | 描述                                                                                                                            |
|:---------|:-------|:-----|:------------------------------------------------------------------------------------------------------------------------------|
| ordType  | String | 是    | 订单类型<br>`conditional`：单向止盈止损<br>`oco`：双向止盈止损<br>`trigger`：计划委托<br>`move_order_stop`：移动止盈止损 |
| state    | String | 可选   | 订单状态<br>`effective`：已生效<br>`canceled`：已经撤销<br>`order_failed`：委托失败<br>`state`和`algoId`必填且只能填其一                                 |
| algoId   | String | 可选   | 策略委托单ID                                                                                                                       |
| instType | String | 否    | 产品类型<br>`SPOT`：币币                                                             |
| instId   | String | 否    | 产品ID，`BTC-USDT`                                                                                                               |
| after    | String | 否    | 请求此ID之前（更旧的数据）的分页内容，传的值为对应接口的`algoId`                                                                                         |
| before   | String | 否    | 请求此ID之后（更新的数据）的分页内容，传的值为对应接口的`algoId`                                                                                         |
| limit    | String | 否    | 返回结果的数量，最大为100，默认100条                                                                                                         |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "activePx": "",
            "actualPx": "",
            "actualSide": "buy",
            "actualSz": "0",
            "algoClOrdId": "",
            "algoId": "681096944655273984",
            "amendPxOnTriggerType": "",
            "attachAlgoOrds": [],
            "cTime": "1708658165774",
            "callbackRatio": "",
            "callbackSpread": "",
            "ccy": "",
            "clOrdId": "",
            "closeFraction": "",
            "failCode": "",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "last": "51014.6",
            "lever": "",
            "moveTriggerPx": "",
            "ordId": "",
            "ordIdList": [],
            "ordPx": "-1",
            "ordType": "trigger",
            "posSide": "net",
            "pxLimit": "",
            "pxSpread": "",
            "pxVar": "",
            "quickMgnType": "",
            "reduceOnly": "false",
            "side": "buy",
            "slOrdPx": "",
            "slTriggerPx": "",
            "slTriggerPxType": "",
            "state": "canceled",
            "sz": "10",
            "szLimit": "",
            "tag": "",
            "tdMode": "cash",
            "tgtCcy": "",
            "timeInterval": "",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "tpTriggerPxType": "",
            "triggerPx": "100",
            "triggerPxType": "last",
            "triggerTime": ""
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**              | **类型**          | **描述**                                                                                                                                    |
|:---------------------|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------|
| instType             | String          | 产品类型                                                                                                                                      |
| instId               | String          | 产品ID                                                                                                                                      |
| ccy                    | String          | 保证金币种，适用于`逐仓杠杆`及`合约模式`下的`全仓杠杆`订单以及交割、永续和期权合约订单。                                                                                                                                                                                                                                                   |
| ordId                | String          | 最新一笔订单ID                                                                                                                                  |
| ordIdList            | Array of strings          | 订单ID列表，当止盈止损存在市价拆单时，会有多个。                                                                                                                 |
| algoId               | String          | 策略委托单ID                                                                                                                                   |
| clOrdId              | String          | 客户自定义订单ID                                                                                                                                 |
| sz                   | String          | 委托数量                                                                                                                                      |
| closeFraction        | String          | 策略委托触发时，平仓的百分比。1 代表100%                                                                                                                   |
| ordType              | String          | 订单类型                                                                                                                                      |
| side                 | String          | 订单方向                                                                                                                                      |
| posSide              | String          | 持仓方向                                                                                                                                      |
| tdMode               | String          | 交易模式                                                                                                                                      |
| tgtCcy               | String          | 币币市价单委托数量`sz`的单位<br>`base_ccy`：交易货币<br>`quote_ccy`：计价货币<br>仅适用于`币币`市价订单<br>默认买单为`quote_ccy`，卖单为`base_ccy`                                    |
| state                | String          | 订单状态 <br>`effective`：已生效<br>`canceled`：已撤销 <br>`order_failed`：委托失败 <br>`partially_failed`：部分委托失败                                          |
| lever                | String          | 杠杆倍数，0.01到125之间的数值<br>仅适用于`币币杠杆`/`交割`/`永续`                                                                                                |
| tpTriggerPx          | String          | 止盈触发价                                                                                                                                     |
| tpTriggerPxType      | String          | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                                                     |
| tpOrdPx              | String          | 止盈委托价                                                                                                                                     |
| slTriggerPx          | String          | 止损触发价                                                                                                                                     |
| slTriggerPxType      | String          | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                                                     |
| slOrdPx              | String          | 止损委托价                                                                                                                                     |
| triggerPx            | String          | 计划委托触发价格                                                                                                                                  |
| triggerPxType        | String          | 计划委托委托价格类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                                                                                                 |
| ordPx                | String          | 计划委托委托价格                                                                                   |
| actualSz             | String          | 实际委托量                                                                                                                                     |
| actualPx             | String          | 实际委托价                                                                                                                                     |
| actualSide           | String          | 实际触发方向<br>`tp`：止盈<br>`sl`：止损<br/>仅适用于`单向止盈止损委托`和`双向止盈止损委托`                                                                                                              |
| triggerTime          | String          | 策略委托触发时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                  |
| pxVar                | String          | 价格比例<br>仅适用于`冰山委托`和`时间加权委托`                                                                                                               |
| pxSpread             | String          | 价距<br>仅适用于`冰山委托`和`时间加权委托`                                                                                                                 |
| szLimit              | String          | 单笔数量<br>仅适用于`冰山委托`和`时间加权委托`                                                                                                               |
| pxLimit              | String          | 挂单限制价<br>仅适用于`冰山委托`和`时间加权委托`                                                                                                              |
| tag                  | String          | 订单标签                                                                                                                                      |
| timeInterval         | String          | 下单间隔<br>仅适用于`时间加权委托`                                                                                                                      |
| callbackRatio        | String          | 回调幅度的比例<br>仅适用于`移动止盈止损`                                                                                                                   |
| callbackSpread       | String          | 回调幅度的价距<br>仅适用于`移动止盈止损`                                                                                                                   |
| activePx             | String          | 移动止盈止损激活价格<br>仅适用于`移动止盈止损`                                                                                                                |
| moveTriggerPx        | String          | 移动止盈止损触发价格<br>仅适用于`移动止盈止损`                                                                                                                |
| reduceOnly           | String          | 是否只减仓<br>`true`或`false`                                                                                                                   |
| quickMgnType         | String          | ~~一键借币类型，仅适用于杠杆逐仓的一键借币模式<br>`manual`：手动，`auto_borrow`：自动借币，`auto_repay`：自动还币~~（已弃用）                                                    |
| last                 | String          | 下单时的最新成交价                                                                                                                                 |
| failCode             | String          | 代表策略触发失败的原因，已撤销和已生效时为""，委托失败时有值，如 51008；<br>仅适用于单向止盈止损委托、双向止盈止损委托、移动止盈止损委托、计划委托。                                                          |
| algoClOrdId          | String          | 客户自定义策略订单ID                                                                                                                               |
| amendPxOnTriggerType | String          | 是否启用开仓价止损，仅适用于分批止盈的止损订单<br>`0`：不开启，默认值 <br>`1`：开启                                                                                         |
| attachAlgoOrds       | Array of objects | 附带止盈止损或移动止盈止损订单信息<br>适用于`合约模式/跨币种保证金模式/组合保证金模式`                                                                                                |
| > attachAlgoClOrdId  | String          | 下单附带止盈止损或移动止盈止损时，客户自定义的策略订单ID，字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。<br>订单完全成交，下附带策略委托单时，该值会传给algoClOrdId。                                      |
| > tpTriggerPx        | String          | 止盈触发价，如果填写此参数，必须填写`止盈委托价`                                                                                                                 |
| > tpTriggerPxType    | String          | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为`last`                                                                        |
| > tpOrdPx            | String          | 止盈委托价，如果填写此参数，必须填写`止盈触发价`<br>委托价格为`-1`时，执行市价止盈                                                                                            |
| > slTriggerPx        | String          | 止损触发价，如果填写此参数，必须填写`止损委托价`                                                                                                                 |
| > slTriggerPxType    | String          | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为`last`                                                                        |
| > slOrdPx            | String          | 止损委托价，如果填写此参数，必须填写`止损触发价`<br>委托价格为`-1`时，执行市价止损                                                                                            |
| cTime                | String          | 订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                    |
