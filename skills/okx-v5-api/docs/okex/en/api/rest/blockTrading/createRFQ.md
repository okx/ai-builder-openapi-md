---
site: okex
lang: en
name: Create RFQ
method: POST
path: /api/v5/rfq/create-rfq
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Create RFQ

Creates a new RFQ

<aside class="notice">
<br>
Please select trading bot "WAGMI" as the counterparty when submitting RFQs in demo trading.<br>
Prices provided on RFQs by the trading bot are for reference only. <br>
</aside>
To learn more, please visit [Support center > FAQ > Trading > Liquid marketplace > Demo trading](/help/demo-trading)
#### Rate Limit: 5 requests per 2 seconds; 80 requests per 12 hours
#### Rate limit rule: User ID
#### HTTP Request

`POST /api/v5/rfq/create-rfq`

> Request Example

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

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# Create RFQ
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

#### Request parameters
| Parameter  | Type   | Required | Description                                                                                                                                                                                                                                                                                                                           |
| :------ | :----- |:--------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| counterparties |Array of strings	| Yes     | The trader code(s) of the counterparties who receive the RFQ. Can be found via /api/v5/rfq/counterparties/                                                                                                                                                                                                                            |
| anonymous	|Boolean            | No      | Submit RFQ on a disclosed or anonymous basis. Valid values are `true` or `false`. <br>If not specified, the default value is `false`. <br>When anonymous = true, the taker’s identify is not disclosed to maker even after trade execution.                                                                                           |
| clRfqId   |String             | No      | Client-supplied RFQ ID. <br>A combination of case-sensitive alpha-numeric, all numbers, or all letters of up to 32 characters.                                                                                                                                                                                                        |
| tag       |String             | No      | RFQ tag. <br>The block trade associated with the RFQ will have the same tag. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters.                                                                                                                                                   |
| allowPartialExecution   | Boolean   | No      | Whether the RFQ can be partially filled provided that the shape of legs stays the same. Valid values are `true` or `false`. <br>`false` by default.                                                                                                                                                                                   |
| legs	    |Array of objects   | Yes     | An Array of objects containing each leg of the RFQ. Maximum 15 legs can be placed per request                                                                                                                                                                                                                                         |
| > instId	|String	            | Yes     | The Instrument ID of each leg. Example : "BTC-USDT-SWAP"                                                                                                                                                                                                                                                                              |
| > tdMode	|String	            | No     | Trade mode <br>Margin mode: `cross` `isolated` <br>Non-Margin mode: `cash`. <br>If not provided, tdMode will inherit default values set by the system shown below: <br>Futures mode & SPOT: `cash` <br>Buy options in Futures mode and Multi-currency Margin: `isolated` <br>Other cases: `cross`                 |
| > ccy   	|String	            | No     | Margin currency. <br>Only applicable to `cross` `MARGIN` orders in `Futures mode`. The parameter will be ignored in other scenarios.                                                                                                                                                                                        |
| > sz	    |String	            | Yes     | The size of each leg                                                                                                                                                                                                                                                                                                                  |
| > lmtPx | String | No |  Taker expected price for the RFQ<br><br> If provided, RFQ trade will be automatically executed if the price from the quote is better than or equal to the price specified until the RFQ is canceled or expired.<br> This field has to be provided for all legs to have the RFQ automatically executed, or leave empty for all legs, otherwise request will be rejected.<br> The auto execution side depends on the leg side of the RFQ.<br> For `SPOT/MARGIN/FUTURES/SWAP`, lmtPx will be in unit of the quote ccy.<br> For `OPTION`, lmtPx will be in unit of settle ccy.<br> The field will not be disclosed to counterparties.|
| > side	|String	            | Yes     | The direction of each leg. Valid values can be `buy` or `sell`.                                                                                                                                                                                                                                                                       |
| > posSide	|String	            | No      | Position side. <br>The default is `net` in the net mode. It can only be `long` or `short` in the long/short mode. <br>If not specified, users in long/short mode always open new positions. <br>Only applicable to `FUTURES`/`SWAP`.                                                                                                  |
| > tgtCcy	|String	            | No      | Defines the unit of the “sz” attribute. <br>Only applicable to instType = SPOT. <br>The valid enumerations are `base_ccy` and `quote_ccy`. When not specified, this is equal to `base_ccy` by default.                                                                                                                                |
| > tradeQuoteCcy	|String	            | No      | The quote currency used for trading. Only applicable to SPOT. <br>The default value is the quote currency of the instId, for example: for `BTC-USD`, the default is `USD`.                                                                                                                          |
| acctAlloc | Array of objects | No | Account level allocation of the RFQ |
| > acct | String | Yes | The name of the allocated account of the RFQ. |
| > legs | Array of objects | Yes | The allocated legs of the account. |
| >> sz | String | Yes | The allocated size of each leg |
| >> instId | String | Yes | The Instrument ID of each leg. Example : "BTC-USDT-SWAP" |
| >> tdMode | String | No | Trade mode |
| >> ccy | String | No | Margin currency |
| >> posSide | String | No | Position side |

> Response Example

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
                    "tdMode":"cross",
                    "ccy":"USDT",
                    "sz":"25",
                    "side":"buy",
                    "posSide": "long",
                    "tgtCcy":""
                },
                {
                    "instId":"ETH-USDT",
                    "tdMode":"cross",
                    "ccy":"USDT",
                    "sz":"150",
                    "side":"buy",
                    "posSide": "long",
                    "tgtCcy":"base_ccy",
                    "tradeQuoteCcy": "USDT"
                }
            ]
        }
    ]
}
```

#### Response Parameters

| Parameter  | Type   | Description                                                                                                                                                                                                                                                                                                                                              |
| :------ | :----- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| code	    |String  | The result code, `0` means success.                                                                                                                                                                                                                                                                                                                      |
| msg	    |String  | The error message, not empty if the code is not 0.                                                                                                                                                                                                                                                                                                       |
| data	    |Array of objects| 	Array of objects containing the results of the RFQ creation.                                                                                                                                                                                                                                                                                            |
| > cTime	|String	| The timestamp the RFQ was created. Unix timestamp format in milliseconds.                                                                                                                                                                                                                                                                                |
| > uTime	|String	| The timestamp the RFQ was last updated. Unix timestamp format in milliseconds.                                                                                                                                                                                                                                                                           |
| > state	|String	| The status of the RFQ. <br>Valid values can be `active` `canceled` `pending_fill` `filled` `expired` `traded_away` `failed`. <br>`filled` indicates the RFQ was successfully executed against the maker's quote. <br>`traded_away` only applies to Maker. The same RFQ can appear as `filled` to one maker and `traded_away` to another. <br>Example: taker creates RFQ → makerA quotes pxA, makerB quotes pxB → pxA is better than pxB → taker executes quoteA → makerA sees `filled`, makerB sees `traded_away`.                                                                                                                                                                                     |
| > counterparties	|Array of strings| 	The list of counterparties traderCode the RFQ was broadcast to.                                                                                                                                                                                                                                                                                         |
| > validUntil	|String	| The timestamp the RFQ expires. Unix timestamp format in milliseconds. <br>If all legs are options, the RFQ will expire after 10 minutes; otherwise, the RFQ will expire after 2 minutes.                                                                                                                                                                 |
| > clRfqId	|String| Client-supplied RFQ ID. This attribute is treated as client sensitive information. It will not be exposed to the Maker, only return empty string.                                                                                                                                                                                                        |
| > tag	    |String| RFQ tag. The block trade associated with the RFQ will have the same tag.                                                                                                                                                                                                                                                                                 |
| > allowPartialExecution   | Boolean| Whether the RFQ can be partially filled provided that the shape of legs stays the same.                                                                                                                                                                                                                                                                  |
| > traderCode	|String| A unique identifier of taker.                                                                                                                                                                                                                                                                                                                            |
| > rfqId	|String	| The unique identifier of the RFQ generated by system.                                                                                                                                                                                                                                                                                                    |
| > legs	|Array of objects| 	An Array of objects containing each leg of the RFQ.                                                                                                                                                                                                                                                                                                     |
| >> instId	|String| Instrument ID, e.g. BTC-USDT-SWAP                                                                                                                                                                                                                                                                                                                        |
| >> tdMode	|String| Trade mode <br>Margin mode: `cross` `isolated` <br>Non-Margin mode: `cash`. <br>If not provided, tdMode will inherit default values set by the system shown below: <br>Futures mode & SPOT: `cash` <br>Buy options in Futures mode and Multi-currency Margin: `isolated` <br>Other cases: `cross`                                    |
| >> ccy   	|String| Margin currency. <br>Only applicable to `cross` `MARGIN` orders in `Futures mode`. The parameter will be ignored in other scenarios.                                                                                                                                                                                                           |
| >> sz	    |String| Size of the leg in contracts or spot.                                                                                                                                                                                                                                                                                                                    |
| >> side	|String| The direction of the leg. Valid values can be buy or sell.                                                                                                                                                                                                                                                                                               |
| >> posSide|String| Position side. <br>The default is `net` in the net mode. If not specified, return "", which is equivalent to net. <br>It can only be `long` or `short` in the long/short mode. If not specified, return "", which corresponds to the direction that opens new positions for the trade (buy => long, sell => short). <br>Only applicable to FUTURES/SWAP. |
| >> tgtCcy	|String| Defines the unit of the “sz” attribute. <br>Only applicable to instType = SPOT. <br>The valid enumerations are `base_ccy` and `quote_ccy`. When not specified this is equal to `base_ccy` by default.                                                                                                                                                    |
| >> tradeQuoteCcy	|String	| The quote currency used for trading. Only applicable to SPOT. <br>The default value is the quote currency of the instId, for example: for `BTC-USD`, the default is `USD`.                                                                                                                          |
| > groupId | String | Group RFQ ID<br>Only applicable to group RFQ, return "" for normal RFQ |
| > acctAlloc | Array of objects | Account level allocation of the RFQ |
| >> acct | String | The name of the allocated account of the RFQ |
| >> sCode | String | The code of the event execution result, 0 means success |
| >> sMsg | String | Rejection message if the request is unsuccessful |
| >> legs | Array of objects | The allocated legs of the account |
| >>> instId | String | Instrument ID |
| >>> sz | String | The calculated size of each leg of allocated account |
| >>> tdMode | String | Trade mode |
| >>> ccy | String | Margin currency |
| >>> posSide | String | Position side |

<aside class="notice">
Group RFQ introduction<br><br>

1. Only a master account can conduct group RFQ and the available scope of allocated subaccounts is its normal and managed subaccounts.<br>
2. Users will pass in acctAlloc request parameter to indicate the details of group RFQ account allocation, account name, instrument ID, allocated size, etc. master account is also allowed and should be indicated as "0". For tdMode, ccy and posSide fields, they will inherit the system default value if you leave them empty.<br>
3. Add groupId, acctAlloc as a new response parameter.<br>
4. The upper limit of the number of allocated subaccounts is 10. You will receive error code 70516 if you exceed the upper limit.<br>
5. For each symbol, the total size of RFQ legs in all accounts should be equal to its combined amount in the group RFQ. If not, you will receive error code 70514.<br>
6. For each sub-account, the ratio of a leg's size to the group RFQ must be the same across all symbols. If not, you will receive error code 70515. Here is an example:<br>
&nbsp;&nbsp;&nbsp;&nbsp;1. Parent RFQ legs<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. Symbol: BTC-USDT, size: 50, symbol: ETH-USDT, size: 100<br>
&nbsp;&nbsp;&nbsp;&nbsp;2. Child RFQ legs, happy case<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. Acct1: symbol: BTC-USDT, size: 30, symbol: ETH-USDT, size: 60 (ratio: 0.6)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. Acct2: symbol: BTC-USDT, size: 20, symbol: ETH-USDT, size: 40 (ratio: 0.4)<br>
&nbsp;&nbsp;&nbsp;&nbsp;3. Child RFQ legs, bad case<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. Acct1: symbol: BTC-USDT, size: 30, symbol: ETH-USDT, size: 50<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. Acct2: symbol: BTC-USDT, size: 20, symbol: ETH-USDT, size: 50<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. The total size is equal. But the ratio is not equal for different legs per subaccount.<br>
7. For allowPartialExecution field, it will be ignored even though users pass it in. For a group RFQ, allowPartialExecution will always be true, since taker can not determine whether the RFQ can be partially or fully filled if any subaccount fails. Thus, makers should regard it as a RFQ that can be partially filled.<br>
8. Group RFQ will not be created if any subaccount fails.<br>

</aside>
