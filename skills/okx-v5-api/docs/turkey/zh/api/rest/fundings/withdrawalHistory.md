---
site: turkey
lang: zh
name: 获取提币记录
method: GET
path: /api/v5/asset/withdrawal-history
module: asset
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取提币记录 [funding-account-rest-api-get-withdrawal-history]

根据币种，提币状态，时间范围获取提币记录，按照时间倒序排列，默认返回100条数据。<br>
支持Websocket订阅，参考 [提币信息频道](/docs-v5/zh/#funding-account-websocket-withdrawal-info-channel)。

#### 限速：6 次/s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/asset/withdrawal-history`

> 请求示例

```shell
# 查询最近的提币记录
GET /api/v5/asset/withdrawal-history

# 查询从2022年06月01日到2022年07月01日之间的BTC的提币记录
GET /api/v5/asset/withdrawal-history?ccy=BTC&after=1654041600000&before=1656633600000

```

#### 请求参数

| 参数名        | 类型       | 是否必须   | 描述                                                                                                                                                                                                                                                                                                        |
|------------|----------|--------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy        | String   | 否      | 币种名称，如 `BTC`                                                                                                                                                                                                                                                                                              |
| wdId       | String   | 否      | 提币申请ID                                                                                                                                                                                                                                                                                                    |
| clientId   | String   | 否      | 客户自定义ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                                                                                                                                                                                                                                        |
| txId       | String   | 否      | 区块转账哈希记录                                                                                                                                                                                                                                                                                                  |
| type       | String   | 否      | 提币方式<br>`3`：内部转账<br>`4`：链上提币                                                                                                                                                                                                                                                                              |
| state      | String   | 否      | 提币状态<br><br><li>阶段1：等待提币</li>`19`：热钱包余额不足<br>`17`：钱包地址正等待国际转账规则认证<br>`10`：等待划转<br>`0`：等待提币<br>`4`/`5`/`6`/`8`/`9`/`12`：等待客服审核<br>`7`：审核通过<br>>`0`, `17`, `19` 可撤销，其他状态不可撤销<br><br><li>阶段2：提币处理中（适用于链上提币，内部转账无此阶段）</li>`1`：正在将您的交易广播到链上<br>`15`：交易待确认<br>`16`：根据当地法律法规，您的提币最多可能需要 24 小时才能到账<br>`-3`：撤销中<br><br><li>最终阶段</li>`-2`：已撤销<br>`-1`：失败<br>`2`：提币成功 |
| after      | String   | 否      | 查询在此之前的内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1654041600000`                                                                                                                                                                                                                                                          |
| before     | String   | 否      | 查询在此之后的内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1656633600000`                                                                                                                                                                                                                                                          |
| limit      | string   | 否      | 返回的结果集数量，默认为100，最大为100，不填默认返回100条                                                                                                                                                                                                                                                                         |

> 返回结果

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "note": "",
      "chain": "ETH-Ethereum",
      "fee": "0.007",
      "feeCcy": "ETH",
      "ccy": "ETH",
      "clientId": "",
      "toAddrType": "1",
      "amt": "0.029809",
      "txId": "0x35c******b360a174d",
      "from": "156****359",
      "areaCodeFrom": "86",
      "to": "0xa30d1fab********7CF18C7B6C579",
      "areaCodeTo": "",
      "state": "2",
      "ts": "1655251200000",
      "nonTradableAsset": false,
      "wdId": "15447421"
    }
  ]
}
```

#### 返回参数

| 参数名              | 类型      | 描述                                                                                   |
|:-----------------|:--------|:-------------------------------------------------------------------------------------|
| ccy              | String  | 币种                                                                                   |
| chain            | String  | 币种链信息<br>有的币种下有多个链，必须要做区分，如`USDT`下有`USDT-ERC20`，`USDT-TRC20`多个链                      |
| nonTradableAsset | Boolean | 是否为不可交易资产<br>`true`：不可交易资产，`false`：可交易资产                                             |
| amt              | String  | 数量                                                                                   |
| ts               | String  | 提币申请时间，Unix 时间戳的毫秒数格式，如 `1655251200000`                                              |
| from             | String  | 提币账户<br>可以是`邮箱`/`手机号`/`子账户名`                                                         |
| areaCodeFrom     | String  | 如果`from`为手机号，该字段为该手机号的区号                                                             |
| to               | String  | 收币地址                                                                                 |
| areaCodeTo       | String  | 如果`to`为手机号，该字段为该手机号的区号                                                               |
| toAddrType       | String  | 地址类型<br>`1`: 钱包地址、邮箱、手机号、登录账户名<br>`2`: UID                                 |
| tag              | String  | 部分币种提币需要标签，若不需要则不返回此字段                                                               |
| pmtId            | String  | 部分币种提币需要此字段（payment_id），若不需要则不返回此字段                                                  |
| memo             | String  | 部分币种提币需要此字段，若不需要则不返回此字段                                                              |
| addrEx           | Object  | 提币地址备注，部分币种提币需要，若不需要则不返回此字段。如币种TONCOIN的提币地址备注标签名为comment,则该字段返回：{'comment':'123456'} |
| txId             | String  | 提币哈希记录<br>内部转账该字段返回""                                                                |
| fee              | String  | 提币手续费数量                                                                              |
| feeCcy           | String  | 提币手续费币种，如 `USDT`                                                                     |
| state            | String  | 提币状态                                                                                 |
| wdId             | String  | 提币申请ID                                                                               |
| clientId         | String  | 客户自定义ID                                                                              |
| note             | String  | 备注信息                                                                                 |
