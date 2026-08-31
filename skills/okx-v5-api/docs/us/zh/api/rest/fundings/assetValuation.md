---
site: us
lang: zh
name: 获取账户资产估值
method: GET
path: /api/v5/asset/asset-valuation
module: asset
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### 获取账户资产估值 [funding-account-rest-api-get-account-asset-valuation]

查看账户资产估值

#### 限速：1次/s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/asset/asset-valuation`

> 请求示例

```shell
GET /api/v5/asset/asset-valuation
```

```python
import okx.Funding as Funding

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# 获取账户资产估值
result = fundingAPI.get_asset_valuation()
print(result)
```

#### 请求参数

| 参数 | 类型   | 是否必须 | 描述                                                                                                                               |
| :--- | :----- | :------- |:---------------------------------------------------------------------------------------------------------------------------------|
| ccy  | String | 否       | 资产估值对应的单位<br>BTC 、USDT<br>USD 、CNY 、JPY、KRW、RUB、EUR<br>VND 、IDR 、INR、PHP、THB、TRY <br>AUD 、SGD 、ARS、SAR、AED、IQD <br>默认为`BTC`为单位的估值 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "details": {
                "classic": "124.6",
                "earn": "1122.73",
                "funding": "0.09",
                "trading": "2544.28"
            },
            "totalBal": "3790.09",
            "ts": "1637566660769"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名    | 类型     | 描述           |
| :-------- |:-------| :------------- |
| totalBal       | String | 账户总资产估值  |
| ts       | String | 数据更新时间，Unix时间戳的毫秒数格式，如 1597026383085           |
| details | Object | 各个账户的资产估值 |
| > funding  | String | 资金账户       |
| > trading  | String | 交易账户       |
| > classic  | String | 经典账户 (已废弃)    |
| > earn     | String | 金融账户       |
