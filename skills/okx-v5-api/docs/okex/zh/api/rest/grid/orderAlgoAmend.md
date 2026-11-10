---
site: okex
lang: zh
name: POST / 修改网格策略订单
method: POST
path: /api/v5/tradingBot/grid/amend-order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 修改网格策略订单 [order-book-trading-grid-trading-post-amend-grid-algo-order]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/grid/amend-order-algo`

> 请求示例

```shell
POST /api/v5/tradingBot/grid/amend-order-algo
body
{
    "algoId":"448965992920907776",
    "instId":"BTC-USDT-SWAP",
    "slTriggerPx":"1200",
    "tpTriggerPx":""
}

POST /api/v5/tradingBot/grid/amend-order-algo
body 
{
   "algoId":"578963447615062016",
   "instId":"BTC-USDT",
   "triggerParams":[
       {
           "triggerAction":"stop",  
           "triggerStrategy":"price",   
           "triggerPx":"1000"
       }
   ]
}

POST /api/v5/tradingBot/grid/amend-order-algo
body 
{
   "algoId":"578963447615062016",
   "instId":"BTC-USDT-SWAP",
   "triggerParams":[
       {
           "triggerAction":"stop",  
           "triggerStrategy":"instant",   
           "stopType":"1"
       }
   ]
}
```

#### 请求参数

| 参数名               | 类型              | 是否必须 | 描述                                                                                                   |
|:------------------|:----------------|:-----|:-----------------------------------------------------------------------------------------------------|
| algoId            | String          | 是    | 策略订单ID                                                                                               |
| instId            | String          | 是    | 产品ID，如`BTC-USDT-SWAP`                                                                                |
| slTriggerPx       | String          | 可选   | 新的止损触发价<br>当值为""则代表取消止损触发价<br>`slTriggerPx`、`tpTriggerPx`至少要传一个值                                     |
| tpTriggerPx       | String          | 可选   | 新的止盈触发价<br>当值为""则代表取消止盈触发价                                                                           |
| tpRatio   | String | 否       | 止盈比率，0.1 代表 10%，仅适用于合约网格<br/>当值为""则代表取消止盈比率                                                                             |
| slRatio   | String | 否       | 止损比率，0.1 代表 10%，仅适用于合约网格<br/>当值为""则代表取消止损比率                                                                             |
| topUpAmt   | String | 否       | 增加的投资额，仅适用于现货网格                                                                          |
| triggerParams     | Array of objects | 否    | 信号触发参数                                                                                               |
| > triggerAction   | String          | 是    | 触发行为<br>`start`：网格启动<br>`stop`：网格停止                                                                  |
| > triggerStrategy | String          | 是    | 触发策略<br>`instant`：立即触发<br>`price`：价格触发<br>`rsi`：rsi指标触发                                              |
| > triggerPx       | String          | 否    | 触发价格<br>该字段只在`triggerStrategy`为`price`下有效                                                            |
| > stopType        | String          | 否    | 策略停止类型<br>现货 `1`：卖出交易币，`2`：不卖出交易币<br>合约网格 `1`：停止平仓，`2`：停止不平仓 <br>该字段只在`triggerAction`为`stop`时有效 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "448965992920907776",
            "sCode": "0",
            "sMsg": "",
            "tag": ""
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**       | **类型** | **描述**                      |
|:--------------| :------- | :---------------------------- |
| algoId        | String   | 策略订单ID                        |
| algoClOrdId   | String   | 用户自定义策略ID                        |
| sCode         | String   | 事件执行结果的code，0代表成功 |
| sMsg          | String   | 事件执行失败时的msg           |
| tag         | String   | 订单标签              |
