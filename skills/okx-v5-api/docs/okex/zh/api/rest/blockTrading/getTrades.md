---
site: okex
lang: zh
name: 获取大宗交易信息
method: GET
path: /api/v5/rfq/trades
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取大宗交易信息 [block-trading-rest-api-get-trades]

获取该用户大宗交易成交信息

#### 限速: 5次/2s
#### 限速规则：User ID
#### HTTP Requests

`GET /api/v5/rfq/trades`

> 请求示例

```shell
GET /api/v5/rfq/trades
```

```python
import okx.BlockTrading as BlockTrading

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘：1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# 获取大宗交易信息
result = blockTradingAPI.get_trades()
print(result)
```

#### 请求参数
| 参数名  | 类型  | 是否必须    | 描述                                                                                                                  |
| :------ | :----- | :----- |:--------------------------------------------------------------------------------------------------------------------|
| rfqId    |String |否 | 询价单ID                                                                                                               |
| clRfqId  |String |否 | 由用户设置的询价单ID. 如果 `clRfqId` 和 `rfqId` 都通过了，rfqId 将被视为主要                                                             |
| quoteId  |String |否 | 报价单ID                                                                                                            |
| blockTdId|String |否 | 大宗交易ID                                                                                                            |
| clQuoteId|String |否 | 由用户设置的报价单ID。如果同时传递了 `clQuoteId` 和 `quoteId`，则 quoteId 将被视为主要标识符                                                 |
| beginId  |String |否 | 请求的起始大宗交易ID，请求此ID之后（更新的数据）的分页内容，不包括 beginId                                                                     |
| endId	   |String |否 | 请求的结束大宗交易ID，请求此ID之前（更旧的数据）的分页内容，不包括 endId                                                                            |
| beginTs  |String |否 | 用开始时间戳筛选交易执行时间（UTC时区）。Unix时间戳的毫秒数格式，如 1597026383085。                                                                                     |
| endTs    |String |否 | 用结束时间戳筛选交易执行时间（UTC时区）。Unix时间戳的毫秒数格式，如 1597026383085。                                                                                             |
| limit	   |String |否 | 返回结果的数量，最大为100，默认100条。<br>如果请求范围内的交易数量大于100，则返回该范围内最近的100笔交易。                                                                                                      |
| isSuccessful  | Boolean |否 | 交易是否成功。<br/>`true`: 成功，默认值。<br/> `false`: 未成功。 |

> 返回示例

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "rfqId": "123456",
            "clRfqId": "",
            "quoteId": "0T5342O",
            "clQuoteId": "",
            "blockTdId": "439127542058958848",
            "tag": "123456",
            "isSuccessful": true,
            "errorCode": "",
            "legs": [
                {
                    "instId": "BTC-USDT",
                    "side": "sell",
                    "sz": "0.666",
                    "px": "100",
                    "tradeId": "439127542058958850",
                    "fee": "-0.0333",
                    "feeCcy": "USDT",
                    "tradeQuoteCcy": "USDT"
                }
            ],
            "cTime": "1650968164900",
            "tTraderCode": "SATS",
            "mTraderCode": "MIKE"
        },
        {
            "rfqId": "1234567",
            "clRfqId": "",
            "quoteId": "0T533T0",
            "clQuoteId": "",
            "blockTdId": "439121886014849024",
            "tag": "123456",
            "isSuccessful": true,
            "errorCode": "",
            "legs": [
                {
                    "instId": "BTC-USDT",
                    "side": "sell",
                    "sz": "0.532",
                    "px": "100",
                    "tradeId": "439121886014849026",
                    "fee": "-0.0266",
                    "feeCcy": "USDT",
                    "tradeQuoteCcy": "USDT"
                }
            ],
            "cTime": "1650966816550",
            "tTraderCode": "SATS",
            "mTraderCode": "MIKE"
        }
    ]
}
```

#### 返回参数

| 参数名  | 类型   | 描述                                               |
| :------ | :----- |:-------------------------------------------------|
| code	            | String| 结果代码，0 表示成功。                                                 |
| msg	            | String| 错误信息，如果代码不为 0，则不为空。                                     |
| data	            | Array of objects| 	包含结果的对象数组                                       |
| > cTime	        | String| 	执行创建的时间，Unix时间戳的毫秒数格式。                                |
| > rfqId       	| String| 	询价单ID                                                           |
| > clRfqId	        | String| 	询价单自定义ID，为客户敏感信息，不会公开，对报价方返回""。                 |
| > quoteId	        | String| 	报价单ID                                                           |
| > clQuoteId	    | String| 	报价单自定义ID，为客户敏感信息，不会公开，对询价方返回""。                 |
| > blockTdId	    | String| 	大宗交易ID                                                         |
| > tag             | String|  交易标签，大宗交易将有与其对应的询价单或报价单相同的标签。                 |
| > tTraderCode	    | String| 	询价方唯一标识代码，询价时 anonymous 设置为 `true` 时不可见             |
| > mTraderCode	    | String| 	报价方唯一标识代码。报价时 anonymous 设置为 `true` 时不可见             |
| > isSuccessful | Boolean | 交易是否成功                     |
| > errorCode    | String | 未成功交易的错误码。<br/>对于成功交易为 ""。 |
| > legs	        | Array of objects| 	组合交易                                               |
| >> instId	        | String| 	产品ID                                                            |
| >> px	            | String| 	成交价格                                                           |
| >> sz	            | String| 	成交数量                                                           |
| >> side	        | String| 	询价单方向，buy 或者 sell。                                          |
| >> fee	        | String| 	手续费，正数代表平台返佣 ，负数代表平台扣除                              |
| >> feeCcy	        | String| 	手续费币种                                                          |
| >> tradeId	    | String| 	最新的成交Id                                                        |
| >> tradeQuoteCcy	        | String           | 	交易使用的计价币种。仅适用于 SPOT。<br>默认值为 instId 的报价币种，例如：对于 `BTC-USD`，默认值为 `USD`。                                                                                                                          |
| > acctAlloc | Array of objects | 组合询价单的账户分配 |
| >> blockTdId | String | 大宗交易ID |
| >> errorCode | String | 事件执行结果的code，0代表成功 |
| >> acct | String | 账户名<br> 只适用于 Taker，对于 Maker 返回"" |
| >> legs | Array of objects | 组合交易 |
| >>> instId | String | 产品ID |
| >>> sz | String | 成交数量 |
| >>> tradeId | String | 最新的成交Id |
| >>> fee | String | 手续费 |
| >>> feeCcy | String | 手续费币种 |

<aside class="notice">
组合询价单介绍<br><br>

1. 该接口返回的交易数据应为父级询价单级别，而不是子级询价单执行级别。<br>
2. 对于账户分配，包含所有已成交和未成交的子级询价单，但添加 errorCode 来指示子级询价单是否已成交。<br>
3. 交易结果将仅返回给组合询价单 Taker 及 Maker。分配的子账户和资管账户将无法看到交易结果。分配的账户应通过交易账单获取这些交易。<br>
4. 交易数据仅在所有子级询价单执行后返回。<br>
5. 对于父级询价单的 isSuccessful 字段，<br>
&nbsp;&nbsp;&nbsp;&nbsp;1. 如果任何子级询价单已成交，则返回 true<br>
&nbsp;&nbsp;&nbsp;&nbsp;2. 否则，如果所有子级询价单均失败，则返回 false<br>
6. 父级询价单的 blockTdId 或 legs 的 tradeId 将为空。但将提供账户分配的详细信息，并附带 blockTdId 以及 tradeId。<br>
</aside>
