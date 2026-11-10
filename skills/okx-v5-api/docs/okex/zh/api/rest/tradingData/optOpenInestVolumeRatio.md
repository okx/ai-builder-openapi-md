---
site: okex
lang: zh
name: 看涨/看跌期权合约 持仓总量比/交易总量比
method: GET
path: /api/v5/rubik/stat/option/open-interest-volume-ratio
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 看涨/看跌期权合约 持仓总量比/交易总量比 [trading-statistics-rest-api-get-put-call-ratio]

获取看涨期权和看跌期权的持仓量比值，以及交易量比值。

#### 限速：5次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/rubik/stat/option/open-interest-volume-ratio`

> 请求示例

```shell
GET /api/v5/rubik/stat/option/open-interest-volume-ratio?ccy=BTC
```
```python
import okx.TradingData as TradingData_api

flag = "0"  # 实盘: 0, 模拟盘: 1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# 看涨/看跌期权合约 持仓总量比/交易总量比
result = tradingDataAPI.get_put_call_ratio(
    ccy="BTC",
)
print(result)
```
#### 请求参数

| 参数名   | 类型   | 是否必须 | 描述                                                         |
| :------- | :----- | :------- | :----------------------------------------------------------- |
| ccy      | String | 是      | 币种 |
| period   | String | 否      | 时间粒度，默认值`8H`。支持[`8H/1D`]  <BR> 每个粒度最多只能查询72条数据                                   |

> 返回结果

```json
{
    "code": "0",
    "data": [
        [
            "1630512000000",
            "2.7261",
            "2.3447"
        ],
        [
            "1630425600000",
            "2.8101",
            "2.3438"
        ]
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| ts         | String     | 数据产生时间                                                |
| oiRatio         | String     | 看涨/看跌 持仓总量比                                         |
| volRatio        | String     | 看涨/看跌 交易总量比                                           |

<aside class="notice">
返回值数组顺序分别为是：[ts,oiRatio,volRatio]
</aside>
