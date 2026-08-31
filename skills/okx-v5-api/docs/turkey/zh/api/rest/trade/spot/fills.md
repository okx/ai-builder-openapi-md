---
site: turkey
lang: zh
name: GET / 获取成交明细（近三天）
method: GET
path: /api/v5/trade/fills
module: trade
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取成交明细（近三天） [order-book-trading-trade-get-transaction-details-last-3-days]

获取近3天的订单成交明细信息

#### 限速：60次/2s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/trade/fills`

> 请求示例

```shell
GET /api/v5/trade/fills

```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# 获取成交明细
result = tradeAPI.get_fills()
print(result)
```

#### 请求参数

| 参数名            | 类型   | 是否必须   | 描述                                        |
|:---------------| :----- |:-------|:------------------------------------------|
| instType       | String | 否      | 产品类型<br>`SPOT`：币币                         |
| instId         | String | 否      | 产品 ID，如`BTC-USDT`                         |
| ordId          | String | 否      | 订单 ID                                     |
| after          | String | 否      | 请求此 ID 之前（更旧的数据）的分页内容，传的值为对应接口的 `billId`  |
| before         | String | 否      | 请求此 ID 之后（更新的数据）的分页内容，传的值为对应接口的 `billId`  |
| begin          | String | 否      | 筛选的开始时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085` |
| end            | String | 否      | 筛选的结束时间戳，Unix 时间戳为毫秒数格式，如 `1597027383085` |
| limit          | String | 否      | 返回结果的数量，最大为100，默认100条                     |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "side": "buy",
            "fillSz": "0.00192834",
            "fillPx": "51858",
            "fillPxVol": "",
            "fillFwdPx": "",
            "fee": "-0.00000192834",
            "fillPnl": "0",
            "ordId": "680800019749904384",
            "feeRate": "-0.001",
            "instType": "SPOT",
            "fillPxUsd": "",
            "instId": "BTC-USDT",
            "clOrdId": "",
            "posSide": "net",
            "billId": "680800019754098688",
            "fillMarkVol": "",
            "tag": "",
            "fillTime": "1708587373361",
            "execType": "T",
            "fillIdxPx": "",
            "tradeId": "744876980",
            "fillMarkPx": "",
            "feeCcy": "BTC",
            "ts": "1708587373362"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**     | **类型**   | **描述**                                                                         |
|:------------|:---------|:-------------------------------------------------------------------------------|
| instType    | String   | 产品类型                                                                           |
| instId      | String   | 产品 ID                                                                          |
| tradeId     | String   | 最新成交 ID                                                                        |
| ordId       | String   | 订单 ID                                                                          |
| clOrdId     | String   | 用户自定义订单ID                                                                      |
| billId      | String   | 账单 ID                                                                          |
| tag         | String   | 订单标签                                                                           |
| fillPx      | String   | 最新成交价格                                                                         |
| fillSz      | String   | 最新成交数量                                                                         |
| fillIdxPx   | String   | 交易执行时的指数价格 <br>对于交叉现货币对，返回 baseCcy-USDT 的指数价格。 例如LTC-ETH，该字段返回 LTC-USDT 的指数价格。 |
| fillPnl     | String   | 最新成交收益，适用于有成交的平仓订单。其他情况均为0。                                                    |
| fillPxVol   | String   | 成交时的隐含波动率，仅适用于期权，其他业务线返回空字符串""                                                 |
| fillPxUsd   | String   | 成交时的期权价格，以USD为单位，仅适用于期权，其他业务线返回空字符串""                                          |
| fillMarkVol | String   | 成交时的标记波动率，仅适用于期权，其他业务线返回空字符串""                                                 |
| fillFwdPx   | String   | 成交时的远期价格，仅适用于期权，其他业务线返回空字符串""                                                  |
| fillMarkPx  | String   | 成交时的标记价格，仅适用于 `交割`/`永续`/`期权`                                                   |
| side        | String   | 订单方向 `buy`：买 `sell`：卖                                                          |
| posSide     | String   | 持仓方向 `long`：多 `short`：空 买卖模式返回 `net`                                           |
| execType    | String   | 流动性方向 `T`：taker `M`：maker<br>不适用于系统订单比如强平和ADL                                  |
| feeCcy      | String   | 交易手续费币种或者返佣金币种                                                                 |
| fee         | String   | 手续费金额或者返佣金额，手续费扣除为‘负数’，如-0.01；手续费返佣为‘正数’，如 0.01                                |
| ts          | String   | 成交明细产生时间，Unix时间戳的毫秒数格式，如`1597026383085`                                        |
| fillTime    | String   | 成交时间，与订单频道的`fillTime`相同                                                        |
| feeRate      | String   | 手续费费率。 该字段仅对 `币币`和`杠杆`返回                                                                                                                             |
