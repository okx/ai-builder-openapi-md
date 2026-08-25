---
site: okex
lang: zh
name: 获取标记价格
method: GET
path: /api/v5/public/mark-price
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取标记价格 [public-data-rest-api-get-mark-price]

为了防止个别用户恶意操控市场导致合约价格波动剧烈，我们根据现货指数和合理基差设定标记价格。

#### 限速：10次/2s
#### 限速规则：IP + Instrument ID

#### HTTP请求

`GET /api/v5/public/mark-price`

> 请求示例

```shell
GET /api/v5/public/mark-price?instType=SWAP
```

```python
import okx.PublicData as PublicData

flag = "0"  # 实盘:0 , 模拟盘：1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# 获取标记价格
result = publicDataAPI.get_mark_price(
    instType="SWAP",
)
print(result)
```

#### 请求参数

| 参数名            | 类型   | 是否必须 | 描述                                                         |
|:---------------| :----- | :------- | :----------------------------------------------------------- |
| instType       | String | 是       | 产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`EVENTS`：事件合约  |
| instFamily     | String | 否       | 交易品种<br>适用于`交割`/`永续`/`期权`     |                                                                                                                                 
| instId         | String | 否       | 产品ID，如 `BTC-USD-SWAP`                                |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
    {
        "instType":"SWAP",
        "instId":"BTC-USDT-SWAP",
        "markPx":"200",
        "ts":"1597026383085"
    }
  ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| instType   | String   | 产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`EVENTS`：事件合约|
| instId      | String   | 产品ID，如 `BTC-USD-200214`                              |
| markPx     | String   | 标记价格                                                     |
| ts         | String   | 接口数据返回时间，Unix时间戳的毫秒数格式，如`1597026383085`  |
