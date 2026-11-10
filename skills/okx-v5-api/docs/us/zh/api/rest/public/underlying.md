---
site: us
lang: zh
name: 获取衍生品标的指数
method: GET
path: /api/v5/public/underlying
module: public
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### 获取衍生品标的指数 [public-data-rest-api-get-underlying]

#### 限速：20次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/public/underlying`

> 请求示例

```shell
GET /api/v5/public/underlying?instType=FUTURES
```

```python
import okx.PublicData as PublicData

flag = "0"  # 实盘:0 , 模拟盘：1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# 获取衍生品标的指数
result = publicDataAPI.get_underlying(
    instType="FUTURES"
)
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                          |
| :----- | :----- | :------- | :-------------------------------------------- |
| instType | String | 是       | 产品类型<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权 |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        [
            "LTC-USDT",
            "BTC-USDT",
            "ETC-USDT"
        ]
    ]
}
```

#### 返回参数

| **参数名**      | **类型** | **描述**                                                |
| :-------------- | :------- | :------------------------------------------------------ |
| uly        | Array of strings   | 标的指数   如：BTC-USDT                        |
