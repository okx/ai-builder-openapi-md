---
site: okex
lang: zh
name: 价差交易
type: guide
---

# 价差交易 [spread-trading]

<aside>
👉 Spread Orderbook 产品使用户能够灵活交易大尺寸价差（Spread），可以用于简化交割合約展期、资金费套利和提高收益率，以及基于基差和期限结构的投机。

</aside>

## 介绍 [spread-trading-introduction]

### 基本概念 [spread-trading-introduction-basic-concepts]

1. 价差（**Spread） -** 做多一种产品并同时做空数量等价的另一种相关产品，形成具有两条风险互相抵消的腿的交易
2. 订单簿（**Order-book） -** 一种或一组交易产品的报价集合。每个报价都包含一个或一组定义的产品、相关数量以及*Maker*(报价者)愿意交易的价格。然后，*Taker*(接受者)可以立即消耗这些报价，直至订单簿上列出的全部数量。价差交易挂单限额为所有价差挂单合计不超过500个。

### 基本工作流程 [spread-trading-introduction-high-level-workflow]

Nitro Spreads 以熟悉的**中央限价订单簿 (CLOB)** 概念为中心：

- Spreads里包含的产品来自OKX交易所，交易之后也在OKX交易所进行清算和结算。
- 任何人都可以充当“Taker”，消耗现有的剩余订单，或“Maker”，其订单被消耗。
- 交易在订单被匹配时发生，之后它们被发送到 OKX 进行清算和结算。

简单来说，Nitro Spreads 工作流程是

1. *Maker 在 Spread 的订单簿上设置限价订单。*
2. *Taker通过限价单消耗一个resting Order。*
3. 被匹配的订单被发送去清算和结算。
4. Taker和Maker收到交易成功或拒绝的确认
5. 所有用户都会收到成功结算和清算交易的通知，除去涉及的交易双方以交易方向 (买入或卖出) 等信息。

Nitro Spreads 的主要方面：

- 所有价差都有**可公开访问**的中央限价订单簿 (**CLOB**)。
- Spreads的可用性由OKX决定。通常，这些Spreads包括同一标的下（如“BTC/USDT”或“ETH/USDC”）中 delta one 衍生品（交割和永续）和现货的所有可能组合。
- **部分成交**和多个订单可以作为单笔交易的一部分。
- 交易对手方**不是**任由用户选择的。任何人都可以参与所有Spread的订单簿，有效地与更广泛的市场进行交易。
- 整个过程保持匿名，所有订单和交易均在**匿名**的基础上进行。
- 用户可以灵活地在订单簿的买卖双方下多个订单，从而实现阶梯式配置。

## 全面的 API 工作流程 [spread-trading-comprehensive-api-workflow]

<aside class="notice">
有关订单和交易的通知将由 *Taker* 和 *Maker* 通过 WebSocket 通知渠道接收。
</aside>

当用户的订单被另一个订单执行时，用户将承担*Maker*的角色。当用户提交的订单与订单簿中的现有订单相匹配时，他们就会成为 *Taker*

### 获取可用Spreads [spread-trading-comprehensive-api-workflow-obtaining-available-spreads]

要检索在 OKX 上交易的所有可用Spreads，您应该向 `GET /api/v5/sprd/spreads` 发出请求

### 检索您的订单[spread-trading-comprehensive-api-workflow-retrieving-your-orders]

要在 OKX 上检索您的订单，您应该向 `GET /api/v5/sprd/order` 发出请求。

### 检索您的交易[spread-trading-comprehensive-api-workflow-retrieving-your-trades]

要检索您在 OKX 上的交易，您应该向 `GET /api/v5/sprd/trades` 发出请求。

### 提交订单 [spread-trading-comprehensive-api-workflow-submitting-an-order]

要向 某个Spread 的订单簿提交订单，您应该请求
`POST /api/v5/sprd/order` 。

### Spread状态 [spread-trading-comprehensive-api-workflow-spread-states]

Spread 的生命周期中存在三种不同的状态：`live`，`suspend`，和 `expired`:

1. `live`: 在 Nitro Spread 上活跃交易的Spreads
2. `suspend`：其中至少一条腿被暂停，另一条在 OKX 订单簿交易所处于活跃或暂停状态的价差；或标的工具仍在 OKX 订单簿交易所中存在但已从 Nitro Spread 中移除的Spread
3. `expired`：至少一条腿在 OKX 订单簿交易所到期的Spread

给定每条腿的状态以及 Nitro Spreads 上的Spread状态（除了在 Nitro Spread上退市的情况），所有可能Spread状态的情况请参考下表：

| 交易产品A | 交易产品B | Spread状态 |
| --- | --- | --- |
| Live | Live | Live |
| Suspend | Live | Suspend |
| Live | Suspend | Suspend |
| Suspend | Suspend | Suspend |
| Expired | Live | Expired |
| Live | Expired | Expired |
| Suspend | Expired | Expired |
| Expired | Suspend | Expired |
| Expired | Expired | Expired |

### 交易生命周期 [spread-trading-comprehensive-api-workflow-trade-lifecycle]

为了进行交易，需要在价差撮合交易中匹配两个订单。
通过订阅 `sprd-orders`WebSocket 通道，您可以获得有关订单状态的信息并确定它是否已达到最终状态。通道中的`state`值表示订单的当前状态。

1. 如果状态为`live` 或 `partially_filled`，则意味着订单仍有未达最终状态（`filled`或`canceled`）数量，创建者或其他用户仍可能可以对其执行操作。
2. 另一方面，如果状态为`canceled`或`filled`，创建者或任何其他用户将无法对此订单执行任何操作。

请密切跟踪以下属性：`sz`（数量）、`pendingFillSz`（待完成数量）、`canceledSz`（被取消数量）和 `accFillSz`（累积完成数量）。这些属性提供了有关订单状态和进展的重要信息。

### 用户的订单状态 [spread-trading-comprehensive-api-workflow-order-state]

通过订阅 `sprd-orders`WebSocket 频道，用户可以跟踪他们的订单状态。

1. 提交订单后，无论是 *Maker* 还是 *Taker*，用户都会通过订单 WebSocket 频道道收到订单更新消息。该消息将指示订单的`state` == `live`。
2. 订单成交和结算是异步的。当订单已成交但还没结算，用户将收到`pendingSettleSz`>0，`fillSz` == ""的订单更新消息
3. 如果订单已部分成交且仍有待处理数量，用户将收到`state` == `partially_filled` 的订单更新消息
4. 如果订单完全成交，用户将收到`state` == `filled`的订单更新消息
5. 如果订单未完全消耗，但已达到最终状态，用户将收到`state` == `canceled`的订单更新消息。
6. 如果订单的某个部分被拒绝，用户会收到更新的订单更新，其中包含更新的 `canceledSz` 和 `pendingFillSz`，以及与错误对应的`code`和`msg`。

### 用户的交易状态 [spread-trading-comprehensive-api-workflow-trade-state]

通过订阅 `sprd-trades`WebSocket 频道，用户可以跟踪他们的交易状态。
1. 一笔已执行的交易在OKX上进行清算结算后，即为最终交易。
2. 对于成功清算的交易，用户会收到一条 WebSocket 消息，其中的`state`表示`filled`。
3. 在交易清算不成功的情况下，用户会收到一条交易更新消息，`state`反映为`rejected`。
4. 如果交易`state`为`rejected`，交易更新消息还将包含错误代码`code`和解释拒绝原因的相应错误消息 `msg`。

### 所有交易 [spread-trading-comprehensive-api-workflow-all-trades]

所有用户都能够接收通过 OKX Nitro Spread 产品发生的所有交易的更新。
请务必注意，OKX Nitro Spreads 不会披露有关交易双方及交易方向（买入或卖出）的信息。

1. 用户可以订阅`sprd-public-trades`频道来获取所有已成功结算的交易。
