---
site: okex
lang: zh
name: POST / 定投策略委托下单
method: POST
path: /api/v5/tradingBot/recurring/order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 定投策略委托下单 [order-book-trading-recurring-buy-post-place-recurring-buy-order]

#### 限速：20次/2s
#### 限速规则 ：User ID

#### HTTP请求

`POST /api/v5/tradingBot/recurring/order-algo`

> 请求示例

```shell
POST /api/v5/tradingBot/recurring/order-algo
body
{
  "stgyName": "BTC|ETH recurring buy monthly",     
  "amt":"100",
  "recurringList":[    
    {
         "ccy":"BTC",
         "ratio":"0.2"
    },
    {
         "ccy":"ETH",
         "ratio":"0.8"
    }
  ],
  "period":"monthly",
  "recurringDay":"1",
  "recurringTime":"0",
  "timeZone":"8",   // 东8区
  "tdMode":"cross",
  "investmentCcy":"USDT"
}
```

#### 请求参数

| 参数名             | 类型   | 是否必须 | 描述                                                                                                            |
|:----------------| :----- |:-----|:--------------------------------------------------------------------------------------------------------------|
| stgyName        | String | 是    | 策略自定义名称，不超过40个字符                                                                                              |
| recurringList   | Array of objects | 是    | 定投信息                                                                                                          |
| > ccy           | String | 是    | 定投币种，如 `BTC`                                                                                                  |
| > ratio         | String | 是    | 定投币种资产占比，如 "0.2"代表占比20%                                                                                       |
| > minPx         | String | 否    | 定投币种价格下限，`""`代表没有限制                                                                                           |
| > maxPx         | String | 否    | 定投币种价格上限，`""`代表没有限制                                                                                           |
| period          | String | 是    | 周期类型<br>`monthly`：月<br>`weekly`：周<br>`daily`：日<br>`hourly`：小时                                                 |
| recurringDay    | String | 可选   | 投资日<br>当周期类型为`monthly`，则取值范围是 [1,28] 的整数<br>当周期类型为`weekly`，则取值范围是 [1,7] 的整数<br>当周期类型为`daily`/`hourly`，该参数可不填。 |
| recurringHour   | String | 可选     | 小时级别定投的间隔<br>`1`/`4`/`8`/`12`<br>如：`1`代表每隔`1`个小时定投<br>当周期类型选择`hourly`，该字段必填。                                  |
| recurringTime   | String | 是    | 投资时间，取值范围是 [0,23] 的整数<br>当周期类型选择`hourly`代表首次定投发生的时间                                                           |
| timeZone        | String | 是    | 时区（UTC），取值范围是 [-12,14] 的整数<br>如 `8`表示UTC+8（东8区），北京时间                                                          |
| amt             | String | 是    | 每期投入数量                                                                                                        |
| investmentCcy   | String | 是    | 投入数量单位，只能是`USDT`/`USDC`                                                                                       |
| tdMode          | String | 是    | 交易模式<br>`跨币种保证金模式`/`组合保证金模式`下选择 `cross`：全仓<br>`现货模式`/`合约模式`下选择 `cash`：非保证金                                 |
| algoClOrdId     | String | 否    | 客户自定义订单ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                                          |
| tag             | String | 否    | 订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间。                                                               |
| tradeQuoteCcy   | String | 否       | 用于交易的计价币种。   |
| source          | Array  | 否    | 资金来源<br>`1`：交易账户<br>`2`：资金账户<br>`3`：简单赚币账户<br>默认为`1`                                                          |
| recurringTimeType | String | 否  | 定投周期类型<br>`1`：自定义时间<br>`2`：立即触发<br>默认为`1`                                                                     |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "algoId":"560472804207104000",
            "algoClOrdId":"",
            "sCode":"0",
            "sMsg":"",
            "tag":""
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                      |
| :--------- | :------- | :---------------------------- |
| algoId      | String   | 策略订单ID                        |
| algoClOrdId      | String   | 客户自定义订单ID                        |
| sCode      | String   | 事件执行结果的code，0代表成功 |
| sMsg       | String   | 事件执行失败时的msg           |
| tag       | String   | 订单标签           |
