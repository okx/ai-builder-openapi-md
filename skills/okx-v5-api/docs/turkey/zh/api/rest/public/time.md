---
site: turkey
lang: zh
name: 获取系统时间
method: GET
path: /api/v5/public/time
module: public
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取系统时间 [public-data-rest-api-get-system-time]

获取系统时间

#### 限速：10次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/public/time`

> 请求示例

```shell
GET /api/v5/public/time
```

```python
import okx.PublicData as PublicData

flag = "0"  # 实盘:0 , 模拟盘：1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# 获取系统时间
result = publicDataAPI.get_system_time()
print(result)
```

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
    {
        "ts":"1597026383085"
    }
  ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                             |
| :--------- | :------- | :--------------------------------------------------- |
| ts         | String   | 系统时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
