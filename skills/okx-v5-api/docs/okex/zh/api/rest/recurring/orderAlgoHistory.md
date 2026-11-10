---
site: okex
lang: zh
name: GET / 获取历史定投策略委托单列表
method: GET
path: /api/v5/tradingBot/recurring/orders-algo-history
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取历史定投策略委托单列表 [order-book-trading-recurring-buy-get-recurring-buy-order-history]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/tradingBot/recurring/orders-algo-history`

> 请求示例

```shell
GET /api/v5/tradingBot/recurring/orders-algo-history
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                         |
| :-------- | :----- | :------- | :----------------------------------------------------------- |
| algoId    | String | 否       | 策略订单ID                                                   |
| after    | String | 否       | 请求此ID之前（更旧的数据）的分页内容，传的值为对应接口的`algoId`  |
| before    | String | 否       | 请求此ID之后（更新的数据）的分页内容，传的值为对应接口的`algoId`  |
| limit    | String | 否       | 返回结果的数量，最大为100，默认100条 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "644496098429767680",
            "algoOrdType": "recurring",
            "amt": "100",
            "cTime": "1699931844050",
            "cycles": "0",
            "instType": "SPOT",
            "investmentAmt": "0",
            "investmentCcy": "USDC",
            "mktCap": "0",
            "period": "hourly",
            "pnlRatio": "0",
            "recurringDay": "",
            "recurringHour": "1",
            "recurringList": [
                {
                    "ccy": "BTC",
                    "ratio": "0.2",
                    "minPx": "",
                    "maxPx": ""
                },
                {
                    "ccy": "ETH",
                    "ratio": "0.8",
                    "minPx": "",
                    "maxPx": ""
                }
            ],
            "recurringTime": "0",
            "state": "stopped",
            "stgyName": "stg1",
            "tag": "",
            "timeZone": "8",
            "totalAnnRate": "0",
            "totalPnl": "0",
            "uTime": "1699932177659",
            "tradeQuoteCcy": "USDT"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**           | **类型** | **描述**                                                                                             |
|:------------------| :------- |:---------------------------------------------------------------------------------------------------|
| algoId            | String          | 策略订单ID                                                                                             |
| algoClOrdId       | String          | 客户自定义订单ID                                                                                          |
| instType          | String          | 产品类型<br>`SPOT`：现货                                                                                  |
| cTime             | String          | 策略订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                           |
| uTime             | String          | 策略订单更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                           |
| algoOrdType       | String          | 策略订单类型<br>`recurring`：定投                                                                           |
| state             | String          | 订单状态<br>`stopped`：已停止                                                                              |
| stgyName          | String          | 策略自定义名称，不超过40个字符                                                                                   |
| recurringList     | Array of objects | 定投信息                                                                                               |
| > ccy             | String          | 定投币种，如 `BTC`                                                                                       |
| > ratio           | String          | 定投币种资产占比，如 "0.2"代表占比20%                                                                            |
| > minPx         | String          | 定投币种价格下限，`""`代表没有限制                                                                                                                                                                                                                                |
| > maxPx         | String          | 定投币种价格上限，`""`代表没有限制                                                                                                                                                                                                                                |
| period            | String          | 周期类型<br>`monthly`：月<br>`weekly`：周<br>`daily`：日<br>`hourly`：小时                                   |
| recurringDay      | String          | 投资日<br>当周期类型为`monthly`，则取值范围是 [1,28] 的整数<br>当周期类型为`weekly`，则取值范围是 [1,7] 的整数 |
| recurringHour     | String          | 小时级别定投的间隔<br>`1`/`4`/`8`/`12`<br>如：`1`代表每隔`1`个小时定投                                               |
| recurringTime     | String          | 投资时间，取值范围是 [0,23] 的整数                                                                              |
| timeZone          | String          | 时区（UTC），取值范围是 [-12,14] 的整数<br>如 `8`表示UTC+8（东8区），北京时间                                               |
| amt               | String          | 每期投入数量                                                                                             |
| investmentAmt     | String          | 累计投入数量                                                                                             |
| investmentCcy     | String          | 投入数量单位，只能是`USDT`/`USDC`                                                                            |
| totalPnl          | String          | 总收益                                                                                                |
| totalAnnRate      | String          | 总年化                                                                                                |
| pnlRatio          | String          | 收益率                                                                                                |
| mktCap            | String          | 当前总市值，单位为`USDT`                                                                                    |
| cycles            | String          | 定投累计轮数                                                                                             |
| tag               | String          | 订单标签                                                                                               |
| tradeQuoteCcy   | String   | 用于交易的计价币种。 |
| source              | Array           | 资金来源<br>`1`：交易账户<br>`2`：资金账户<br>`3`：简单赚币账户                                                                                                                                                                                                          |
| recurringTimeType   | String          | 定投周期类型<br>`1`：自定义时间<br>`2`：立即触发                                                                                                                                                                                                                      |
| recurringTimeMinutes | String         | 定投时间（分钟），取值范围是 [0,59] 的整数                                                                                                                                                                                                                            |
