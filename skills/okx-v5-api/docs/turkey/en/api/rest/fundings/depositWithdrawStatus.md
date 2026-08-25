---
site: turkey
lang: en
name: Get deposit withdraw status
method: GET
path: /api/v5/asset/deposit-withdraw-status
module: asset
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get deposit withdraw status

Retrieve deposit's and withdrawal's detailed status and estimated complete time.

#### Rate Limit: 1 request per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/asset/deposit-withdraw-status`

> Request Example

```shell
# For deposit
GET /api/v5/asset/deposit-withdraw-status?txId=xxxxxx&to=1672734730284&ccy=USDT&chain=USDT-ERC20

# For withdrawal
GET /api/v5/asset/deposit-withdraw-status?wdId=200045249

```

#### Request Parameters
| **Parameters** | **Types** | **Required** | **Description**                                                                                                        |
|:---------------| :----- | :------- |:-----------------------------------------------------------------------------------------------------------------------|
| wdId           | String | Conditional | Withdrawal ID, use to retrieve withdrawal status <br>Required to input one and only one of `wdId` and `txId`           |
| txId           | String | Conditional | Hash record of the deposit, use to retrieve deposit status <br>Required to input one and only one of `wdId` and `txId` |
| ccy            | String | Conditional | Currency type, e.g. `USDT` <br>Required when retrieving deposit status with `txId`                                     |
| to             | String | Conditional | To address, the destination address in deposit <br>Required when retrieving deposit status with `txId`                 |
| chain          | String | Conditional | Currency chain information, e.g. USDT-ERC20 <br>Required when retrieving deposit status with `txId`                    |

> Response Example

```json
{
    "code":"0",
    "data":[
        {
            "wdId": "200045249",
            "txId": "16f3638329xxxxxx42d988f97", 
            "state": "Pending withdrawal: Wallet is under maintenance, please wait.",
            "estCompleteTime": "01/09/2023, 8:10:48 PM"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter       | Type   | Description                                                                                                                                                                |
|:----------------| :----- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| estCompleteTime | String | Estimated complete time<br>The timezone is `UTC+8`. The format is MM/dd/yyyy, h:mm:ss AM/PM <br>estCompleteTime is only an approximate estimated time, for reference only.   |
| state           | String | The detailed stage and status of the deposit/withdrawal <br>The message in front of the colon is the stage; the message after the colon is the ongoing status.             |
| txId            | String | Hash record on-chain<br>For withdrawal, if the `txId` has already been generated, it will return the value, otherwise, it will return "".                                  |
| wdId            | String | Withdrawal ID<br>When retrieving deposit status, wdId returns blank "".                                                                                                    |

<aside class="notice">
Stage References<br>
Deposit<br>
Stage 1: On-chain transaction detection <br>
Stage 2: Push deposit data to associated account <br>
Stage 3: Receiving account credit <br>
Final stage: Deposit complete<br>
Withdrawal<br>
Stage 1: Pending withdrawal <br>
Stage 2: Withdrawal in progress <br>
Final stage: Withdrawal complete / cancellation complete <br>
</aside>
