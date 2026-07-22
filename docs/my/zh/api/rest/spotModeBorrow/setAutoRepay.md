---
site: my
lang: zh
name: 设置自动还币
method: POST
path: /api/v5/account/set-auto-repay
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### 设置自动还币 [trading-account-rest-api-set-auto-repay]

仅适用于`现货模式`已开通借币的情况。

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/account/set-auto-repay`

> 请求示例

```shell
POST /api/v5/account/set-auto-repay
body
{
    "autoRepay": true
}
```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1
accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

result = accountAPI.set_auto_repay(autoRepay=True)
print(result)
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                          |
|:-----| :----- |:-----|:--------------------------------------------|
| autoRepay  | Boolean | 是    | 是否支持`现货模式`下自动还币<br>`true`：支持<br>`false`：不支持 |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "autoRepay": true
        }
    ]
}
```

#### 返回参数

| 参数名  | 类型   | 描述                                          |
| :------ | :----- |:--------------------------------------------|
| autoRepay  | Boolean | 是否支持`现货模式`下自动还币<br>`true`：支持<br>`false`：不支持 |
