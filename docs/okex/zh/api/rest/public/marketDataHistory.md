---
site: okex
lang: zh
name: 获取历史市场数据
method: GET
path: /api/v5/public/market-data-history
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取历史市场数据[public-data-rest-api-get-historical-market-data]

<aside class="notice">
<strong>数据覆盖范围</strong><br>
历史数据回填正在进行中，不同模块、产品和时间段的数据覆盖范围可能有所差异。数据集将持续扩展，以提供更全面的历史数据覆盖。
</aside>

<aside class="notice">
<strong>旧数据格式注意</strong><br>
对于模块1（交易历史），一些旧的历史文件可能包含同时带有中文字符和英文列名的列标题。数据回填完成后，所有中文字符将被移除。请在解析数据时考虑到这一点。
</aside>

<aside class="notice">
<strong>数据发布安排</strong><br>
模块 1、2、3、11 的数据通常在 T+2 可用；订单簿数据通常在 T+3 可用。
</aside>

获取OKX历史市场数据。

#### 限速：2次/5s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/public/market-data-history`

> 请求示例

```shell
GET /api/v5/public/market-data-history?module=1&instType=SWAP&instFamilyList=BTC-USDT&dateAggrType=daily&begin=1756604295000&end=1756777095000
```

#### 请求参数

| 参数名称       | 类型           | 是否必须 | 描述                                                                                                               |
|:---------------|:---------------|:---------|:--------------------------------------------------------------------------------------------------------------------------|
| module         | String         | 是      | 数据模块类型<br>`1`: 逐笔成交历史<br>`2`: 1分钟K线<br>`3`: 资金费率<br> `4`: 400档位深度 <br> `5`: 5000档位深度（自2025年11月1日起支持）<br>`6`: 50档位深度 (将逐步弃用，请使用 module = `4`,`5` 代替)<br>`11`: 借币利率 |
| instType       | String         | 是      | 产品类型<br>`SPOT`<br>`FUTURES`<br>`SWAP`<br>`OPTION` |
| instIdList     | String         | 可选     | 产品ID列表，例如 `BTC-USDT` 或 `ANY` 表示所有产品（`ANY` 仅支持 module = `1`, `2`, `3`, `11` & dateAggrType = `daily`）<br>多个产品请用英文逗号分隔，如 `BTC-USDT,ETH-USDT`<br>最大长度 = 10 <br>仅适用于instType = `SPOT`|
| instFamilyList | String         | 可选     | 交易品种列表，例如 `BTC-USDT` 或 `ANY` 表示所有产品（`ANY` 仅支持 module = `1`, `2`, `3`, `11` & dateAggrType = `daily`）<br>多个品种请用英文逗号分隔，如 `BTC-USDT,ETH-USDT`<br>最大长度 = 10 (当module = `6` & instType = `OPTION`时为1) <br>仅适用于instType ≠ `SPOT`|
| dateAggrType   | String         | 是      | 日期聚合类型<br> `daily` (不支持 module = `3` & instFamilyList ≠ `ANY`)<br> `monthly` （不支持module = `6`） |
| begin          | String         | 是      | 开始时间戳，Unix时间戳格式为毫秒数（包含该时间）<br>日度最大范围：20天，月度最大范围：20个月 |
| end            | String         | 是      | 结束时间戳，Unix时间戳格式为毫秒数（包含该时间）<br>当module = `6` & instType = `OPTION`时，仅返回`end`指定日期的数据 |

> 返回示例

```json
{
  "code": "0",
  "data": [{
    "dateAggrType": "daily",
    "details": [{
      "dateRangeEnd": "1756656000000",
      "dateRangeStart": "1756569600000",
      "groupDetails": [{
        "dateTs": "1756656000000",
        "filename": "BTC-USDT-SWAP-trades-2025-09-01.zip",
        "sizeMB": "10.82",
        "url": "https://static.okx.com/cdn/okex/traderecords/trades/daily/20250901/BTC-USDT-SWAP-trades-2025-09-01.zip"
      },
      {
        "dateTs": "1756569600000",
        "filename": "BTC-USDT-SWAP-trades-2025-08-31.zip",
        "sizeMB": "4.82",
        "url": "https://static.okx.com/cdn/okex/traderecords/trades/daily/20250831/BTC-USDT-SWAP-trades-2025-08-31.zip"
      }],
      "groupSizeMB": "15.64",
      "instFamily": "BTC-USDT",
      "instId": "",
      "instType": "SWAP"
    }],
    "totalSizeMB": "15.64",
    "ts": "1756882260390"
  }],
  "msg": ""
}
```

> 返回示例，当没有数据文件时

```json
{
    "code": "0",
    "data": [
        {
            "dateAggrType": "monthly",
            "details": [],
            "totalSizeMB": "0",
            "ts": "1756889595507"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名称**    | **类型** | **描述**                                                                                                           |
|:-----------------|:---------|:--------------------------------------------------------------------------------------------------------------------------|
| ts               | String   | 响应时间戳，Unix时间戳格式为毫秒数 |
| totalSizeMB      | String   | 所有数据文件总大小，单位MB |
| dateAggrType     | String   | 日期聚合类型<br> `daily`<br> `monthly` |
| details          | Array    | |
| > instId         | String   | 产品ID |
| > instFamily     | String   | 交易品种 |
| > dateRangeStart | String   | 数据范围开始日期，Unix时间戳格式为毫秒数（包含该时间） |
| > dateRangeEnd   | String   | 数据范围结束日期，Unix时间戳格式为毫秒数（包含该时间） |
| > groupSizeMB    | String   | 数据组大小，单位MB |
| > groupDetails   | Array    | |
| >> filename      | String   | 数据文件名，例如 `BTC-USDT-SWAP-trades-2025-05-15.zip` |
| >> dataTs        | String   | 数据日期时间戳，Unix时间戳格式为毫秒数 |
| >> sizeMB        | String   | 文件大小，单位MB |
| >> url           | String   | 下载链接 |

<aside class="notice">
<strong>数据查询规则</strong><br>
• 仅使用时间戳的日期部分（yyyy-mm-dd），忽略时间部分<br>
• begin和end时间戳均为包含该时间<br>
• 数据按倒序时间顺序返回（越接近end的数据越靠前）<br>
• 如果查询超出记录限制，返回最接近end时间戳的数据<br>
• <strong>例外：</strong> 当 module = 6 且 instType = OPTION 时，仅返回 end 指定日期的数据
</aside>

<aside class="notice">
<strong>时间戳解析的时区规范</strong><br>
将Unix时间戳转换为日期时，以下时区约定适用于所有时间戳字段（begin, end, dateRangeStart, dateRangeEnd, dataTs）：<br>
• <strong>深度数据</strong>（模块4、5、6）：UTC+0<br>
• <strong>其他数据模块</strong>（模块1、2、3、11）：UTC+8
</aside>
