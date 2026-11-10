---
site: okex
lang: zh
name: 获取期权持仓量及交易量
method: GET
path: /api/v5/rubik/stat/option/open-interest-volume
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取期权持仓量及交易量 [trading-statistics-rest-api-get-options-open-interest-and-volume]

获取期权的持仓量和交易量。

#### 限速：5次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/rubik/stat/option/open-interest-volume`

> 请求示例

```shell
GET /api/v5/rubik/stat/option/open-interest-volume?ccy=BTC
```
```python
import okx.TradingData as TradingData_api

flag = "0"  # 实盘: 0, 模拟盘: 1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# 获取期权持仓量及交易量
result = tradingDataAPI.get_options_interest_volume(
    ccy="BTC",
)
print(result)
```
#### 请求参数

| 参数名   | 类型   | 是否必须 | 描述                                                         |
| :------- | :----- | :------- | :----------------------------------------------------------- |
| ccy      | String | 是      | 币种 |
| period   | String | 否      | 时间粒度，默认值`8H`。支持[`8H/1D`]  <BR> 每个粒度最多只能查询72条数据                                        |

> 返回结果

```json
{
    "code": "0",
    "data": [
        [
            "1630368000000",
            "3458.1000",
            "78.8000"
        ]
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述**             |
| :--------- | :------- |:-------------------|
| ts         | String     | 数据产生时间             |
| oi         | String     | 持仓总量，单位为请求参数的`ccy` |
| vol        | String     | 交易总量，单位为请求参数的`ccy`      |

<aside class="notice">
返回值数组顺序分别为是：[ts,oi,vol]
</aside>
