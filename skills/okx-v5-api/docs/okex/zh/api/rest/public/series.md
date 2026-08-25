---
site: okex
lang: zh
name: 获取系列
method: GET
path: /api/v5/public/event-contract/series
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取系列 [public-data-rest-api-get-series]

获取 OKX 预测市场的系列列表。

#### 限速：10次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/public/event-contract/series`

> 请求示例

```shell
GET /api/v5/public/event-contract/series?seriesId=BTC-ABOVE-DAILY
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
|:------|:-----|:--------|:----|
| seriesId | String | 否 | 系列 ID，如 `BTC-ABOVE-DAILY`。不传则返回所有系列。 |

> 返回示例

```json
{
    "code": "0",
    "data": [
        {
            "seriesId": "BTC-ABOVE-DAILY",
            "freq": "daily",
            "title": "BTC price above 15k",
            "category": "Crypto",
            "settlement": {
                "method": "price_above",
                "closeEarly": false,
                "srcName": "okx_index",
                "underlying": "BTC-USDT"
            }
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名 | 类型 | 描述 |
|:------|:-----|:----|
| seriesId | String | 系列 ID，如 `BTC-ABOVE-DAILY` |
| freq | String | 系列频率<br>`five_min`<br>`fifteen_min`<br>`hourly`<br>`daily`<br>`monthly` |
| title | String | 系列标题 |
| category | String | 所属分类，如 `Crypto` |
| settlement | Object | 结算信息 |
| > method | String | 结算方式。<br>`price_up_down`：价格涨跌<br>`price_above`：价格高于<br>`hit`：触及（价格触达行权价格，立即结算）<br>`between`：区间（结算价格在 [floorStrike, capStrike) 范围内） |
| > closeEarly | Boolean | 是否可以在到期时间前提前结算。<br>`true`<br>`false` |
| > srcName | String | 结算数据来源名称，如 `okx_index`、`cf_benchmark_index` |
| > underlying | String | OKX 交易对格式的标的价格，如 `BTC-USDT`。仅适用于价格相关结算方式。 |
