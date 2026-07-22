---
site: okex
lang: zh
name: 常见错误码
type: guide
---

# 常见错误码 [error-codes]

## 1. 响应格式 [error-codes-1-response-format]

所有异常统一返回以下 JSON 结构：

```json
{
  "code": 100015,
  "msg": "calldata 无效或字段不合法"
}
```

### 1.1 通用 [error-codes-1-response-format-1-1-common]

所有 OpenAPI v5 接口都可能产生。 更多通用错误码详见：[OKX 公共错误码文档](https://www.okx.com/docs-v5/zh/#error-code-rest-api-public)

| Code | Meaning |
| --- | --- |
| 10000 | User not logged in |
| 10001 | Parameter validation failed |
| 10002 | Authentication failed |
| 206004 | Request oddsType does not match the account |

### 1.2 事件/市场类 [error-codes-1-response-format-1-2-event-market]

| Code | Meaning |
| --- | --- |
| 201001 | Event does not exist |
| 201002 | Market does not exist |

适用接口：

- GET /api/v5/predictions/events

- GET /api/v5/predictions/events/{eventId}

- GET /api/v5/predictions/events/{eventId}/markets

- GET /api/v5/predictions/markets/{marketId}

- GET /api/v5/predictions/events/search

## 2. 下单 / 写操作类 [error-codes-2-order-write-operations]

涵盖：下单、撤单、全部撤单、心跳、Split、Merge、Redeem。

| Code | Meaning |
| --- | --- |
| 100001 | Market is not tradable |
| 100002 | Insufficient balance |
| 100006 | Account is frozen |
| 100010 | Insufficient token balance |
| 100011 | Order does not exist |
| 100012 | Order status does not allow cancellation |
| 100013 | Request address does not match the user address |
| 100015 | Invalid calldata or malformed fields |
| 100016 | Nonce already used |
| 100017 | Nonce expired |
| 100018 | Order amount is below the minimum notional |
| 100101 | TradeZone SDK signature exception |
| 120007 | User does not exist |
| 120022 | Account is in escape (exit) process |
| 201002 | Market does not exist |
| 213003 | Signature verified but TradeZone submission failed |

适用接口：

- `POST /api/v5/predictions/orders` 下单

- `POST /api/v5/predictions/orders/cancel` 撤销单订单

- `POST /api/v5/predictions/orders/cancel-all` 撤销全部 / 指定市场订单

- `POST /api/v5/predictions/heartbeat` 心跳

- `POST /api/v5/predictions/positions/split` Split

- `POST /api/v5/predictions/positions/merge` Merge

- `POST /api/v5/predictions/positions/redeem` Redeem

## 3. 查询订单 / 仓位类 [error-codes-3-order-position-queries]

涵盖：查询单订单、订单列表、成交历史、仓位查询。

| Code | Meaning |
| --- | --- |
| 100011 | Order does not exist |
| 400 | Path parameter parsing failed |

适用接口：

- `GET /api/v5/predictions/orders/{orderId}` 查询单订单

- `GET /api/v5/predictions/orders` 查询订单列表

- `GET /api/v5/predictions/trades` 查询成交记录

- `GET /api/v5/predictions/positions` 查询仓位
