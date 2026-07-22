---
site: hongkong
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

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/max-avail-size`

> 请求示例

```shell
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

| **参数名** | **类型** | 是否必须 | 描述                                                      |
| :--------- | :------- | :------- | :-------------------------------------------------------- |
| instId     | String   | 是       | 产品ID，如 `BTC-USDT`<br>支持多产品ID查询（不超过5个），半角逗号分隔                          |
| tdMode     | String   | 是       | 交易模式<br>`cash`：非保证金 |

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
| availBuy   | String   | 最大买入可用数量 |
| availSell  | String   | 最大卖出可用数量 |

<aside class="notice">
币币时availBuy为计价货币，availSell为交易货币。
</aside>
