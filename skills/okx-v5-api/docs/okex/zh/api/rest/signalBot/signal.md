---
site: okex
lang: zh
name: POST / 创建信号
method: POST
path: /api/v5/tradingBot/signal/create-signal
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 创建信号 [order-book-trading-signal-bot-trading-post-create-signal]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/signal/create-signal`

> 请求示例

```shell
POST /api/v5/tradingBot/signal/create-signal
body
{
  "signalChanName": "long short",
  "signalDesc": "this is the first version"
}
```

#### 请求参数

| 参数名                   | 类型               | 是否必须 | 描述                                                                                                                          |
|:----------------------|:-----------------|:-----|:----------------------------------------------------------------------------------------------------------------------------|
| signalChanName          | String          | 是    | 信号名称                                                                                                            |
| signalChanDesc          | String          | 否    | 信号描述                                                                                                            |

> 返回结果

```json
{
    "code": "0",
    "data": [
       {
           "signalChanId" :"572112109",
           "signalChanToken":"dojuckew331lkx"
       }

    ],
    "msg": ""
}

```

#### 返回参数

| **参数名**     | **类型** | **描述**            |
|:------------| :------- |:------------------|
| signalChanId      | String   | 信号ID                    |
| signalChanToken   | String   | 信号单的用户身份标识         |
