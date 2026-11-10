---
site: us
lang: zh
name: 获取交割和行权记录
method: GET
path: /api/v5/public/delivery-exercise-history
module: public
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### 获取交割和行权记录 [public-data-rest-api-get-delivery-exercise-history]

获取3个月内的交割合约的交割记录和期权的行权记录

#### 限速：40次/2s
#### 限速规则：IP + (Instrument Type + instFamily)

#### HTTP请求

`GET /api/v5/public/delivery-exercise-history`

> 请求示例

```shell
GET /api/v5/public/delivery-exercise-history?instType=OPTION&uly=BTC-USD
```

```python
import okx.PublicData as PublicData

flag = "0"  # 实盘:0 , 模拟盘：1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# 获取交割和行权记录
result = publicDataAPI.get_delivery_exercise_history(
    instType="FUTURES",
    uly="BTC-USD"
)
print(result)
```

#### 请求参数

| 参数名            | 类型   | 是否必须   | 描述                                                   |
|:---------------| :----- |:-------|:-----------------------------------------------------|
| instType       | String | 是      | 产品类型<br>`FUTURES`：交割合约<br>`OPTION`：期权             |
| instFamily     | String | 是     | 交易品种 |
| after          | String | 否      | 请求此时间戳之前（更旧的数据）的分页内容，传的值为对应接口的`ts`                   |
| before         | String | 否      | 请求此时间戳之后（更新的数据）的分页内容，传的值为对应接口的`ts`                   |
| limit          | String | 否      | 分页返回的结果集数量，最大为100，不填默认返回100条                         |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "ts":"1597026383085",
            "details":[
                {
                    "type":"delivery",
                    "insId":"BTC-USD-190927",
                    "px":"0.016"
                }
            ]
        },
        {
            "ts":"1597026383085",
            "details":[
                {
                    "insId":"BTC-USD-200529-6000-C",
                    "type":"exercised",
                    "px":"0.016"
                },
                {
                    "insId":"BTC-USD-200529-8000-C",
                    "type":"exercised",
                    "px":"0.016"
                }
            ]
        }
    ]
}
```

#### 返回参数

| 参数名 | 类型     | 描述                                                         |
| :----- |:-------| :----------------------------------------------------------- |
| ts     | String | 交割/行权日期，Unix时间戳的毫秒数格式，如 `1597026383085`    |
| details  | Array of objects  |详细数据                                           |
| > insId  | String | 交割/行权的合约ID                                            |
| > px     | String | 交割/行权的价格                                              |
| > type   | String | 类型  <br>`delivery`：交割 <br>`exercised`：实值已行权 <br>`expired_otm`：虚值已过期 |
