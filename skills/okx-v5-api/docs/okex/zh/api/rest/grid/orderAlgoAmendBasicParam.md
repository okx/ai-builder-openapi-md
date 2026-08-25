---
site: okex
lang: zh
name: POST / 修改网格策略基本参数
method: POST
path: /api/v5/tradingBot/grid/amend-algo-basic-param
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 修改网格策略基本参数

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/grid/amend-algo-basic-param`

> 请求示例

```shell
POST /api/v5/tradingBot/grid/amend-algo-basic-param
body
    {
        "algoId":"448965992920907776",
        "maxPx": "100",
        "minPx": "10",
        "gridNum": "5"
        "topupAmount": "123.45"
    }
```

#### 请求参数

| 参数名         | 类型    | 是否必须 | 描述           |
| :---------    | :------ | :------- |:--------------------  |
| algoId        | String  | 是       | 策略订单ID            |
| minPx         | String  | 是       | 最小价格  |
| maxPx         | String  | 是       | 最大价格  |
| gridNum       | String     | 是    | 网格数        |
| topupAmount   | String     | 不是  | 仅限合约网格。可选填写用户自行提供的追加投资金额。若未填写，或明确填写为“0”，在编辑网格参数时，所需的追加投资金额将默认自动追加。|

> 返回结果

```json
{
    "code": "55186",
    "msg": "Due to market fluctuations, your investment amount is too large to apply these modifications.",
    "data": [
        {
            "algoId": "4283223775520665600",
            "maxTopupAmount": "12456.78",
            "requiredTopupAmount": "12.34"
        }
    ]
}
```

#### 返回参数

| **参数名**           | **类型** | **描述**                          |
|:------------------  | :------- |:-------------------------------- |
| algoId              | String   | 策略订单ID                        |
| requiredTopupAmount | String   | 修改网格参数所需补充金额            |
| maxTopupAmount | String   | 仅限合约网格。编辑网格参数时的最大追加投资金额。            |

#### 报错码
| **报错码**  | **HTTP Status 代码** | **报错文案**                                                           |
|:--------------  | :-------      |:--------------------------------------------------------------------------|
| 51000           | 400   | {param} 参数错误。
| 51346           | 400   | 最高价格应高于最低价格。
| 55123           | 400   | 您的交易账户余额不足，无法使此修改生效。请您向交易账户转入资金后再试。      |
| 55124           | 200   | 由于行情波动，您的投入金额不足，修改后的参数无法生效。       |
| 55186           | 200   | 由于行情波动，您的投入金额过大，修改后的参数无法生效。    |
