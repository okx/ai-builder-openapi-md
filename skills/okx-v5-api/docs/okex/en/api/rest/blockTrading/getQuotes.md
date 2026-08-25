---
site: okex
lang: en
name: Get quotes
method: GET
path: /api/v5/rfq/quotes
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get quotes

Retrieve all Quotes that the user is a counterparty to (either as the creator or the receiver).

#### Rate Limit: 2 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP Request

`GET /api/v5/rfq/quotes`

> Request Example

```shell
GET /api/v5/rfq/quotes
```

```python
import okx.BlockTrading as BlockTrading

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# Retrieve all Quotes that the user is a counterparty to
result = blockTradingAPI.get_quotes()
print(result)
```

#### Request parameters
| Parameter | Type | Required | Description                                              |
|:-----------| :----- | :----- | :-----------------------------------------------------------  |
| rfqId      |String |No |RFQ ID .|
| clRfqId    |String |No |Client-supplied RFQ ID. If both `clRfqId` and `rfqId` are passed, `rfqId` will be be treated as primary identifier.|
| quoteId    |String |No |Quote ID                                                                                                                                          |
| clQuoteId  |String |No |Client-supplied Quote ID. If both clQuoteId and quoteId are passed, quoteId will be treated as primary identifier                                 |
| state	     |String |No |The status of the quote. Valid values can be `active` `canceled` `pending_fill` `filled` `expired` or `failed`.                                                                         |
| beginId    |String |No |Start quote id the request to begin with. Pagination of data to return records newer than the requested quoteId, not including beginId                |
| endId	     |String |No |End quote id the request to end with. Pagination of data to return records earlier than the requested quoteId, not including endId                   |
| limit	     |String |No |Number of results per request. The maximum is 100 which is also the default value.                                                                |

> Response Example

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
                    "tgtCcy":""
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

#### Response Parameters

| Parameter    | Type   | Description                                                                                                                                                                                                                                                                                                                                                  |
|:--------------| :----- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| code	         | String| The result code, `0` means success.                                                                                                                                                                                                                                                                                                                          |
| msg	          | String| The error message, not empty if the code is not 0.                                                                                                                                                                                                                                                                                                           |
| data	         | Array of objects| 	Array of objects containing the results of the Quote creation.                                                                                                                                                                                                                                                                                              |
| > cTime	      | String| 	The timestamp the Quote was created, Unix timestamp format in milliseconds.                                                                                                                                                                                                                                                                                 |
| > uTime	      | String| 	The timestamp the Quote was last updated, Unix timestamp format in milliseconds.                                                                                                                                                                                                                                                                            |
| > state	      |String	| The status of the quote. Valid values can be `active` `canceled` `pending_fill` `filled` `expired` or `failed`.                                                                                                                                                                                                                                              |
| > reason	      |String	| Reasons of state. Valid values can be `mmp_canceled`.                                                                                                                                                                                                                                                                                                        |
| > validUntil	 | String| 	The timestamp the Quote expires. Unix timestamp format in milliseconds.                                                                                                                                                                                                                                                                                     |
| > rfqId	      | String| 	RFQ ID.                                                                                                                                                                                                                                                                                                                                                     |
| > clRfqId	    | String| 	Client-supplied RFQ ID. This attribute is treated as client sensitive information. It will not be exposed to the Maker, only return empty string.                                                                                                                                                                                                           |
| > quoteId	    | String| 	Quote ID.                                                                                                                                                                                                                                                                                                                                                   |
| > clQuoteId	  | String| 	Client-supplied Quote ID. This attribute is treated as client sensitive information. It will not be exposed to the Taker, only return empty string.                                                                                                                                                                                                         |
| > tag	    |String| Quote tag. The block trade associated with the Quote will have the same tag.                                                                                                                                                                                                                                                                                 |
| > traderCode	 | String| 	A unique identifier of maker. Empty If the anonymous parameter of the Quote is set to be `true`.                                                                                                                                                                                                                                                            |
| > quoteSide	  | String| 	Top level direction of Quote. Its value can be buy or sell.                                                                                                                                                                                                                                                                                                 |
| > legs	       | Array of objects| 	The legs of the Quote.                                                                                                                                                                                                                                                                                                                                      |
| >> instId	    | String| 	The instrument ID of the quoted leg.                                                                                                                                                                                                                                                                                                                        |
| >> tdMode	    |String | Trade mode <br>Margin mode: `cross` `isolated` <br>Non-Margin mode: `cash`. <br>If not provided, tdMode will inherit default values set by the system shown below: <br>Futures mode & SPOT: `cash` <br>Buy options in Futures mode and Multi-currency Margin: `isolated` <br>Other cases: `cross`                                        |
| >> ccy   	    |String | Margin currency. <br>Only applicable to `cross` `MARGIN` orders in `Futures mode`. The parameter will be ignored in other scenarios.                                                                                                                                                                                                               |
| >> sz	        | String| 	Size of the leg in contracts or spot.                                                                                                                                                                                                                                                                                                                       |
| >> px	        | String| 	The price of the leg.                                                                                                                                                                                                                                                                                                                                       |
| >> side	      | String| 	The direction of the leg. Valid values can be buy or sell.                                                                                                                                                                                                                                                                                                  |
| >> posSide    | String| Position side. <br>The default is `net` in the net mode. If not specified, return "", which is equivalent to net. <br>It can only be `long` or `short` in the long/short mode. If not specified, return "", which corresponds to the direction that opens new positions for the trade (buy => long, sell => short). <br>Only applicable to `FUTURES`/`SWAP`. |
| >> tgtCcy	    | String| 	Defines the unit of the “sz” attribute. <br>Only applicable to instType = SPOT. <br>The valid enumerations are base_ccy and quote_ccy. When not specified this is equal to base_ccy by default.                                                                                                                                                             |
| >> tradeQuoteCcy	        | String           | 	The quote currency used for trading. Only applicable to SPOT. <br>The default value is the quote currency of the instId, for example: for `BTC-USD`, the default is `USD`.                                                                                                                          |
