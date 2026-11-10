---
site: okex
lang: zh
name: POST / 网格跟单下单
method: POST
path: /api/v5/tradingBot/grid/copy-order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 网格跟单下单 [order-book-trading-grid-trading-post-copy-grid-algo-order]
#### 限速：20次/2s
#### 限速规则：User ID + Instrument ID
#### HTTP请求

`POST /api/v5/tradingBot/grid/copy-order-algo`

> 请求示例

```shell
# 现货网格跟单
POST /api/v5/tradingBot/grid/copy-order-algo
body
{
    "instId": "BTC-USDT",
    "algoOrdType": "grid",
    "sourceAlgoId": "580007082221121536",
    "quoteSz": "1000"
}
```
```shell
# 合约网格跟单
POST /api/v5/tradingBot/grid/copy-order-algo
body
{
    "instId": "BTC-USDT-SWAP",
    "algoOrdType": "contract_grid",
    "sourceAlgoId": "580007082221121536",
    "lever": "3",
    "autoReserve": true,
    "sz": "5000"
}
```

#### 请求参数
| 参数名 | 类型 | 是否必须 | 描述 |
| :--- | :--- | :--- | :--- |
| instId | String | 是 | 产品ID，如 `BTC-USDT` |
| algoOrdType | String | 是 | 策略订单类型<br>`grid`：现货网格<br>`contract_grid`：合约网格 |
| sourceAlgoId | String | 是 | 被跟单的策略订单ID |
| quoteSz | String | 否 | 计价币投入金额<br>仅适用于 `grid` |
| lever | String | 否 | 杠杆倍数<br>仅适用于 `contract_grid` |
| autoReserve | Boolean | 否 | 是否自动预留保证金，仅适用于 `contract_grid`<br>`true`：自动计算实际保证金和额外保证金<br>`false`：手动指定 `actualMarginSz` 和 `extraMarginSz` |
| sz | String | 否 | 合约网格总投入金额（USDT），当 `autoReserve` 为 `true` 时必填<br>仅适用于 `contract_grid` |
| actualMarginSz | String | 否 | 实际保证金，当 `autoReserve` 为 `false` 时必填<br>仅适用于 `contract_grid` |
| extraMarginSz | String | 否 | 额外保证金，当 `autoReserve` 为 `false` 时选填，默认为 `0`<br>仅适用于 `contract_grid` |
| algoClOrdId | String | 否 | 客户自定义策略单ID |
| tag | String | 否 | 订单标签 |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "algoId": "581234567890123456",
            "algoClOrdId": "",
            "sCode": "0",
            "sMsg": "",
            "tag": ""
        }
    ]
}
```

#### 返回参数
| 参数名 | 类型 | 描述 |
| :--- | :--- | :--- |
| algoId | String | 策略订单ID |
| algoClOrdId | String | 客户自定义策略单ID |
| sCode | String | 事件执行结果的 code，0 代表成功 |
| sMsg | String | 事件执行失败时的 msg |
| tag | String | 订单标签 |
