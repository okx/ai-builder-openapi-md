---
site: my
lang: zh
name: GET / 获取市场借贷历史（私有）
method: GET
path: /api/v5/finance/savings/lending-rate-history-priv
module: finance
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### GET / 获取市场借贷历史（私有） [financial-product-savings-get-public-borrow-history-private]

这是一个私有接口，用户需要通过鉴权才能访问。

获取用户所在站点的市场借贷历史。

#### 限速：2次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/finance/savings/lending-rate-history-priv`

> 请求示例

```shell
GET /api/v5/finance/savings/lending-rate-history-priv

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
| ts  | String | 时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
