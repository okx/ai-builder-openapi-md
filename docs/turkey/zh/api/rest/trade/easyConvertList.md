---
site: turkey
lang: zh
name: GET / 获取一键兑换主流币币种列表
method: GET
path: /api/v5/trade/easy-convert-currency-list
module: trade
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取一键兑换主流币币种列表 [order-book-trading-trade-get-easy-convert-currency-list]

获取小币一键兑换主流币币种列表。仅可兑换余额在 $10 以下币种。

#### 限速：1次/2s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/trade/easy-convert-currency-list`

> 请求示例

```shell
GET /api/v5/trade/easy-convert-currency-list
```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# 获取小币一键兑换主流币币种列表
result = tradeAPI.get_easy_convert_currency_list()
print(result)
```
#### 请求参数

| 参数名    | 类型     | 是否必须 | 描述                                     |
| :------- |:-------|:-----|:---------------------------------------|
| source  | String | 否    | 资金来源<br>`1`：交易账户<br>`2`：资金账户<br>默认为`1` |
                                                                     

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "fromData": [
                {
                    "fromAmt": "6.580712708344864",
                    "fromCcy": "ADA"
                },
                {
                    "fromAmt": "2.9970000013055097",
                    "fromCcy": "USDC"
                }
            ],
            "toCcy": [
                "USDT",
                "BTC",
                "ETH",
                "OKB"
            ]
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**  | **类型**          | **描述**                                                                                 |
| :--------- |:----------------| :--------------------------------------------------------------------------------------- |
| fromData   | Array of objects | 当前拥有并可兑换的小币币种列表信息                                                                |
| > fromCcy   | String          | 可兑换币种                                                                                 |
| > fromAmt   | String          | 可兑换币种数量                                                                              |
| toCcy      | Array of strings | 可转换成的主流币币种列表                                                                      |
