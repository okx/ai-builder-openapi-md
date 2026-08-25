---
site: okex
lang: en
name: WS / Cancel all orders
type: guide
---

### WS / Cancel all orders

#### URL Path

/ws/v5/business (required login)

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

> Request Example

```shell
{
    "id": "1512",
    "op": "sprd-mass-cancel",
    "args": [{
        "sprdId": "BTC-USDT_BTC-USDT-SWAP"
    }]
}
```

#### Request Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| id | String | Yes | Unique identifier of the message provided by client. It will be returned in response message to identify the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op | String | Yes | Operation<br>`sprd-mass-cancel` |
| args | Array of objects | Yes | Request parameters |
| > sprdId | String | No | spread ID |

> ##### Successful Response Example

```json
{
    "id": "1512",
    "op": "sprd-mass-cancel",
    "data": [
        {
            "result": true
        }
    ],
    "code": "0",
    "msg": ""
} 
```

> Response Example When Format Error

```json
{
  "id": "1512",
  "op": "sprd-mass-cancel",
  "data": [],
  "code": "60013",
  "msg": "Invalid args"
}
```

#### Response Parameters

| Parameter | Type   | Description                        |
|:----------| :----- | :--------------------------------- |
| id        | String | Unique identifier of the message   |
| op        | String | Operation                          |
| code      | String | Error Code                         |
| msg       | String | Error message                      |
| data      | Array of objects  | Data                               |
| > result  | Boolean | Result of the request `true`, `false` |
