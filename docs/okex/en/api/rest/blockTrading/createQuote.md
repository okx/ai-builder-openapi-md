---
site: okex
lang: en
name: Create Quote
method: POST
path: /api/v5/rfq/create-quote
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Create Quote

Allows the user to Quote an RFQ that they are a counterparty to. The user MUST quote the entire RFQ and not part of the legs or part of the quantity. Partial quoting is not allowed.

Only one active quote is allowed per RFQ at a time. Submitting a new quote for the same `rfqId` will automatically cancel the existing active quote before the new one is created. Two-sided quoting (providing simultaneous bid and ask for the same RFQ) is not supported — only the most recently submitted quote remains active.

#### Rate Limit: 50 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP Request

`POST /api/v5/rfq/create-quote`

> Request Example

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

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# Create quote
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

#### Request parameters
| Parameter      | Type             | Required | Description                                              |
|:---------------|:-----------------| :-----  | :-----------------------------------------------------------  |
| rfqId          | String           |Yes|   RFQ ID .|
| clQuoteId      | String           |No |   Client-supplied Quote ID. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| tag            | String           |No |   Quote tag. <br>The block trade associated with the Quote will have the same tag. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters.                                            |
| anonymous	     | Boolean          |No |   Submit Quote on a disclosed or anonymous basis. <br>Valid value is `true` or `false`. `false` by default. |
| quoteSide	     | String	          |Yes|	The trading direction of the Quote. Its value can be `buy` or `sell`. <br>For example, if quoteSide is `buy`, all the legs are executed in their leg sides; otherwise, all the legs are executed in the opposite of their leg sides.|
| expiresIn	     | String	          |No |	Seconds that a quote expires in. <br>Must be an integer between 10-120. Default is 60.       |
| legs	          | Array of objects |Yes|   The legs of the Quote.|
| > instId	      | String	          |Yes|	The instrument ID of quoted leg.|
| > tdMode	      | String	          | No     | Trade mode <br>Margin mode: `cross` `isolated` <br>Non-Margin mode: `cash`. <br>If not provided, tdMode will inherit default values set by the system shown below: <br>Futures mode mode & SPOT: `cash` <br>Buy options in Futures mode and Multi-currency Margin: `isolated` <br>Other cases: `cross`                 |
| > ccy   	      | String	          | No     | Margin currency. <br>Only applicable to `cross` `MARGIN` orders in `Futures mode`. The parameter will be ignored in other scenarios.                                                                             |
| > sz	          | String	          |Yes|	Size of the leg in contracts or spot.|
| > px	          | String	          |Yes|	The price of the leg.|
| > side	        | String	          |Yes|	The direction of the leg. Valid values can be buy or sell.|
| > posSide	     | String	          |No |   Position side. <br>The default is `net` in the net mode. It can only be `long` or `short` in the long/short mode. <br>If not specified, users in long/short mode always open new positions. <br>Only applicable to `FUTURES`/`SWAP`.  |
| > tgtCcy	      | String	          |No	|   Defines the unit of the “sz” attribute. <br>Only applicable to instType = SPOT. <br>The valid enumerations are `base_ccy` and `quote_ccy`. When not specified this is equal to `base_ccy` by default.|
| > tradeQuoteCcy	|String	            |No| 	The quote currency used for trading. Only applicable to SPOT. <br>The default value is the quote currency of the instId, for example: for `BTC-USD`, the default is `USD`.                                                                                                                          |

> Response Example

```json
{
    "code":"",
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
                    "px":"4000",
                    "sz":"25",
                    "instId":"ETH-USD-220114-25000-C",
                    "tdMode":"cross",
                    "ccy":"USDT",
                    "side":"buy",
                    "posSide": "long",
                    "tgtCcy":""
                }
            ],
            "quoteId":"25092",
            "rfqId":"18753",
            "tag":"123456",
            "quoteSide":"sell",
            "state":"active",
            "reason": "mmp_canceled",
            "clQuoteId":"",
            "clRfqId":"",
            "traderCode":"Aksha"
        }
    ]
}
```

#### Response Parameters

| Parameter  | Type   | Description                                                                                                                                                                                                                                                                                                                                                          |
| :------ | :----- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| code	    |String	            | The result code, `0` means success.                                                                                                                                                                                                                                                                                                                                  |
| msg	    |String	            | The error message, not empty if the code is not 0.                                                                                                                                                                                                                                                                                                                   |
| data	    |Array of objects   | Array of objects containing the results                                                                                                                                                                                                                                                                                                                              |
|> cTime	|String	| The timestamp the Quote was created, Unix timestamp format in milliseconds.                                                                                                                                                                                                                                                                                          |
|> uTime	|String	| The timestamp the Quote was last updated, Unix timestamp format in milliseconds.                                                                                                                                                                                                                                                                                     |
|> state	|String	| The status of the quote. Valid values can be `active` `canceled` `pending_fill` `filled` `expired` or `failed`.                                                                                                                                                                                                                                                      |
|> reason	|String	| Reasons of state. Valid values can be `mmp_canceled`.                                                                                                                                                                                                                                                                                                                |
|> validUntil	|String	| The timestamp the Quote expires. Unix timestamp format in milliseconds.                                                                                                                                                                                                                                                                                              |
|> rfqId	|String	| RFQ ID                                                                                                                                                                                                                                                                                                                                                               |
|> clRfqId	|String	| Client-supplied RFQ ID. <br>This attribute is treated as client sensitive information. It will not be exposed to the Maker, only return empty string.                                                                                                                                                                                                                |
|> quoteId	|String	| Quote ID.                                                                                                                                                                                                                                                                                                                                                            |
|> clQuoteId	|String	| Client-supplied Quote ID. <br>This attribute is treated as client sensitive information. It will not be exposed to the Taker, only return empty string.                                                                                                                                                                                                              |
|> tag	    |String| Quote tag. <br>The block trade associated with the Quote will have the same tag.                                                                                                                                                                                                                                                                                     |
|> traderCode	|String	| A unique identifier of maker.                                                                                                                                                                                                                                                                                                                                        |
|> quoteSide	|String	| The trading direction of the Quote. <br>Its value can be `buy` or `sell`. For example, if quoteSide is `buy`, all the legs are executed in their leg sides; otherwise, all the legs are executed in the opposite of their leg sides.                                                                                                                                 |
|> legs	        |Array of objects| The legs of the Quote.                                                                                                                                                                                                                                                                                                                                               |
|>> instId	    |String	| Instrument ID, e.g. `BTC-USDT-SWAP`                                                                                                                                                                                                                                                                                                                                  |
|>> tdMode	|String	| Trade mode <br>Margin mode: `cross` `isolated` <br>Non-Margin mode: `cash`. <br>If not provided, tdMode will inherit default values set by the system shown below: <br>Futures mode & SPOT: `cash` <br>Buy options in Futures mode and Multi-currency Margin: `isolated` <br>Other cases: `cross`                                                |
|>> ccy   	|String	| Margin currency. <br>Only applicable to `cross` `MARGIN` orders in `Futures mode`. The parameter will be ignored in other scenarios.                                                                                                                                                                                                                       |
|>> sz	        |String	| Size of the leg in contracts or spot.                                                                                                                                                                                                                                                                                                                                |
|>> px	        |String	| The price of the leg.                                                                                                                                                                                                                                                                                                                                                |
|>> side	    |String	| The direction of the leg. Valid values can be buy or sell.                                                                                                                                                                                                                                                                                                           |
|>> posSide     |String | Position side. <br>The default is `net` in the net mode. If not specified, return "", which is equivalent to net. <br>It can only be `long` or `short` in the long/short mode. If not specified, return "", which corresponds to the direction that opens new positions for the trade (buy => long, sell => short). <br>Only applicable to FUTURES/SWAP.             |
|>> tgtCcy	    |String	| Defines the unit of the “sz” attribute. <br>Only applicable to instType = SPOT. <br>The valid enumerations are `base_ccy` and `quote_ccy`. When not specified this is equal to `base_ccy` by default.                                                                                                                                                                |
|>> tradeQuoteCcy	|String	| The quote currency used for trading. Only applicable to SPOT. <br>The default value is the quote currency of the instId, for example: for `BTC-USD`, the default is `USD`.                                                                                                                          |
