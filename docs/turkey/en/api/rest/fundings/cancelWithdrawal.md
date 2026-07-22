---
site: turkey
lang: en
name: Cancel withdrawal
method: POST
path: /api/v5/asset/cancel-withdrawal
module: asset
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Cancel withdrawal

You can cancel normal withdrawal requests, but you cannot cancel withdrawal requests on Lightning.

#### Rate Limit: 6 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/asset/cancel-withdrawal`

> Request Example

```shell
POST /api/v5/asset/cancel-withdrawal
body {
   "wdId":"1123456"
}

```

```python
import okx.Funding as Funding

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading: 0, Demo trading: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# Cancel withdrawal
result = fundingAPI.cancel_withdrawal(
    wdId="123456"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                         |
| :----- | :----- | :------- | :----------------------------------------------------------- |
| wdId    | String | Yes       | Withdrawal ID                                |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "wdId": "1123456"   
    }
  ]
}
```

#### Response Parameters

| Parameter | Type   | Description                |
| :----- | :----- | :------------------ |
| wdId    | String | Withdrawal ID |

<aside class="notice">
If the code is equal to 0, it cannot be strictly considered that the withdrawal has been revoked. It only means that your request is accepted by the server. The actual result is subject to the status in the withdrawal history.
</aside>
