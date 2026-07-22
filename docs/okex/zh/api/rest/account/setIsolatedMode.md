---
site: okex
lang: zh
name: 逐仓交易设置
method: POST
path: /api/v5/account/set-isolated-mode
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 逐仓交易设置 [trading-account-rest-api-isolated-margin-trading-settings]

可以通过该接口设置币币杠杆和交割、永续的逐仓仓位保证金的划转模式

#### 限速：5次/2s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/account/set-isolated-mode`

> 请求示例

```shell
POST /api/v5/account/set-isolated-mode
body
{
    "isoMode":"automatic",
    "type":"MARGIN"
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

# 逐仓交易设置
result = accountAPI.set_isolated_mode(
    isoMode="automatic",
    type="MARGIN"
)
print(result)
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                                |
| :------ | :----- | :------- |:------------------------------------------------------------------|
| isoMode  | String | 是     | 逐仓保证金划转模式<br>`auto_transfers_ccy`：新版开仓自动划转，支持交易货币及计价货币作为保证金，仅适用于`币币杠杆`<br>`automatic`：开仓自动划转  |
| type     | String | 是    | 业务线类型<br>`MARGIN`：币币杠杆<br>`CONTRACTS`：合约                          |

<aside class="notice">
当前账户内有持仓和挂单时，不能调整逐仓保证金划转模式。
</aside>

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "isoMode": "automatic"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名  | 类型   | 描述                                                     |
| :------ | :----- |:-------------------------------------------------------|
| isoMode   | String | 逐仓保证金划转模式<br>`auto_transfers_ccy`：新版开仓自动划转<br>`automatic`：开仓自动划转 |

<aside class="notice">
衍生品<br>
开仓划转：在开仓和平仓时自动占用和释放保证金
</aside>

<aside class="notice">
杠杆<br>
开仓划转：在开仓和平仓时自动借币和还币
</aside>
