---
site: okex
lang: zh
name: 获取报价单信息
method: GET
path: /api/v5/rfq/quotes
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取报价单信息 [block-trading-rest-api-get-quotes]

获取用户发出的或收到的报价单信息

#### 限速: 2次/2s
#### 限速规则：User ID
#### HTTP Requests

`GET /api/v5/rfq/quotes`

> 请求示例

```shell
GET /api/v5/rfq/quotes
```

```python
import okx.BlockTrading as BlockTrading

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘：1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# 获取报价单信息
result = blockTradingAPI.get_quotes()
print(result)
```

#### 请求参数
| 参数名  | 类型  | 是否必须    | 描述                                                                                                                                |
| :------ | :----- | :----- |:----------------------------------------------------------------------------------------------------------------------------------|
| rfqId    |String |否 | 询价单ID                                                                                                                             |
| clRfqId  |String |否 | 询价单自定义ID， 当 clRfqId 和 rfqId 都传时，以 rfqId 为准。                                                                                       |
| quoteId  |String |否 | 报价单ID                                                                                                                             |
| clQuoteId|String |否 | 报价单自定义ID，当 clRfqId 和 rfqId 都传时，以 rfqId 为准。                                                                                        |
| state	   |String |否 | 报价单的状态<br>有效值为 `active` `canceled` `pending_fill` `filled` `expired` `failed` |
| beginId |String |否 | 请求的起始报价单ID，请求此ID之后（更新的数据）的分页内容，不包括 beginId                                                                                        |
| endId	  |String |否 | 请求的结束报价单ID，请求此ID之前（更旧的数据）的分页内容，不包括 endId                                                                                          |
| limit	  |String |否 | 返回结果的数量，最大为100，默认100条                                                                                                                   |

> 返回示例

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "validUntil":"1608997227834",
            "uTime":"1608267227834",
            "cTime":"1608267227834",
            "legs":[
                {
                    "px":"46000",
                    "sz":"25",
                    "instId":"BTC-USD-220114-25000-C",
                    "tdMode":"cross",
                    "ccy":"USDT",
                    "side":"sell",
                    "posSide": "long",
                    "tgtCcy":"",
                    "tradeQuoteCcy": ""
                },
                {
                    "px":"45000",
                    "sz":"25",
                    "instId":"BTC-USDT",
                    "tdMode":"cross",
                    "ccy":"USDT",
                    "side":"buy",
                    "posSide": "long",
                    "tgtCcy":"base_ccy",
                    "tradeQuoteCcy": "USDT"
                }
            ],
            "quoteId":"25092",
            "rfqId":"18753",
            "quoteSide":"sell",
            "state":"canceled",
            "reason":"mmp_canceled",
            "clQuoteId":"cq001",
            "clRfqId":"cr001",
            "tag":"123456",
            "traderCode":"Trader1"
        }
    ]
}
```

#### 返回参数

| 参数名  | 类型   | 描述                                                                          |
| :------ | :----- |:----------------------------------------------------------------------------|
| code	            | String| 结果代码，0 表示成功。                                                                          |
| msg	            | String| 错误信息，如果代码不为 0，则不为空。                                                              |
| data	            | Array of objects| 	包含结果的数组                                                                   |
| > cTime	        | String| 	报价单创建时间，Unix时间戳的毫秒数格式                                                          |
| > uTime	        | String| 	报价单状态更新时间，Unix时间戳的毫秒数格式。                                                     |
| > state	        | String| 	报价单的状态<br>`active` `canceled` `pending_fill` `filled` `expired` `failed`             |
| > reason	      |String	| 状态原因. 有效值包括 `mmp_canceled`.                                      |
| > validUntil	    | String| 	报价单的过期时间，Unix时间戳的毫秒数格式。                                                      |
| > rfqId	        | String| 	询价单ID                                                                                   |
| > clRfqId	        | String| 	询价单自定义ID，为客户敏感信息，不会公开，对报价方返回""。                                         |
| > quoteId	        | String| 	报价单ID                                                                                   |
| > clQuoteId	    | String| 	报价单自定义ID，为客户敏感信息，不会公开，对询价方返回""。                                         |
| > tag             |String | 报价单标签，与此报价单关联的大宗交易将有相同的标签。                                            |
| > traderCode	    | String| 	报价方唯一标识代码，报价时 Anonymous 设置为 `True` 时不可见。                                    |
| > quoteSide	    | String| 	报价单方向，`buy`或者`sell`。当报价单方向为`buy`，对maker来说，执行方向与legs里的方向相同，对taker来说相反。反之同理                                                                |
| > legs	        | Array of objects| 	组合交易                                                                        |
| >> instId	        | String| 	产品ID                                                                                     |
| >> tdMode	        |String	| 交易模式 <br>保证金模式：`cross`全仓 `isolated`逐仓 <br>非保证金模式：`cash`非保证金. <br>如未提供，tdMode 将继承系统设置的默认值： <br>合约模式 & 现货: `cash` <br>合约模式和跨币种保证金模式下买入期权： `isolated` <br>其他情况: `cross`                 |
| >> ccy   	        |String	| 保证金币种，仅适用于`合约模式`下的`全仓杠杆`订单 <br>在其他情况下该参数将被忽略。                                                                             |
| >> sz	            | String| 	委托数量                                                                                    |
| >> px	            | String| 	委托价格.                                                                                   |
| >> side	        | String| 	报价单方向                                                                                  |
| >> posSide        | String|   持仓方向 <br>买卖模式下默认为`net`。如未指定，则返回""，相当于`net`。 <br>在开平仓模式下仅可选择`long`或`short`。 如未指定，则返回""，对应于为交易开新仓位的方向（买入=>`long`，卖出=>`short`）。<br>仅适用交割、永续。   |
| >> tgtCcy	        | String| 	委托数量的类型 <br>定义`sz`属性的单位。仅适用于 instType=`SPOT`。有效值为`base_ccy`和`quote_ccy`。未指定时，默认为`base_ccy`。                                                                                                                                            |
| >> tradeQuoteCcy	        | String           | 	交易使用的计价币种。仅适用于 SPOT。<br>默认值为 instId 的报价币种，例如：对于 `BTC-USD`，默认值为 `USD`。                                                                                                                          |
