---
site: turkey
lang: en
name: Get system time
method: GET
path: /api/v5/public/time
module: public
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get system time

Retrieve API server time.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/public/time`

> Request Example

```shell
GET /api/v5/public/time
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Retrieve API server time
result = publicDataAPI.get_system_time()
print(result)
```

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
    {
        "ts":"1597026383085"
    }
  ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                             |
| :--------- | :------- | :--------------------------------------------------- |
| ts         | String   | System time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
