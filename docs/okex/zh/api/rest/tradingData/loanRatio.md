---
site: okex
lang: zh
name: 获取杠杆多空比
method: GET
path: /api/v5/rubik/stat/margin/loan-ratio
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取杠杆多空比 [trading-statistics-rest-api-get-margin-long-short-ratio]

获取借入计价货币与借入交易货币的累计数额比值。

#### 限速：5次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/rubik/stat/margin/loan-ratio`

> 请求示例

```shell
GET /api/v5/rubik/stat/margin/loan-ratio?ccy=BTC
```
```python
import okx.TradingData as TradingData_api

flag = "0"  # 实盘: 0, 模拟盘: 1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# 获取杠杆多空比
result = tradingDataAPI.get_margin_lending_ratio(
    ccy="BTC",
)
print(result)
```
#### 请求参数

| 参数名   | 类型   | 是否必须 | 描述                                                                                                                              |
| :------- | :----- | :------- |:--------------------------------------------------------------------------------------------------------------------------------|
| ccy      | String | 是      | 币种                                                                                                                              |
| begin    | String | 否      | 开始时间，如 `1597026383085`                                                                                                         |
| end    | String | 否      | 结束时间，如 `1597026383011`                                                                                                         |
| period   | String | 否      | 时间粒度<br>`m`：分钟，`H`：小时，`D`：天<br>默认值`5m`，支持[`5m`/`1H`/`1D`]  <BR> `5m`粒度最多只能查询两天之内的数据<BR>`1H`粒度最多只能查询30天之内的数据 <BR>`1D`粒度最多只能查询180天之内的数据 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        [
            "1630492800000",
            "0.4614"
        ],
        [
            "1630492500000",
            "0.5767"
        ]
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| ts         | String     | 数据产生时间                                                |
| ratio     | String     | 多空比值                                                |

<aside class="notice">
返回值数组顺序分别为是：[ts,ratio]
</aside>
