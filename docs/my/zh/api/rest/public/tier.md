---
site: my
lang: zh
name: 获取衍生品仓位档位
method: GET
path: /api/v5/public/position-tiers
module: public
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### 获取衍生品仓位档位 [public-data-rest-api-get-position-tiers]

全部仓位档位对应信息，当前最高可开杠杆倍数由您的借币持仓和维持保证金率决定。

#### 限速：10次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/public/position-tiers`

> 请求示例

```shell
GET /api/v5/public/position-tiers?tdMode=cross&instType=SWAP&instFamily=BTC-USDT
```

```python
import okx.PublicData as PublicData

flag = "0"  # 实盘:0 , 模拟盘：1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# 获取衍生品仓位档位
result = publicDataAPI.get_position_tiers(
    instType="SWAP",
    tdMode="cross",
    uly="BTC-USD"
)
print(result)
```

#### 请求参数

| 参数名            | 类型   | 是否必须 | 描述                                                                                                                         |
|:---------------| :----- | :------- |:---------------------------------------------------------------------------------------------------------------------------|
| instType       | String | 是       | 产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权                                                 |
| tdMode         | String | 是       | 保证金模式<br>`isolated`：逐仓 ；`cross`：全仓                                                                                         |
| instFamily     | String | 可选     | 交易品种，支持多instFamily，半角逗号分隔，最大不超过5个<br>当产品类型是`永续`/`交割`/`期权` 之一时，`instFamily` 必填                 |
| instId         | String | 可选     | 产品ID，支持多instId，半角逗号分隔，最大不超过5个<br>仅适用`币币杠杆`，`instId`和`ccy`必须传一个，若传两个，以`instId`为主                                            |
| ccy            | String | 可选     | 保证金币种<br>仅适用杠杆全仓，该值生效时，返回的是`跨币种保证金模式`和`组合保证金模式`下的借币量                                                                     |
| tier           | String | 否       | 查指定档位                                                                                                                      |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
    {
            "baseMaxLoan": "50",
            "imr": "0.1",
            "instId": "BTC-USDT",
            "instFamily": "",
            "maxLever": "10",
            "maxSz": "50",
            "minSz": "0",
            "mmr": "0.03",
            "optMgnFactor": "0",
            "quoteMaxLoan": "500000",
            "tier": "1",
            "uly": ""
        }
  ]
}
```

#### 返回参数

| **参数名**      | **类型** | **描述**                                                                           |
|:-------------| :------- |:---------------------------------------------------------------------------------|
| uly          | String   | 标的指数<br>适用于`交割`/`永续`/`期权`                                                        |
| instFamily   | String   | 交易品种<br>适用于`交割`/`永续`/`期权`                                                        |
| instId       | String   | 币对                                                                               |
| tier         | String   | 仓位档位                                                                             |
| minSz        | String   | 该档位最少借币量或者持仓数量  `杠杆`/`期权`/`永续`/`交割` 最小持仓量 默认0  <br>当 `ccy` 参数生效时，返回 `ccy` 的最小借币量 |
| maxSz        | String   | 该档位最多借币量或者持仓数量  `杠杆`/`期权`/`永续`/`交割` <br>当 `ccy` 参数生效时，返回 `ccy` 的最大借币量            |
| mmr          | String   | 仓位维持保证金率                                                                         |
| imr          | String   | 最低初始维持保证金率                                                                         |
| maxLever     | String   | 最高可用杠杆倍数                                                                         |
| optMgnFactor | String   | 期权保证金系数  （仅适用于期权）                                                                |
| quoteMaxLoan | String   | 计价货币  最大借币量（仅适用于杠杆，且`instId`参数生效时），如 BTC-USDT   里的 USDT最大借币量                     |
| baseMaxLoan  | String   | 交易货币  最大借币量（仅适用于杠杆，且`instId`参数生效时），如 BTC-USDT   里的 BTC最大借币量                      |
