---
site: turkey
lang: zh
name: POST / 申请成交明细（近两年）
method: POST
path: /api/v5/trade/fills-archive
module: trade
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 申请成交明细（近两年） [order-book-trading-trade-post-transaction-details-in-the-past-2-years]

申请近2年的历史成交成交明细信息，不包括最近三个月。

#### 限速：10 次/天
#### 限速规则：User ID
#### HTTP 请求

`POST /api/v5/trade/fills-archive`

> 请求示例

```shell
POST /api/v5/trade/fills-archive
body
{
    "year":"2023",
    "quarter":"Q1"
}
```

#### 请求参数

| 参数名            | 类型   | 是否必须 | 描述                                                                                                              |
|:---------------| :----- | :------- | :---------------------------------------------------------------------------------------------------------------- |
| year       | String | 是       | 4位数字的年份，如 `2023`|
| quarter    | String | 是       | 季度，有效值 `Q1` `Q2` `Q3` `Q4` |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "result": "true",
            "ts": "1646892328000"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名 | 类型   | 描述                                                                                |
| :----- | :----- |:----------------------------------------------------------------------------------|
| result     | String | 是否已经存在该区间的下载链接<br>`true`：已存在，可以通过"获取返佣明细下载链接"接口获取<br>`false`：不存在，正在生成，请30个小时后查看下载链接 |
| ts          | String   | 服务端首次收到请求的时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                      |

<aside class="notice">
平台需求量较多的情况下，生成数据所需要的时间会有所延长，如果超过 48 小时，请联系客服进行反馈。
</aside>
