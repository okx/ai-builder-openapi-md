---
site: okex
lang: zh
name: 预设置账户模式切换
method: POST
path: /api/v5/account/account-level-switch-preset
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 预设置账户模式切换 [trading-account-rest-api-preset-account-mode-switch]

预设置账户模式切换的必要信息，若由`组合保证金模式`切换到`合约模式`/`跨币种保证金模式`，且存在全仓交割、永续仓位，则必须预设置lever，令所有仓位具有相同杠杆倍数。

若用户未按照规定进行设置，在预检查或设置账户模式时将接收到报错或提示信息。

#### 限速：5次/2s

#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/account/account-level-switch-preset`

> 请求示例

```shell
# 1. 合约模式 -> 跨币种
POST /api/v5/account/account-level-switch-preset
body
{
    "acctLv": "3"
}

# 2. 跨币种 -> 合约模式
POST /api/v5/account/account-level-switch-preset
body
{
    "acctLv": "2"
}

# 3. 组合保证金 -> 合约模式/跨币种，且有全仓合约仓位，则必须传入lever
POST /api/v5/account/account-level-switch-preset
body
{
    "acctLv": "2",
    "lever": "10"
}

# 4. 组合保证金 -> 合约模式/跨币种，没有全仓合约仓位，则不需传入lever，不进行校验
POST /api/v5/account/account-level-switch-preset
body
{
    "acctLv": "3"
}

```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| --- | --- | --- | --- |
| acctLv | String | 是 | 账户模式<br> `2`: 合约模式<br> `3`: 跨币种保证金模式<br> `4`: 组合保证金模式 |
| lever | String | 可选 | 在`组合保证金模式`向`合约模式/跨币种保证金模式`切换，且用户有全仓仓位时，必须传入 |
| riskOffsetType | String | 可选 | ~~风险对冲模式<br> `1`：现货对冲(USDT)<br> `2`：现货对冲(币)<br> `3`：衍生品对冲（未开启现货对冲）<br> `4`：现货对冲(USDC)<br> 适用于`合约模式/跨币种保证金模式`向`组合保证金模式`切换~~（已弃用） |

> 返回结果 1. 合约模式 -> 跨币种

```json
{
    "acctLv": "3",
    "curAcctLv": "2",
    "lever": "",
    "riskOffsetType": ""
}
```

> 返回结果 2. 跨币种 -> 合约模式

```json
{
    "acctLv": "2",
    "curAcctLv": "3",
    "lever": "",
    "riskOffsetType": ""
}
```
> 返回结果 3. 组合保证金 -> 合约模式/跨币种

```json
{
    "acctLv": "2",
    "curAcctLv": "4",
    "lever": "10",
    "riskOffsetType": ""
}
```
> 返回结果 4. 组合保证金 -> 合约模式/跨币种，没有全仓合约仓位，则不需传入lever，不进行校验

```json
{
    "acctLv": "3",
    "curAcctLv": "4",
    "lever": "",
    "riskOffsetType": ""
}

```

#### 返回参数

| **参数名** | **类型** | **描述** |
| --- | --- | --- |
| curAcctLv | String | 当前账户类型 |
| acctLv | String | 切换后的账户类型 |
| lever | String | 用户预设置的全仓合约仓位杠杆倍数 |
| riskOffsetType | String | ~~用户预设置的风险对冲模式~~（已弃用） |

<aside class="notice">
<br>
lever：`保证金模式`向`合约模式`/`跨币种保证金模式`切换，且用户有全仓合约仓位，则必须传入此参数，不传则报错50014。传此参数，允许设置的最大值为各个合约的仓位大小对应合约模式/跨币种账户模式下最大杠杆倍数的最小值。例如，用户在PM模式下，有三个全仓仓位，当前仓位大小对应目标账户模式下最大杠杆倍数分别为20x/50x/100x，那么用户能够设置的最大杠杆倍数为20x。
</aside>
