---
site: okex
lang: zh
name: 报价
method: POST
path: /api/v5/rfq/create-quote
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 报价 [block-trading-rest-api-create-quote]

允许询价单指定的报价方进行报价，需要对整个询价单报价，不允许部分报价。

同一询价单（`rfqId`）下同一时间只能有一个有效报价单。针对同一 `rfqId` 提交新的报价单，会自动取消当前已有的有效报价单。不支持双边报价（即同时持有买卖两个方向的有效报价），仅最新提交的报价单保持有效。

#### 限速: 50次/2s
#### 限速规则：User ID
#### HTTP Requests

`POST /api/v5/rfq/create-quote`

> 请求示例

```shell
POST /api/v5/rfq/create-quote
{
    "rfqId":"22539",
    "clQuoteId":"q001",
    "tag":"123456",
    "quoteSide":"buy",
    "anonymous": true,
    "expiresIn":"30",
    "legs":[
        {
            "px":"39450.0",
            "sz":"200000",
            "instId":"BTC-USDT-SWAP",
            "tdMode":"cross",
            "ccy":"USDT",
            "side":"buy",
            "posSide": "long"
        },
        {
            "px":"39450.0",
            "sz":"200000",
            "instId":"BTC-USDT-SWAP",
            "tdMode":"cross",
            "ccy":"USDT",
            "side":"buy",
            "posSide": "long"
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

# 报价
result = blockTradingAPI.create_quote(
    rfqId="22539",
    clQuoteId="q001",
    anonymous=True,
    quoteSide="buy",
    expiresIn="30",
    legs=[
        {
            "px": "39450.0",
            "sz": "200000",
            "instId": "BTC-USDT-SWAP",
            "side": "buy"
        }
    ]
)
print(result)
```

#### 请求参数
| 参数名  | 类型  | 是否必须    | 描述                                                                            |
| :------ | :----- | :----- |:------------------------------------------------------------------------------|
| rfqId     |String             |是| 询价单ID                                                                         |
| clQuoteId |String             |否| 报价单自定义ID                                                                      | 
| tag       |String             |否| 报价单标签，与此报价单关联的大宗交易将有相同的标签。 <br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间。                                            |
| anonymous	|Boolean            |否| 是否匿名报价，`true`表示匿名报价，`false`表示公开报价，默认值为`false`，为`true`时，即使在交易执行之后，身份也不会透露给询价方。 |
| quoteSide	|String	            |是| 报价单方向，`buy`或者`sell`。当报价单方向为`buy`，对maker来说，执行方向与legs里的方向相同，对taker来说相反。反之同理                                                      |
| expiresIn	|String	            |否| 报价单的有效时长（以秒为单位）。 10到120之间的任何整数。 默认值为60                  |
| legs	    |Array of objects   |是| 组合交易                                                                         |
| > instId	|String	            |是| 产品ID                                                                          |
| > tdMode	|String	            | 否 | 交易模式 <br>保证金模式：`cross`全仓 `isolated`逐仓 <br>非保证金模式：`cash`非保证金. <br>如未提供，tdMode 将继承系统设置的默认值： <br>合约模式 & 现货: `cash` <br>合约模式和跨币种保证金模式下买入期权： `isolated` <br>其他情况: `cross`                 |
| > ccy   	|String	            | 否 | 保证金币种，仅适用于`合约模式`下的`全仓杠杆`订单 <br>在其他情况下该参数将被忽略。                                                                             |
| > sz	    |String	            |是| 委托数量                                                                         |
| > px	    |String	            |是| 委托价格                                                                         |
| > side	|String	            |是| 报价单方向                                                                        |
| > posSide	|String	            |否| 持仓方向 <br>买卖模式下默认为`net`。在开平仓模式下仅可选择`long`或`short`。 <br>如未指定，则处于开平仓模式下的用户始终会开新仓位。 <br>仅适用交割、永续。  |
| > tgtCcy	|String	            |否| 委托数量的类型 <br>定义`sz`属性的单位。仅适用于 instType=`SPOT`。有效值为`base_ccy`和`quote_ccy`。未指定时，默认为`base_ccy`。                                                              |
| > tradeQuoteCcy	|String	            |否| 交易使用的计价币种。仅适用于 SPOT。<br>默认值为 instId 的报价币种，例如：对于 `BTC-USD`，默认值为 `USD`。                                                                                                                          |

> 返回示例

```json
{
    "code": "",
    "msg": "",
    "data": [
        {
            "validUntil": "1608997227834",
            "uTime": "1608267227834",
            "cTime": "1608267227834",
            "legs": [
                {
                    "px": "46000",
                    "sz": "25",
                    "instId": "BTC-USD-220114-25000-C",
                    "tdMode": "cross",
                    "ccy": "USDT",
                    "side": "sell",
                    "posSide": "long",
                    "tgtCcy": ""
                },
                {
                    "px": "4000",
                    "sz": "25",
                    "instId": "ETH-USD-220114-25000-C",
                    "tdMode": "cross",
                    "ccy": "USDT",
                    "side": "buy",
                    "posSide": "long",
                    "tgtCcy": ""
                }
            ],
            "quoteId": "25092",
            "rfqId": "18753",
            "tag": "123456",
            "quoteSide": "sell",
            "state": "active",
            "reason": "mmp_canceled",
            "clQuoteId": "",
            "clRfqId": "",
            "traderCode": "Aksha"
        }
    ]
}
```

#### 返回参数

| 参数名  | 类型   | 描述                                                                                                                                                                              |
| :------ | :----- |:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| code	    |String	            | 结果代码，0表示成功。                                                                                                                                                                     |
| msg	    |String	            | 错误信息，如果代码不为0，则不为空。                                                                                                                                                              |
| data	    |Array of objects   | 包含结果的对象数组                                                                                                                                                                       |
| > cTime	|String	| 报价单创建时间，Unix时间戳的毫秒数格式。                                                                                                                                                          |
| > uTime	|String	| 报价单状态更新时间，Unix时间戳的毫秒数格式。                                                                                                                                                        |
| > state	|String	| 报价单的状态<br>有效值为 `active` `canceled` `pending_fill` `filled` `expired` `failed`                                                                                                   |
| > reason	    |String	| 状态原因. 有效值包括 `mmp_canceled`.                                                                                                                                                     |
| > validUntil	|String	| 报价单的过期时间，Unix时间戳的毫秒数格式。                                                                                                                                                         |
| > rfqId	    |String	| 询价单ID                                                                                                                                                                           |
| > clRfqId	    |String	| 询价单自定义ID，为客户敏感信息，不会公开，对报价方返回""。                                                                                                                                                 |
| > quoteId	    |String	| 报价单ID                                                                                                                                                                           |
| > clQuoteId	|String	| 报价单自定义ID，为客户敏感信息，不会公开，对询价方返回""。                                                                                                                                                 |
| > tag         |String | 报价单标签，与此报价单关联的大宗交易将有相同的标签。                                                                                                                                                      |
| > traderCode	|String	| 报价方唯一标识代码。                                                                                                                                                                      |
| > quoteSide	|String	| 报价单方向，有效值为`buy`或者`sell`。当报价单方向为`buy`，对maker来说，执行方向与legs里的方向相同，对taker来说相反。反之同理。                                                                                                  |
| > legs	    |Array of objects| 	组合交易                                                                                                                                                                           |
| >> instId	    |String	| 产品ID                                                                                                                                                                            |
| >> tdMode	    |String	| 交易模式 <br>保证金模式：`cross`全仓 `isolated`逐仓 <br>非保证金模式：`cash`非保证金. <br>如未提供，tdMode 将继承系统设置的默认值： <br>`合约模式`/`现货模式`: `cash` <br>`合约模式`/`跨币种保证金模式`下买入期权： `isolated` <br>其他情况: `cross` |
| >> ccy   	    |String	| 保证金币种，仅适用于`合约模式`下的`全仓杠杆`订单 <br>在其他情况下该参数将被忽略。                                                                                                                                |
| >> sz	        |String	| 委托数量                                                                                                                                                                            |
| >> px	        |String	| 委托价格                                                                                                                                                                            |
| >> side	    |String	| 腿的方向，有效值为`buy`或者`sell`。                                                                                                                                                         |
| >> posSide	|String | 持仓方向 <br>买卖模式下默认为`net`。如未指定，则返回""，相当于`net`。 <br>在开平仓模式下仅可选择`long`或`short`。 如未指定，则返回""，对应于为交易开新仓位的方向（买入=>`long`，卖出=>`short`）。<br>仅适用交割、永续。                                       |
| >> tgtCcy	    |String	| 委托数量的类型 <br>定义`sz`属性的单位。仅适用于 instType=`SPOT`。有效值为`base_ccy`和`quote_ccy`。未指定时，默认为`base_ccy`。                                                                                     |
| >> tradeQuoteCcy	|String	| 交易使用的计价币种。仅适用于 SPOT。<br>默认值为 instId 的报价币种，例如：对于 `BTC-USD`，默认值为 `USD`。                                                                                                                          |
