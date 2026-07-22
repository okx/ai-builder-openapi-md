---
site: okex
lang: zh
name: 获取预估交割/行权价格
method: GET
path: /api/v5/public/estimated-price
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取预估交割/行权价格 [public-data-rest-api-get-estimated-delivery-exercise-price]

获取交割合约和期权预估交割/行权价。交割/行权预估价只有交割/行权前一小时才有返回值

#### 限速：10次/2s
#### 限速规则：IP + Instrument ID

#### HTTP请求

`GET /api/v5/public/estimated-price`

> 请求示例

```shell
GET /api/v5/public/estimated-price?instId=BTC-USD-200214
```

```python
import okx.PublicData as PublicData

flag = "0"  # 实盘:0 , 模拟盘：1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# 获取预估交割/行权价格
result = publicDataAPI.get_estimated_price(
    instId="BTC-USD-200214",
)
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                                  |
| :----- | :----- | :------- |:----------------------------------------------------|
| instId | String | 是       | 产品ID，如 `BTC-USD-200214`<br>仅适用于`交割`/`期权`/`事件合约`             |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
    {
        "instType":"FUTURES",
        "instId":"BTC-USDT-201227",
        "settlePx":"200",
        "ts":"1597026383085"
    }
  ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                 |
| :--------- | :------- |:---------------------------------------|
| instType   | String   | 产品类型<br>`FUTURES`：交割合约<br>`OPTION`：期权  |
| instId     | String   | 产品ID， 如 `BTC-USD-200214`                |
| settlePx   | String   | 预估交割/行权价格                              |
| ts         | String   | 数据返回时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
