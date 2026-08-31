---
site: us
lang: zh
name: 设置持仓模式
method: POST
path: /api/v5/account/set-position-mode
module: account
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### 设置持仓模式 [trading-account-rest-api-set-position-mode]

`合约模式`和`跨币种保证金模式`：交割和永续合约支持开平仓模式和买卖模式。买卖模式只会有一个方向的仓位；开平仓模式可以分别持有多、空2个方向的仓位。<br>
`组合保证金模式`：交割和永续仅支持买卖模式

#### 限速：5次/2s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/account/set-position-mode`

> 请求示例

```shell
POST /api/v5/account/set-position-mode
body 
{
    "posMode":"long_short_mode"
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

# 设置持仓模式
result = accountAPI.set_position_mode(
    posMode="long_short_mode"
)
print(result)
```

#### 请求参数

| 参数名  | 类型 | 是否必须 | 描述                                                       |
| :------ | :------- | :------- | :--------------------------------------------------------- |
| posMode | String   | 是       | 持仓方式<br>`long_short_mode`：开平仓模式 `net_mode`：买卖模式<br>仅适用`交割/永续` |

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"posMode": "long_short_mode"
	}]
}
```

#### 返回参数

| 参数名  | 类型   | 描述     |
| :------ | :----- | :------- |
| posMode | String | 持仓方式 |
