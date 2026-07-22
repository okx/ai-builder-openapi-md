---
site: hongkong
lang: zh
name: 闪电网络充币
method: GET
path: /api/v5/asset/deposit-lightning
module: asset
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 闪电网络充币 [funding-account-rest-api-lightning-deposits]

一个用户在最近24小时内可以生成不超过1万个不同的invoice。

#### 限速：2次/s
#### 限速规则：User ID

#### HTTP请求
`GET /api/v5/asset/deposit-lightning`

> 请求示例

```shell
GET /api/v5/asset/deposit-lightning?ccy=BTC&amt=0.01

```

```python
import okx.Funding as Funding

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# 闪电网络充币
result = fundingAPI.get_deposit_lightning(
    ccy="USDT",
    amt="100"
)
print(result)
```

#### 请求参数
| 参数 | 类型   | 是否必须 | 描述                                          |
| :--- | :----- | :------- |:--------------------------------------------|
| ccy  | String | 是       | 币种，仅支持`BTC`                                 |
| amt  | String | 是       | 充值数量，推荐在0.000001〜0.1之间                      |
| to   | String | 否       | 资金充值到账账户<br>`6`：资金账户<BR>`18`：交易账户<BR>默认是`6` |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "cTime": "1631171307612",
            "invoice": "lnbc100u1psnnvhtpp5yq2x3q5hhrzsuxpwx7ptphwzc4k4wk0j3stp0099968m44cyjg9sdqqcqzpgxqzjcsp5hzzdszuv6yv6yw5svctl8kc8uv6y77szv5kma2kuculj86tk3yys9qyyssqd8urqgcensh9l4zwlwr3lxlcdqrlflvvlwldutm6ljx486h7lylqmd06kky6scas7warx69sregzrx20ffmsr4sp865x3wasrjd8ttgqrlx3tr",
            "verifiedName":"John Corner"
        }
    ],
    "msg": ""
}
```
#### 返回参数

| 参数名             | 类型   | 描述           |
|:----------------| :----- | :------------- |
| invoice         | String | invoice 号码 |
| cTime           | String | 生成invoice时间          |
| verifiedName    | String | (接受方)已验证姓名                                                                                                                                    |

<aside class="notice">
仅针对部分用户开放此API功能服务，如果需要此功能服务请发邮件至`pablo.magro@okx.com`申请
</aside>
