---
site: okex
lang: zh
name: POST / 一键兑换主流币交易
method: POST
path: /api/v5/trade/easy-convert
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 一键兑换主流币交易 [order-book-trading-trade-post-place-easy-convert]

进行小币一键兑换主流币交易。

#### 限速：1次/2s
#### 限速规则：User ID

#### HTTP 请求

`POST /api/v5/trade/easy-convert`

> 请求示例

```shell
POST /api/v5/trade/easy-convert
body
{
    "fromCcy": ["ADA","USDC"], //逗号分隔小币
    "toCcy": "OKB" 
}
```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# 进行小币一键兑换主流币交易
result = tradeAPI.easy_convert(
    fromCcy=["ADA", "USDC"],
    toCcy="OKB"
)
print(result)
```

#### 请求参数

| 参数名       | 类型              | 是否必须  | 描述                                                                 |
|:----------|:----------------| :------- | :-------------------------------------------------------------------- |
| fromCcy   | Array of strings | 是       | 小币支付币种 <br>单次最多同时选择5个币种，如有多个币种则用逗号隔开        |
| toCcy     | String          | 是       | 兑换的主流币 <br>只选择一个币种，且不能和小币支付币种重复               |
| source    | String          | 否    | 资金来源<br>`1`：交易账户<br>`2`：资金账户<br>默认为`1` |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "fillFromSz": "6.5807127",
            "fillToSz": "0.17171580105126",
            "fromCcy": "ADA",
            "status": "running",
            "toCcy": "OKB",
            "uTime": "1661419684687"
        },
        {
            "fillFromSz": "2.997",
            "fillToSz": "0.1683755161661844",
            "fromCcy": "USDC",
            "status": "running",
            "toCcy": "OKB",
            "uTime": "1661419684687"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**  | **类型**  | **描述**                                                                                 |
| :--------- | :------- | :--------------------------------------------------------------------------------------- |
| status     | String   | 当前兑换进度/状态 <br>`running`: 进行中 <br>`filled`: 已完成 <br>`failed`: 失败             |
| fromCcy    | String   | 小币支付币种                                                                |
| toCcy      | String   | 兑换的主流币                                                                      |
| fillFromSz | String   | 小币偿还币种支付数量                                                                |
| fillToSz   | String   | 兑换的主流币成交数量                                                                      |
| uTime      | String   | 交易时间戳，Unix时间戳为毫秒数格式，如 1597026383085                                 |
