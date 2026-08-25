---
site: my
lang: en
name: Query the API Key of a sub-account
method: GET
path: /api/v5/users/subaccount/apikey
module: users
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### Query the API Key of a sub-account

Applies to master accounts only

#### Rate limit：20 request per 2 seconds
#### Rate limit rule: User ID
#### HTTP request

`GET /api/v5/users/subaccount/apikey`

> Request sample

```shell
GET /api/v5/users/subaccount/apikey?subAcct=panpanBroker2
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                 |
| :------ | :----- | :------- | :----------------------------------------------------- |
| subAcct | String | Yes    | Sub-account name |
| apiKey  | String | No    | API public key|

> Returned results

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "label": "v5",
            "apiKey": "******",
            "perm": "read_only,trade",
            "ip": "1.1.1.1,2.2.2.2",
            "ts": "1597026383085"
        },
        {
            "label": "v5.1",
            "apiKey": "******",
            "perm": "read_only",
            "ip": "1.1.1.1,2.2.2.2",
            "ts": "1597026383085"
        }
    ]
}
```

#### Response parameters

| **Parameter name** | **Type** | **Description**                                              |
| :--------- | :------- | :---------------------------------------------------- |
| label   | String | API Key note|
| apiKey   | String | API public key|
| perm    | String |  API Key access <br>read_only: Read only; trade: Trade  |
| ip      | String | IP address that linked with API Key |
| ts      | String | Creation time |
