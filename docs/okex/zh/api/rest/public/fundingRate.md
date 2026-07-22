---
site: okex
lang: zh
name: 获取合约当前资金费率
method: GET
path: /api/v5/public/funding-rate
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取合约当前资金费率 [public-data-rest-api-get-funding-rate]

获取合约当前资金费率

#### 限速：10次/2s
#### 限速规则：IP + Instrument ID

#### HTTP请求

`GET /api/v5/public/funding-rate`

> 请求示例

```shell
GET /api/v5/public/funding-rate?instId=BTC-USD-SWAP
```

```python
import okx.PublicData as PublicData

flag = "0"  # 实盘:0 , 模拟盘：1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# 获取合约当前资金费率
result = publicDataAPI.get_funding_rate(
    instId="BTC-USD-SWAP",
)
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                          |
| :----- | :----- | :------- | :-------------------------------------------- |
| instId | String | 是       | 产品ID，如 `BTC-USD-SWAP` 或 X-Perps 交割合约 instId，传入 `ANY` 时返回所有 X-Perps 交割合约及永续合约的资金费率信息<br>适用于`永续`及 X-Perps `交割` |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "formulaType": "noRate",
            "fundingRate": "0.0000182221218054",
            "fundingTime": "1743609600000",
            "impactValue": "",
            "instId": "BTC-USDT-SWAP",
            "instType": "SWAP",
            "interestRate": "",
            "maxFundingRate": "0.00375",
            "method": "current_period",
            "minFundingRate": "-0.00375",
            "nextFundingRate": "",
            "nextFundingTime": "1743638400000",
            "premium": "0.0000910113652644",
            "settFundingRate": "0.0000145824401745",
            "settState": "settled",
            "ts": "1743588686291"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**      | **类型** | **描述**                                                |
| :-------------- | :------- | :------------------------------------------------------ |
| instType        | String   | 产品类型<br>`SWAP`：永续合约<br>`FUTURES`：X-Perps 交割合约 |
| instId           | String   | 产品ID，如`BTC-USD-SWAP` 或 `ANY`                               |
| method | String | 资金费收取逻辑 <br>`current_period`：当期收 ~~<br>`next_period`：跨期收~~（不再支持跨期收合约） |
| formulaType | String | 公式类型<br>`noRate`：旧资金费率计算公式<br>`withRate`：新资金费率计算公式 |
| fundingRate     | String   | 下一结算周期的预测资金费率。正数表示多头向空头支付资金费；负数表示空头向多头支付资金费。此为预测值，最终结算费率可能有所不同，请参阅 `settFundingRate` 查看上次实际结算费率。注意：结算周期通常为8小时，但可能调整；实际周期请通过 `fundingTime` 与 `nextFundingTime` 之差确定。                                                |
| nextFundingRate | String | ~~下一期预测资金费率 <br>当收取逻辑为`current_period`时，nextFundingRate字段将返回""~~（不再支持跨期收合约） |
| fundingTime     | String   | 资金费时间 ，Unix时间戳的毫秒数格式，如 `1597026383085` |
| nextFundingTime  | String   | 下一期资金费时间 ，Unix时间戳的毫秒数格式，如 `1622851200000` |
| minFundingRate | String | 资金费率下限 |
| maxFundingRate | String | 资金费率上限 |
| interestRate | String | 利率 |
| impactValue | String | 深度加权金额（计价币数量） |
| settState | String | 资金费率结算状态 <br>`processing`：结算中 <br>`settled`：已结算 |
| settFundingRate | String | 若 settState = `processing`，该字段代表用于本轮结算的资金费率；若 settState = `settled`，该字段代表用于上轮结算的资金费率 |
| premium | String | 溢价指数<br> 公式：[max (0，深度加权买价 - 指数价格) – max (0，指数价格 – 深度加权卖价)] / 指数价格|
| ts | String | 数据更新时间，Unix时间戳的毫秒数格式，如 `1597026383085` |

<aside class="notice">
针对一些资金费率波动较大的小币种，OKX也将实时关注行情变化，在必要时候，将资金费率收取频率从8小时收付，改成频率较高的6小时/4小时/2小时/1小时收付。因此，用户应关注`fundingTime`及`nextFundingTime`字段以确定合约的资金费收取频率。
</aside>
