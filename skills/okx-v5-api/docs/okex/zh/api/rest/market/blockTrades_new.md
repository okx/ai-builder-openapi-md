---
site: okex
lang: zh
name: 获取大宗交易公共单腿成交数据
method: GET
path: /api/v5/public/block-trades
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取大宗交易公共单腿成交数据 [block-trading-rest-api-get-public-single-leg-transactions-of-block-trades]

查询市场上交易产品维度的大宗交易公共成交数据，根据 tradeId 倒序排序。数据将在大宗交易执行15分钟后更新。

#### 限速：20次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/public/block-trades`

> 请求示例

```shell
GET /api/v5/public/block-trades?instId=BTC-USDT
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                               |
| :----- | :----- | :------- | :------------------------------------------------- |
| instId | String | 是       | 产品ID，如 `BTC-USDT`                      |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "fillVol": "5",
            "fwdPx": "26857.86591585",
            "groupId": "",
            "idxPx": "26889.7",
            "instId": "BTC-USD-231013-22000-P",
            "markPx": "0.0000000000000001",
            "px": "0.0026",
            "side": "buy",
            "sz": "1",
            "tradeId": "632960608383700997",
            "ts": "1697181568974"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                             |
| :--------- | :------- | :--------------------------------------------------- |
| instId     | String   | 产品ID                                               |
| tradeId    | String   | 成交ID                                               |
| px         | String   | 成交价格                                             |
| sz         | String   | 成交数量<br>对于币币交易，成交数量的单位为交易货币<br/>对于交割、永续以及期权，单位为张。 |
| side       | String   | 成交方向 <br>`buy`：买  <br>`sell`：卖                 |
| fillVol | String | 成交时的隐含波动率 <br>仅适用于 `期权` | 
| fwdPx | String | 成交时的远期价格 <br>仅适用于 `期权` | 
| idxPx | String | 成交时的指数价格  <br>适用于 `交割`, `永续`, `期权` | 
| markPx | String | 成交时的标记价格 <br>适用于 `交割`, `永续`, `期权` |  
| groupId | String | 组合询价单ID<br>只适用于组合询价单，普通询价单返回 "" |
| ts         | String   | 成交时间，Unix时间戳的毫秒数格式， 如`1597026383085` |

<aside class="notice">
最多获取最近500条历史公共成交数据
</aside>

<aside class="notice">
组合询价单介绍<br><br>

1. 新增返回参数 groupId，协助用户将子账户执行映射到组合询价单。仅适用于组合询价单，对普通询价单返回 ""。<br>
2. 该接口返回的交易数据应为子级询价单，但拆分为单腿，tradeId 有值
</aside>
