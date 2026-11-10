---
site: okex
lang: zh
name: 看跌/看涨期权合约 主动买入/卖出量
method: GET
path: /api/v5/rubik/stat/option/taker-block-volume
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 看跌/看涨期权合约 主动买入/卖出量 [trading-statistics-rest-api-get-taker-flow]

该指标展示某一时刻，单位时间内看跌/看涨期权的主动（taker）买入/卖出交易量

#### 限速：5次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/rubik/stat/option/taker-block-volume`

> 请求示例

```shell
GET /api/v5/rubik/stat/option/taker-block-volume?ccy=BTC
```
```python
import okx.TradingData as TradingData_api

flag = "0"  # 实盘: 0, 模拟盘: 1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# 看跌/看涨期权合约 主动买入/卖出量
result = tradingDataAPI.get_taker_block_volume(
    ccy="BTC",
)
print(result)
```
#### 请求参数

| 参数名   | 类型   | 是否必须 | 描述                                                         |
| :------- | :----- | :------- | :----------------------------------------------------------- |
| ccy      | String | 是      | 币种 |
| period   | String | 否      | 时间粒度，默认值`8H`。支持[`8H/1D`]  <BR> 每个粒度仅展示最新的一份数据                                  |

> 返回结果

```json
{
    "code": "0",
    "data": [
        "1630512000000",
        "8.55",
        "67.3",
        "16.05",
        "16.3",
        "126.4",
        "40.7"
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| ts         | String     | 数据产生时间                                                |
| callBuyVol         | String     | 看涨买入量  以结算货币为单位                                        |
| callSellVol        | String     | 看涨卖出量  以结算货币为单位                                            |
| putBuyVol          | String     | 看跌买入量  以结算货币为单位                                        |
| putSellVol         | String     | 看跌卖出量  以结算货币为单位                                            |
| callBlockVol       | String     | 看涨大单                                         |
| putBlockVol        | String     | 看跌大单                                            |

<aside class="notice">
返回值数组顺序分别为是：[ts,callBuyVol,callSellVol,putBuyVol,putSellVol,callBlockVol,putBlockVol]
</aside>
