---
site: my
lang: zh
name: GET / Status
method: GET
path: /api/v5/system/status
module: system
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

# Status [status]

## GET / Status [status-get-status]

获取系统升级事件的状态。

由计划系统维护引起的短暂不可用（<5秒）和WebSocket闪断连接（用户可以立即重连）将不会公布。此类维护只会在市场波动性低的时期进行。

#### 限速：1次/5s 

#### HTTP请求

`GET /api/v5/system/status`

> 请求示例

```shell
GET /api/v5/system/status

GET /api/v5/system/status?state=canceled

```

```python
import okx.Status as Status

flag = "0"  # 实盘:0 , 模拟盘：1
statusAPI = Status.StatusAPI(
    domain="https://openapi.okx.com",
    flag=flag,
)

# 获取系统升级事件的状态
result = statusAPI.status()
print(result)
```

#### 请求参数

#### 请求示例

| 参数名   | 类型   | 是否必须 | 描述                                                                                                                                                               |
| :------- | :----- | :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| state | String | 否       | 系统的状态<br>`scheduled`：等待中<br>`ongoing`：进行中<br>`pre_open`：预开放<br>`completed`：已完成<br>`canceled`：已取消 <br>当维护时间过长，会存在预开放时间，一般持续10分钟左右。<br>不填写此参数，默认返回 等待中、进行中和预开放 的数据 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "begin": "1672823400000",
            "end": "1672823520000",
            "href": "",
            "preOpenBegin": "",
            "scheDesc": "",
            "serviceType": "8",
            "state": "completed",
            "maintType": "1",
            "env": "1",
            "system": "unified",
            "title": "Trading account system upgrade (in batches of accounts)"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名          | 类型   | 描述                                                                                                                                                            |
|:-------------| :----- |:--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| title        | String | 系统维护说明的标题                                                                                                                                                     |
| state        | String | 系统维护的状态                                                                                                                                                       |        
| begin        | String | 系统维护的开始时间，Unix时间戳的毫秒数格式 如：`1617788463867`                                                                                                                     |
| end          | String | 交易全面开放的时间，Unix时间戳的毫秒数格式 如：`1617788463867` <br>在维护完成前，是预期结束时间；维护完成后，会变更为实际结束时间。                                                                               |
| preOpenBegin | String | 预开放开始的时间，开放撤单、Post Only 下单和资金转入功能的时间                                                                                                                          |
| href         | String | 系统维护详情的超级链接,若无返回值，默认值为空，如 ""                                                                                                                                  |
| serviceType  | String | 服务类型<br>`0`：WebSocket<br>`5`：交易服务<br>`6`：大宗交易<br>`7`：策略交易<br>`8`：交易服务 (按账户分批次)<br>`9`：交易服务 (按产品分批次)<br>`10`：价差交易<br>`11`：跟单交易<br>`99`：其他（如：停止部分产品交易）          |
| system       | String | 系统<br>`unified`：交易账户                                                                                                                                          |
| scheDesc     | String | 改期进度说明，如 `由 2021-01-26T16:30:00.000Z`改期到`2021-01-28T16:30:00.000Z`                                                                                            |
| maintType    | String | 维护类型<br>`1`：计划维护<br>`2`：临时维护<br>`3`：系统故障                                                                                                                      |
| env          | String | 环境<br>`1`：实盘<br>`2`：模拟盘                                                                                                                                       |
