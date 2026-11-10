---
site: turkey
lang: zh
name: 获取法币汇率
method: GET
path: /api/v5/market/exchange-rate
module: market
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取法币汇率 [public-data-rest-api-get-exchange-rate]

该接口提供的是2周的平均汇率数据

#### 限速：1次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/market/exchange-rate`

> 请求示例

```shell
GET /api/v5/market/exchange-rate
```

```python
import okx.MarketData as MarketData

flag = "0"  # 实盘:0 , 模拟盘：1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# 获取法币汇率
result = marketDataAPI.get_exchange_rate(
)
print(result)
```

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "usdCny": "7.162"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| usdCny     | String     | 人民币兑美元汇率                                                   |
