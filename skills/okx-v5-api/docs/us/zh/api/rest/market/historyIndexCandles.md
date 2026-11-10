---
site: us
lang: zh
name: 获取指数历史K线数据
method: GET
path: /api/v5/market/history-index-candles
module: market
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### 获取指数历史K线数据 [public-data-rest-api-get-index-candlesticks-history]

获取最近几年的指数K线数据

#### 限速：10次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/market/history-index-candles`

> 请求示例

```shell
GET /api/v5/market/history-index-candles?instId=BTC-USD
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                                         |
| :----- | :----- | :------- | :----------------------------------------------------------- |
| instId | String | 是       | 现货指数，如`BTC-USD`<br>与 `uly` 含义相同。                                        |
| after  | String | 否       | 请求此时间戳之前（更旧的数据）的分页内容，传的值为对应接口的`ts` |
| before | String | 否       | 请求此时间戳之后（更新的数据）的分页内容，传的值为对应接口的`ts`, 单独使用时，会返回最新的数据。 |
| bar    | String | 否       | 时间粒度，默认值`1m`<br>如 [1m/3m/5m/15m/30m/1H/2H/4H] <br>UTC+8开盘价k线：[6H/12H/1D/1W/1M]<br>UTC+0开盘价k线：[/6Hutc/12Hutc/1Dutc/1Wutc/1Mutc] |
| limit  | String | 否       | 分页返回的结果集数量，最大为100，不填默认返回100条           |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
     [
        "1597026383085",
        "3.721",
        "3.743",
        "3.677",
        "3.708",
        "1"
    ],
    [
        "1597026383085",
        "3.731",
        "3.799",
        "3.494",
        "3.72",
        "1"
    ]
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                              |
| :--------- | :------- | :---------------------------------------------------- |
| ts         | String   | 开始时间，Unix时间戳的毫秒数格式，如  `1597026383085` |
| o          | String   | 开盘价格                                              |
| h          | String   | 最高价格                                              |
| l          | String   | 最低价格                                              |
| c          | String   | 收盘价格                                              |
| confirm   | String  | K线状态 <br>`0` 代表 K 线未完结，`1` 代表 K 线已完结。 |

<aside class="notice">
返回值数组顺序分别为是：[ts,o,h,l,c,confirm]
</aside>
