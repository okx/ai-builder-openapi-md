---
site: okex
lang: zh
name: 获取 Delta 对冲币种
method: GET
path: /api/v5/public/delta-hedge-currencies
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取 Delta 对冲币种 [public-data-rest-api-get-delta-hedge-currencies]

获取具有相同标的资产、可构成 Delta 对冲关系的币种，如 `ETH` 与 `BETH`、`AAPL` 与 `XAAPL`。

#### 限速：每2秒20次请求
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/public/delta-hedge-currencies`

> 请求示例

```shell
GET /api/v5/public/delta-hedge-currencies

GET /api/v5/public/delta-hedge-currencies?ccy=ETH
```

```python
import okx.PublicData as PublicData

flag = "0"  # 实盘:0 , 模拟盘：1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# 获取 Delta 对冲币种
result = publicDataAPI.get_delta_hedge_currencies(
    ccy="ETH"
)
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述 |
|:------|:-------|:---------|:-----|
| ccy   | String | 否       | 币种，如 `ETH`<br>指定时仅返回该币种的对应关系<br>未指定时返回全部对应关系 |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ccy": "ETH",
            "hedgeCcy": ["BETH"]
        },
        {
            "ccy": "XAU",
            "hedgeCcy": ["XAUT"]
        },
        {
            "ccy": "AAPL",
            "hedgeCcy": ["XAAPL"]
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
|:----------|:-------|:------------|
| ccy       | String | 币种，如 `AAPL`、`ETH` |
| hedgeCcy  | Array of strings | 与 `ccy` 具有相同标的资产、可与其构成 Delta 对冲关系的币种，如 `ccy` 为 `AAPL` 时返回 `["XAAPL"]`。该关系是双向的。 |
