---
site: okex
lang: zh
name: GET / 获取一键还债历史记录
method: GET
path: /api/v5/trade/one-click-repay-history
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取一键还债历史记录 [order-book-trading-trade-get-one-click-repay-history]

查询一键还债近7天的历史记录与进度状态。仅适用于`跨币种保证金模式`/`组合保证金模式`。

#### 限速：1次/2s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/trade/one-click-repay-history`

> 请求示例

```shell
GET /api/v5/trade/one-click-repay-history
```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# 获取一键还债历史记录
result = tradeAPI.oneclick_repay_history()
print(result)
```

#### 请求参数

| 参数名    | 类型    | 是否必须  | 描述                                                                 |
| :------- | :----- | :------- | :-------------------------------------------------------------------- |
| after    | String | 否       | 查询在此之前的内容，值为时间戳，Unix时间戳为毫秒数格式，如`1597026383085`  |
| before   | String | 否       | 查询在此之后的内容，值为时间戳，Unix时间戳为毫秒数格式，如`1597026383085`  |
| limit    | String | 否       | 返回的结果集数量，默认为100，最大为100  |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "debtCcy": "USDC",
            "fillDebtSz": "6950.4865447900000000",
            "fillRepaySz": "4.3067975995094930",
            "repayCcy": "ETH",
            "status": "filled",
            "uTime": "1661256148746"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**  | **类型**  | **描述**                                                                                 |
| :--------- | :------- | :--------------------------------------------------------------------------------------- |
| debtCcy    | String   | 负债币种                                                                |
| fillDebtSz     | String   | 对应的负债币种成交数量                                                                |
| repayCcy   | String   | 偿还币种                                                                      |
| fillRepaySz    | String   | 偿还币种实际支付数量                                                                      |
| status     | String   | 当前还债进度/状态 <br>`running`: 进行中 <br>`filled`: 已完成 <br>`failed`: 失败             |
| uTime      | String   | 交易时间戳，Unix时间戳为毫秒数格式，如 1597026383085                                 |
