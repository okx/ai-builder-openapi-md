---
site: my
lang: zh
name: 撤销提币
method: POST
path: /api/v5/asset/cancel-withdrawal
module: asset
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### 撤销提币 [funding-account-rest-api-cancel-withdrawal]

可以撤销普通提币，但不支持撤销闪电网络上的提币。

#### 限速：6次/s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/asset/cancel-withdrawal`

> 请求示例

```shell
POST /api/v5/asset/cancel-withdrawal
body {
   "wdId":"1123456"
}
```

```python
import okx.Funding as Funding

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# 撤销提币
result = fundingAPI.cancel_withdrawal(
    wdId="123456"
)
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                                         |
| :----- | :----- | :------- | :----------------------------------------------------------- |
| wdId    | String | 是       | 提币申请ID                                              |

> 返回结果

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

#### 返回参数

| 参数名 | 类型   | 描述       |
| :----- | :----- | :--------- |
| wdId    | String | 提币申请ID   |

<aside class="notice">
接口返回code等于0不能严格认为提币已经被撤销，只表示您的请求被系统服务器所接受，实际结果以获取提币记录中的状态为准。
</aside>
