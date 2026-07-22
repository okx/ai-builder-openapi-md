---
site: okex
lang: zh
name: Stable Rewards
type: guide
---

## Stable Rewards [financial-product-stable-rewards]

OKX Stable Rewards 自动为持有合格稳定币（如 `USDG`）的用户每日发放奖励，启用后无需任何操作即可持续赚取收益。<br>
订阅时从资金账户扣款；收益及赎回默认结算至交易账户。

> **下线说明**
>
> `POST /api/v5/finance/stable-rewards/quote`、`POST /api/v5/finance/stable-rewards/trade` 及 `GET /api/v5/finance/stable-rewards/subscribe-redeem-history` 接口已停用。如需交易 USDG 等稳定币，请使用标准[订单簿交易 API](/docs-v5/zh/#order-book-trading)。
