---
site: turkey
lang: zh
name: GET / 获取成交明细（近两年）
method: GET
path: /api/v5/trade/fills-archive
module: trade
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取成交明细（近两年） [order-book-trading-trade-get-transaction-details-in-the-past-2-years]

获取近2年的历史成交明细信息

#### 限速：10 次/2s
#### 限速规则：User ID
#### HTTP 请求

`GET /api/v5/trade/fills-archive`

> 请求示例

```shell
GET /api/v5/trade/fills-archive?year=2023&quarter=Q4

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
            "fileHref": "http://xxx",
            "state": "finished",
            "ts": "1646892328000"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名 | 类型   | 描述                                        |
| :----- | :----- |:------------------------------------------|
| fileHref   | String | 文件链接                                      |
| ts    | String | 下载链接生成时间，Unix时间戳的毫秒数格式 ，如 `1597026383085` |
| state   | String | 下载链接状态<br>`finished`：已生成<br>`ongoing`：进行中  |

#### 解压后CSV里的字段说明

| **参数名**     | **类型**   | **描述**                                                                         |
|:------------|:---------|:-------------------------------------------------------------------------------|
| instType    | String   | 产品类型                                                                           |
| instId      | String   | 产品 ID                                                                          |
| tradeId     | String   | 最新成交 ID                                                                        |
| ordId       | String   | 订单 ID                                                                          |
| clOrdId     | String   | 用户自定义订单ID                                                                      |
| billId      | String   | 账单 ID                                                                          |
| tag         | String   | 订单标签                                                                           |
| fillPx      | String   | 最新成交价格                                                                         |
| fillSz      | String   | 最新成交数量                                                                         |
| side        | String   | 订单方向<br>`buy`：买<br>`sell`：卖                                                    |
| posSide     | String   | 持仓方向<br>`long`：多<br>`short`：空<br>买卖模式返回 `net`                                  |
| execType    | String   | 流动性方向<br>`T`：taker<br>`M`：maker<br>不适用于系统订单比如强平和ADL                            |
| feeCcy      | String   | 交易手续费币种或者返佣金币种                                                                 |
| fee         | String   | 手续费金额或者返佣金额<br>手续费扣除为‘负数’，如 `-0.01`<br>手续费返佣为‘正数’，如 `0.01`                         |
| ts          | String   | 成交明细产生时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                       |
| subType    | String  | 成交类型 |
