---
site: turkey
lang: zh
name: GET / 获取一键兑换主流币历史记录
method: GET
path: /api/v5/trade/easy-convert-history
module: trade
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取一键兑换主流币历史记录 [order-book-trading-trade-get-easy-convert-history]

查询一键兑换主流币过去7天内的历史记录与进度状态。

#### 限速：1次/2s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/trade/easy-convert-history`

> 请求示例

```shell
GET /api/v5/trade/easy-convert-history
```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# 获取一键兑换主流币历史记录
result = tradeAPI.get_easy_convert_history()
print(result)
```

#### 请求参数

| 参数名    | 类型    | 是否必须  | 描述                                                  |
| :------- | :----- | :------- |:----------------------------------------------------|
| after    | String | 否       | 查询在此之前(不包含)的内容，值为时间戳，Unix时间戳为毫秒数格式，如`1597026383085` |
| before   | String | 否       | 查询在此之后(不包含)的内容，值为时间戳，Unix时间戳为毫秒数格式，如`1597026383085` |
| limit    | String | 否       | 返回的结果集数量，默认为100，最大为100                              |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "fillFromSz": "0.1761712511667539",
            "fillToSz": "6.7342205900000000",
            "fromCcy": "OKB",
            "status": "filled",
            "toCcy": "ADA",
            "acct": "18",
            "uTime": "1661313307979"
        },
        {
            "fillFromSz": "0.1722106121112177",
            "fillToSz": "2.9971018300000000",
            "fromCcy": "OKB",
            "status": "filled",
            "toCcy": "USDC",
            "acct": "18",
            "uTime": "1661313307979"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**    | **类型**    | **描述**                                                          |
|:-----------|:----------|:----------------------------------------------------------------|
| fromCcy    | String    | 小币支付币种                                                          |
| fillFromSz | String    | 对应的小币支付数量                                                       |
| toCcy      | String    | 兑换到的主流币                                                         |
| fillToSz   | String    | 兑换到的主流币数量                                                       |
| acct       | String    | 兑换到的主流币所在的账户<br>`6`：资金账户 <br>`18`：交易账户                          |
| status     | String    | 当前兑换进度/状态 <br>`running`: 进行中 <br>`filled`: 已完成 <br>`failed`: 失败 |
| uTime      | String    | 交易时间戳，Unix时间戳为毫秒数格式，如 1597026383085                             |
