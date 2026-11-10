---
site: us
lang: zh
name: 获取合约历史资金费率
method: GET
path: /api/v5/public/funding-rate-history
module: public
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### 获取合约历史资金费率 [public-data-rest-api-get-funding-rate-history]

获取合约历史资金费率，最多返回近三个月数据

#### 限速：10次/2s
#### 限速规则：IP + Instrument ID

#### HTTP请求

`GET /api/v5/public/funding-rate-history`

> 请求示例

```shell
GET /api/v5/public/funding-rate-history?instId=BTC-USD-SWAP
```

```python
import okx.PublicData as PublicData

flag = "0"  # 实盘:0 , 模拟盘：1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# 获取合约历史资金费率
result = publicDataAPI.funding_rate_history(
    instId="BTC-USD-SWAP",
)
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                                         |
| :----- | :----- | :------- | :----------------------------------------------------------- |
| instId | String | 是       | 产品ID，如 `BTC-USD-SWAP` 或 X-Perps 交割合约 instId<br>适用于`永续`及 X-Perps `交割` |
| before | String | 否       | 请求此时间戳之后（更新的数据）的分页内容，传的值为对应接口的`fundingTime` |
| after  | String | 否       | 请求此时间戳之前（更旧的数据）的分页内容，传的值为对应接口的`fundingTime` |
| limit  | String | 否       | 分页返回的结果集数量，最大为400，不填默认返回400条           |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "formulaType": "noRate",
            "fundingRate": "0.0000746604960499",
            "fundingTime": "1703059200000",
            "instId": "BTC-USD-SWAP",
            "instType": "SWAP",
            "method": "next_period",
            "realizedRate": "0.0000746572360545"
        },
        {
            "formulaType": "noRate",
            "fundingRate": "0.000227985782722",
            "fundingTime": "1703030400000",
            "instId": "BTC-USD-SWAP",
            "instType": "SWAP",
            "method": "next_period",
            "realizedRate": "0.0002279755647389"
        }
  ]
}
```

#### 返回参数

| **参数名**   | **类型** | **描述**                                               |
| :----------- | :------- | :----------------------------------------------------- |
| instType     | String   | 产品类型<br>`SWAP`：永续合约<br>`FUTURES`：X-Perps 交割合约 |
| instId        | String   | 产品ID，如 `BTC-USD-SWAP`                              |
| formulaType | String | 公式类型<br>`noRate`：旧资金费率计算公式<br>`withRate`：新资金费率计算公式 |
| fundingRate  | String   | 预计资金费率                                               |
| realizedRate | String   | 实际资金费率                                           |
| fundingTime  | String   | 资金费时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| method | String | 资金费收取逻辑 <br>`current_period`：当期收 <br>`next_period`：跨期收 |

<aside class="notice">
针对一些资金费率波动较大的小币种，OKX也将实时关注行情变化，在必要时候，将资金费率收取频率从8小时收付，改成频率较高的6小时/4小时/2小时/1小时收付。因此，用户应关注`fundingTime`及`nextFundingTime`字段以确定合约的资金费收取频率。
</aside>
