---
site: us
lang: en
name: WebSocket
type: guide
---

## WebSocket
### Deposit info channel

A push notification is triggered when a deposit is initiated or the deposit status changes.<br>
Supports subscriptions for accounts<br>

- If it is a master account subscription, you can receive the push of the deposit info of both the master account and the sub-account.
- If it is a sub-account subscription, only the push of sub-account deposit info you can receive.

#### URL Path

/ws/v5/business (required login)

> Request Example

```shell
{
    "id": "1512",
    "op": "subscribe",
    "args": [
        {
            "channel": "deposit-info"
        }
    ]
}
```
```python
import asyncio
from okx.websocket.WsPrivateAsync import WsPrivateAsync

def callbackFunc(message):
    print(message)

async def main():
    ws = WsPrivateAsync(
        apiKey = "YOUR_API_KEY",
        passphrase = "YOUR_PASSPHRASE",
        secretKey = "YOUR_SECRET_KEY",
        url = "wss://ws.okx.com:8443/ws/v5/business",
        useServerTime=False
    )
    await ws.start()
    args = [
        {
            "channel": "deposit-info"
        }
    ]
    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### Request Parameters

| Parameter | Type   | Required | Description                         |
|:----------| :----- |:---------|-------------------------------------|
| id        | String | No       | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op        | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>|
| args      | Array of objects  | Yes      | List of subscribed channels         |
| > channel | String | Yes      | Channel name<br>`deposit-info`  |
| > ccy     | String | No       | Currency, e.g. `BTC`                  |

> Successful Response Example

```json
{
    "id": "1512",
    "event": "subscribe",
    "arg": {
        "channel": "deposit-info"
    },
	"connId": "a4d3ae55"
}
```

> Failure Response Example

```json
{
    "id": "1512",
    "event": "error",
    "code": "60012",
    "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"deposit-info\""}]}",
	"connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type   | Required | Description                                  |
| :-------- | :----- | :------- | -------------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event     | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object | No       | Subscribed channel                           |
| > channel | String | Yes      | Channel name<br>`deposit-info`           |
| > ccy     | String | No       | Currency, e.g. `BTC`                  |
| code      | String | No       | Error code                                   |
| msg       | String | No       | Error message                                |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
	"arg": {
		"channel": "deposit-info",
		"uid": "289320****60975104"
	},
	"data": [{
		"actualDepBlkConfirm": "0",
		"amt": "1",
		"areaCodeFrom": "",
		"ccy": "USDT",
		"chain": "USDT-TRC20",
		"depId": "88165462",
		"from": "",
        "fromWdId": "",
		"pTime": "1674103661147",
		"state": "0",
		"subAcct": "test",
		"to": "TEhFAqpuHa3LY*****8ByNoGnrmexeGMw",
		"ts": "1674103661123",
		"txId": "bc5376817*****************dbb0d729f6b",
		"uid": "289320****60975104"
	}]
}
```

#### Push data parameters

| **Parameters**    | **Types** | **Description**                                                                                                                                                                                                                                                                                                                |
|-------------------| --------- |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg               | Object   | Successfully subscribed channel                                                                                                                                                                                                                                                                                                |
| > channel         | String   | Channel name<br>`deposit-info`                                                                                                                                                                                                                                                                                                                   |
| > uid             | String   | User Identifier                                                                                                                                                                                                                                                                                                                |
| > ccy             | String | Currency, e.g. `BTC`                                                                                                                                                                                                                                                                                                           |
| data              | Array of objects    | Subscribed data                                                                                                                                                                                                                                                                                                                |
| > uid	            | String	 | User Identifier of the message producer                                                                                                                                                                                                                                                                                        |
| > subAcct	        | String	 | Sub-account name<br>If the message producer is master account, the parameter will return ""                                                                                                                                                                                                                                    |
| > pTime	          | String	 | Push time, the millisecond format of the Unix timestamp, e.g. `1597026383085`                                                                                                                                                                                                                                                  |
| > ccy             | String | Currency                                                                                                                                                                                                                                                                                                                       |
| > chain           | String | Chain name                                                                                                                                                                                                                                                                                                                     |
| > amt             | String | Deposit amount                                                                                                                                                                                                                                                                                                                 |
| > from            | String | Deposit account<br>Only the internal OKX account (masked mobile phone number or email address) is returned, not the address on the blockchain.                                                                                                                                                                                      |
| > areaCodeFrom    | String | If `from` is a phone number, this parameter return area code of the phone number                                                                                                                                                                                                                                               |
| > to              | String | Deposit address                                                                                                                                                                                                                                                                                                                |
| > txId            | String | Hash record of the deposit                                                                                                                                                                                                                                                                                                     |
| > ts              | String | Time of deposit record is created, Unix timestamp format in milliseconds, e.g. `1655251200000`                                                                                                                                                                                                                                 |
| > state           | String | Status of deposit<br>`0`: waiting for confirmation<br>`1`: deposit credited  <br>`2`: deposit successful <br>`8`: pending due to temporary deposit suspension on this crypto currency<br>`11`: match the address blacklist<br>`12`: account or deposit is frozen<br>`13`: sub-account deposit interception<br>`14`: KYC limit |
| > depId           | String | Deposit ID                                                                                                                                                                                                                                                                                                                     |
| > fromWdId           | String | Internal transfer initiator's withdrawal ID<br>If the deposit comes from internal transfer, this field displays the withdrawal ID of the internal transfer initiator, and will return "" in other cases                                                                                                                        |
| > actualDepBlkConfirm | String | The actual amount of blockchain confirmed in a single deposit                                                                                                                                                                                                                                                                        |
