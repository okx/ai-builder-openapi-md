---
site: turkey
lang: zh
name: 获取用户的节点返佣信息
method: GET
path: /api/v5/users/partner/if-rebate
module: users
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取用户的节点返佣信息 [affiliate-rest-api-get-the-user-39-s-affiliate-rebate-information]

该接口即将下线，建议使用 [获取被邀请人返佣信息](/docs-v5/zh/#affiliate-rest-api-get-the-invitee-39-s-detail)<br>

该接口用于节点查询用户的返佣信息

#### 限速：20次/2s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/users/partner/if-rebate`

> 请求示例

```shell
GET /api/v5/users/partner/if-rebate?apiKey=86b02e93-67ab-497d-9970-8cce00a028c3
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                |
| :------ | :----- |:-----|:----------------------------------|
| apiKey      | String | 是    | 用户的 API key，仅支持使用被邀请人母账号的 API key |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": {
        "result": true,
        "type": "0"
    }
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                                                                                             |
| :--------- | :------- |:-----------------------------------------------------------------------------------------------------------------------------------|
| result     | Boolean   | 用户是否与当前节点有邀请关系。 `true`, `false`                                                                                                    |
| type      | String     | 是否有节点返佣<br>`0` 有节点返佣<br>`1` 没有节点返佣，因为调用该接口的账户不是节点身份<br>`2` 没有节点返佣，因为不存在邀请/召回关系，如：api key不存在 <br>`4` 没有节点返佣，因为用户的手续费等级大于等于VIP6 |
