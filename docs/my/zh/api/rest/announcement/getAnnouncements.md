---
site: my
lang: zh
name: GET / 公告
method: GET
path: /api/v5/support/announcements
module: support
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---


# 公告  [announcement]

## GET / 公告 [announcement-get-announcements]

获取公告信息，以`pTime`和`businessPTime`倒序排序，公告更新不会影响排序。每页默认有 20 条公告<br>

请求头中 Accept-Language 设置为 en-US 时返回英文公告；设置为 zh-CN 时返回中文公告<br><br>

该接口鉴权是可选的：<br>

当 HTTP 请求头中有 OK-ACCESS-KEY 时，该接口会被视为私有接口且鉴权是必须的<br>
当 HTTP 请求头中没有 OK-ACCESS-KEY 时，该接口会被视为公共接口，不需要鉴权<br><br>

当为公共接口时，响应根据请求 IP 进行限制<br>
当为私有接口时，响应会根据居住国家进行限制<br>

#### 限速：5次/2s 
#### 限速规则：User ID(私有接口时) 或者 IP (公共接口时)

#### HTTP请求

`GET /api/v5/support/announcements`

> 请求示例

```shell
GET /api/v5/support/announcements

```

#### 请求参数

#### 请求示例

| 参数名   | 类型   | 是否必须 | 描述                                                                                                                                                               |
| :------- | :----- | :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| annType     | String | 否       | 公告类型。仅支持传从"GET / 公告类型" 接口返回的公告类型<br> 不传时返回所有类型 |
| page     | String | 否       | 查询页数. <br>默认为`1` |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "details": [
                {
                    "annType": "announcements-new-listings",
                    "title": "OKX to list Virtuals Protocol (VIRTUAL) for spot trading",
                    "url": "https://www.okx.com/help/okx-to-list-virtuals-protocol-virtual-for-spot-trading",
                    "pTime": "1761620404821",
                    "businessPTime": "1761620400000"
                },
                {
                    "annType": "announcements-web3",
                    "title": "Completion of X Layer Mainnet Upgrade",
                    "url": "https://www.okx.com/help/completion-of-x-layer-mainnet-upgrade",
                    "pTime": "1761582756071",
                    "businessPTime": "1761580800000"
                },
            ],
            "totalPage": "123"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名        | 类型              | 描述                                                                                                    |
|:-------------|:------------------|:--------------------------------------------------------------------------------------------------------|
| totalPage    | String            | 总的页数                                                                                               |
| details      | Array of objects  | 公告列表                                                                                                |
| > title      | String            | 公告标题                                                                                                |
| > annType    | String            | 公告类型                                                                                                |
| > businessPTime | String         | 公告页面展示时间，供用户参考。Unix 毫秒时间戳，例如 `1597026383085`                                   |
| > pTime      | String            | 公告首次实际发布时间，Unix时间戳的毫秒数格式，如 `1597026383085`。响应可能延迟约 5 分钟。 |
| > url        | String            | 公告链接                                                                                                |
