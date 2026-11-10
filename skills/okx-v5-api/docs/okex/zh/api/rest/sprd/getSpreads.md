---
site: okex
lang: zh
name: 获取Spreads（公共）
method: GET
path: /api/v5/sprd/spreads
module: sprd
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取Spreads（公共） [spread-trading-rest-api-get-spreads-public]

获取可交易的Spreads。

#### 限速：20次/2s

#### 限速规则：IP

#### HTTP请求

`GET /api/v5/sprd/spreads`

> 请求示例
> 

```shell
GET /api/v5/sprd/spreads?instId=BTC-USDT

```
```python
import okx.SpreadTrading as SpreadTrading

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

spreadAPI = SpreadTrading.SpreadTradingAPI(apikey, secretkey, passphrase, False, flag)

# 获取价差产品
result = spreadAPI.get_spreads()
print(result)
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述                                                         |
| --- | --- | --- |------------------------------------------------------------|
| baseCcy | string | 否 | Spread 币种，如 `BTC`                                          |
| instId | String | 否 | Spread 里包含的产品ID                                            |
| sprdId | String | 否 | Spread ID                                                  |
| state | string | 否 | Spread 状态<br>`live`：交易中<br>`suspend`：暂停中<br>`expired`：订单过期 |

> 返回示例
> 

```json
{
	"code": "0",
	"msg": "",
	"data": [{
			"sprdId": "ETH-USD-SWAP_ETH-USD-231229",
			"sprdType": "inverse",
			"state": "live",
			"baseCcy": "ETH",
			"szCcy": "USD",
			"quoteCcy": "USD",
			"tickSz": "0.01",
			"minSz": "10",
			"lotSz": "10",
			"listTime": "1686903000159",
			"legs": [{
					"instId": "ETH-USD-SWAP",
					"side": "sell"
				},
				{
					"instId": "ETH-USD-231229",
					"side": "buy"
				}
			],
			"expTime": "1703836800000",
			"uTime": "1691376905595"
		},
		{
			"sprdId": "BTC-USDT_BTC-USDT-SWAP",
			"sprdType": "linear",
			"state": "live",
			"baseCcy": "BTC",
			"szCcy": "BTC",
			"quoteCcy": "USDT",
			"tickSz": "0.0001",
			"minSz": "0.001",
			"lotSz": "1",
			"listTime": "1597026383085",
			"expTime": "1597029999085",
			"uTime": "1597028888085",
			"legs": [{
					"instId": "BTC-USDT",
					"side": "sell"
				},
				{
					"instId": "BTC-USDT-SWAP",
					"side": "buy"
				}
			]
		},
		{
			"sprdId": "BTC-USDT_BTC-USDT-230317",
			"sprdType": "linear",
			"state": "live",
			"baseCcy": "BTC",
			"szCcy": "BTC",
			"quoteCcy": "USDT",
			"tickSz": "0.0001",
			"minSz": "0.001",
			"lotSz": "1",
			"listTime": "1597026383085",
			"expTime": "1597029999085",
			"uTime": "1597028888085",
			"legs": [{
					"instId": "BTC-USDT",
					"side": "sell"
				},
				{
					"instId": "BTC-USDT-230317",
					"side": "buy"
				}
			]
		}
	]
}

```

#### 返回参数

| 参数名      | 类型               | 描述                                                       |
|----------|------------------|----------------------------------------------------------|
| sprdId   | String           | spread ID                                                |
| sprdType | String           | Spread类型，有效值为`linear`, `inverse`, `hybrid`               |
| state    | String           | Spread状态<br>`live`：交易中<br>`suspend`：暂停中<br>`expired`：已过期 |
| baseCcy  | String           | Spread币种，如 `BTC`                                         |
| szCcy    | String           | Spread数量单位，如 USD, BTC, ETH, USD。                         |
| quoteCcy | String           | Spread计价单位。如 USDT，USD。                                   |
| tickSz   | String           | 下单价格精度，如 0.0001。单位为Spread计价单位quoteCcy。                   |
| minSz    | String           | 最小下单数量。单位为Spread数量单位szCcy。                               |
| lotSz    | String           | 下单数量精度。单位为Spread数量单位szCcy。                               |
| listTime | String           | 上线日期。Unix时间戳的毫秒数格式，如 `1597026383085`                     |
| expTime  | String           | 失效日期。Unix时间戳的毫秒数格式，如 `1597026383085`                     |
| uTime    | String           | 上次更新时间。Unix时间戳的毫秒数格式，如 `1597026383085`                   |
| legs     | array of objects | 腿                                                        |
| > instId | String           | 产品ID                                                     |
| > side   | String           | 产品方向<br>`buy`：买入<br>`sell`：卖出                            |
