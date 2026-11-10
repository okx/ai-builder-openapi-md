---
site: my
lang: en
name: Withdrawal info channel
type: guide
---

### Withdrawal info channel

A push notification is triggered when a withdrawal is initiated or the withdrawal status changes.<br>
Supports subscriptions for accounts<br>

- If it is a master account subscription, you can receive the push of the withdrawal info of both the master account and the sub-account.
- If it is a sub-account subscription, only the push of sub-account withdrawal info you can receive.

#### URL Path

/ws/v5/business (required login)

> Request Example

```shell
{
    "id": "1512",
    "op": "subscribe",
    "args": [
        {
            "channel": "withdrawal-info"
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
        url = "wss://ws.okx.com/ws/v5/business",
        useServerTime=False
    )
    await ws.start()
    args = [
        {
            "channel": "withdrawal-info"
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
| > channel | String | Yes      | Channel name<br>`withdrawal-info`  |
| > ccy     | String | No       | Currency, e.g. `BTC`                  |

> Successful Response Example

```json
{
    "id": "1512",
    "event": "subscribe",
    "arg": {
        "channel": "withdrawal-info"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"withdrawal-info\"}]}",
	"connId": "a4d3ae55"
}
```

#### Response parameters

| Parameter | Type   | Required | Description                                  |
| :-------- | :----- | :------- | -------------------------------------------- |
| id           | String   | No       | Unique identifier of the message                        |
| event     | String | Yes      | Operation<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object | No       | Subscribed channel                           |
| > channel | String | Yes      | Channel name<br>`withdrawal-info`           |
| > ccy     | String | No       | Currency, e.g. `BTC`                  |
| code      | String | No       | Error code                                   |
| msg       | String | No       | Error message                                |
| connId   | String   | Yes  | WebSocket connection ID |

> Push Data Example

```json
{
	"arg": {
		"channel": "withdrawal-info",
		"uid": "289320*****0975104"
	},
	"data": [{
		"addrEx": null,
		"amt": "2",
		"areaCodeFrom": "",
		"areaCodeTo": "",
		"ccy": "USDT",
		"chain": "USDT-TRC20",
		"clientId": "",
		"fee": "0.8",
        "feeCcy": "USDT",
		"from": "",
		"memo": "",
        "nonTradableAsset": false,
        "note": "",
		"pTime": "1674103268578",
		"pmtId": "",
		"state": "0",
		"subAcct": "test",
		"tag": "",
		"to": "TN8CKTQMnpWfT******8KipbJ24ErguhF",
        "toAddrType": "1",
		"ts": "1674103268472",
		"txId": "",
		"uid": "289333*****1101696",
		"wdId": "63754560"
	}]
}
```

#### Push data parameters

| **Parameters**     | **Types** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|--------------------| --------- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg                | Object   | Successfully subscribed channel                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| > channel          | String   | Channel name                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| > uid              | String   | User Identifier                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| > ccy              | String | Currency, e.g. `BTC`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| data               | Array of objects    | Subscribed data                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| > uid	             | String	 | User Identifier of the message producer                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| > subAcct	         | String	 | Sub-account name<br>If the message producer is master account, the parameter will return ""                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| > pTime	           | String	 | Push time, the millisecond format of the Unix timestamp, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > ccy              | String  | Currency                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > chain            | String  | Chain name, e.g. `USDT-ERC20`, `USDT-TRC20`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| > nonTradableAsset | String  | Whether it is a non-tradable asset or not<br>`true`: non-tradable asset, `false`: tradable asset                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > amt              | String  | Withdrawal amount                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| > ts               | String  | Time the withdrawal request was submitted, Unix timestamp format in milliseconds, e.g. `1655251200000`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| > from             | String  | Withdrawal account<br>It can be `email`/`phone`/`sub-account name`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| > areaCodeFrom     | String  | Area code for the phone number<br>If `from` is a phone number, this parameter returns the area code for the phone number                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| > to               | String  | Receiving address                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| > areaCodeTo       | String  | Area code for the phone number<br>If `to` is a phone number, this parameter returns the area code for the phone number                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| > toAddrType       | String  | Address type<br>`1`: wallet address, email, phone, or login account name<br>`2`: UID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > tag              | String  | Some currencies require a tag for withdrawals                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > pmtId            | String  | Some currencies require a payment ID for withdrawals                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > memo             | String  | Some currencies require this parameter for withdrawals                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| > addrEx           | Object  | Withdrawal address attachment, e.g. `TONCOIN` attached tag name is comment, the return will be {'comment':'123456'}                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| > txId             | String  | Hash record of the withdrawal <br>This parameter will return "" for internal transfers.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| > fee              | String  | Withdrawal fee amount                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| > feeCcy           | String  | Withdrawal fee currency, e.g. `USDT`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > state            | String  | Status of withdrawal<br><br><li>Stage 1 : Pending withdrawal</li>`17`: Pending response from Travel Rule vendor<br>`10`: Waiting transfer<br>`0`: Waiting withdrawal<br>`4`/`5`/`6`/`8`/`9`/`12`: Waiting manual review<br>`7`: Approved<br><br><li>Stage 2 : Withdrawal in progress (Applicable to on-chain withdrawals, internal transfers do not have this stage)</li>`1`: Broadcasting your transaction to chain<br>`15`: Pending transaction validation<br>`16`: Due to local laws and regulations, your withdrawal may take up to 24 hours to arrive<br>`-3`: Canceling <br><br><li>Final stage</li>`-2`: Canceled <br>`-1`: Failed<br>`2`: Success |
| > wdId             | String  | Withdrawal ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| > clientId         | String  | Client-supplied ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| > note             | String  | Withdrawal note                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
