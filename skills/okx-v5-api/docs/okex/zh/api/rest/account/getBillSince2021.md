---
site: okex
lang: zh
name: 获取账单流水（自 2021 年）
method: GET
path: /api/v5/account/bills-history-archive
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取账单流水（自 2021 年） [trading-account-rest-api-get-bills-details-since-2021]

获取自 2021 年 2 月 1 日以来的账单数据

#### 限速：10 次/2s
#### 限速规则：User ID
#### HTTP 请求

`GET /api/v5/account/bills-history-archive`

> 请求示例

```shell
GET /api/v5/account/bills-history-archive?year=2023&quarter=Q4

```

#### 请求参数

| 参数名            | 类型   | 是否必须 | 描述                                                                                                              |
|:---------------| :----- | :------- | :---------------------------------------------------------------------------------------------------------------- |
| year       | String | 是       | 4位数字的年份，如 `2023`|
| quarter    | String | 是       | 季度，有效值 `Q1` `Q2` `Q3` `Q4` |
| type | String | 否 | 账单类型，支持多个，用英文逗号分隔，如 `1,2,3`；不填则返回所有类型。<br>枚举值请通过 [获取账单类型](/docs-v5/zh/#trading-account-rest-api-get-bill-types) 接口查询。 |

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

| 参数名 | 类型   | 描述                                                               |
| :----- | :----- |:-----------------------------------------------------------------|
| fileHref   | String | 文件链接。<br/>每个链接的有效期为 5 个半小时，如果已经申请过同一季度的数据，则30天内无需再次申请。     |
| ts    | String | 服务端首次收到请求的时间，Unix时间戳的毫秒数格式 ，如 `1597026383085`                    |
| state   | String | 下载链接状态<br>`finished`：已生成<br>`ongoing`：进行中<br>`failed`：生成失败，请重新生成 |

#### 解压后CSV里的字段说明

| **参数名**     | **类型**   | **描述**                                                                         |
|:------------|:---------|:-------------------------------------------------------------------------------|
| instType    | String | 产品类型                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| billId      | String | 账单ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| subType     | String | 账单子类型                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ts          | String | 余额更新完成的时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| balChg      | String | 账户层面的余额变动数量                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| posBalChg   | String | 仓位层面的余额变动数量                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| bal         | String | 账户层面的余额数量                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| posBal      | String | 仓位层面的余额数量                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| sz          | String | 数量                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| px          | String | 价格，与 subType 相关<br><li>为成交价格时有</li>`1`：买入<br>`2`：卖出<br>`3`：开多<br>`4`：开空<br>`5`：平多<br>`6`：平空<br>`204`：大宗交易买<br>`205`：大宗交易卖<br>`206`：大宗交易开多<br>`207`：大宗交易开空<br>`208`：大宗交易平多<br>`209`：大宗交易平空<br>`114`：自动换币买入<br>`115`：自动换币卖出<br><li>为强平价格时有</li>`100`：强减平多 `101`：强减平空 `102`：强减买入 `103`：强减卖出 `104`：强平平多 `105`：强平平空 `106`：强平买入 `107`：强平卖出 `16`：强制还币 `17`：强制借币还息 `110`：强平换币转入 `111`：强平换币转出<br><li>为交割价格时有</li>`112`：交割平多 `113`：交割平空<br><li>为行权价格时有</li>`170`：到期行权 `171`：到期被行权 `172`：到期作废<br><li>为标记价格时有</li>`173`：资金费支出 `174`：资金费收入         |
| ccy         | String | 账户余额币种                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| pnl         | String | 收益                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| fee         | String | 手续费<br>正数代表平台返佣 ，负数代表平台扣除<br>[手续费规则](/cn/fees)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| mgnMode     | String | 保证金模式<br>`isolated`：逐仓<br>`cross`：全仓<br>`cash`：非保证金<br/>如果账单不是由交易产生的，该字段返回 "" |
| instId      | String | 产品ID，如 `BTC-USDT`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ordId       | String | 订单ID<br>无订单时，该字段返回 ""                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| execType    | String | 流动性方向<br>`T`：taker<br>`M`：maker                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| interest    | String | 利息                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| tag         | String | 订单标签                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| fillTime    | String | 最新成交时间                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| tradeId     | String | 最新成交ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| clOrdId     | String | 客户自定义订单ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| fillIdxPx   | String | 交易执行时的指数价格 <br>对于交叉现货币对，返回 baseCcy-USDT 的指数价格。 例 LTC-ETH，该字段返回 LTC-USDT 的指数价格。                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| fillMarkPx  | String | 成交时的标记价格，仅适用于 `交割`/`永续`/`期权`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| fillPxVol   | String | 成交时的隐含波动率，仅适用于 `期权`，其他业务线返回空字符串""                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| fillPxUsd   | String | 成交时的期权价格，以USD为单位，仅适用于 `期权`，其他业务线返回空字符串""                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| fillMarkVol | String | 成交时的标记波动率，仅适用于 `期权`，其他业务线返回空字符串""                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| fillFwdPx   | String | 成交时的远期价格，仅适用于 `期权`，其他业务线返回空字符串""                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
