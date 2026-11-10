---
site: us
lang: zh
name: POST / 策略委托下单
method: POST
path: /api/v5/trade/order-algo
module: trade
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### POST / 策略委托下单 [order-book-trading-algo-trading-post-place-algo-order]

提供单向止盈止损委托、双向止盈止损委托、计划委托、移动止盈止损委托

#### 限速：20次/2s
#### 限速规则：User ID + Instrument ID

#### HTTP请求

`POST /api/v5/trade/order-algo`

> 请求示例

```shell
# 止盈止损策略下单
POST /api/v5/trade/order-algo
body
{
    "instId":"BTC-USDT",
    "tdMode":"cash",
    "side":"buy",
    "ordType":"conditional",
    "sz":"2",
    "tpTriggerPx":"15",
    "tpOrdPx":"18"
}

# 计划委托策略下单
POST /api/v5/trade/order-algo
body
{
    "instId": "BTC-USDT",
    "tdMode": "cash",
    "side": "buy",
    "ordType": "trigger",
    "sz": "10",
    "triggerPx": "100",
    "orderPx": "-1"
}

# 移动止盈止损策略下单
POST /api/v5/trade/order-algo
body
{
	"instId": "BTC-USDT",
	"tdMode": "cash",
	"side": "buy",
	"ordType": "move_order_stop",
	"sz": "10",
	"callbackRatio": "0.05"
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

# 单向止盈止损
result = tradeAPI.place_algo_order(
    instId="BTC-USDT",
    tdMode="cross",
    side="buy",
    ordType="conditional",
    sz="2",
    tpTriggerPx="15",
    tpOrdPx="18"
)
print(result)
```

#### 请求参数

| 参数名           | 类型       | 是否必须  | 描述                                                                                                                                                                                                                                                          |
|:--------------|:---------|:------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId        | String   | 是     | 产品ID，如 `BTC-USDT`                                                                                                                                                                                                                                           |
| tdMode        | String   | 是     | 交易模式<br>`cash`：非保证金                                                                                                                                                      |
| side          | String   | 是     | 订单方向<br>`buy`：买<br>`sell`：卖                                                                                                                                                                                                                                 |
| ordType       | String   | 是     | 订单类型<br>`conditional`：单向止盈止损<br>`oco`：双向止盈止损<br>`trigger`：计划委托<br>`move_order_stop`：移动止盈止损                                                                                                                                            |
| sz            | String   | 是     | 委托数量                                                                                                                                                                                                                        |
| tgtCcy        | String   | 否     | 委托数量的类型<br>`base_ccy`: 交易货币 ；`quote_ccy`：计价货币<br>仅适用于`币币`单向止盈止损市价买单<br>默认买为`计价货币`，卖为`交易货币`                                                                                                                                                                  |
| algoClOrdId   | String   | 否     | 客户自定义策略订单ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                                                                                                                                                                                      |
| tag           | String   | 否     | 订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间                                                                                                                                                                                                              |

<b>止盈止损</b>

<aside class="notice">
用户可预先设置触发价和委托价，等市场价到达触发价时，系统会按委托价自动下单。<br>单向止盈止损可设置单边的止盈或止损；双向止盈止损可设置双边，一边触发后另一边失效。<br>该委托不会预先占用仓位或保证金。
</aside>

了解更多 [止盈止损](/cn/help/11015447687437)

| 参数名             | 类型   | 是否必须 | 描述                                                                                                                                                                                                                                                                      |
|:----------------| :----- | :------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tpTriggerPx     | String | 否       | 止盈触发价，如果填写此参数，必须填写`止盈委托价`                                                                                                                                                                                                                                               |
| tpTriggerPxType | String | 否   | 止盈触发价类型<br>`last`：最新价格<br>默认为`last`                                                                                                                                                                                                      |
| tpOrdPx         | String | 否       | 止盈委托价，如果填写此参数，必须填写`止盈触发价` <br>委托价格为-1时，执行市价止盈                                                                                                                                                                                                                           |
| slTriggerPx     | String | 否       | 止损触发价，如果填写此参数，必须填写`止损委托价`                                                                                                                                                                                                                                               |
| slTriggerPxType | String | 否   | 止损触发价类型<br>`last`：最新价格<br>默认为`last`                                                                                                                                                                                                      |
| slOrdPx         | String | 否       | 止损委托价，如果填写此参数，必须填写`止损触发价` <br>委托价格为`-1`时，执行市价止损                                                                                                                                                                                                                           |

<aside class="notice">
止盈止损<br>
当用户进行单向止盈止损委托（ordType=conditional）时，如果用户同时传了止盈止损四个参数，只进行止损的功能校验，忽略止盈的业务逻辑校验。
</aside>

<b>计划委托</b>

<aside class="notice">
当市场价格到达触发价格时，系统将按预先设置的委托价格和数量自动下单。<br>该委托不会预先占用仓位或保证金。
</aside>
了解更多 [计划委托](/cn/help/11015447687437)

| 参数名                 | 类型              | 是否必须  | 描述                                                                                                   |
|:--------------------|:----------------|:------|:-----------------------------------------------------------------------------------------------------|
| triggerPx           | String          | 是     | 计划委托触发价格                                                                                             |
| orderPx             | String          | 是     | 委托价格 <br>委托价格为`-1`时，执行市价委托                                                                           |
| triggerPxType       | String          | 否     | 计划委托触发价格类型<br>`last`：最新价格<br>默认为`last`                                |

<b>移动止盈止损</b>

<aside class="notice">
移动止盈止损是一种跟踪市场价格的止盈止损，它的触发价格会跟随市场波动而变化，触发成功后会下市价单。<br>
实际触发价格的计算：卖出或开空时，实际触发价格 = 下单成功后最高价-回调幅度 (价距)，或下单成功后最高价 *(1-回调幅度 %) (比例)；买入或开多，实际触发价格 = 下单成功后最低价 + 回调幅度，或下单成功后最低价 *(1+ 回调幅度 %)。同时，您可以利用激活价格来设置委托被激活的价格。
</aside>
了解更多 [移动止盈止损](/cn/help/11015447687437)

| 参数名             | 类型   | 是否必须   | 描述                                                                                                                   |
|:----------------| :----- |:-------|:---------------------------------------------------------------------------------------------------------------------|
| callbackRatio   | String | 可选     | 回调幅度的比例，如 "0.05"代表"5%"<br>`callbackRatio`和`callbackSpread`只能传入一个 |
| callbackSpread  | String | 可选     | 回调幅度的价距                                                          |
| activePx        | String | 否      | 激活价格<br>激活价格是移动止盈止损的激活条件，当市场最新成交价达到或超过激活价格，委托被激活。激活后系统开始计算止盈止损的实际触发价格。如果不填写激活价格，即下单后就被激活。                            |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "algoId":"12345689",
            "clOrdId": "",
            "algoClOrdId": "",
            "sCode":"0",
            "sMsg":"",
            "tag":""
        }
    ]
}
```

#### 返回参数

| **参数名**     | **类型** | **描述**                    |
|:------------| :------- |:--------------------------|
| algoId      | String   | 策略委托单ID                   |
| clOrdId     | String   | <del>客户自定义订单ID</del>（已废弃） |
| algoClOrdId | String   | 客户自定义策略订单ID               |
| sCode       | String   | 事件执行结果的code，0代表成功         |
| sMsg        | String   | 事件执行失败时的msg               |
| tag         | String   | 订单标签                      |
