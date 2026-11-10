---
site: okex
lang: zh
name: 询价
method: POST
path: /api/v5/rfq/create-rfq
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 询价 [block-trading-rest-api-create-rfq]

创建一个询价单。

<aside class="notice">
<br>
在模拟交易中询价时，请选择交易机器人“WAGMI”作为交易对手。<br>
交易机器人提供的报价仅供参考。 
</aside>
了解更多，请访问[帮助中心 > 常见问题 > 交易 > 流动性市场 > 模拟交易](/cn/help/demo-trading)

#### 限速: 5次/2s；80次/12h
#### 限速规则：User ID
#### HTTP Requests

`POST /api/v5/rfq/create-rfq`

> 请求示例

```shell
POST /api/v5/rfq/create-rfq

{
    "anonymous": true,
    "counterparties":[
        "Trader1",
        "Trader2"
    ],
    "allowPartialExecution":false,
    "clRfqId":"rfq01",
    "tag":"123456",
    "legs":[
        {
            "sz":"25",
            "side":"buy",
            "posSide": "long",
            "tdMode":"cross",
            "ccy":"USDT",
            "instId":"BTC-USD-221208-100000-C"
        },
        {
            "sz":"150",
            "side":"buy",
            "posSide": "long",
            "tdMode":"cross",
            "ccy":"USDT",
            "instId":"ETH-USDT",
            "tgtCcy":"base_ccy"
        }
    ]
}
```

```python
import okx.BlockTrading as BlockTrading

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘：1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# 询价
result = blockTradingAPI.create_rfq(
    anonymous=True,
    counterparties=[
        "Trader1",
        "Trader2"
    ],
    clRfqId= "rfq01",
    legs=[
        {
            "sz":"25",
            "side":"buy",
            "posSide": "long",
            "tdMode":"cross",
            "ccy":"USDT",
            "instId":"BTC-USD-221208-100000-C"
        },
        {
            "sz":"150",
            "side":"buy",
            "posSide": "long",
            "tdMode":"cross",
            "ccy":"USDT",
            "instId":"ETH-USDT",
            "tgtCcy":"base_ccy"
        }
    ]
)
print(result)
```

#### 请求参数
| 参数名  | 类型                | 是否必须    | 描述                                                                                                                                                                                                                                |
| :------ |:------------------| :----- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| counterparties| Array of strings	 |是| 	希望收到询价的报价方列表，可通过`/api/v5/rfq/counterparties/`获取。                                                                                                                                                                                 |
| anonymous	| Boolean           |否| 	是否匿名询价，`true`表示匿名询价，`false`表示公开询价，默认值为 `false`，为`true`时，即使在交易执行之后，身份也不会透露给报价方。                                                                                                                                                   |
| clRfqId   | String            |否| 询价单自定义ID，字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                                                                                                                                                                  |
| tag       | String            |否| 询价单标签，与此询价单关联的大宗交易将有相同的标签。 <br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间。                                                                                                                                                            |
| allowPartialExecution   | Boolean           |否| RFQ是否可以被部分执行，如果腿的比例和原RFQ一致。有效值为`true`或`false`。默认为`false`。                                                                                                                                                                         |
| legs	    | Array of objects  |是| 组合交易，每次最多可以提交15组交易信息                                                                                                                                                                                                              |
| > instId	| String	           |是| 产品ID                                                                                                                                                                                                                              |
| > tdMode	|String	            | 否 | 交易模式 <br>保证金模式：`cross`全仓 `isolated`逐仓 <br>非保证金模式：`cash`非保证金. <br>如未提供，tdMode 将继承系统设置的默认值： <br>合约模式 & 现货: `cash` <br>`合约模式`和`跨币种保证金模式`下买入期权： `isolated` <br>其他情况: `cross`                                                          |
| > ccy   	|String	            | 否 | 保证金币种，仅适用于`合约模式`下的`全仓杠杆`订单 <br>在其他情况下该参数将被忽略。                                                                                                                                                                                  |
| > sz	    | String	           |是| 委托数量                                                                                                                                                                                                                              |
| > lmtPx | String | 否 | 询价方期望的报价价格<br> 若提供了该字段，在报价价格优于或等于所指定价格，询价将自动被执行，直到该询价单被取消或过期为止。<br> 该字段必须提供所有组合交易的价格，以便自动执行询价；或者对所有组合交易留空，否则请求将被拒绝。<br> 自动执行的方向取决于询价单的腿方向。<br> 对于`币币/币币杠杆/交割/永续`，lmtPx将以计价货币单位计算。<br> 对于`期权`，lmtPx将以结算货币单位计算。<br> 该字段不会被披露给交易对手方。 |
| > side	| String	           |是| 询价单方向                                                                                                                                                                                                                             |
| > posSide	| String	           |否| 持仓方向 <br>买卖模式下默认为`net`。在开平仓模式下仅可选择`long`或`short`。 <br>如未指定，则处于开平仓模式下的用户始终会开新仓位。 <br>仅适用交割、永续。                                                                                                                                     |
| > tgtCcy	| String	           |否| 委托数量的类型 <br>定义`sz`属性的单位。仅适用于 instType=`SPOT`。有效值为`base_ccy`和`quote_ccy`。未指定时，默认为`base_ccy`。                                                                                                                                       |
| > tradeQuoteCcy	|String	            |否| 交易使用的计价币种。仅适用于 SPOT。<br>默认值为 instId 的报价币种，例如：对于 `BTC-USD`，默认值为 `USD`。                                                                                                                          |
| acctAlloc | Array of objects | No | 组合询价单的账户分配 |
| > acct | String | Yes | 账户名 |
| > legs | Array of objects | Yes | 组合交易 |
| >> sz | String | Yes | 委托数量 |
| >> instId | String | Yes | 产品ID |
| >> tdMode | String | No | 交易模式 |
| >> ccy | String | No | 保证金币种 |
| >> posSide | String | No | 持仓方向 |

> 返回示例

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "cTime":"1611033737572",
            "uTime":"1611033737572",
            "traderCode":"SATOSHI",
            "tag":"123456",
            "rfqId":"22534",
            "clRfqId":"rfq01",
            "allowPartialExecution":false,
            "state":"active",
            "validUntil":"1611033857557",
            "counterparties":[
                "Trader1",
                "Trader2"
            ],
            "legs":[
                {
				    "instId":"BTC-USD-221208-100000-C",
                    "sz":"25",
                    "side":"buy",
                    "posSide": "long",
                    "tdMode":"cross",
                    "ccy":"USDT",
                    "tgtCcy":""
                },
                {
                    "instId":"ETH-USDT",
                    "sz":"150",
                    "side":"buy",
                    "posSide": "long",
                    "tdMode":"cross",
                    "ccy":"USDT",
                    "tgtCcy":"base_ccy",
                    "tradeQuoteCcy": "USDT"
                }
            ]
        }
    ]
}
```

#### 返回参数
| 参数名               | 类型               | 描述                                                                                                                                |
|:------------------|:-----------------|:----------------------------------------------------------------------------------------------------------------------------------|
| code	             | String           | 结果代码，0 表示成功。                                                                                                                            |
| msg	              | String           | 错误信息，如果代码不为 0，则不为空。                                                                                                               |
| data	             | Array of objects | 	询价单结果                                                                                                                                    |
| > cTime           | String           | 询价单创建时间，Unix时间戳的毫秒数格式。                                                                                                             |
| > uTime	          | String           | 询价单状态更新时间，Unix时间戳的毫秒数格式。                                                                                                        |
| > state	          | String           | 询价单的状态<br>有效值为 `active` `canceled` `pending_fill` `filled` `expired` `traded_away` `failed` <br>`filled` 表示询价单已成功按照做市商的报价成交。<br>`traded_away` 仅适用于报价方。同一笔询价单可能对一个报价方显示为 `filled`，而对另一个报价方显示为 `traded_away`。<br>示例：询价方创建询价单 → 做市商A报价 pxA，做市商B报价 pxB → pxA 优于 pxB → 询价方执行做市商A的报价 → 做市商A看到 `filled`，做市商B看到 `traded_away`。   |
| > counterparties	 | Array of strings | 报价方列表	                                                                                                                                  |
| > validUntil	     | String           | 	询价单的过期时间，Unix时间戳的毫秒数格式。<br>若所有腿都为期权，则询价单将在10分钟后过期；其他情况，询价单将在2分钟后过期。                                                                                                         |
| > clRfqId	        | String           | 询价单自定义ID，为客户端敏感信息，不会公开，对报价方返回""。                                                                                             |
| > tag             | String           | RFQ标签，与此RFQ关联的大宗交易将有相同的标签。                                             |
| > allowPartialExecution   | Boolean| RFQ是否可以被部分执行，如果腿的比例和原RFQ一致。有效值为`true`或`false`。未指定时，默认为`false`。|
| > traderCode	     | String           | 	询价方唯一标识代码。                                                                                                                            |
| > rfqId	          | String           | 	询价单ID                                                                                                                                      |
| > legs	           | Array of objects | 组合交易，每个请求最多可放置15条腿                                                                                                                                      |
| >> instId	        | String           | 	产品ID，如 "BTC-USDT-SWAP"                                                                                                                                      |
| >> tdMode	        |String	           | 交易模式 <br>保证金模式：`cross`全仓 `isolated`逐仓 <br>非保证金模式：`cash`非保证金. <br>如未提供，tdMode 将继承系统设置的默认值： <br>合约模式 & 现货: `cash` <br>`合约模式`和`跨币种保证金模式`下买入期权： `isolated` <br>其他情况: `cross`                 |
| >> ccy   	        |String	           | 保证金币种，仅适用于`合约模式`下的`全仓杠杆`订单 <br>在其他情况下该参数将被忽略。                                                                             |
| >> sz	            | String           | 	委托数量                                                                                                                                      |
| >> side	          | String           | 	询价单方向 <br>有效值为`buy`和`sell`。                                                                                                                                   |
| >> posSide	    |String            | 持仓方向 <br>买卖模式下默认为`net`。如未指定，则返回""，相当于`net`。 <br>在开平仓模式下仅可选择`long`或`short`。 如未指定，则返回""，对应于为交易开新仓位的方向（买入=>`long`，卖出=>`short`）。<br>仅适用交割、永续。   |
| >> tgtCcy	        | String           | 	委托数量的类型 <br>定义`sz`属性的单位。仅适用于 instType=`SPOT`。有效值为`base_ccy`和`quote_ccy`。未指定时，默认为`base_ccy`。                                                                                                                                 |
| >> tradeQuoteCcy	|String	| 交易使用的计价币种。仅适用于 SPOT。<br>默认值为 instId 的报价币种，例如：对于 `BTC-USD`，默认值为 `USD`。                                                                                                                          |
| > groupId | String | 组合询价单ID<br>只适用于组合询价单，普通询价单返回 "" |
| > acctAlloc | Array of objects | 组合询价单的账户分配 |
| >> acct | String | 账户名 |
| >> sCode | String | 事件执行结果的code，0代表成功 |
| >> sMsg | String | 事件执行失败或成功时的msg |
| >> legs | Array of objects | 组合交易 |
| >>> instId | String | 产品ID |
| >>> sz | String | 委托数量 |
| >>> tdMode | String | 交易模式 |
| >>> ccy | String | 保证金币种 |
| >>> posSide | String | 持仓方向 |

<aside class="notice">
组合询价单功能介绍<br><br>

1. 只有母账户能创建组合询价单，可分配的子账户范围为其普通子账户和资管子账户。<br>
2. 用户将传入 acctAlloc 请求参数来指示组合询价单的账户分配详情，包括账户名称、产品ID、分配的数量等。母账户也允许参与，并应标识为 "0"。对于 tdMode、ccy 和 posSide 字段，如果留空，则继承系统默认值。<br>
3. 新增 groupId，acctAlloc 作为响应参数。<br>
4. 分配子账户的上限为 10 个。如果超过上限，将收到错误代码 70516。<br>
5. 对于每个交易产品，所有账户中腿数量的总和应等于组合询价单中的总量。如果不相等，将收到错误代码 70514。<br>
6. 对于每个子账户，腿数量与组合询价单的比例必须在所有交易产品中保持一致。如果不一致，将收到错误代码 70515。以下是一个示例：<br>
&nbsp;&nbsp;&nbsp;&nbsp;1. 父级询价单腿<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. 产品：BTC-USDT，数量：50；产品：ETH-USDT，数量：100<br>
&nbsp;&nbsp;&nbsp;&nbsp;2. 子级询价单腿，正常情况<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. 账户1：产品：BTC-USDT，数量：30；产品：ETH-USDT，数量：60（比例：0.6）<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. 账户2：产品：BTC-USDT，数量：20；产品：ETH-USDT，数量：40（比例：0.4）<br>
&nbsp;&nbsp;&nbsp;&nbsp;3. 子级询价单腿，异常情况<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. 账户1：产品：BTC-USDT，数量：30；产品：ETH-USDT，数量：50<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. 账户2：产品：BTC-USDT，数量：20；产品：ETH-USDT，数量：50<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. 总数量相等，但不同子账户的比例不一致。<br>
7. 对于 allowPartialExecution 字段，即使用户传入，也将被忽略。对于组合询价单，allowPartialExecution 始终为 true，因为任何子账户都有可能执行失败， Taker 无法确定询价单是否可以部分或完全成交。因此，Maker 应将其视为可以部分成交的询价单。<br>
8. 若任何子账户执行失败，则不会创建组合询价单。<br>
</aside>
