---
site: okex
lang: en
name: Get rfqs
method: GET
path: /api/v5/rfq/rfqs
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get rfqs

Retrieves details of RFQs that the user is a counterparty to (either as the creator or the receiver of the RFQ). 

#### Rate Limit: 2 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP Request

`GET /api/v5/rfq/rfqs`

> Request Example

```shell
GET /api/v5/rfq/rfqs
```

```python
import okx.BlockTrading as BlockTrading

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# Retrieves details of RFQs that the user is a counterparty to
result = blockTradingAPI.get_rfqs()
print(result)
```

#### Request parameters
| Parameter | Type | Required | Description                                              |
|:-----------| :----- | :----- | :-----------------------------------------------------------  |
| rfqId      |String |No |RFQ ID .|
| clRfqId    |String |No |Client-supplied RFQ ID. If both `clRfqId` and `rfqId` are passed, `rfqId` will be treated as primary identifier|
| state	     |String |No |The status of the RFQ. <br>Valid values can be `active` `canceled` `pending_fill` `filled` `expired` `failed` `traded_away`. <br>`filled` indicates the RFQ was successfully executed against the maker's quote. <br>`traded_away` only applies to Maker. The same RFQ can appear as `filled` to one maker and `traded_away` to another. <br>Example: taker creates RFQ → makerA quotes pxA, makerB quotes pxB → pxA is better than pxB → taker executes quoteA → makerA sees `filled`, makerB sees `traded_away`.                 |
| beginId    |String |No |Start rfq id the request to begin with. Pagination of data to return records newer than the requested rfqId, not including beginId                |
| endId	     |String |No |End rfq id the request to end with. Pagination of data to return records earlier than the requested rfqId, not including endId                   |
| limit	     |String |No |Number of results per request. The maximum is 100 which is also the default value.                                                                |

> Response Example

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

#### Response Parameters

| Parameter  | Type             | Description                                                                                                                                                                                                                                                                                                                                                    |
| :------ |:-----------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| code	            | String           | The result code, `0` means success.                                                                                                                                                                                                                                                                                                                            |
| msg	            | String           | The error message, not empty if the code is not 0.                                                                                                                                                                                                                                                                                                             |
| data	            | Array of objects | 	Array of objects containing the results of the RFQ creation.                                                                                                                                                                                                                                                                                                  |
| > cTime	        | String           | 	The timestamp the RFQ was created. Unix timestamp format in milliseconds.                                                                                                                                                                                                                                                                                     |
| > uTime	        | String           | 	The timestamp the RFQ was last updated. Unix timestamp format in milliseconds.                                                                                                                                                                                                                                                                                |
| > state	| String	          | The status of the RFQ. <br>Valid values can be `active` `canceled` `pending_fill` `filled` `expired` `failed` `traded_away`. <br>`filled` indicates the RFQ was successfully executed against the maker's quote. <br>`traded_away` only applies to Maker. The same RFQ can appear as `filled` to one maker and `traded_away` to another. <br>Example: taker creates RFQ → makerA quotes pxA, makerB quotes pxB → pxA is better than pxB → taker executes quoteA → makerA sees `filled`, makerB sees `traded_away`.                                                                                                                                                                                           |
| > counterparties	| Array of strings | 	The list of counterparties traderCode the RFQ was broadcasted to.                                                                                                                                                                                                                                                                                             |
| > validUntil	    | String           | 	The timestamp the RFQ expires. Unix timestamp format in milliseconds.                                                                                                                                                                                                                                                                                         |
| > clRfqId	        | String           | 	Client-supplied RFQ ID. <br>This attribute is treated as client sensitive information. It will not be exposed to the Maker, only return empty string.                                                                                                                                                                                                         |
| > tag	            | String           | RFQ tag. <br>The block trade associated with the RFQ will have the same tag.                                                                                                                                                                                                                                                                                   |
| > flowType | String           | Identify the type of the RFQ. <br>Only applicable to Makers, return "" for Takers                                                                                                                                                                                                                                                                              |
| > traderCode	    | String           | 	A unique identifier of taker. Empty if the anonymous parameter of the RFQ is set to be `true`.                                                                                                                                                                                                                                                                |
| > rfqId	        | String           | 	RFQ ID.                                                                                                                                                                                                                                                                                                                                                       |
| > allowPartialExecution	| Boolean          | 	Whether the RFQ can be partially filled provided that the shape of legs stays the same.  <br>Valid value is `true` or `false`. `false` by default.                                                                                                                                                                                                            |
| > legs	        | Array of objects | 	Legs of RFQ                                                                                                                                                                                                                                                                                                                                                   |
| >> instId	        | String           | 	Instrument ID, e.g. BTC-USDT-SWAP                                                                                                                                                                                                                                                                                                                             |
| >> tdMode	        | String           | Trade mode <br>Margin mode: `cross` `isolated` <br>Non-Margin mode: `cash`. <br>If not provided, tdMode will inherit default values set by the system shown below: <br>Futures mode & SPOT: `cash` <br>Buy options in Futures mode and Multi-currency Margin: `isolated` <br>Other cases: `cross`                                          |
| >> ccy   	        | String           | Margin currency. <br>Only applicable to `cross` `MARGIN` orders in `Futures mode`. The parameter will be ignored in other scenarios.                                                                                                                                                                                                                 |
| >> sz	            | String           | 	Size of the leg in contracts or spot.                                                                                                                                                                                                                                                                                                                         |
| >> side	        | String           | 	The direction of the leg. Valid values can be buy or sell.                                                                                                                                                                                                                                                                                                    |
| >> posSide        | String           | Position side. <br>The default is `net` in the net mode. If not specified, return "", which is equivalent to net. <br>It can only be `long` or `short` in the long/short mode. If not specified, return "", which corresponds to the direction that opens new positions for the trade (buy => long, sell => short). <br>Only applicable to `FUTURES`/`SWAP`.   |
| >> tgtCcy	        | String           | 	Defines the unit of the “sz” attribute. <br>Only applicable to instType = SPOT. <br>The valid enumerations are base_ccy and quote_ccy. When not specified this is equal to base_ccy by default.                                                                                                                                                               |
| >> tradeQuoteCcy	        | String           | 	The quote currency used for trading. Only applicable to SPOT. <br>The default value is the quote currency of the instId, for example: for `BTC-USD`, the default is `USD`.                                                                                                                          |
| > groupId | String | Group RFQ ID<br> Only applicable to group RFQ, return "" for normal RFQ |
| > acctAlloc | Array of objects | Account level allocation of the RFQ<br> This is only applicable to the taker. |
| >> acct | String | The name of the allocated account of the RFQ. |
| >> legs | Array of objects | The allocated legs of the account. |
| >>> instId | String | The Instrument ID of each leg. Example : "BTC-USDT-SWAP" |
| >>> sz | String | The allocated size of each leg. |
| >>> tdMode | String | Trade mode |
| >>> ccy | String | Margin currency |
| >>> posSide | String | Position side |

<aside class="notice">
Group RFQ introduction<br><br>

1. allowPartialExecution field is always true for group RFQ for taker and maker.<br>
2. Add a new response parameter acctAlloc with all account allocation the same as the initial request, but it is only applicable to takers.<br>
3. Add a new response parameter groupId, applicable to both takers and makers.<br>
4. For group RFQ state,<br>
&nbsp;&nbsp;&nbsp;&nbsp;1. if any allocated account is pending execution, then pending_fill<br>
&nbsp;&nbsp;&nbsp;&nbsp;2. otherwise,<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. if any allocated account is filled, then filled<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. if all allocated accounts are failed, then failed<br>
</aside>
