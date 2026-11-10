---
site: okex
lang: en
name: Execute Quote
method: POST
path: /api/v5/rfq/execute-quote
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Execute Quote

Executes a Quote. It is only used by the creator of the RFQ

#### Rate Limit: 2 requests per 3 seconds
#### Rate limit rule: User ID
#### HTTP Request

`POST /api/v5/rfq/execute-quote`

> Request Example

```shell
POST /api/v5/rfq/execute-quote
{
    "rfqId":"22540",
    "quoteId":"84073",
    "legs":[
        {
            "sz":"25",
            "instId":"BTC-USD-20220114-13250-C"
        },
        {
            "sz":"25",
            "instId":"BTC-USDT"
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

# Execute quote
result = blockTradingAPI.execute_quote(
    rfqId="22540",
    quoteId="84073"
)
print(result)

```

#### Request parameters
| Parameter  | Type   | Required | Description                                              |
| :------     | :-----  | :----- | :-----------------------------------------------------------  |
|rfqId      |String     |Yes|   RFQ ID .                                             |
|quoteId	|String	    |Yes|   Quote ID.                                            |
|legs	|Array of objects	    |No|   An Array of objects containing the execution size of each leg of the RFQ. <br>The ratio of the leg sizes needs to be the same as the RFQ. <br>*Note: `tgtCcy` and `side` of each leg will be same as ones in the RFQ. `px` will be the same as the ones in the Quote.                                            |
|> instId	|String	    |Yes|   The Instrument ID, for example: "BTC-USDT-SWAP".                                            |
|> sz	|String	    |Yes|   The size of each leg                                            |

> Response Example

```json
{  
   "code":"0",
   "msg":"",
   "data":[
       {
            "blockTdId":"180184",
            "rfqId":"1419",
            "clRfqId":"r0001",
            "quoteId":"1046",
            "clQuoteId":"q0001",
            "tag":"123456",
            "tTraderCode":"Trader1",
            "mTraderCode":"Trader2",
            "cTime":"1649670009",
            "legs":[
                {
                    "px":"0.1",
                    "sz":"25",
                    "instId":"BTC-USD-20220114-13250-C",
                    "side":"sell",
                    "fee":"-1.001",
                    "feeCcy":"BTC",
                    "tradeId":"10211"
                },
                {
                    "px":"0.2",
                    "sz":"25",
                    "instId":"BTC-USDT",
                    "side":"buy",
                    "fee":"-1.001",
                    "feeCcy":"BTC",
                    "tradeId":"10212"
                }
            ]
        }
   ]
}
```

#### Response Parameters

| Parameter  | Type   | Description                                                                                                                                                    |
| :------ | :----- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| code	    |String| 	 The result code, `0` means success.                                                                                                                                 |
| msg	    |String| 	 The error message, not empty if the code is not 0.                                                                                                              |
| data	    |Array of objects| Array of objects containing the results                                                                                                                   |
| > cTime	|String| 	The execution time for the trade. Unix timestamp in milliseconds.                                                                                                 |
| > rfqId	|String| 	RFQ ID.                                                                                                                                                          |
| > clRfqId	|String| 	Client-supplied RFQ ID. This attribute is treated as client sensitive information. It will not be exposed to the Maker, only return empty string.   |
| > quoteId	|String| 	Quote ID.                                                                                                                                                        |
| > clQuoteId	|String| 	Client-supplied Quote ID. This attribute is treated as client sensitive information. It will not be exposed to the Taker, only return empty string.|
| > blockTdId	|String| 	Block trade ID.                                                        |
| > tag	        |String|    RFQ tag.                             |
| > tTraderCode	|String| 	A unique identifier of the taker. Empty if the anonymous parameter of the RFQ is set to be `true`.  |
| > mTraderCode	|String| 	A unique identifier of the maker. Empty if the anonymous parameter of the Quote is set to be `true`. |
| > legs	|Array of objects| 	Legs of trade                                                                                                                                                |
| >> instId	|String| 	Instrument ID, e.g. BTC-USDT-SWAP                                                                                                                                   |
| >> px	|String| 	The price the leg executed                                                                                                                                               |
| >> sz	|String| 	Size of the leg in contracts or spot.                                                                                                                                                         |
| >> side	|String| 	The direction of the leg from the Takers perspective. Valid value can be buy or sell.                                                                                |
| >> fee	|String| 	Fee for the individual leg. <br>Negative fee represents the user transaction fee charged by the platform. Positive fee represents rebate.                                |
| >> feeCcy	|String| 	Fee currency. To be read in conjunction with fee                                                                                                                     |
| >> tradeId	|String| 	Last traded ID.                                                                                                                                                  |
| > acctAlloc | Array of objects | Account level allocation of the RFQ |
| >> acct | String | The name of the allocated account of the RFQ. |
| >> blockTdId | String | Block trade ID |
| >> sCode | String | The code of the event execution result, 0 means success |
| >> sMsg | String | Rejection message if the request is unsuccessful |
| >> legs | Array of objects | The allocated legs of the account. |
| >>> instId | String | The Instrument ID of each leg. Example : "BTC-USDT-SWAP" |
| >>> sz | String | The size of each account leg is filled. |
| >>> fee | String | The fee of each account level leg |
| >>> feeCcy | String | Fee currency. To be read in conjunction with fee |
| >>> tradeId | String | Last traded ID of each account leg |

<aside class="notice">
Group RFQ introduction<br><br>

1. Takers are not allowed to partially execuate the quote for group RFQ. You will receive error code 70507 if you don't pass in the full leg size.<br>
2. Parent RFQ leg size will be the summation of the filled size of each child RFQ leg size while fee should also be the summation.<br>
3. The blockTdId of parent RFQ and the tradeId of parent RFQ legs will be emoty. But there will be subaccount breakdown attached with blockTdId and tradeId populated.<br>
</aside>
