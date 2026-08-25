---
site: okex
lang: zh
name: 获取交易大数据支持币种
method: GET
path: /api/v5/rubik/stat/trading-data/support-coin
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

# 交易大数据 [trading-statistics]
## REST API [trading-statistics-rest-api]
### 获取交易大数据支持币种 [trading-statistics-rest-api-get-support-coin]

获取支持交易大数据的币种

#### 限速：5次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/rubik/stat/trading-data/support-coin`

> 请求示例

```shell
GET /api/v5/rubik/stat/trading-data/support-coin
```
```python
import okx.TradingData as TradingData_api

flag = "0"  # 实盘: 0, 模拟盘: 1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# 获取交易大数据支持币种
result = tradingDataAPI.get_support_coin()
print(result)
```
> 返回结果

```json
{
    "code": "0",
    "data": {
        "contract": [
            "ADA",
            "BTC",
        ],
        "option": [
            "BTC"
        ],
        "spot": [
            "ADA",
            "BTC",
        ]
    },
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型**                    | **描述**                                                     |
| :--------- |:--------------------------| :----------------------------------------------------------- |
| contract     | Array of strings          | 合约交易大数据接口功能支持的币种                                              |
| option       | Array of strings          | 期权交易大数据接口功能支持的币种                                              |
| spot         | Array of strings          | 现货交易大数据接口功能支持的币种                                              |
