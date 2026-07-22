---
site: okex
lang: zh
name: 获取最大可用余额/保证金
method: GET
path: /api/v5/account/max-avail-size
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取最大可用余额/保证金 [trading-account-rest-api-get-maximum-available-balance-equity]

币币和逐仓时为可用余额，全仓时为可用保证金

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/max-avail-size`

> 请求示例

```shell
# 获取BTC-USDT全仓币币杠杆指定BTC作为保证金最大可用数量
GET /api/v5/account/max-avail-size?instId=BTC-USDT&tdMode=cross&ccy=BTC

# 获取BTC-USDT币币最大可用数量
GET /api/v5/account/max-avail-size?instId=BTC-USDT&tdMode=cash

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 获取BTC-USDT币币最大可用数量
result = accountAPI.get_max_avail_size(
    instId="BTC-USDT",
    tdMode="cash"
)
print(result)
```

#### 请求参数

| **参数名**      | **类型** | 是否必须 | 描述                                                                                                         |
|:-------------| :------- | :------- |:-----------------------------------------------------------------------------------------------------------|
| instId       | String   | 是       | 产品ID，如 `BTC-USDT`<br>支持多产品ID查询（不超过5个），半角逗号分隔                                                               |
| tdMode       | String   | 是       | 交易模式<br>`cross`：全仓<br>`isolated`：逐仓<br>`cash`：非保证金<br>`spot_isolated`：现货逐仓，仅适用于`合约模式`                                     |
| ccy          | String   | 可选     | 保证金币种，适用于`逐仓杠杆`及`合约模式`下的`全仓杠杆`                                                                                |
| reduceOnly   | Boolean  | 否       | 是否为只减仓模式，仅适用于`币币杠杆`                                                                                        |
| px           | String | 否    | 平仓价格，默认为市价。<br>仅适用于杠杆只减仓                                                                                   |
| tradeQuoteCcy    | String | 否       | 用于交易的计价币种。仅适用于`币币`。<br>默认值为 `instId` 的计价币种，比如：对于 `BTC-USD`，默认取 `USD`。 |

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"instId": "BTC-USDT",
		"availBuy": "100",
		"availSell": "1"
	}]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**         |
| :--------- | :------- | :--------------- |
| instId     | String   | 产品ID       |
| availBuy   | String   | 最大买入可用余额/保证金 |
| availSell  | String   | 最大卖出可用余额/保证金 |

<aside class="notice">
币币/币币杠杆时availBuy为计价货币，availSell为交易货币。<br >全仓币币杠杆时，availBuy和availSell均为指定保证金的币种。
</aside>
