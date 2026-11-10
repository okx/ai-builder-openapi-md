---
site: okex
lang: zh
name: GET / 获取市场借贷历史（公共）
method: GET
path: /api/v5/finance/savings/lending-rate-history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取市场借贷历史（公共） [financial-product-savings-get-public-borrow-history-public]

公共接口无须鉴权<br>
返回2021年12月14日后的记录<br>

#### 限速：6次/s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/finance/savings/lending-rate-history`

> 请求示例

```shell
GET /api/v5/finance/savings/lending-rate-history

```

```python
import okx.Finance.Savings as Savings

flag = "0"  # 实盘: 0, 模拟盘: 1

SavingsAPI = Savings.SavingsAPI(flag=flag)

result = SavingsAPI.get_public_borrow_history()
print(result)
```

#### 请求参数

| 参数 | 类型   | 是否必须 | 描述                                                                  |
| :--- | :----- | :------- |:--------------------------------------------------------------------|
| ccy  | String | 否       | 币种，如 `BTC`                                                          |
| after  | String | 否       | 查询在此之前的内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085`                    |
| before | String | 否       | 查询在此之后的内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085`                    |
| limit  | String | 否       | 分页返回的结果集数量，最大为100，不填默认返回100条<br>如果不指定`ccy`,会返回同一个`ts`下的全部数据，不受`limit`限制 |

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"ccy": "BTC",
		"amt": "0.01",
		"rate": "0.001",
		"lendingRate": "0.001",
		"ts": "1597026383085"
	}]
}
```

#### 返回参数

| 参数名    | 类型   | 描述           |
| :-------- | :----- | :------------- |
| ccy       | String | 币种，如 `BTC` |
| amt       | String | ~~市场总出借数量~~（已弃用） |
| rate  | String | 出借年利率 |
| lendingRate | String | 年化出借利率 |
| ts  | String | 时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
