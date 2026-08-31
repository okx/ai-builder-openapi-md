---
site: okex
lang: zh
name: 移仓
method: POST
path: /api/v5/account/move-positions
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 移仓 [trading-account-rest-api-move-positions]

仅适用于交易等级大于等于VIP6的用户，仅能通过母账户的API Key调用。用户可通过[我的手续费](https://www.okx.com/balance/fee)页面的手续费详情表格查看自己的交易等级。<br>

支持同一母账户下的子账户间仓位划转。每个源账户每24小时最多可触发15次移仓请求，目标账户接受移仓不受次数限制。参考下文“注意事项”部分，以获取详情。

#### 限速：1次/1s

#### 限速规则：母账户 User ID

#### HTTP请求

`POST /api/v5/account/move-positions`

> 请求示例

```shell
{
   "fromAcct":"0",
   "toAcct":"test",
   "legs":[
      {
         "from":{
            "posId":"2065471111340792832",
            "side":"sell",
            "sz":"1"
         },
         "to":{
            "posSide":"net",
            "tdMode":"cross"
         }
      },
      {
         "from":{
            "posId":"2063111180412153856",
            "side":"sell",
            "sz":"1"
         },
         "to":{
            "posSide":"net",
            "tdMode":"cross"
         }
      }
   ],
   "clientId":"test"
}

```

#### 请求参数

| **参数名**    | **类型**           | **是否必须** | **描述**                                                                                                                                           |
|------------|------------------| --- |--------------------------------------------------------------------------------------------------------------------------------------------------|
| fromAcct   | String           | 是 | 源账户名，使用"0"代表母账户                                                                                                                                  |
| toAcct     | String           | 是 | 目标账户名，使用"0"代表母账户                                                                                                                                 |
| legs       | Array of Objects | 是 | 移仓仓位列表，每次最多支持30个仓位                                                                                                                               |
| > from     | Object           | 是 | 源账户仓位                                                                                                                                            |
| >> posId   | String           | 是 | 源账户持仓ID                                                                                                                                          |
| >> sz      | String           | 是 | 合约数量                                                                                                                                             |
| >> side    | String           | 是 | 源账户的交易方向<br> `buy`<br> `sell`                                                                                                                    |
| > to       | Object           | 是 | 目标账户移仓配置                                                                                                                                         |
| >> tdMode  | String           | 否 | 目标账户的交易模式<br> `cross`：全仓<br> `isolated`：逐仓<br> 若未提供，tdMode会采用以下默认值：<br> 在合约模式或跨币种保证金模式下买入期权：`isolated`<br> 其他情况：`cross`                          |
| >> posSide | String           | 否 | 持仓方向<br> `long`：开平仓模式开多<br> `short`：开平仓模式开空<br> `net`：买卖模式<br> 当目标账户处于买卖模式时，用户不需传入该参数，若传入，唯一有效值为`net`；当处于开平仓模式时，有效值为`long`，`short`，若未指定，目标账户将总是开仓 |
| >> ccy     | String           | 否 | 目标账户保证金币种<br> 仅适用于`合约模式`下的全仓杠杆仓位                                                                                                                  |
| clientId   | String           | 是 | 客户自定义ID，字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间                                                                                                   |

> 返回示例

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "clientId": "test",
            "blockTdId": "2065832911119076864",
            "state": "filled",
            "ts": "1734069018526",
            "fromAcct": "0",
            "toAcct": "test",
            "legs": [
                {
                    "from": {
                        "posId": "2065471111340792832",
                        "instId": "BTC-USD-SWAP",
                        "px": "100042.7",
                        "side": "sell",
                        "sz": "1",
                        "sCode": "0",
                        "sMsg": ""
                    },
                    "to": {
                        "instId": "BTC-USD-SWAP",
                        "px": "100042.7",
                        "side": "buy",
                        "sz": "1",
                        "tdMode": "cross",
                        "posSide": "net",
                        "ccy": "",
                        "sCode": "0",
                        "sMsg": ""
                    }
                },
                {
                    "from": {
                        "posId": "2063111180412153856",
                        "instId": "BTC-USDT-SWAP",
                        "px": "100008.1",
                        "side": "sell",
                        "sz": "1",
                        "sCode": "0",
                        "sMsg": ""
                    },
                    "to": {
                        "instId": "BTC-USDT-SWAP",
                        "px": "100008.1",
                        "side": "buy",
                        "sz": "1",
                        "tdMode": "cross",
                        "posSide": "net",
                        "ccy": "",
                        "sCode": "0",
                        "sMsg": ""
                    }
                }
            ]
        }
    ]
}

```

> 返回示例:失败

```json
// 目标账户处于开平仓模式，传入posSide:net不匹配
{
    "code": "51000",
    "msg": "Incorrect type of posSide (leg with Instrument Id [BTC-USD-SWAP])",
    "data": []
}

// 目标账户的BTC余额不足以开新仓位
{
    "code": "51008",
    "msg": "Order failed. Insufficient BTC margin in account",
    "data": []
}

```

#### 返回参数

| **参数名**    | **类型** | **描述** |
|------------| --- | --- |
| code       | String | 结果代码，`0`表示成功 |
| msg        | String | 错误信息，代码为0时，该字段为空 |
| blockTdId  | String | 大宗交易ID |
| clientId   | String | 客户自定义ID |
| state      | String | 移仓状态，`filled` `failed` |
| fromAcct   | String | 源账户名 |
| toAcct     | String | 目标账户名 |
| legs       | Array | 移仓仓位列表 |
| > from     | Object | 源账户仓位 |
| >> instId  | String | 产品ID |
| >> posId   | String | 持仓ID |
| >> px      | String | 移仓价格，过去60分钟的标记价格TWAP |
| >> side    | String | 源账户的交易方向<br> `buy`<br> `sell` |
| >> sz      | String | 合约数量 |
| >> sCode   | String | 事件执行结果的code，0代表成功 |
| >> sMsg    | String | 事件执行失败或成功时的msg |
| > to       | Object | 目标账户移仓配置 |
| >> instId  | String | 产品ID |
| >> side    | String | 目标账户交易方向 |
| >> posSide | String | 目标账户持仓方向 |
| >> tdMode  | String | 目标账户的交易模式 |
| >> px      | String | 移仓价格，过去60分钟的标记价格TWAP |
| >> ccy     | String | 保证金币种 |
| >> sCode   | String | 事件执行结果的code，0代表成功 |
| >> sMsg    | String | 事件执行失败或成功时的msg |
| ts         | String | 移仓请求处理时间戳，Unix时间戳的毫秒数格式，如`1597026383085` |

#### 注意事项

1. 仅适用于交易等级大于等于VIP6的用户，仅能通过母账户的API Key调用
2. 移仓的源账户和目标账户必须是统一主账户下的子账户，且两者不能相同
3. 对于源账户，24小时内最多可触发15次移仓请求，目标账户接收仓位没有次数限制，只有成功的请求才会计入该限制
4. 每个移仓请求最多支持30个仓位
5. 目前暂不收取移仓手续费
6. 目前币币杠杆交易产生的仓位不支持移仓
7. 支持TradeFi仓位（含equity perps/XPerp）移仓
8. 移仓价格采用过去60分钟内每分钟标记价格收盘价的TWAP（时间加权平均价格），若交易对为新上币且无法获取60分钟TWAP，移仓将被拒绝并返回错误码70065
9. 移仓适用于订单簿相同的限价，若标记价格TWAP超出限价范围，移仓将失败
10. 对源账户而言，移仓必须以只减仓模式进行；必须选择当前持仓的相反方向，且划转数量需小于或等于现有持仓量；系统将以尽力而为的方式按只减仓原则处理移仓请求
11. 当持有多仓时，源账户的side字段应为sell，目标账户则应为buy；空仓时，方向相反
12. 目标账户若为买卖模式，posSide应为net；若为开平仓模式，则需指定posSide为long/short以决定平仓或反向开仓，未指定时默认开新仓：
    1. 开多：买入开多（side: buy; posSide: long）
    2. 开空：卖出开空（side: sell; posSide: short）
    3. 平多：卖出平多（side: sell; posSide: long）
    4. 平空：买入平空（side: buy; posSide: short
13. 移仓历史可通过”获取移仓历史”接口查询，该接口仅包含处理中或成功的请求
14. 移仓操作计数示例

| 移仓操作      | 账户A总计次数 | 账户B总计次数 | 账户C总计次数 | 账户D总计次数 |
|-----------| --- | --- | --- | --- |
| 账户A到账户B   | 1 | 0 | 0 | 0 |
| 账户B到账户C   | 1 | 1 | 0 | 0 |
| 账户B到账户D   | 1 | 2 | 0 | 0 |
