---
site: okex
lang: zh
name: 获取询价单信息
method: GET
path: /api/v5/rfq/rfqs
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取询价单信息 [block-trading-rest-api-get-rfqs]

获取用户发出的或收到的询价单信息

#### 限速: 2次/2s
#### 限速规则：User ID
#### HTTP Requests

`GET /api/v5/rfq/rfqs`

> 请求示例

```shell
GET /api/v5/rfq/rfqs
```

```python
import okx.BlockTrading as BlockTrading

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘：1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# 获取询价单信息
result = blockTradingAPI.get_rfqs()
print(result)
```

#### 请求参数
| 参数名  | 类型  | 是否必须    | 描述                                                                                                                                 |
| :------ | :----- | :----- |:-----------------------------------------------------------------------------------------------------------------------------------|
| rfqId   |String |否 | 询价单ID .                                                                                                                            |
| clRfqId |String |否 | 客户询价单自定义ID，当 clRfqId 和 rfqId 都传时，以 rfqId 为准                                                                                        |
| state	  |String |否 | 询价单的状态<br>`active` `canceled` `pending_fill` `filled` `expired` `failed` `traded_away`<br>`filled` 表示询价单已成功按照做市商的报价成交。<br>`traded_away` 仅适用于报价方。同一笔询价单可能对一个报价方显示为 `filled`，而对另一个报价方显示为 `traded_away`。<br>示例：询价方创建询价单 → 做市商A报价 pxA，做市商B报价 pxB → pxA 优于 pxB → 询价方执行做市商A的报价 → 做市商A看到 `filled`，做市商B看到 `traded_away`。  |
| beginId |String |否 | 请求的起始询价单ID，请求此ID之后（更新的数据）的分页内容，不包括 beginId                                                                                         |
| endId	  |String |否 | 请求的结束询价单ID，请求此ID之前（更旧的数据）的分页内容，不包括 endId                                                                                           |
| limit	  |String |否 | 返回结果的数量，最大为100，默认100条                                                                                                                    |

> 返回示例

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "rfqId": "123456",
            "clRfqId": "",
            "tag": "123456",
            "traderCode": "VITALIK",
            "validUntil": "1650969031817",
            "allowPartialExecution": false,
            "state": "filled",
            "flowType": "",
            "counterparties": [
                "SATOSHI"
            ],
            "legs": [
                {
                    "instId": "BTC-USDT",
                    "tdMode": "cross",
                    "ccy": "USDT",
                    "side": "buy",
                    "posSide": "long",
                    "sz": "25",
                    "tgtCcy": "base_ccy",
                    "tradeQuoteCcy": "USDT"
                }
            ],
            "cTime": "1650968131817",
            "uTime": "1650968164944"
        },
        {
            "rfqId": "1234567",
            "clRfqId": "",
            "tag": "1234567",
            "traderCode": "VITALIK",
            "validUntil": "1650967623729",
            "state": "filled",
            "flowType": "",
            "counterparties": [
                "SATOSHI"
            ],
            "legs": [
                {
                    "instId": "BTC-USDT",
                    "tdMode": "cross",
                    "ccy": "USDT",
                    "side": "buy",
                    "posSide": "long",
                    "sz": "1500000",
                    "tgtCcy": "quote_ccy",
                    "tradeQuoteCcy": "USDT"
                }
            ],
            "cTime": "1650966723729",
            "uTime": "1650966816577"
        }
    ]
}
```

#### 返回参数

| 参数名  | 类型               | 描述                                                                                                                                 |
| :------ |:-----------------|:-----------------------------------------------------------------------------------------------------------------------------------|
| code	            | String           | 结果代码，0 表示成功。                                                                                                                       |
| msg	            | String           | 错误信息，如果代码不为 0，则不为空。                                                                                                                |
| data	            | Array of objects | 	包含结果的对象数组                                                                                                                         |
| > cTime	        | String           | 	询价单创建时间，Unix时间戳的毫秒数格式。                                                                                                            |
| > uTime	        | String           | 	询价单状态更新时间，Unix时间戳的毫秒数格式。                                                                                                          |
| > state	        | String           | 	询价单的状态<br>`active` `canceled` `pending_fill` `filled` `expired` `failed` `traded_away`<br>`filled` 表示询价单已成功按照做市商的报价成交。<br>`traded_away` 仅适用于报价方。同一笔询价单可能对一个报价方显示为 `filled`，而对另一个报价方显示为 `traded_away`。<br>示例：询价方创建询价单 → 做市商A报价 pxA，做市商B报价 pxB → pxA 优于 pxB → 询价方执行做市商A的报价 → 做市商A看到 `filled`，做市商B看到 `traded_away`。 |
| > counterparties	| Array of strings | 	报价方列表                                                                                                                             |
| > validUntil	    | String           | 	询价单的过期时间，Unix时间戳的毫秒数格式。                                                                                                           |
| > clRfqId	        | String           | 	询价单自定义ID，为客户敏感信息，不会公开，对报价方返回""。                                                                                                   |
| > tag             | String           |   询价单标签，与此询价单关联的大宗交易将有相同的标签。                                             |
| > flowType | String           | 识别询价单的类型。 <br>仅适用于报价方，返回""给询价方。|
| > traderCode	    | String           | 	询价方唯一标识代码，询价时 anonymous 设置为 `true` 时不可见                                                                                           |
| > rfqId	        | String           | 	询价单ID                                                                                                                             |
| > allowPartialExecution	        | Boolean          | RFQ是否可以被部分执行，如果腿的比例和原RFQ一致。有效值为`true`或`false`。未指定时，默认为`false`。	                                          |
| > legs	        | Array of objects | 组合交易，每个请求最多可放置15条腿                                                                                                                               |
| >> instId	        | String           | 	产品ID，如 "BTC-USDT-SWAP"                                                                                                                              |
| >> tdMode	        | String	          | 交易模式 <br>保证金模式：`cross`全仓 `isolated`逐仓 <br>非保证金模式：`cash`非保证金. <br>如未提供，tdMode 将继承系统设置的默认值： <br>合约模式 & 现货: `cash` <br>合约模式和跨币种保证金模式下买入期权： `isolated` <br>其他情况: `cross`                 |
| >> ccy   	        | String	          | 保证金币种，仅适用于`合约模式`下的`全仓杠杆`订单 <br>在其他情况下该参数将被忽略。                                                                             |
| >> sz	            | String           | 	委托数量                                                                                                                              |
| >> side	        | String           | 	询价单方向 <br>有效值为`buy`和`sell`。                                                                                                                         |
| >> posSide	    | String           |   持仓方向 <br>买卖模式下默认为`net`。如未指定，则返回""，相当于`net`。 <br>在开平仓模式下仅可选择`long`或`short`。 如未指定，则返回""，对应于为交易开新仓位的方向（买入=>`long`，卖出=>`short`）。<br>仅适用交割、永续。   |
| >> tgtCcy	        | String           | 	委托数量的类型 <br>定义`sz`属性的单位。仅适用于 instType=`SPOT`。有效值为`base_ccy`和`quote_ccy`。未指定时，默认为`base_ccy`。                                                                                                                          |
| >> tradeQuoteCcy	        | String           | 	交易使用的计价币种。仅适用于 SPOT。<br>默认值为 instId 的报价币种，例如：对于 `BTC-USD`，默认值为 `USD`。                                                                                                                          |
| > groupId | String | 组合询价单ID<br> 只适用于组合询价单，普通询价单返回 "" |
| > acctAlloc | Array of objects | 组合询价单的账户分配<br> 只适用于 Taker |
| >> acct | String | 账户名 |
| >> legs | Array of objects | 组合交易 |
| >>> instId | String | 产品ID |
| >>> sz | String | 委托数量 |
| >>> tdMode | String | 交易模式 |
| >>> ccy | String | 保证金币种 |
| >>> posSide | String | 持仓方向 |

<aside class="notice">
组合询价单介绍<br><br>

1. allowPartialExecution 字段始终为 true，适用于 Taker 和 Maker 的组合询价单。<br>
2. 新增返回参数 acctAlloc ，包含所有账户分配信息，但仅适用于 Taker。<br>
3. 新增返回参数 groupId，适用于 Taker 和 Maker。<br>
4. 对于组合询价单状态<br>
&nbsp;&nbsp;&nbsp;&nbsp;1. 如果任何分配账户处于待执行状态，则状态为 pending_fill<br>
&nbsp;&nbsp;&nbsp;&nbsp;2. 否则，<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. 如果任何分配账户已成交，则状态为 filled<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. 如果所有分配账户均失败，则状态为 failed<br>
</aside>
