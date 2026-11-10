---
site: us
lang: zh
name: 张币转换
method: GET
path: /api/v5/public/convert-contract-coin
module: public
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### 张币转换 [public-data-rest-api-unit-convert]

由币转换为张，或者张转换为币。

#### 限速：10次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/public/convert-contract-coin`

> 请求示例

```shell
GET /api/v5/public/convert-contract-coin?instId=BTC-USD-SWAP&px=35000&sz=0.888
```

```python
import okx.PublicData as PublicData

flag = "0"  # 实盘:0 , 模拟盘：1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# 张币转换
result = publicDataAPI.get_convert_contract_coin(
    instId="BTC-USD-SWAP",
    px="35000",
    sz="0.888"
)
print(result)
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                                              |
|:-------| :----- |:-----|:--------------------------------------------------------------------------------|
| type   | String | 否    | 转换类型<br>`1`：币转张 <br>`2`：张转币<br>默认为`1`                                           |
| instId | String | 是    | 产品ID，仅适用于`交割`/`永续`/`期权`                                                         |
| sz     | String | 是    | 数量，币转张时，为币的数量，张转币时，为张的数量。                                                       |
| px     | String | 可选   | 委托价格<br>币本位合约的张币转换时必填<br>U本位合约，usdt 与张的转换时，必填；coin 与张的转换时，可不填<br>期权的张币转换时，可不填。  |
| unit   | String | 否    | 币的单位<br>`coin`：币<br>`usds`：usdt/usdc<br>默认为 `coin`，仅适用于`交割`/`永续`的U本位合约          |
| opType | String | 否    | 将要下单的类型<br>`open`：开仓时将sz舍位<br>`close`：平仓时将sz四舍五入<br>默认值为`close`<br>适用于`交割`/`永续` |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "instId": "BTC-USD-SWAP",
            "px": "35000",
            "sz": "311",
            "type": "1",
            "unit": "coin"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**    | **类型**  | **描述**                               |
|:-----------|:--------|:-------------------------------------|
| type       |  String | 转换类型 <br>`1`：币转张 <br>`2`：张转币       |
| instId     | String  | 产品ID                                 |
| px         | String  | 委托价格                                 |
| sz         | String  | 数量<br>张转币时，为币的数量；币转张时，为张的数量。         |
| unit       | String  | 币的单位<br>`coin`：币<br>`usds`：usdt/usdc |
