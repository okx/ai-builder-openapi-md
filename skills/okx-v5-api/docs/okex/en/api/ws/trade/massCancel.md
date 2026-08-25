---
site: okex
lang: en
name: WS / Mass cancel order
type: guide
---

### WS / Mass cancel order

Cancel all the MMP pending orders of an instrument family.<br>

Only applicable to Option in Portfolio Margin mode, and MMP privilege is required.

#### URL Path

/ws/v5/private (required login)

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

<aside class="notice">
Rate limit is shared with the `Mass Cancel Order` REST API endpoints
</aside>

> Request Example

```shell
{
    "id": "1512",
    "op": "mass-cancel",
    "args": [{
        "instType":"OPTION",
        "instFamily":"BTC-USD"
    }]
}
```

#### Request Parameters

| Parameter    | Type    | Required | Description                                                                                                                                                                                                                                         |
|:-------------| :------ | :------- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id           | String  | Yes      | Unique identifier of the message  <br>Provided by client. It will be returned in response message for identifying the corresponding request. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters. |
| op           | String  | Yes      | Operation<br>`mass-cancel`                                                                                                                                                                                                                          |
| args         | Array of objects   | Yes      | Request parameters                                                                                                                                                                                                                                  |
| > instType   | String | Yes       | Instrument type<br>`OPTION`                                                                                                                                                                                                                         |
| > instFamily | String | Yes        | Instrument family                                                                                                                                                                                                                                   |
| > lockInterval | String | No | Lock interval(ms)<br> The range should be [0, 10 000]<br> The default is 0. You can set it as "0" if you want to unlock it immediately.<br> Error 54008 will be returned when placing order during lock interval, it is different from 51034 which is thrown when MMP is triggered                                                                                                                                                                                                                                                |

> ##### Successful Response Example

```json
{
    "id": "1512",
    "op": "mass-cancel",
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
  "op": "mass-cancel",
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
