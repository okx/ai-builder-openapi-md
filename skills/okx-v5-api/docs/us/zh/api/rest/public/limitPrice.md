---
site: us
lang: zh
name: 获取限价
method: GET
path: /api/v5/public/price-limit
module: public
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### 获取限价 [public-data-rest-api-get-limit-price]

查询单个交易产品的最高买价和最低卖价

#### 限速：20次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/public/price-limit`

> 请求示例

```shell
GET /api/v5/public/price-limit?instId=BTC-USDT-SWAP
```

```python
import okx.PublicData as PublicData

flag = "0"  # 实盘:0 , 模拟盘：1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# 获取限价
result = publicDataAPI.get_price_limit(
    instId="BTC-USD-SWAP",
)
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                                      |
| :----- | :----- | :------- | :-------------------------------------------------------- |
| instId | String | 是 | 产品ID，如 `BTC-USDT-SWAP` |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
    {
        "instType":"SWAP",
        "instId":"BTC-USDT-SWAP",
        "buyLmt":"17057.9",
        "sellLmt":"16388.9",
        "ts":"1597026383085",
        "enabled": true
    }
  ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| instType | String | 产品类型<br>`SPOT`：币币<br>`MARGIN`：杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权 <br>若产品ID支持杠杆交易，则返回`MARGIN`；否则，返回`SPOT`。 |
| instId | String | 产品ID ，如 `BTC-USDT-SWAP` |
| buyLmt | String | 最高买价 <br>当enabled为false时，返回""  |
| sellLmt | String | 最低卖价 <br>当enabled为false时，返回"" |
| ts | String | 限价数据更新时间 ，Unix时间戳的毫秒数格式，如 `1597026383085` |
| enabled | Boolean | 限价是否生效 <br>`true`：限价生效 <br>`false`：限价不生效 |
