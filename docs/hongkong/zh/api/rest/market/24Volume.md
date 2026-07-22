---
site: hongkong
lang: zh
name: GET / 获取平台24小时总成交量
method: GET
path: /api/v5/market/platform-24-volume
module: market
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取平台24小时总成交量 [order-book-trading-market-data-get-24h-total-volume]

24小时成交量滚动计算

#### 限速：2次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/market/platform-24-volume`

> 请求示例

```shell
GET /api/v5/market/platform-24-volume

```

```python
import okx.MarketData as MarketData

flag = "0"  # 实盘:0 , 模拟盘：1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# 获取平台24小时总成交量
result = marketDataAPI.get_volume()
print(result)
```

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
     {
         "volCny": "230900886396766",
         "volUsd": "34462818865189",
         "ts": "1657856040389"
     }
  ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| volUsd     | String     | 订单簿交易近24小时总成交量，以美元为单位  |
| volCny     | String     | 订单簿交易近24小时总成交量，以人民币为单位 |
| ts         | String     | 接口返回数据时间                                                   |
