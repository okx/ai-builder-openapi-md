---
site: us
lang: zh
name: 获取持仓总量
method: GET
path: /api/v5/public/open-interest
module: public
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### 获取持仓总量 [public-data-rest-api-get-open-interest]

查询单个交易产品的市场的持仓总量

#### 限速：20次/2s
#### 限速规则：IP + Instrument ID

#### HTTP请求

`GET /api/v5/public/open-interest`

> 请求示例

```shell
GET /api/v5/public/open-interest?instType=SWAP
```

```python
import okx.PublicData as PublicData

flag = "0"  # 实盘:0 , 模拟盘：1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# 获取持仓总量
result = publicDataAPI.get_open_interest(
    instType="FUTURES",
)
print(result)
```

#### 请求参数

| 参数名            | 类型   | 是否必须 | 描述                                                     |
|:---------------| :----- |:-----|:-------------------------------------------------------|
| instType       | String | 是    | 产品类型<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`EVENTS`：事件合约 |
| instFamily     | String | 可选    | 交易品种<br>适用于`交割`/`永续`/`期权`<br>`期权`下必传                                  |                                                                                                                                 
| instId         | String | 否    | 产品ID，如  `BTC-USDT-SWAP`  <br>仅适用于`交割`/`永续`/`期权`/`事件合约`           |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
    {
        "instType":"SWAP",
        "instId":"BTC-USDT-SWAP",
        "oi":"5000",
        "oiCcy":"555.55",
        "oiUsd": "50000",
        "ts":"1597026383085"
    }
  ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                   |
| :--------- | :------- | :--------------------------------------------------------- |
| instType   | String   | 产品类型                                                   |
| instId      | String   | 产品ID                                                     |
| oi         | String   | 持仓量（按`张`折算）                                       |
| oiCcy      | String   | 持仓量（按`币`折算）                                       |
| oiUsd | String | 持仓量（按`USD`折算） |
| ts         | String   | 数据返回时间，Unix时间戳的毫秒数格式 ，如 `1597026383085` |
