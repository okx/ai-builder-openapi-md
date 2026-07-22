---
site: us
lang: zh
name: 获取免息额度和币种折算率等级
method: GET
path: /api/v5/public/discount-rate-interest-free-quota
module: public
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### 获取免息额度和币种折算率等级 [public-data-rest-api-get-discount-rate-and-interest-free-quota]

获取免息额度和币种折算率等级

#### 限速：2 次/2s
#### 限速规则：IP

#### HTTP 请求

`GET /api/v5/public/discount-rate-interest-free-quota`

> 请求示例

```shell
GET /api/v5/public/discount-rate-interest-free-quota?ccy=BTC
```

```python
import okx.PublicData as PublicData

flag = "0"  # 实盘:0 , 模拟盘：1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# 获取免息额度和币种折算率等级
result = publicDataAPI.discount_interest_free_quota()
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述 |
| :----- | :----- | :------- | :--- |
| ccy    | String | 否       | 币种 |
| discountLv | String | 否 | <del>折算率等级（已废弃）<del> |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "amt": "0",
            "ccy": "BTC",
            "collateralRestrict": false,
            "details": [
                {
                    "discountRate": "0.98",
                    "liqPenaltyRate": "0.02",
                    "maxAmt": "20",
                    "minAmt": "0",
                    "tier": "1",
                    "disCcyEq": "1000"
                },
                {
                    "discountRate": "0.9775",
                    "liqPenaltyRate": "0.0225",
                    "maxAmt": "25",
                    "minAmt": "20",
                    "tier": "2",
                    "disCcyEq": "2000"
                }
            ],
            "discountLv": "1",
            "minDiscountRate": "0"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                                                                                                                                                                            |
| :--------- | :------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| ccy        | String   | 币种                                                                                                                                                                                                                |
|  colRes               | String          | 平台维度质押限制状态<br> `0`：限制未触发<br> `1`：限制未触发，但该币种接近平台质押上限<br> `2`：限制已触发。该币种不可用作新订单的保证金，这可能会导致下单失败。但它仍会被计入账户有效保证金，保证金率不会收到影响。<br> 更多详情，请参阅[平台总质押借币上限说明](https://www.okx.com/zh-hans/help/introduction-to-the-platforms-collateralized-borrowing-limit-mechanism)。 |
| collateralRestrict | Boolean | ~~平台维度的质押借币限制<br> `true`<br> `false`~~（已弃用，请使用colRes） |
| amt        | String   | 免息金额                                                                                                                                                                                                            |
| discountLv     | String | <del>折算率等级<del>（已废弃）               |
| minDiscountRate          | String   | 最小折算率，针对数量超过最后一档的最大值时              |
| details          | Array of objects   | 新的币种折算率详情                          |
| > discountRate   | String   | 折算率                                |
| > maxAmt         | String   | 梯度区间上限，单位为币种，如 BTC，"" 表示正无穷        |
| > minAmt         | String   | 梯度区间下限，单位为币种，如 BTC，最小值是0           |
| > tier           | String   | 档位                                 |
| > liqPenaltyRate | String   | 强平罚金费率                             |
| > disCcyEq | String| 折扣后的币种权益（取当前梯度区间上限），便于快速计算 |
