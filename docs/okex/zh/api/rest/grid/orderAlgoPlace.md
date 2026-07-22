---
site: okex
lang: zh
name: POST / 网格策略委托下单
method: POST
path: /api/v5/tradingBot/grid/order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 网格策略委托下单 [order-book-trading-grid-trading-post-place-grid-algo-order]

#### 限速：20次/2s
#### 限速规则：User ID + Instrument ID

#### HTTP请求

`POST /api/v5/tradingBot/grid/order-algo`

> 请求示例

```shell
# 现货网格下单
POST /api/v5/tradingBot/grid/order-algo
body
{
	"instId": "BTC-USDT",
	"algoOrdType": "grid",
	"maxPx": "5000",
	"minPx": "400",
	"gridNum": "10",
	"runType": "1",
	"quoteSz": "25",
	"triggerParams":[
      {
         "triggerAction":"stop",
         "triggerStrategy":"price",  
         "triggerPx":"1000"
      }
    ]
}

# 合约网格下单
POST /api/v5/tradingBot/grid/order-algo
body
{
	"instId": "BTC-USDT-SWAP",
	"algoOrdType": "contract_grid",
	"maxPx": "5000",
	"minPx": "400",
	"gridNum": "10",
	"runType": "1",
	"sz": "200", 
	"direction": "long",
	"lever": "2",
	"triggerParams":[
      {
         "triggerAction":"start", 
         "triggerStrategy":"rsi", 
         "timeframe":"30m",
         "thold":"10",
         "triggerCond":"cross",
         "timePeriod":"14"
      },
      {
         "triggerAction":"stop",
         "triggerStrategy":"price",
         "triggerPx":"1000",
         "stopType":"2"
      }
   ]
}

```

#### 请求参数

| 参数名                   | 类型               | 是否必须 | 描述                                                                                                                          |
|:----------------------|:-----------------|:-----|:----------------------------------------------------------------------------------------------------------------------------|
| instId                | String           | 是    | 产品ID，如`BTC-USDT`                                                                                                            |
| algoOrdType           | String           | 是    | 策略订单类型<br>`grid`：现货网格委托<br>`contract_grid`：合约网格委托                                                     |
| maxPx                 | String           | 是    | 区间最高价格                                                                                                                      |
| minPx                 | String           | 是    | 区间最低价格                                                                                                                      |
| gridNum               | String           | 是    | 网格数量                                                                                                                        |
| runType               | String           | 否    | 网格类型<br>`1`：等差，`2`：等比<br>默认为等差                                                                              |
| tpTriggerPx           | String           | 否    | 止盈触发价<br>适用于`现货网格`/`合约网格`                                                                                                   |
| slTriggerPx           | String           | 否    | 止损触发价<br>适用于`现货网格`/`合约网格`                                                                                                   |
| algoClOrdId           | String           | 否    | 用户自定义策略ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                                                        |
| tag                   | String           | 否    | 订单标签                                                                                                                        |
| profitSharingRatio   | String | 否       | 带单员分润比例，仅支持固定比例分润<br/>`0`,`0.1`,`0.2`,`0.3` |
| triggerParams         | Array of objects  | 否    | 信号触发参数<br>适用于`现货网格`/`合约网格`                                                                                                  |
| > triggerAction       | String           | 是    | 触发行为<br>`start`：网格启动<br>`stop`：网格停止                                                                                         |
| > triggerStrategy     | String           | 是    | 触发策略<br>`instant`：立即触发<br>`price`：价格触发<br>`rsi`：rsi指标触发<br>默认为`instant`                                                            |
| > delaySeconds        | String           | 否    | 延迟触发时间，单位为秒，默认为`0`                                                                                                          |
| > timeframe           | String           | 否    | K线种类<br>`3m`, `5m`, `15m`, `30m` (`m`代表分钟)<br>`1H`, `4H` (`H`代表小时)<br>`1D` (`D`代表天)<br>该字段只在`triggerStrategy`为`rsi`时有效      |
| > thold               | String           | 否    | 阈值<br>取值[1,100]的整数<br>该字段只在`triggerStrategy`为`rsi`时有效                                                                       |
| > triggerCond         | String           | 否    | 触发条件<br>`cross_up`：上穿<br>`cross_down`：下穿<br>`above`：上方<br>`below`：下方<br>`cross`：交叉<br>该字段只在`triggerStrategy`为`rsi`时有效       |
| > timePeriod          | String           | 否    | 周期<br>`14`<br>该字段只在`triggerStrategy`为`rsi`下有效                                                                               |
| > triggerPx           | String           | 否    | 触发价格<br>该字段只在`triggerStrategy`为`price`下有效                                                                                   |
| > stopType            | String           | 否    | 策略停止类型<br>现货 `1`：卖出交易币，`2`：不卖出交易币<br>合约网格 `1`：停止平仓，`2`：停止不平仓 <br>该字段只在`triggerAction`为`stop`时有效                        |

现货网格

| 参数名      | 类型   | 是否必须 | 描述                                                         |
| :---------- | :----- | :------- | :----------------------------------------------------------- |
| quoteSz | String | 可选       | 计价币投入数量<br>`quoteSz`和`baseSz`至少指定一个             |
| baseSz | String | 可选   | 交易币投入数量<br>`quoteSz`和`baseSz`至少指定一个         |
| tradeQuoteCcy | String | No       | 用于交易的计价币种。仅适用于现货网格。<br>默认值为 instId 的计价币种，例如 BTC-USD 的计价币种为 USD。   

合约网格

| 参数名      | 类型   | 是否必须 | 描述                                                         |
| :---------- | :----- | :------- | :----------------------------------------------------------- |
| sz | String | 是      | 投入保证金,单位为`USDT`             |
| direction | String | 是   | 合约网格类型<br>`long`：做多，`short`：做空，`neutral`：中性        |
| lever | String | 是   | 杠杆倍数        |
| basePos | Boolean | 否   | 是否开底仓<br>默认为`false`<br>中性合约网格忽略该参数      |
| tpRatio   | String | 否       | 止盈比率，0.1 代表 10%  |
| slRatio   | String | 否       | 止损比率，0.1 代表 10%  |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "447053782921515008",
            "sCode": "0",
            "sMsg": "",
            "tag": ""
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**     | **类型** | **描述**            |
|:------------| :------- |:------------------|
| algoId      | String   | 策略订单ID            |
| algoClOrdId | String   | 用户自定义策略ID         |
| sCode       | String   | 事件执行结果的code，0代表成功 |
| sMsg        | String   | 事件执行失败时的msg       |
| tag         | String   | 订单标签              |
