---
site: okex
lang: zh
name: 获取指数成分数据
method: GET
path: /api/v5/market/index-components
module: market
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取指数成分数据 [public-data-rest-api-get-index-components]

查询市场上的指数成分信息数据

#### 限速：20次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/market/index-components`

> 请求示例

```shell
GET /api/v5/market/index-components?index=BTC-USD
```

```python
import okx.MarketData as MarketData

flag = "0"  # 实盘:0 , 模拟盘：1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# 获取指数成分数据
result = marketDataAPI.get_index_components(
    index="BTC-USD"
)
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                               |
| :----- | :----- | :------- | :------------------------------------------------- |
| index | String | 是       | 指数，如 `BTC-USDT`<br>与 `uly` 含义相同。                      |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": {
        "components": [
            {
                "symbol": "BTC/USDT",
                "symPx": "52733.2",
                "wgt": "0.25",
                "cnvPx": "52733.2",
                "exch": "OKEx"
            },
            {
                "symbol": "BTC/USDT",
                "symPx": "52739.87000000",
                "wgt": "0.25",
                "cnvPx": "52739.87000000",
                "exch": "Binance"
            },
            {
                "symbol": "BTC/USDT",
                "symPx": "52729.1",
                "wgt": "0.25",
                "cnvPx": "52729.1",
                "exch": "Huobi"
            },
            {
                "symbol": "BTC/USDT",
                "symPx": "52739.47929397",
                "wgt": "0.25",
                "cnvPx": "52739.47929397",
                "exch": "Poloniex"
            }
        ],
        "last": "52735.4123234925",
        "index": "BTC-USDT",
        "ts": "1630985335599"
    }
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                             |
| :--------- | :------- | :--------------------------------------------------- |
| index     | String   | 指数名称                                               |
| last      | String   | 最新指数价格                                               |
| ts         | String   | 数据产生时间，Unix时间戳的毫秒数格式， 如`1597026383085`       |
| components | String   |  成分       |
| > exch         | String   | 交易所名称                                             |
| > symbol       | String   | 采集的币对名称                                         |
| > symPx        | String   | 采集的币对价格 |
| > wgt          | String   | 权重 |
| > cnvPx        | String   | 参与指数计算的成分价格，由 `symPx` 经配置处理后得出，处理可能包括报价单位换算、倍数调整（如 ×10 或 ×0.1）或 EMA 平滑，因此可能与 `symPx` 不同 |
