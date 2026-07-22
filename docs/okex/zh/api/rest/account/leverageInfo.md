---
site: okex
lang: zh
name: 获取杠杆倍数
method: GET
path: /api/v5/account/leverage-info
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取杠杆倍数 [trading-account-rest-api-get-leverage]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/leverage-info`

> 请求示例

```shell
GET /api/v5/account/leverage-info?instId=BTC-USDT-SWAP&mgnMode=cross

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 获取杠杆倍数
result = accountAPI.get_leverage(
    instId="BTC-USDT-SWAP",
    mgnMode="cross"
)
print(result)
```

#### 请求参数

| 参数名     | 类型   | 是否必须 | 描述                                                                                       |
|:--------| :----- |:-----|:-----------------------------------------------------------------------------------------|
| instId  | String | 可选   | 产品ID<br>支持多个instId查询，半角逗号分隔。instId个数不超过20个。                                              |
| ccy     | String | 可选   | 币种，用于币种维度的杠杆。<br>仅适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`的全仓币币杠杆。<br>支持多ccy查询，半角逗号分隔。ccy个数不超过20个。 |
| mgnMode | String | 是    | 保证金模式<br>`isolated`：逐仓<br>`cross`：全仓                                                        |

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [{
        "ccy":"",
		"instId": "BTC-USDT-SWAP",
		"mgnMode": "cross",
		"posSide": "long",
		"lever": "10"
	},{
        "ccy":"",
		"instId": "BTC-USDT-SWAP",
		"mgnMode": "cross",
		"posSide": "short",
		"lever": "10"
	}]
}
```

#### 返回参数

| **参数名**   | **类型**   | **描述**                                                                                  |
|:----------|:---------|:----------------------------------------------------------------------------------------|
| instId    | String   | 产品ID                                                                                    |
| ccy       | String   | 币种，用于币种维度的杠杆。<br>仅适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`的全仓币币杠杆。                                |
| mgnMode   | String   | 保证金模式                                                                                   |
| posSide   | String   | 持仓方向<br>`long`：开平仓模式开多<br>`short`：开平仓模式开空<br>`net`：买卖模式<br>开平仓模式下会返回两个方向的杠杆倍数           |
| lever     | String   | 杠杆倍数                                                                                    |

<aside class="notice">
组合保证金账户下交割和永续的全仓不能获取杠杆倍数。
</aside>
