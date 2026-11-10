---
site: my
lang: zh
name: 查看账户配置
method: GET
path: /api/v5/account/config
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### 查看账户配置 [trading-account-rest-api-get-account-configuration]

查看当前账户的配置信息。  

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/config`

> 请求示例

```shell
GET /api/v5/account/config

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 查看账户配置
result = accountAPI.get_account_config()
print(result)
```

#### 请求参数

无

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "acctLv": "2",
            "acctStpMode": "cancel_maker",
            "autoLoan": false,
            "ctIsoMode": "automatic",
            "enableSpotBorrow": false,
            "greeksType": "PA",
            "feeType": "0",
            "ip": "",
            "type": "0",
            "kycLv": "3",
            "label": "v5 test",
            "level": "Lv1",
            "levelTmp": "",
            "liquidationGear": "-1",
            "mainUid": "44705892343619584",
            "mgnIsoMode": "automatic",
            "opAuth": "1",
            "perm": "read_only,withdraw,trade",
            "posMode": "long_short_mode",
            "roleType": "0",
            "spotBorrowAutoRepay": false,
            "spotOffsetType": "",
            "spotRoleType": "0",
            "spotTraderInsts": [],
            "stgyType": "0",
            "traderInsts": [],
            "uid": "44705892343619584",
            "settleCcy": "USDC",
            "settleCcyList": ["USD", "USDC", "USDG"]
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**             | **类型**  | **描述**                                                                                                                                                |
|:--------------------|:--------|:------------------------------------------------------------------------------------------------------------------------------------------------------|
| uid                 | String  | 当前请求的账户ID，账户uid和app上的一致                                                                                                                               |
| mainUid             | String  | 当前请求的母账户ID<br>如果 uid = mainUid，代表当前账号为母账户；如果 uid != mainUid，代表当前账户为子账户。                                                                               |
| acctLv              | String  | 账户模式<br>`1`：现货模式<br>`2`：合约模式<br>`3`：跨币种保证金模式<br>`4`：组合保证金模式                                                                                        |
| acctStpMode         | String  | 账户自成交保护模式 <br>`cancel_maker`：撤销挂单 <br>`cancel_taker`：撤销吃单 <br>`cancel_both`：撤销挂单和吃单 <br>默认为`cancel_maker`，用户可通过母账户登录网页修改该配置                                             |
| posMode             | String  | 持仓方式<br>`long_short_mode`：开平仓模式<br>`net_mode`：买卖模式<br>仅适用`交割/永续`                                                                                      |
| autoLoan            | Boolean | 是否自动借币<br>`true`：自动借币 `false`：非自动借币                                                                                                                   |
| greeksType          | String  | 当前希腊字母展示方式<br>`PA`：币本位 `BS`：美元本位                                                                                                                      |
| feeType             | String  | 手续费类型<br>`0`：手续费以获取币种收取<br>`1`：手续费以计价币种收取                                                                                                                      |
| level               | String  | 当前在平台上真实交易量的用户等级，如  `Lv1`，代表普通用户等级。                                                                                                                            |
| levelTmp            | String  | 特约用户的临时体验用户等级，如 `Lv1`                                                                                                                                |
| ctIsoMode           | String  | 衍生品的逐仓保证金划转模式<br>`automatic`：开仓划转<br>`autonomy`：自主划转                                                                                                  |
| mgnIsoMode          | String  | 币币杠杆的逐仓保证金划转模式<br>`automatic`：开仓划转<br> `autonomy`：自主划转      |
| spotOffsetType      | String  | <del>现货对冲类型<br>`1`：现货对冲模式U模式<br>`2`：现货对冲模式币模式<br>`3`：非现货对冲模式<br>适用于`组合保证金模式`</del><br>已废弃                                                             |
| stgyType | String | 策略类型<br>`0`：普通策略模式<br>`1`：delta 中性策略模式 |
| roleType            | String  | 用户角色<br>`0`：普通用户<br>`1`：带单者<br>`2`：跟单者                                                                                                                |
| traderInsts         | Array of strings   | 当前账号已经设置的带单合约，仅适用于带单者                                                                                                                                 |
| spotRoleType        | String  | 现货跟单角色。<br>`0`：普通用户；`1`：带单者；`2`：跟单者                                                                                                                   |
| spotTraderInsts     | Array of strings   | 当前账号已经设置的带单币对，仅适用于带单者                                                                                                                                 |
| opAuth              | String  | 是否开通期权交易<br>`0`：未开通<br>`1`：已经开通                                                                                                                       |
| kycLv               | String  | 母账户KYC等级<br>`0`: 未认证<br>`1`: 已完成 level 1 认证<br>`2`: 已完成 level 2 认证<br>`3`: 已完成 level 3认证<br>如果请求来自子账户, kycLv 为其母账户的等级<br>如果请求来自母账户, kycLv 为当前请求的母账户等级 |
| label               | String  | 当前请求API key的备注名，不超过50位字母（区分大小写）或数字，可以是纯字母或纯数字。                                                                                                        |
| ip                  | String  | 当前请求API key绑定的ip地址，多个ip用半角逗号隔开，如：`117.37.203.58,117.37.203.57`。<br>如果没有绑定ip，会返回空字符串""                                                                 |
| perm                | String  | 当前请求的 API key 或 Access token 的权限<br>`read_only`：读取<br>`trade`：交易<br>`withdraw`：提币                                                                     |
| liquidationGear     | String  | 强平提醒的维持保证金率水平<br>`3` 和 `-1` 代表维持保证金率达到 300% 时，每隔 1 小时 app 和 ”爆仓风险预警推送频道“会推送通知。`-1` 是初始值，与`-3`有着同样效果<br/>`0` 代表不提醒                                         |
| enableSpotBorrow    | Boolean | `现货模式`下是否支持借币<br>`true`：支持<br>`false`：不支持                                                                                                             |
| spotBorrowAutoRepay | Boolean | `现货模式`下是否支持自动还币<br>`true`：支持<br>`false`：不支持                                                                                                           |
| type        | String  | 账户类型 <br>`0`：母账户 <br>`1`：普通子账户 <br>`2`：资管子账户 <br>`5`：托管交易子账户 - Copper<br>  `9`：资管交易子账户 - Copper<br> `12`：托管交易子账户 - Komainu                            |
| settleCcy     | String    | 当前账户的 USD 本位合约结算币种 |
| settleCcyList     | String    | 当前账户的 USD 本位合约结算币种列表，如 ["USD", "USDC", "USDG"]。 |
