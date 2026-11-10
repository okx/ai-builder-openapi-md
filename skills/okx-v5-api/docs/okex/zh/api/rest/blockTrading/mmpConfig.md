---
site: okex
lang: zh
name: 设置 MMP
method: POST
path: /api/v5/rfq/mmp-config
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 设置 MMP [block-trading-rest-api-set-mmp]

该接口用于设置 MMP 的配置，仅适用于大宗交易中的maker。

#### 限速：1次/10s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/rfq/mmp-config`

> 请求示例

```shell
POST /api/v5/rfq/mmp-config
body
{
    "timeInterval":"5000",
    "frozenInterval":"2000",
    "countLimit": "100"
}

```

#### 请求参数

| 参数名  | 类型  | 是否必须    | 描述                                                                                                                                 |
| :------ | :----- | :----- |:-----------------------------------------------------------------------------------------------------------------------------------|
| timeInterval  | String | 是       | 时间窗口 (毫秒)。<br>"0" 代表不使用 MMP。最大为 600,000。          |
| frozenInterval  | String | 是       | 冻结时间长度 (毫秒)。<br>"0" 代表一直冻结，直到调用 "重置 MMP 状态" 接口解冻   |
| countLimit  | String | 是      | 尝试执行次数限制                                                                |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "frozenInterval": "2000",
            "countLimit": "100",
            "timeInterval": "5000"
        }
    ]
}
```

#### 返回参数

| 参数名  | 类型   | 描述                                                                                                                                 |
| :------ | :----- |:-----------------------------------------------------------------------------------------------------------------------------------|
| timeInterval     | String | 时间窗口 (毫秒) |
| frozenInterval     | String | 冻结时间长度 (毫秒)|
| countLimit     | String | 尝试执行次数限制|

<aside class="notice">
组合询价单介绍<br><br>

对于 Maker，组合询价单的执行尝试将只计入一次 MMP，无论涉及多少账户分配。
</aside>
