---
site: turkey
lang: zh
name: GET / 公告类型
method: GET
path: /api/v5/support/announcement-types
module: support
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---


## GET / 公告类型 [announcement-get-announcement-types]

公共接口不需要鉴权<br>

响应根据请求 IP 进行限制。

#### 限速：1次/2s 
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/support/announcement-types`

> 请求示例

```shell
GET /api/v5/support/announcement-types

```

#### 请求参数

#### 请求示例

无

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "annType": "announcements-new-listings",
            "annTypeDesc": "New listings"
        },
        {
            "annType": "announcements-delistings",
            "annTypeDesc": "Delistings"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名          | 类型   | 描述                                                                                                                                                            |
|:-------------| :----- |:--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| annType        | String | 公告类型                                                                                                                                                                                                                                                                                                               |
| annTypeDesc        | String | 公告类型描述                                                                                                                                                                                                                                                                                                                                 |
