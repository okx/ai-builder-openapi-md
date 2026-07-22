---
site: okex
lang: en
name: Move positions
method: POST
path: /api/v5/account/move-positions
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Move positions

Only applicable to users with a trading level greater than or equal to VIP6, and can only be called through the API Key of the master account. Users can check their trading level through the fee details table on the [My trading fees](https://www.okx.com/balance/fee) page.<br>

To move positions between different accounts under the same master account. Each source account can trigger up to fifteen move position requests every 24 hours. There is no limitation to the destination account to receive positions. Refer to the "Things to note" part for more details.

#### Rate limit: 1 request per second

#### Rate limit rule: Master account User ID

#### HTTP Request

`POST /api/v5/account/move-positions`

> Request example

```shell
{
   "fromAcct":"0",
   "toAcct":"test",
   "legs":[
      {
         "from":{
            "posId":"2065471111340792832",
            "side":"sell",
            "sz":"1"
         },
         "to":{
            "posSide":"net",
            "tdMode":"cross"
         }
      },
      {
         "from":{
            "posId":"2063111180412153856",
            "side":"sell",
            "sz":"1"
         },
         "to":{
            "posSide":"net",
            "tdMode":"cross"
         }
      }
   ],
   "clientId":"test"
}
```

#### Request parameters

| **Parameter** | **Type** | **Required** | **Description**                                                                                                                                                                                                                                                                                                                                                        |
| --- | --- | --- |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| fromAcct | String | Yes | Source account name. If it's a master account, it should be "0"                                                                                                                                                                                                                                                                                                        |
| toAcct | String | Yes | Destination account name. If it's a master account, it should be "0"                                                                                                                                                                                                                                                                                                   |
| legs | Array of Objects | Yes | An array of objects containing details of each position to be moved                                                                                                                                                                                                                                                                                                    |
| > from | Object | yes | Details of the position in the source account                                                                                                                                                                                                                                                                                                                          |
| >> posId | String | Yes | Position ID in the source account                                                                                                                                                                                                                                                                                                                                      |
| >> sz | String | Yes | Number of contracts.                                                                                                                                                                                                                                                                                                                                                   |
| >> side | String | Yes | Trade side from the perspective of source account<br> `buy`<br> `sell`                                                                                                                                                                                                                                                                                                 |
| > to | Object | Yes | Details of the configuration of the destination account                                                                                                                                                                                                                                                                                                                |
| >> tdMode | String | No | Trading mode in the destination account.<br> `cross`<br> `isolated`<br> If not provided, tdMode will take the default values as shown below:<br>Buy options in `Futures mode`/`Multi-currency margin mode`: `isolated`<br> Other cases: `cross`                                                                                                                        |
| >> posSide | String | No | Position side<br> `net`<br> `long`<br> `short`<br> This parameter is not mandatory if the destination sub-account is in **net** mode. If you pass it through, the only valid value is `net`.It can only be `long` or `short` if the destination sub-account is in long/short mode. If not specified, destination account in long/short mode always open new positions. |
| >> ccy | String | No | Margin currency in destination accountOnly applicable to cross margin positions in `Futures mode`.                                                                                                                                                                                                                                                           |
| clientId | String | Yes | Client-supplied ID. A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                                                                                                                                                                                                                 |

> Response example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "clientId": "test",
            "blockTdId": "2065832911119076864",
            "state": "filled",
            "ts": "1734069018526",
            "fromAcct": "0",
            "toAcct": "test",
            "legs": [
                {
                    "from": {
                        "posId": "2065471111340792832",
                        "instId": "BTC-USD-SWAP",
                        "px": "100042.7",
                        "side": "sell",
                        "sz": "1",
                        "sCode": "0",
                        "sMsg": ""
                    },
                    "to": {
                        "instId": "BTC-USD-SWAP",
                        "px": "100042.7",
                        "side": "buy",
                        "sz": "1",
                        "tdMode": "cross",
                        "posSide": "net",
                        "ccy": "",
                        "sCode": "0",
                        "sMsg": ""
                    }
                },
                {
                    "from": {
                        "posId": "2063111180412153856",
                        "instId": "BTC-USDT-SWAP",
                        "px": "100008.1",
                        "side": "sell",
                        "sz": "1",
                        "sCode": "0",
                        "sMsg": ""
                    },
                    "to": {
                        "instId": "BTC-USDT-SWAP",
                        "px": "100008.1",
                        "side": "buy",
                        "sz": "1",
                        "tdMode": "cross",
                        "posSide": "net",
                        "ccy": "",
                        "sCode": "0",
                        "sMsg": ""
                    }
                }
            ]
        }
    ]
}

```

> Response example:failure

```json
// The destination account position mode (net/longShort) is not matched with the posSide field
{
    "code": "51000",
    "msg": "Incorrect type of posSide (leg with Instrument Id [BTC-USD-SWAP])",
    "data": []
}

// The BTC amount in the destination account is not enough to open the position.
{
    "code": "51008",
    "msg": "Order failed. Insufficient BTC margin in account",
    "data": []
}

// TradeFi positions are not supported.
{
    "code": "70004",
    "msg": "Invalid instrument ID XAG-USDT-SWAP",
    "data": []
}

```

#### Response parameters

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| code | String | The result code, `0` means success |
| msg | String | The error message, empty if the code is 0 |
| blockTdId | String | Block trade ID |
| clientId | String | Client-supplied ID |
| state | String | Status of the order `filled`, `failed` |
| fromAcct | String | Source account name |
| toAcct | String | Destination account name |
| legs | Array | An array of objects containing details of each position to be moved |
| > from | Object | Object describing the "from" leg |
| >> instId | String | Instrument ID |
| >> posId | String | Position ID |
| >> px | String | Transfer price, typically a 60-minute TWAP of the mark price |
| >> side | String | Direction of the leg in the source account<br> `buy`<br> `sell` |
| >> sz | String | Number of Contracts |
| >> sCode | String | The code of the event execution result, 0 means success |
| >> sMsg | String | Rejection message if the request is unsuccessful |
| > to | Object | Object describing the "to" leg |
| >> instId | String | Instrument ID |
| >> side | String | Trade side of the trade in the destination account |
| >> posSide | String | Position side of the trade in the destination account |
| >> tdMode | String | Trade mode |
| >> px | String | Transfer price, typically a 60-minute TWAP of the mark price |
| >> ccy | String | Margin currency |
| >> sCode | String | The code of the event execution result, 0 means success |
| >> sMsg | String | Rejection message if the request is unsuccessful |
| ts | String | Unix timestamp in milliseconds indicating when the transfer request was processed |

#### Things to note
1. Only applicable to users with a trading level greater than or equal to VIP6, and can only be called through the API Key of the master account.
2. The source and destination accounts for move positions must be accounts under the same master account and they must be different.
3. For source account, a maximum of fifteen move position requests can be triggered within a 24-hour period. There is no limitation to the destination account to receive positions. Only successful requests are counted toward this limit.
4. The maximum number of legs per move position request is 30.
5. No move position fee will be charged at this time.
6. Moving positions is not supported in margin trading now.
7. TradeFi positions are not supported.
8. The move position price is determined by the TWAP (Time-Weighted Average Price) of the mark price over the past 60 minutes, using the closing mark price per minute. If the symbol is newly listed and a 60-minute TWAP is unavailable, the move position will be rejected with error code 70065
9. The move position will share the same price limit as those in the order book. The move position will fail if the 60-minute mark price TWAP is outside of the price limit.
10. For the source account, move positions must be conducted in a reduce-only manner. You must choose the opposite side of your current position and specify a size equal to or smaller than your existing position size. The system will also process move position requests in a best-effort reduce-only manner.
11. The side field of source account leg (from) should be `sell` if you are holding a long position while the side of destination account leg (to) should be `buy`, vice versa for a short position.
12. The posSide field of destination account (to) should be `net` if it's in one-way mode; `long`/`short` if it's in hedge mode. If in hedge mode, you need to specify `long`/`short` to decide whether to close current positions or open reverse positions. Otherwise, it will always open new positions.
    1. Open long: buy and open long (side: buy; posSide: long)
    2. Open short: sell and open short (side: sell; posSide: short)
    3. Close long: sell and close long (side: sell; posSide: long)
    4. Close short: buy and close short (side: buy; posSide: short)
13. Historical records of move positions can be fetched from the *Get move positions history* endpoint but only for pending or successful requests.
14. Move positions operation counting example.

| Transfer done within the day | Account A count (total) | Account B count (total) | Account C count (total) | Account D count (total) |
| --- | --- | --- | --- | --- |
| Account A to Account B | 1 | 0 | 0 | 0 |
| Account B to Account C | 1 | 1 | 0 | 0 |
| Account B to Account D | 1 | 2 | 0 | 0 |
