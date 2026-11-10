---
site: okex
lang: zh
name: 倒计时全部撤单
method: POST
path: /api/v5/rfq/cancel-all-after
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 倒计时全部撤单 [block-trading-rest-api-cancel-all-after]

在倒计时结束后，取消所有报价单。

#### 限速：1次/s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/rfq/cancel-all-after`

> 请求示例

```shell
POST /api/v5/rfq/cancel-all-after
body
{
   "timeOut":"60"
}
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                        |
| :------ | :----- |:-----|:----------------------------------------------------------|
| timeOut  | String | 是    | 取消报价单的倒计时，单位为秒。<br>取值范围为 0, [10, 120]<br>0 代表不使用该功能。 |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "triggerTime":"1587971460",
            "ts":"1587971400"
        }
    ]
}
```

#### 返回参数

| 参数名  | 类型   | 描述                                   |
| :------ | :----- |:-------------------------------------|
| triggerTime     | String | 触发撤单的时间.<br>triggerTime=0 代表未使用该功能。 |
| ts     | String | 请求被接收到的时间                                 |

<aside class="notice">
建议用户每一秒调用接口一次。当倒计时全部撤单被触发时，交易引擎将为用户逐一取消报价单，该操作可能持续数秒。该功能起到保护用户的作用，不应作为交易策略使用。
</aside>
