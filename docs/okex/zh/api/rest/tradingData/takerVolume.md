---
site: okex
lang: zh
name: 获取主动买入/卖出情况
method: GET
path: /api/v5/rubik/stat/taker-volume
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取主动买入/卖出情况 [trading-statistics-rest-api-get-taker-volume]

获取taker主动买入和卖出的交易量

#### 限速：5次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/rubik/stat/taker-volume`

> 请求示例

```shell
GET /api/v5/rubik/stat/taker-volume?ccy=BTC&instType=SPOT
```
```python
import okx.TradingData as TradingData_api

flag = "0"  # 实盘: 0, 模拟盘: 1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# 获取主动买入/卖出情况
result = tradingDataAPI.get_taker_volume(
    ccy="BTC",
    instType="SPOT"
)
print(result)
```
#### 请求参数

| 参数名        | 类型   | 是否必须 | 描述                                                                                                       |
|:-----------| :----- | :------- |:---------------------------------------------------------------------------------------------------------|
| ccy        | String | 是      | 币种                                                                                                       |
| instType   | String | 是      | 产品类型<br>`SPOT`：币币<br>`CONTRACTS`：衍生品                                                                     |
| begin      | String | 否      | 开始时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                     |
| end        | String | 否      | 结束时间，Unix时间戳的毫秒数格式，如 `1597026383011`                                                                     |
| period     | String | 否      | 时间粒度，默认值`5m`。支持[`5m`/`1H`/`1D`] <br>`5m`粒度最多只能查询两天之内的数据<BR>`1H`粒度最多只能查询30天之内的数据<br>`1D`粒度最多只能查询180天之内的数据 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        [
            "1630425600000",
            "7596.2651",
            "7149.4855"
        ],
        [
            "1630339200000",
            "5312.7876",
            "7002.7541"
        ]
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| ts         | String     | 数据产生时间                                                |
| sellVol     | String     | 卖出量                                                |
| buyVol      | String     | 买入量                                                |

<aside class="notice">
返回值数组顺序分别为是：[ts,sellVol,buyVol]
</aside>
