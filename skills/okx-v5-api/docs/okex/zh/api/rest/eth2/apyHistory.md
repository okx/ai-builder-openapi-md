---
site: okex
lang: zh
name: GET / 获取历史收益率(公共)
method: GET
path: /api/v5/finance/staking-defi/eth/apy-history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取历史收益率(公共) [financial-product-eth-staking-get-apy-history-public]

公共接口无须鉴权

#### 限速：6次/s
#### 限速规则：IP

#### HTTP 请求

`GET /api/v5/finance/staking-defi/eth/apy-history`

> 请求示例

```shell
GET /api/v5/finance/staking-defi/eth/apy-history?days=2

```

```python
import okx.Finance.EthStaking as EthStaking

flag = "0"  # 实盘: 0, 模拟盘: 1

StackingAPI = EthStaking.EthStakingAPI(flag=flag)

result = StackingAPI.eth_apy_history(days="7")
print(result)
```

#### 请求参数

| 参数   | 类型   | 是否必须 | 描述                  |
|:-----| :----- |:-----|:--------------------|
| days | String | 是    | 查询最近多少天内的数据，不超过365天 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "rate": "0.02690000",
            "ts": "1734195600000"
        },
        {
            "rate": "0.02840000",
            "ts": "1734109200000"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名  | 类型     | 描述                                       |
|------|--------|------------------------------------------|
| rate | String | 年化收益率，如 `0.01`代表`1%`                     |
| ts   | String | 时间，值为时间戳，Unix时间戳为毫秒数格式，如 `1597026383085` |
