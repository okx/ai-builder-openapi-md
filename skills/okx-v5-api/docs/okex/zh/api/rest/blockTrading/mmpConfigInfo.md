---
site: okex
lang: zh
name: 查看 MMP 配置
method: GET
path: /api/v5/rfq/mmp-config
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 查看 MMP 配置 [block-trading-rest-api-get-mmp-config]

该接口用于获取 MMP 的配置信息，仅适用于大宗交易中的maker。

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/rfq/mmp-config`

> 请求示例

```shell
GET /api/v5/rfq/mmp-config

```

#### 请求参数
none

> 返回结果

```json
{
  "code": "0",
  "data": [
    {
      "frozenInterval": "2000",
      "mmpFrozen": true,
      "mmpFrozenUntil": "1000",
      "countLimit": "10",
      "timeInterval": "5000"
    }
  ],
  "msg": ""
}
```

#### 返回参数

| 参数名  | 类型   | 描述                                                                                                                                 |
| :------ | :----- |:-----------------------------------------------------------------------------------------------------------------------------------|
| timeInterval      | String   | 时间窗口 (毫秒)。<br>"0" 代表不使用 MMP。                                                                                                                         |
| frozenInterval      | String   | 冻结时间长度 (毫秒)。 <br>如果为"0"，代表一直冻结，直到调用 "重置 MMP 状态" 接口解冻，且`mmpFrozenUntil`为 ""。                                                                                                                                                                                                |
| countLimit      | String   | 尝试执行次数限制                                                                                                                                                                                                 |
| mmpFrozen    | Boolean   | MMP 是否被触发。 `true` 或者 `false` |
| mmpFrozenUntil    | String   | 如果配置了 frozenInterval 且 mmpFrozen = `true`，则为不再触发MMP时的时间窗口（单位为ms），否则为""。  |
