---
site: okex
lang: zh
name: 获取借/还币历史
method: GET
path: /api/v5/account/spot-borrow-repay-history
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取借/还币历史 [trading-account-rest-api-get-borrow-repay-history]

获取`现货模式`下的借/还币历史。

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/spot-borrow-repay-history`

> 请求示例

```shell
GET /api/v5/account/spot-borrow-repay-history
```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1
accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

result = accountAPI.spot_borrow_repay_history(ccy="USDT", type="auto_borrow")
print(result)
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                                                                               |
|:-----| :----- |:-----|:-----------------------------------------------------------------------------------------------------------------|
| ccy       | String | 否       | 币种，如 `BTC`                                                                                                       |
| type      | String | 否       | 事件类型<br>`auto_borrow`：自动借币<br>`auto_repay`：自动还币<br>`manual_borrow`：手动借币<br>`manual_repay`：手动还币                     |
| after     | String | 否       | 请求发生时间`ts`之前（包含）的分页内容，Unix时间戳的毫秒数格式，如 `1597026383085`                                                            |
| before    | String | 否       | 请求发生时间`ts`之后（包含）的分页内容，Unix时间戳的毫秒数格式，如 `1597026383085`                                                            |
| limit     | String | 否       | 返回结果的数量，最大为100，默认100条                                                                                            |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "accBorrowed": "0",
            "amt": "6764.802661157592",
            "ccy": "USDT",
            "ts": "1725330976644",
            "type": "auto_repay"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名  | 类型   | 描述                                                                                             |
| :------ | :----- |:-----------------------------------------------------------------------------------------------|
| ccy         | String | 币种，如 `BTC`                                                                                     |
| type        | String | 事件类型<br>`auto_borrow`：自动借币<br>`auto_repay`：自动还币<br>`manual_borrow`：手动借币<br>`manual_repay`：手动还币 |
| amt         | String | 数量                                                                                             |
| accBorrowed | String | 累计借币数量                                                                                         |
| ts          | String | 事件发生时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                         |
