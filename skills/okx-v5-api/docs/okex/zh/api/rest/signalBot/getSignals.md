---
site: okex
lang: zh
name: GET / 查询所有信号
method: GET
path: /api/v5/tradingBot/signal/signals
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 查询所有信号 [order-book-trading-signal-bot-trading-get-signals]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/tradingBot/signal/signals`

> 请求示例

```shell
GET /api/v5/tradingBot/signal/signals
```

#### 请求参数

| 参数名                   | 类型               | 是否必须 | 描述                                                                                                                          |
|:----------------------|:-----------------|:-----|:----------------------------------------------------------------------------------------------------------------------------|
| signalSourceType      | String          | 是    | 信号来源类型<br>`1`：自己创建的<br>`2`：订阅他人<br>`3`：免费信号    |
| signalChanId          | String          | 否    | 信号ID                                                       |
| after                 | String	      | 否	  | 请求此ID之前（更旧的数据）的分页内容，传的值为对应接口的signalChanId   |
| before	            | String	      | 否	  | 请求此ID之后（更新的数据）的分页内容，传的值为对应接口的signalChanId   |
| limit	                | String	      | 否	  | 返回结果的数量，最大为100，默认100条                               |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "signalChanId": "623833708424069120",
            "signalChanName": "test",
            "signalChanDesc": "test",
            "signalChanToken": "test",
            "signalSourceType": "1"
        }
    ],
    "msg": ""
}

```

#### 返回参数

| **参数名**     | **类型** | **描述**            |
|:------------| :------- |:------------------|
| signalChanId      | String   | 信号ID                    |
| signalChanName	| String   | 信号名称                   |
| signalChanDesc	| String   | 信号描述                   |
| signalChanToken   | String   | 信号单的用户身份标识         |
| signalSourceType  | String   | 信号来源类型<br>`1`：自己创建的<br>`2`：订阅他人<br>`3`：免费信号    |
