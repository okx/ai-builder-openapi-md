---
site: turkey
lang: en
name: Get the user's affiliate rebate information
method: GET
path: /api/v5/users/partner/if-rebate
module: users
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get the user's affiliate rebate information

This endpoint will be offline soon, please use [Get the invitee's detail](/docs-v5/en/#affiliate-rest-api-get-the-invitee-39-s-detail) <br>

It is used to get the user's affiliate rebate information for affiliate.

#### Rate limit：20 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP request

`GET /api/v5/users/partner/if-rebate`

> Request sample

```shell
GET /api/v5/users/partner/if-rebate?apiKey=86b02e93-67ab-497d-9970-8cce00a028c3
```

#### Request Parameters

| Parameter | Type  | Required| Description                                                                  |
| :------ | :----- | :------- |:-----------------------------------------------------------------------------|
| apiKey      | String | Yes      | The user's API key. Only applicable to the API key of invitee master account |

> Returned results

```json
{
    "code": "0",
    "msg": "",
    "data": {
        "result": true,
        "type": "0"
    }
}
```

#### Response parameters

| **Parameter name** | **Type** | **Description**                                                                                                                                                                                                                                                                                                                                                                                            |
| :--------- | :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| result     | Boolean   | Whether the user is invited by the current affiliate. `true`, `false`                                                                                                                                                                                                                                                                                                                                      |
| type      | String     | Whether there is affiliate rebate.<br>`0` There is affiliate rebate<br>`1` There is no affiliate rebate. Because the account which is requesting this endpoint is not affiliate <br>`2` There is no affiliate rebate. Because there is no relationship of invitation or recall, e.g. api key does not exist <br>`4` There is no affiliate rebate. Because the user level is equal to or more than VIP6 |
