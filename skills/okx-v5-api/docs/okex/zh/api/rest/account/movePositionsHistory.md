---
site: okex
lang: zh
name: 获取移仓历史
method: GET
path: /api/v5/account/move-positions-history
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取移仓历史 [trading-account-rest-api-get-move-positions-history]

仅适用于交易等级大于等于VIP6的用户，仅能通过母账户的API Key调用。用户可通过[我的手续费](https://www.okx.com/balance/fee)页面的手续费详情表格查看自己的交易等级。<br>

获取过去三天的移仓明细。

#### 限速：2次/2s

#### 限速规则：母账户 User ID

#### HTTP请求

`GET /api/v5/account/move-positions-history`

> 请求示例

```shell
Get /api/v5/account/move-positions-history

```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| --- | --- | --- | --- |
| blockTdId | String | 否 | 大宗交易ID |
| clientId | String | 否 | 客户自定义ID字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间 |
| beginTs | String | 否 | 用开始时间戳筛选执行时间，Unix时间戳的毫秒数格式，如`1597026383085` |
| endTs | String | 否 | 用结束时间戳筛选执行时间，Unix时间戳的毫秒数格式，如`1597026383085` |
| limit | String | 否 | 返回结果的数量，最大为100，默认100条 |
| state | String | 否 | 移仓状态，`filled` `pending` |

> 返回示例

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "clientId": "test",
            "blockTdId": "2066393411110139648",
            "state": "filled",
            "ts": "1734085725000",
            "fromAcct": "0",
            "toAcct": "test",
            "legs": [
                {
                    "from": {
                        "posId": "2065477911110792832",
                        "instId": "BTC-USD-SWAP",
                        "px": "100123.8",
                        "side": "sell",
                        "sz": "1"
                    },
                    "to": {
                        "instId": "BTC-USD-SWAP",
                        "px": "100123.8",
                        "side": "buy",
                        "sz": "1",
                        "tdMode": "cross",
                        "posSide": "net",
                        "ccy": ""
                    }
                },
                {
                    "from": {
                        "posId": "2063533111112153856",
                        "instId": "BTC-USDT-SWAP",
                        "px": "100078.7",
                        "side": "sell",
                        "sz": "1"
                    },
                    "to": {
                        "instId": "BTC-USDT-SWAP",
                        "px": "100078.7",
                        "side": "buy",
                        "sz": "1",
                        "tdMode": "cross",
                        "posSide": "net",
                        "ccy": ""
                    }
                }
            ]
        }
   ]
}

```

#### 返回参数

| **参数名** | **类型** | **描述** |
| --- | --- | --- |
| clientId | String | 客户自定义ID |
| blockTdId | String | 大宗交易ID |
| state | String | 移仓状态，`filled` `failed` |
| ts | String | 移仓请求处理时间戳，Unix时间戳的毫秒数格式，如`1597026383085` |
| fromAcct | String | 源账户名 |
| toAcct | String | 目标账户名 |
| legs | Array | 移仓仓位列表 |
| > from | Object | 源账户仓位 |
| >> instId | String | 产品ID |
| >> posId | String | 持仓ID |
| >> px | String | 移仓价格，过去60分钟的标记价格TWAP |
| >> side | String | 源账户的交易方向<br> `buy`<br> `sell` |
| >> sz | String | 合约数量 |
| > to | Object | 目标账户移仓配置 |
| >> instId | String | 产品ID |
| >> px | String | 移仓价格，过去60分钟的标记价格TWAP |
| >> side | String | 目标账户交易方向 |
| >> sz | String | 合约数量 |
| >> tdMode | String | 目标账户的交易模式 |
| >> posSide | String | 目标账户持仓方向 |
| >> ccy | String | 保证金币种 |
