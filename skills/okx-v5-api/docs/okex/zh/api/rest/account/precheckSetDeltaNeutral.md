---
site: okex
lang: zh
name: 设置Delta中性预检查
method: GET
path: /api/v5/account/precheck-set-delta-neutral
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 设置Delta中性预检查 [trading-account-rest-api-precheck-set-delta-neutral]

#### **限速：1次/2s**

#### **限速规则：User ID**

#### **HTTP请求**

`GET /api/v5/account/precheck-set-delta-neutral`

> 请求示例

```shell
GET /api/v5/account/precheck-set-delta-neutral?stgyType=1

```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| --- | --- | --- | --- |
| stgyType | String | Yes | 策略类型<br> `0`：普通策略模式<br> `1`：delta 中性策略模式 |

> 返回示例

```shell
{
    "code": "0",
    "data": [
        {
            "unmatchedInfoCheck": [
                {
                    "posList": [],
                    "ordList": [],
                    "deltaLever": "",
                    "type": "spot_mode"
                },
               {
                    "posList": ["123","123","123"],
                    "ordList": [],
                    "deltaLever": "",
                    "type": "isolated_margin"
                }
            ]
        }
    ],
    "msg": ""
}

```

#### 返回参数

| **参数名** | **类型** | **描述** |
| --- | --- | --- |
| unmatchedInfoCheck | Array of objects | 包含不匹配信息对象的列表 |
| > type | String | 不匹配信息类型<br>`spot_mode`：delta 中性策略模式不支持现货模式<br>`futures_mode`：delta 中性策略模式不支持合约模式<br>`isolated_margin`：delta 中性策略模式不支持逐仓杠杆仓位<br>`isolated_contract`：delta 中性策略模式不支持逐仓合约仓位<br>`positions_options`：delta 中性策略模式不支持期权仓位<br>`isolated_pending_orders`：delta 中性策略模式不支持逐仓挂单<br>`pending_orders_options`：delta 中性策略模式不支持期权挂单<br>`trading_bot`：delta 中性策略模式不支持策略交易<br>`repay_borrowings`：在转换后，在目前策略下的负债量超过母账户维度借币限额，请偿还负债后重试<br>`loan`：不支持delta 中性策略模式使用活期借币<br>`delta_risk`：Delta风险检查失败，降低delta后重试<br>`collateral_all`：delta 中性策略模式下，所有币种必要被设置为质押币<br>`risk_unit_type`：该账户在Delta中性风险单元内，无法切换至通用模式。请在切换策略前将其从风险单元中移除。 |
| > deltaLever | String | Delta权益比率<br>仅适用于type为`delta_risk` |
| > ordList | Array of strings | 不匹配订单列表，返回订单ID<br>在type为`isolated_pending_orders`/`pending_orders_options`时适用 |
| > posList | Array of strings | 不匹配仓位列表，返回仓位ID<br>在type为`isolated_margin`/`isolated_contract`/`positions_options`时适用 |
