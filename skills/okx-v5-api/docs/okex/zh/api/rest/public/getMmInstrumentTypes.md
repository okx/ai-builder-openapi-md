---
site: okex
lang: zh
name: 获取 MM 币对分类类型
method: GET
path: /api/v5/public/mm-instrument-types
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取 MM 币对分类类型 [public-data-rest-api-get-mm-instrument-types]

获取当前做市商（MM）计划 SPOT 和 SWAP 产品的币对分类类型列表。

#### 限速：每2秒5次请求
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/public/mm-instrument-types`

> 请求示例

```shell
GET /api/v5/public/mm-instrument-types?instType=SWAP
```

```python
import okx.PublicData as PublicData

flag = "0"  # 实盘:0 , 模拟盘：1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# 获取 MM 币对分类类型
result = publicDataAPI.get_mm_instrument_types(
    instType="SWAP"
)
print(result)
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述 |
|:---------|:-------|:---------|:-----|
| instType | String | 否       | 产品类型<br>`SPOT`<br>`SWAP`<br>未指定时返回全部类型 |
| instId   | String | 否       | 产品ID，如 `BTC-USDT`、`BTC-USDT-SWAP`<br>指定时返回至多一条记录 |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "instId": "BTC-USDT-SWAP",
            "instType": "SWAP",
            "pairType": "A"
        },
        {
            "instId": "ETH-USDT-SWAP",
            "instType": "SWAP",
            "pairType": "A"
        },
        {
            "instId": "XAU-USDT-SWAP",
            "instType": "SWAP",
            "pairType": "B-TradFi"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
|:----------|:-------|:------------|
| instId    | String | 产品ID，如 `BTC-USDT-SWAP` |
| instType  | String | 产品类型<br>`SPOT`<br>`SWAP` |
| pairType  | String | MM 计划分类类型<br>`A`：高流动性品种<br>`B-Crypto`：中低流动性加密资产<br>`B-TradFi`：传统金融品种（仅 SWAP） |
