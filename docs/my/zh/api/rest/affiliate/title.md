---
site: my
lang: zh
name: 节点
type: guide
---

# 节点 [affiliate]

节点计划在欧洲支持两种激励模式，具体取决于司法辖区是否适用：基于佣金的模式（CPS，按销售付费）和基于交易量的模式（TVB）。

在 CPS 模式下，奖励根据来自适用司法辖区的被邀请人所产生的符合条件的交易手续费计算。在 TVB 模式下，奖励根据来自适用司法辖区的符合条件的交易量计算。TVB 模式不适用于西班牙、荷兰或马耳他。

在使用 Affiliate API 前，节点应确认其账户适用的激励模式。CPS 和 TVB 模式下的数据独立计算，不应合并统计。

## 各激励模式的字段范围 [affiliate-field-coverage-by-incentive-model]

Affiliate API 返回的字段分为两类：

- 模式专属字段仅表示在一种模式下累计的奖励。
  commission、totalCommission、commission24h、totalFee 和 accFee 在 CPS 模式下返回。
  tradingVolBonus、validVol、eligibleVol、bonusRate 和 multiplier 在 TVB 模式下返回。

- 共享字段描述被邀请人的活动——被邀请人人数、入金、交易人数和交易量——并按照所调用接口的定义反映这些活动。

由于共享字段可能描述同一批被邀请人，因此不得将 CPS 接口和 TVB 接口返回的值相加。每个接口都应视为相应激励模式下节点账户的独立完整视图。

CPS 接口使用 `/api/v5/affiliate/…` 路径；TVB 接口使用 `/api/v5/affiliate/tvb/…` 前缀。

| API 名称 | 路径 | 激励模式 |
| --- | --- | --- |
| 获取 CPS 节点业绩概览 | `/api/v5/affiliate/performance/summary` | CPS |
| 获取 CPS 被邀请人返佣信息 | `/api/v5/affiliate/invitee/detail` | CPS |
| 获取 CPS 直客列表 | `/api/v5/affiliate/invitee/list` | CPS |
| 获取 CPS 邀请链接列表 | `/api/v5/affiliate/link/list` | CPS |
| 获取 TVB 业绩汇总 | `/api/v5/affiliate/tvb/performance/summary` | TVB |
| 获取 TVB 分档返佣明细 | `/api/v5/affiliate/tvb/tier-breakdown` | TVB |
| 获取 TVB 被邀请人列表 | `/api/v5/affiliate/tvb/invitee/list` | TVB |
| 获取 TVB 被邀请人详情 | `/api/v5/affiliate/tvb/invitee/detail` | TVB |
| 获取 TVB 邀请链接列表 | `/api/v5/affiliate/tvb/link/list` | TVB |

## REST API [affiliate-rest-api]
