---
site: my
lang: zh
name: 设置杠杆倍数
method: POST
path: /api/v5/account/set-leverage
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### 设置杠杆倍数 [trading-account-rest-api-set-leverage]

<br>一个产品可以有如下10种杠杆倍数的设置场景：<br><br>

1. 在`逐仓`交易模式下，设置`币币杠杆`的杠杆倍数（币对层面）；<br>
2. `现货模式`账户已开通借币功能，在`全仓`交易模式下，设置`币币杠杆`的杠杆倍数（币种层面）；<br>
3. `合约模式`账户在`全仓`交易模式下，设置`币币杠杆`的杠杆倍数（币对层面）；<br>
4. `跨币种保证金模式`账户在`全仓`交易模式下，设置`币币杠杆`的杠杆倍数（币种层面）；<br>
5. `组合保证金模式`账户在`全仓`交易模式下，设置`币币杠杆`的杠杆倍数（币种层面）；<br>
6. 在`全仓`交易模式下，设置`交割`的杠杆倍数（指数层面）；<br>
7. 在`逐仓`交易模式、`买卖`持仓模式下，设置`交割`的杠杆倍数（合约层面）；<br>
8. 在`逐仓`交易模式、`开平仓`持仓模式下，设置`交割`的杠杆倍数（合约与持仓方向层面）；<br>
9. 在`全仓`交易模式下，设置`永续`的杠杆倍数（合约层面）；<br>
10. 在`逐仓`交易模式、`买卖`持仓模式下，设置`永续`的杠杆倍数（合约层面）；<br>
11. 在`逐仓`交易模式、`开平仓`持仓模式下，设置`永续`的杠杆倍数（合约与持仓方向层面）；<br>

注意请求参数 posSide 仅在`交割/永续`的`开平仓`持仓模式下才需要填写（参见场景8和11）。<br>
请参阅右侧对应的每个案例的请求示例。<br>

#### 限速：20次/2s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/account/set-leverage`

> 请求示例

```shell
# 1.在`逐仓`交易模式下，设置`币币杠杆`的杠杆倍数（币对层面）
POST /api/v5/account/set-leverage
body
{
    "instId":"BTC-USDT",
    "lever":"5",
    "mgnMode":"isolated"
}

# 2.`现货模式`账户已开通借币功能，在`全仓`交易模式下，设置`币币杠杆`的杠杆倍数（币种层面）
POST /api/v5/account/set-leverage
body
{
    "ccy":"BTC",
    "lever":"5",
    "mgnMode":"cross"
}

# 3.`合约模式`账户在`全仓`交易模式下，设置`币币杠杆`的杠杆倍数（币对层面）
POST /api/v5/account/set-leverage
body
{
    "instId":"BTC-USDT",
    "lever":"5",
    "mgnMode":"cross"
}

# 4.`跨币种保证金模式`账户在`全仓`交易模式下，设置`币币杠杆`的杠杆倍数（币种层面）
POST /api/v5/account/set-leverage
body
{
    "ccy":"BTC",
    "lever":"5",
    "mgnMode":"cross"
}

# 5. `组合保证金模式`账户在`全仓`交易模式下，设置`币币杠杆`的杠杆倍数（币种层面）
POST /api/v5/account/set-leverage
body
{
    "ccy":"BTC",
    "lever":"5",
    "mgnMode":"cross"
}

# 6.在`全仓`交易模式下，设置`交割`的杠杆倍数（指数层面）
POST /api/v5/account/set-leverage
body
{
    "instId":"BTC-USDT-200802",
    "lever":"5",
    "mgnMode":"cross"
}

# 7.在`逐仓`交易模式、`买卖`持仓模式下，设置`交割`的杠杆倍数（合约层面）
POST /api/v5/account/set-leverage
body
{
    "instId":"BTC-USDT-200802",
    "lever":"5",
    "mgnMode":"isolated"
}

# 8.在`逐仓`交易模式、`开平仓`持仓模式下，设置`交割`的杠杆倍数（合约与头寸层面）
POST /api/v5/account/set-leverage
body
{
    "instId":"BTC-USDT-200802",
    "lever":"5",
    "posSide":"long",
    "mgnMode":"isolated"
}

# 9.在`全仓`交易模式下，设置`永续`的杠杆倍数（合约层面）
POST /api/v5/account/set-leverage
body
{
    "instId":"BTC-USDT-SWAP",
    "lever":"5",
    "mgnMode":"cross"
}

# 10.在`逐仓`交易模式、`买卖`持仓模式下，设置`永续`的杠杆倍数（合约层面）
POST /api/v5/account/set-leverage
body
{
    "instId":"BTC-USDT-SWAP",
    "lever":"5",
    "mgnMode":"isolated"
}

# 11.在`逐仓`交易模式、`开平仓`持仓模式下，设置`永续`的杠杆倍数（合约与头寸层面）
POST /api/v5/account/set-leverage
body
{
    "instId":"BTC-USDT-SWAP",
    "lever":"5",
    "posSide":"long",
    "mgnMode":"isolated"
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

# 在逐仓交易模式下，设置币币杠杆的杠杆倍数（币对层面）
result = accountAPI.set_leverage(
    instId="BTC-USDT",
    lever="5",
    mgnMode="isolated"
)
print(result)
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                                                       |
| :------ | :----- | :------- |:-----------------------------------------------------------------------------------------|
| instId  | String | 可选     | 产品ID：币对、合约<br>仅适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`的`全仓``交割``永续`，`合约模式`的`全仓``币币杠杆``交割``永续` 以及`逐仓`。<br> 且在适用场景下必填。                             |
| ccy     | String | 可选     | 保证金币种，用于设置开启自动借币模式下币种维度的杠杆。<br>仅适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`的`全仓``币币杠杆`。<br> 且在适用场景下必填。 |
| lever   | String | 是       | 杠杆倍数                                                                                     |
| mgnMode | String | 是       | 保证金模式<br>`isolated`：逐仓<br>`cross`：全仓<br>如果`ccy`有效传值，该参数值只能为`cross`。                        |
| posSide | String | 可选     | 持仓方向<br>`long`：开平仓模式开多<br>`short`：开平仓模式开空<br>仅适用于逐仓`交割`/`永续`<br>在开平仓模式且保证金模式为逐仓条件下必填     |

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"lever": "30",
		"mgnMode": "isolated",
		"instId": "BTC-USDT-SWAP",
		"posSide": "long"
	}]
}
```

#### 返回参数

| 参数名  | 类型   | 描述       |
| :------ | :----- | :--------- |
| lever   | String | 杠杆倍数   |
| mgnMode | String | 保证金模式<br>`isolated`：逐仓<br>`cross`：全仓 |
| instId  | String | 产品ID |
| posSide | String | 持仓方向   |

<aside class="notice">
当希望在指数层面设置交割/永续的全仓杠杆倍数时，传入任意产品ID 和保证金模式（全仓）即可。
</aside>
<aside class="notice">
组合保证金账户下交割和永续的全仓不能调整杠杆倍数。
</aside>
