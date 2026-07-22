---
site: hongkong
lang: zh
name: GET / 获取账户限速
method: GET
path: /api/v5/trade/account-rate-limit
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取账户限速 [order-book-trading-trade-get-account-rate-limit]

获取账户限速相关信息<br>

仅有新订单及修改订单请求会被计入此限制。对于包含多个订单的批量请求，每个订单将被单独计数。<br>

更多细节，请见 [基于成交比率的子账户限速](/docs-v5/zh/#overview-rate-limits-fill-ratio-based-sub-account-rate-limit)

#### 限速：1次/s

#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/trade/account-rate-limit`

> 请求示例

```shell

# 获取账户限速相关信息
GET /api/v5/trade/account-rate-limit

```

#### 请求参数

None

> 返回结果

```json
{
   "code":"0",
   "data":[
      {
         "accRateLimit":"2000",
         "fillRatio":"0.1234",
         "mainFillRatio":"0.1234",
         "nextAccRateLimit":"2000",
         "ts":"123456789000"
      }
   ],
   "msg":`""`
}

```

#### 返回参数

| 参数名 | 类型 | 描述 |
| --- | --- | --- |
| fillRatio | String | 监测期内子账户的成交比率。 <br>适用于交易费等级 >= VIP 5 的用户，其他用户返回 `""`。<br>若账户在过去 7 天内无任何成交数据，则返回 `""`。<br>若监测期内无成交量，则返回 `"0"`。<br>若监测期内有成交量但无下单操作数，则返回 `"9999"`。|
| mainFillRatio | String | 监测期内母账户合计成交比率。<br>适用于交易费等级 >= VIP 5 的用户，其他用户返回 `""`。<br>若账户在过去 7 天内无任何成交数据，则返回 `""`。<br>若监测期内无成交量，则返回 `"0"`。|
| accRateLimit | String | 当前子账户交易限速（每两秒） |
| nextAccRateLimit | String | 下一评估周期预计的子账户交易限速（每两秒）。<br>适用于交易费等级 >= VIP 5的用户，其余用户返回 `""` 。|
| ts | String | 数据更新时间 <br>对于交易费等级>= VIP 5的用户，数据将于每日 08:00（UTC）生成 <br>对于交易费等级 < VIP 5的用户，返回当前时间戳 。|
