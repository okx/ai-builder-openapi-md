---
site: okex
lang: zh
name: GET / 获取成交明细（近三天）
method: GET
path: /api/v5/trade/fills
module: trade
rest: https://www.okx.com
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

| 参数名          | 类型   | 是否必须   | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|:-------------| :----- |:-------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | 否      | 产品类型<br>`SPOT`：币币<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`EVENTS`：事件合约                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| instFamily   | String | 否      | 交易品种<br>适用于`交割`/`永续`/`期权`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| instId       | String | 否      | 产品 ID，如`BTC-USDT`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ordId        | String | 否      | 订单 ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| subType      | String | 否 | 成交类型 <br>`1`：买入<br>`2`：卖出 <br>`3`：开多 <br>`4`：开空 <br>`5`：平多 <br>`6`：平空 <br>`100`：强减平多 <br>`101`：强减平空 <br>`102`：强减买入 <br>`103`：强减卖出 <br>`104`：强平平多 <br>`105`：强平平空 <br>`106`：强平买入 <br>`107`：强平卖出 <br>`110`：强平换币转入 <br>`111`：强平换币转出 <br>`118`：系统换币转入 <br>`119`：系统换币转出 <br>`112`：交割平多 <br> `113`：交割平空 <br>`125`：自动减仓平多 <br>`126`：自动减仓平空 <br>`127`：自动减仓买入 <br>`128`：自动减仓卖出 <br>`212`：一键借币的自动借币 <br>`213`：一键借币的自动还币 <br>`204`：大宗交易买 <br>`205`：大宗交易卖 <br>`206`：大宗交易开多 <br>`207`：大宗交易开空 <br>`208`：大宗交易平多 <br>`209`：大宗交易平空<br>`236`：小额兑换买入<br>`237`：小额兑换卖出<br>`270`：价差交易买<br>`271`：价差交易卖<br>`272`：价差交易开多<br>`273`：价差交易开空<br>`274`：价差交易平多<br>`275`：价差交易平空<br> `324`：移仓买入<br> `325`：移仓卖出<br> `326`：移仓开多<br> `327`：移仓开空<br> `328`：移仓平多<br> `329`：移仓平空<br>`376`：质押借币超限买入<br>`377`： 质押借币超限卖出<br>`410`：买入yes<br>`411`：买入no<br>`412`：卖出yes<br>`413`：卖出no<br>`414`：yes结算<br>`415`：no结算|
| after        | String | 否      | 请求此 ID 之前（更旧的数据）的分页内容，传的值为对应接口的`billId`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| before       | String | 否      | 请求此 ID 之后（更新的数据）的分页内容，传的值为对应接口的`billId`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| begin        | String | 否      | 筛选的开始时间戳 `ts`，Unix 时间戳为毫秒数格式，如 `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| end          | String | 否      | 筛选的结束时间戳 `ts`，Unix 时间戳为毫秒数格式，如 `1597027383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| limit        | String | 否      | 返回结果的数量，最大为100，默认100条                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |

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
            "subType": "1",
            "fillMarkVol": "",
            "tag": "",
            "fillTime": "1708587373361",
            "execType": "T",
            "fillIdxPx": "",
            "tradeId": "744876980",
            "fillMarkPx": "",
            "feeCcy": "BTC",
            "ts": "1708587373362",
            "tradeQuoteCcy": "USDT"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                                       |
| :--------- | :------- |:-----------------------------------------------------------------------------|
| instType   | String   | 产品类型                                                                         |
| instId     | String   | 产品 ID                                                                        |
| tradeId    | String   | 最新成交 ID                                                                      |
| ordId      | String   | 订单 ID                                                                        |
| clOrdId    | String   | 用户自定义订单ID                                                                    |
| billId     | String   | 账单 ID                                                                        |
| subType    | String  | 成交类型                                                                         |
| tag        | String   | 订单标签                                                                         |
| fillPx     | String   | 最新成交价格，同"账单流水查询"的 px                                                         |
| fillSz     | String   | 最新成交数量                                                                       |
| fillIdxPx  | String   | 交易执行时的指数价格 <br>对于交叉现货币对，返回 baseCcy-USDT 的指数价格。 如 LTC-ETH，该字段返回LTC-USDT的指数价格。 |
| fillPnl    | String   | 本次成交的已实现盈亏，以结算货币（见 `feeCcy`）计价，仅适用于平仓交易。正数为盈利，负数为亏损。公式：正向合约 = (fillPx − avgPx) × fillSz × ctVal；反向合约 = (1/avgPx − 1/fillPx) × fillSz × ctVal。开仓交易返回0。                                                  |
| fillPxVol | String | 成交时的隐含波动率，仅适用于期权，其他业务线返回空字符串""                                               |
| fillPxUsd | String | 成交时的期权价格，以USD为单位，仅适用于期权，其他业务线返回空字符串""                                        |
| fillMarkVol | String | 成交时的标记波动率，仅适用于期权，其他业务线返回空字符串""                                               |
| fillFwdPx | String | 成交时的远期价格，仅适用于期权，其他业务线返回空字符串""                                                |
| fillMarkPx | String | 成交时的标记价格，仅适用于 `交割`/`永续`/`期权`                                                 |
| side       | String   | 订单方向 `buy`：买 `sell`：卖                                                        |
| posSide    | String   | 持仓方向 `long`：多 `short`：空 买卖模式返回 `net`                                         |
| execType   | String   | 流动性方向 `T`：taker `M`：maker<br>不适用于系统订单比如强平和ADL                                |
| feeCcy     | String   | 交易手续费币种或者返佣金币种                                                               |
| fee        | String   | 手续费金额或者返佣金额，手续费扣除为‘负数’，如-0.01；手续费返佣为‘正数’，如 0.01                              |
| ts         | String   | 系统生成该成交记录的时间戳，Unix毫秒数格式（UTC）。注意：此字段与 `fillTime`（实际撮合成交时间）不同。若需按时间顺序排列成交记录，请使用 `fillTime` 而非 `ts` 进行排序。                                      |
| fillTime         | String   | 成交时间，与订单频道的`fillTime`相同                                                      |
| feeRate      | String   | 手续费费率。 该字段仅对 `币币`和`杠杆`返回                                                     |
| tradeQuoteCcy | String | 用于交易的计价币种。 |

<aside class="notice">
tradeId<br>
当订单种类（category）为 partial_liquidation：强制减仓、full_liquidation：强制平仓、adl：ADL自动减仓时，成交明细 tradeId 字段的值为负数，以便和其他撮合成交场景区分，订单信息 tradeId 字段的值为 0<br>
</aside>
<aside class="notice">
ordId<br>
订单ID, 对于大宗交易总是 "" 。<br>
</aside>
<aside class="notice">
clOrdId<br>
用户自定义订单ID, 对于大宗交易总是 "" 。
</aside>
