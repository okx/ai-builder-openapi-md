---
site: us
lang: zh
name: 获取溢价历史数据
method: GET
path: /api/v5/public/premium-history
module: public
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### 获取溢价历史数据 [public-data-rest-api-get-premium-history]

获取最近6个月的溢价历史数据

#### 限速：20次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/public/premium-history`

> 请求示例

```shell
GET /api/v5/public/premium-history?instId=BTC-USDT-SWAP
```

```python
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                |
|:-------| :----- |:-----|:----------------------------------|
| instId | String | 是    | 产品ID，如 `BTC-USDT-SWAP`<br>适用于`永续` |
| after  | String | 否    | 请求此时间戳（不包含）之前的分页内容，传的值为对应接口的`ts`  |
| before | String | 否    | 请求此时间戳（不包含）之后的分页内容，传的值为对应接口的`ts`  |
| limit  | String | 否    | 分页返回的结果集数量，最大为`100`。默认返回`100`条。   |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "instId": "BTC-USDT-SWAP",
            "premium": "0.0000578896878167",
            "ts": "1713925924000"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**   | **类型** | **描述**                                                                                                                    |
|:----------| :------- |:--------------------------------------------------------------------------------------------------------------------------|
| instId    | String | 产品ID ，如 `BTC-USDT-SWAP`                                                                                                   |
| premium | String | 溢价指数<br> 公式：[max (0，深度加权买价 - 指数价格) – max (0，指数价格 – 深度加权卖价)] / 指数价格|
| ts        | String | 数据产生的时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                     |
