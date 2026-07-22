---
site: turkey
lang: zh
name: 申请月结单 (近一年)
method: POST
path: /api/v5/asset/monthly-statement
module: asset
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 申请月结单 (近一年)[funding-account-rest-api-apply-for-monthly-statement-last-year]

申请最近一年的月结单。

#### 限速：20 次/月
#### 限速规则：User ID

#### HTTP Request

`POST /api/v5/asset/monthly-statement`

> 请求示例

```shell
POST /api/v5/asset/monthly-statement
body
{
    "month":"Jan"
}
```

#### 请求参数

| 参数 | 类型   | 是否必须 | 描述                    |
|:-----------| :----- | :------- | :----------------------------------------------------------- |
| month| String | 否       | 月份,默认上一个月。有效值是`Jan`, `Feb`, `Mar`, `Apr`,`May`, `Jun`, `Jul`,`Aug`, `Sep`,`Oct`,`Nov`,`Dec`      |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ts": "1646892328000"
        }
    ],
    "msg": ""
}
```
#### 返回参数

| 参数名      | 类型      | 描述                                 |
| :------------ | :------- | :--------------------------------- |
| ts    | String | 下载链接生成时间，Unix时间戳的毫秒数格式 ，如, `1597026383085`              |
