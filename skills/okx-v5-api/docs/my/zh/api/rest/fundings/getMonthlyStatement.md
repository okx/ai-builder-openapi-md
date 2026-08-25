---
site: my
lang: zh
name: 获取月结单 (近一年)
method: GET
path: /api/v5/asset/monthly-statement
module: asset
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### 获取月结单 (近一年)[funding-account-rest-api-get-monthly-statement-last-year]

获取近一年的月结单

#### 限速：10 次/2s
#### 限速规则：User ID

#### HTTP Request

`GET /api/v5/asset/monthly-statement`

> 请求示例

```shell
GET /api/v5/asset/monthly-statement?month=Jan

```

#### 请求参数

| 参数 | 类型   | 是否必须 | 描述                                                                                       |
|:-----------| :----- | :------- |:-----------------------------------------------------------------------------------------|
| month | String | 是       | 月份, 有效值是`Jan`, `Feb`, `Mar`, `Apr`,`May`, `Jun`, `Jul`,`Aug`, `Sep`, `Oct`, `Nov`, `Dec` |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "fileHref": "http://xxx",
            "state": "finished",
            "ts": 1646892328000
        }
    ],
    "msg": ""
}
```
#### 返回参数

| 参数名      | 类型     | 描述                                         |
| :------------ |:-------|:-------------------------------------------|
| fileHref   | String | 文件链接                                       |
| ts    | Int    | 下载链接生成时间，Unix时间戳的毫秒数格式 ，如 1597026383085    |
| state | String | 下载链接状态 <br>`finished`：已生成<br>`ongoing`：进行中 |
