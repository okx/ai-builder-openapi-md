---
site: hongkong
lang: zh
name: SDK API 参考
type: guide
---

# SDK API 参考 [sdk-api-reference]

`okx-outcomes-sdk` Rust crate 公开的每一个公共方法、请求体、响应结构、错误变体和 WebSocket 频道的完整参考。README 是快速入门；本文档是详细版本。

通篇约定：

- 所有 Rust 类型都是从 `okx_outcomes_sdk::*` 重新公开导出的 `pub` 类型（每个章节顶部给出具体模块路径）。
- 每一个 REST 调用的"认证"指的是通过 `ApiCredentials` 用 HMAC-SHA256 在本地签名后写入的 OKX REST `OK-ACCESS-*` 请求头。SDK 在本地签名；密钥永远不会离开进程。
- "写"操作（下单 / 撤单 / 拆分 / 合并 / 赎回 / 心跳）还额外要求对类型化的 action 进行 EIP-712 ECDSA 签名。参见下面的 **签名** 章节。
- 所有十进制数值（价格、数量、余额、盈亏）都以十进制**字符串**形式交换，避免浮点精度损失。
- 所有时间戳都是 Unix 毫秒。
- 通信中的字段名使用 camelCase；Rust struct 字段使用 snake_case，通过 `serde(rename_all = "camelCase")` 转换。

## 1. 安装 [sdk-api-reference-1-installation]

在 `Cargo.toml` 中通过 Git 添加依赖：

> Cargo.toml 依赖配置

```toml
[dependencies]
okx-outcomes-sdk = { git = "https://github.com/okx/outcomes-sdk.git", features = ["signing", "websocket"] }
```

## 2. 客户端构造 [sdk-api-reference-2-client-construction]

模块： `okx_outcomes_sdk::{OutcomesSdkClient, ApiCredentials}`。

```rust
pub struct ApiCredentials {
    pub api_key:    String, // OK-ACCESS-KEY 请求头的值
    pub secret_key: String, // HMAC-SHA256 签名密钥;永不传输
    pub passphrase: String, // OK-ACCESS-PASSPHRASE 请求头的值
}

impl OutcomesSdkClient {
    pub fn with_credentials(creds: ApiCredentials) -> Self;
    pub fn with_credentials_and_url(creds: ApiCredentials, base_url: impl Into<String>) -> Self;
}
```

Base URL 解析顺序：传给 `with_credentials_and_url` 的显式参数 > `PREDICTIONS_API_BASE` 环境变量 > `https://www.okx.com`。Endpoint 常量是完整的绝对路径（`/api/v5/predictions/...`、`/api/v5/market/...`），与 base URL 拼接，因此一个主机配置同时覆盖预测与市场数据两类调用。

## 3. 错误 [sdk-api-reference-3-errors]

模块： `okx_outcomes_sdk::SdkError`。

每一个可失败的调用都返回 `Result<T, SdkError>`。该枚举有七个变体：

```rust
pub enum SdkError {
    /// 网络故障:连接被拒绝、DNS、超时、TLS 握手失败。
    /// 传输层;通常重试是安全的。
    Http(reqwest::Error),

    /// 服务端在响应信封中返回了非零业务错误码。
    /// `code` 是上游 OKX 的业务码;根据它决定重试 / 退避 / 中止。
    Api { code: i64, message: String },

    /// 响应体无法按预期 schema 反序列化。
    /// 通常意味着 SDK 与服务端版本不匹配。
    Deserialize(serde_json::Error),

    /// WS 连接、发送、登录或关闭失败。
    /// 包括登录被拒(`60xxx` 错误码)和登录过程中的超时。
    WebSocket { message: String },

    /// 保留给绕过公共构造函数、最终没有附带凭据的调用方。
    /// 通过 `with_credentials` / `with_credentials_and_url` 构造的客户端不会
    /// 返回此变体——它们始终携带凭据。
    NotAuthenticated { hint: String },

    /// URL 不合法、状态缺失或意料之外的内部前置条件。
    /// 几乎总是调用方的编程错误。
    Internal { message: String },

    /// 序列化请求体或 WS 载荷失败。
    /// 实际很少见(仅在出现非有限浮点数等情况下才触发)。
    Serialization { message: String },
}
```

各变体通过 `thiserror` 实现了 `Display`，因此 `format!("{e}")` 会生成可读的一行日志。

## 4. 事件与市场 [sdk-api-reference-4-events-and-markets]

模块： `okx_outcomes_sdk::models::event::*`。API 实现位于 `okx_outcomes_sdk::api::events`。

### 4.1 获取事件列表 `get_events` [sdk-api-reference-4-events-and-markets-4-1-get_events]

获取预测市场事件的分页列表。

- **端点：** `GET /api/v5/predictions/events`
- **认证：** 必需

```rust
pub async fn get_events(
    &self,
    status:    Option<&str>,   // "active"(默认)| "resolved"
    tag:       Option<&str>,   // 体育标签 ID
    league_id: Option<&str>,   // 体育联赛 ID
    sort:      Option<&str>,   // "volume" | "volume_24h"(默认) | "ending_soon" | "newest"
    cursor:    Option<&str>,   // 上一次 `EventsResponse.pagination.next_cursor` 返回的分页游标
    page_size: Option<i32>,    // 每页条目数,最大 50(默认 10)
) -> Result<EventsResponse, SdkError>;

pub struct EventsResponse {
    events:     Vec<EventObject>,
    pagination: Pagination,    // 见通用类型
}

pub struct EventObject {
    id:                       String,         // 全局唯一事件 ID
    event_id:                 String,         // 事件 ID
    neg_risk:                 bool,           // 互斥(negRisk)事件
    status:                   EventStatus,    // Active / Paused / Resolved / Unknown
    event_title:              String,         // 展示标题
    description:              String,         // 长描述
    event_icon:               Option<String>, // 图标 URL
    volume:                   String,         // 所有市场的总交易量
    start_time:               Option<i64>,    // 开始交易时间(ms)
    end_time:                 Option<i64>,    // 结束交易时间(ms)
    created_at:               i64,            // 创建时间戳(ms)
    total_markets_count:      i32,            // 该事件下的市场数量
    final_outcomes_market_id: Option<String>, // 结算后的胜出市场 ID
    markets:                  Vec<MarketObject>,      // 列表端点最多返回 2 个市场;
                                                      // 需要完整列表请调用 `get_event_markets`
}
```

### 4.2 搜索事件 `search` [sdk-api-reference-4-events-and-markets-4-2-search]

通过关键字搜索事件和市场。

- **端点：** `GET /api/v5/predictions/events/search`
- **认证：** 必需

```rust
pub async fn search(
    &self,
    keyword:   &str,           // 自由文本查询(必需)
    cursor:    Option<&str>,   // 分页游标
    page_size: Option<i32>,    // 默认 10
) -> Result<EventsResponse, SdkError>;
```

响应： `EventsResponse`（与 `get_events` 形状相同）。

### 4.3 获取单个事件 `get_event` [sdk-api-reference-4-events-and-markets-4-3-get_event]

获取单个事件，并内联其完整市场列表。

- **端点：** `GET /api/v5/predictions/events/{eventId}`
- **认证：** 必需

```rust
pub async fn get_event(&self, event_id: &str) -> Result<EventObject, SdkError>;
```

事件 ID 不存在时返回 `Api { code: 40404, ... }`。

### 4.4 获取事件下市场 `get_event_markets` [sdk-api-reference-4-events-and-markets-4-4-get_event_markets]

获取事件的全部市场（无分页、无列表上限）。

- **端点：** `GET /api/v5/predictions/events/{eventId}/markets`
- **认证：** 必需

```rust
pub async fn get_event_markets(&self, event_id: &str) -> Result<MarketsResponse, SdkError>;

pub struct MarketsResponse {
    markets: Vec<MarketObject>,
}

pub struct MarketObject {
    id:                        String,         // 全局唯一市场 ID
    market_id:                 String,         // 市场 ID
    neg_risk:                  bool,           // negRisk 市场标志
    status:                    MarketStatus,   // Active / Paused / Settling / Resolved / Unknown
    settle_stage:              i32,            // 0 = 未开始,5 = 已结算
    question:                  String,         // 完整市场问题
    short_question:            Option<String>, // 简短问题
    description:               String,         // 长描述
    market_icon:               Option<String>, // 图标 URL
    start_time:                String,         // 交易开始时间(ms,字符串)
    end_time:                  String,         // 交易结束时间(ms,字符串)
    resolve_start_at:          String,         // 结算窗口开始时间(ms,字符串)
    resolve_at:                String,         // 结算时间(ms,字符串)
    best_bid:                  Option<String>, // [0, 1] 区间内的十进制;没有买单时为 None
    best_ask:                  Option<String>, // [0, 1] 区间内的十进制;没有卖单时为 None
    last_trade_price:          Option<String>, // 第一笔成交前为 None
    volume:                    String,         // 市场交易量
    probability:               Option<String>, // [0, 1] 区间内的 YES 结果概率
    resolution_sources:        Vec<String>,    // 用于裁定的来源 URL
    yes_outcome:               OutcomeObject,
    no_outcome:                OutcomeObject,
}

pub struct OutcomeObject {
    token_id:     Option<String>, // 条件代币地址;部署前为 None
    asset_id:     Option<String>, // 资产 ID;在下单 / 行情中作为 `inst_id` 使用
    name:         String,         // "Yes" 或 "No"
    price:        String,         // [0, 1] 区间内的十进制
    final_result: Option<bool>,   // Some(true) = 胜出, Some(false) = 落败, None = 未结算
}
```

### 4.5 获取单个市场 `get_market` [sdk-api-reference-4-events-and-markets-4-5-get_market]

获取单个市场。

- **端点：** `GET /api/v5/predictions/markets/{marketId}`
- **认证：** 必需

```rust
pub async fn get_market(&self, market_id: &str) -> Result<MarketObject, SdkError>;
```

## 5. 账户：余额 [sdk-api-reference-5-account-balance]

模块： `okx_outcomes_sdk::models::balance::*`。API 位于 `okx_outcomes_sdk::api::balance`。

### 5.1 查询余额 `get_balance` [sdk-api-reference-5-account-balance-5-1-get_balance]

返回已认证用户按赔率类型分组的可用余额。

- **端点：** `GET /api/v5/predictions/balance`
- **认证：** 必需

```rust
pub async fn get_balance(&self) -> Result<BalanceResponse, SdkError>;

pub type BalanceResponse = Vec<BalanceEntry>;

pub struct BalanceEntry {
    odds_type: OddsType, // Spots / Points / Unknown(spots = 实盘,points = 积分盘)
    balance:   String, // 总余额(单位由 odds_type 决定)
    available: String, // 可用余额(总余额 - 被未成交订单冻结的金额)
}
```

## 6. 账户：订单 [sdk-api-reference-6-account-orders]

模块： `okx_outcomes_sdk::models::order::*`。API 位于 `okx_outcomes_sdk::api::orders`。

### 6.1 下单 `place_order` [sdk-api-reference-6-account-orders-6-1-place_order]

提交一个已签名的限价（或触发）订单。

- **端点：** `POST /api/v5/predictions/orders`
- **认证：** 必需（REST 凭据）+ EIP-712 签名

```rust
pub async fn place_order(&self, req: &PlaceOrderRequest) -> Result<TxHashResponse, SdkError>;

struct PlaceOrderRequest {
    action:    PlaceOrderAction,
    nonce:     i64,                // 毫秒时间戳,防重放
    signature: SignatureWrapper,   // { Ecdsa: { r, s, v } }
}

struct PlaceOrderAction {
    action_type: String,           // 始终为 "placeOrder"
    grouping:    String,           // 始终为 "na"
    orders:      Vec<OrderItem>,
}

struct OrderItem {
    asset_id:        String,           // 结果 assetId
    side:            SigningOrderSide, // Buy / Sell(下单侧小写 wire,字节用于 EIP-712 哈希)
    market_type:     String,           // 始终为 "prediction"
    client_order_id: Option<String>,   // 34 字符客户端订单 ID;见签名 > 客户端订单 ID
    price:           String,           // [0, 1] 区间内的十进制
    reduce_only:     bool,
    size:            String,           // 十进制
    size_type:       SizeType,         // Base(默认,wire 上省略)/ Quote
    order_type:      OrderTypeSpec,    // { limit: { tif } }
}

struct OrderTypeSpec   { limit: LimitOrderType }
struct LimitOrderType  { tif: LimitTif /* Gtc | Gtd { expires_after } | Ioc | Fok | Alo */ }
```

响应： `TxHashResponse { tx_hash: String }`。

构造类型化的 `signing::types::OrderRequest`，用 `signing::sign_to_wrapper` 签名，再通过 `OrderItem::from(&OrderRequest)` 推导出通信侧的 `OrderItem`，以保证签名字节和 JSON 请求体不会发生漂移。参见**签名**。

### 6.2 撤单 `cancel_order` [sdk-api-reference-6-account-orders-6-2-cancel_order]

撤销一个活跃订单（按服务端 ID 或客户端订单 ID）。

- **端点：** `POST /api/v5/predictions/orders/cancel`
- **认证：** 必需 + EIP-712 签名

```rust
pub async fn cancel_order(&self, req: &CancelOrderRequest) -> Result<TxHashResponse, SdkError>;

struct CancelOrderRequest {
    action:    CancelOrderAction,
    nonce:     i64,
    signature: SignatureWrapper,
}

struct CancelOrderAction {
    action_type: String,           // 始终为 "cancel"
    cancels:     Vec<CancelItem>,
}

struct CancelItem {
    asset_id:     String,
    market_type:  String,           // "prediction"
    // 二者必选其一:
    by: CancelBy,                   // 扁平化序列化为 { "oid": ... } 或 { "clientOrderId": ... }
}

enum CancelBy {
    Oid           { oid: String },          // 服务端分配,十进制字符串
    ClientOrderId { client_order_id: String }, // 34 字符客户端订单 ID,带 0x 前缀的十六进制
}
```

响应： `TxHashResponse { tx_hash: String }`。

### 6.3 全部撤单 `cancel_all` [sdk-api-reference-6-account-orders-6-3-cancel_all]

撤销所有活跃订单，或者撤销指定 asset ID 集合下的所有活跃订单。

- **端点：** `POST /api/v5/predictions/orders/cancel-all`
- **认证：** 必需 + EIP-712 签名

```rust
pub async fn cancel_all(&self, req: &CancelAllRequest) -> Result<TxHashResponse, SdkError>;

struct CancelAllRequest {
    action:        CancelAllAction,
    nonce:         i64,
    expires_after: i64,            // 过期时间戳(ms),必填
    signature:     SignatureWrapper,
}

struct CancelAllAction {
    action_type: String,            // 始终为 "cancelAll"
    asset_ids:   Vec<String>,       // 空 = 所有市场;非空 = 过滤
    market_type: String,            // "prediction"
}
```

响应： `TxHashResponse`。

### 6.4 心跳 `heartbeat` [sdk-api-reference-6-account-orders-6-4-heartbeat]

刷新用于保护活跃订单的断线自动撤单开关（dead-man's switch）。

- **端点：** `POST /api/v5/predictions/heartbeat`
- **认证：** 必需 + EIP-712 签名

```rust
pub async fn heartbeat(&self, req: &CancelAllRequest) -> Result<HeartbeatResponse, SdkError>;

struct HeartbeatResponse {
    server_timestamp: i64, // 服务端当前时间(ms)
    expire_at:        i64, // 本次心跳过期的时间(ms)
}
```

请求体使用同样的 `CancelAllRequest` 形状：签名好的载荷**就是**预授权的 cancel-all——心跳超时后服务端会代为执行它。将 `nonce` 设为 `now_ms`，`expires_after` 设为 `now_ms + 300_000`（5 分钟）。心跳调用频率应高于每 5 分钟一次。

### 6.5 查询单个订单 `get_order` [sdk-api-reference-6-account-orders-6-5-get_order]

通过服务端分配的 ID 查询单个订单。

- **端点：** `GET /api/v5/predictions/orders/{orderId}`
- **认证：** 必需

```rust
pub async fn get_order(&self, order_id: &str) -> Result<OrderRecord, SdkError>;

pub struct OrderRecord {
    id:              String,         // 服务端分配的订单 ID
    oid:             String,         // 订单 oid(与 `id` 不同)
    market_id:       String,
    token_id:        String,         // YES/NO 代币合约地址
    asset_id:        String,         // YES 或 NO 结果资产 ID
    client_order_id: Option<String>, // 下单时提供的客户端订单 ID(如果有)
    side:            OrderSide,        // Buy / Sell / Unknown
    order_type:      TimeInForce,      // Gtc / Gtd / Ioc / Fok / PostOnly / Unknown
    size_type:       OrderSizeType,    // Base / Quote / Unknown
    size:            String,         // 十进制
    price:           String,         // 十进制
    expiration:      Option<String>, // GTD 过期时间(ms,字符串);非 GTD 时为 None
    tx_hash:         String,         // 提交交易哈希
    status:          RestOrderStatus, // PendingPlace / Active / PendingCancel / Filled /
                                      // PartiallyFilled / Failed / Cancelled / Expired / Unknown
    filled_size:     String,          // 十进制
    filled_amount:   String,          // 十进制
    fail_reason:     Option<String>,  // 仅当 status == RestOrderStatus::Failed 时出现
    cancel_reason:   Option<String>,  // 服务端发起撤单时设置(心跳超时、市场结算等)
    odds_type:       OddsType,        // Spots / Points / Unknown
    created_at:      String,         // Unix ms(字符串)
    updated_at:      String,         // Unix ms(字符串)
}
```

### 6.6 查询订单列表 `list_orders` [sdk-api-reference-6-account-orders-6-6-list_orders]

列出已认证用户的订单。

- **端点：** `GET /api/v5/predictions/orders`
- **认证：** 必需

```rust
pub async fn list_orders(
    &self,
    market_id: Option<&str>,   // 按市场 ID 过滤
    status:    Option<&str>,   // "open"(待处理 + 活跃)| "closed"(成交 / 已撤 / 已过期 / 失败)
    cursor:    Option<&str>,   // 分页游标
    limit:     Option<i32>,    // 最大 50,默认 20
) -> Result<OrdersResponse, SdkError>;

// 共享分页响应外壳的类型别名。
pub type OrdersResponse = PagedListResponse<OrderRecord>;
// pub struct PagedListResponse<T> { list: Vec<T>, next_cursor: Option<String>, has_next: bool }
```

## 7. 账户：持仓 [sdk-api-reference-7-account-positions]

模块： `okx_outcomes_sdk::models::position::*`。API 位于 `okx_outcomes_sdk::api::positions`。

### 7.1 查询持仓 `get_positions` [sdk-api-reference-7-account-positions-7-1-get_positions]

查询已认证用户的持仓。

- **端点：** `GET /api/v5/predictions/positions`
- **认证：** 必需

```rust
pub async fn get_positions(
    &self,
    status:    Option<&str>,   // "open" | "closed";不传则全部
    market_id: Option<&str>,
    cursor:    Option<&str>,   // 分页游标
    limit:     Option<i32>,    // 最大 100,默认 20
) -> Result<PositionsResponse, SdkError>;

pub type PositionsResponse = PagedListResponse<PositionRecord>;

pub struct PositionRecord {
    id:                         String,         // 标识符
    token_id:                   String,
    market_id:                  String,
    token_index:                String,         // "1" = YES, "2" = NO
    token_name:                 String,         // "Yes" 或 "No"
    size:                       String,         // 当前剩余数量
    available_size:             String,         // 可用数量(= size − 被卖单冻结的部分)
    value:                      String,         // cur_price * size
    avg_price:                  String,         // 加权平均建仓成本
    un_realized_pnl:            String,         // 未实现盈亏
    un_realized_pnl_percentage: String,
    title:                      String,         // 展示字符串
    icon:                       String,         // 展示字符串
    event_id:                   String,
    winning_token:              Option<String>, // 结算后的胜出代币 ID;结算前为 None
    position_status:            i32,            // 持仓状态码(完整枚举见 API 参考)
    cur_price:                  String,         // 当前代币价格
    realized_pnl:               String,         // 已实现盈亏
    realized_pnl_percentage:    String,
    odds_type:                  OddsType,       // Spots / Points / Unknown
                                                 //(已经线上验证:wire 值为 "points" 或 "spots",不是 "real")
}
```

## 8. 账户：成交 [sdk-api-reference-8-account-trades]

模块： `okx_outcomes_sdk::models::trade::*`。API 位于 `okx_outcomes_sdk::api::trades`。

### 8.1 查询成交 `get_trades` [sdk-api-reference-8-account-trades-8-1-get_trades]

查询已认证用户的成交历史。

- **端点：** `GET /api/v5/predictions/trades`
- **认证：** 必需

```rust
pub async fn get_trades(
    &self,
    market_id:  Option<&str>,   // 按市场 ID 过滤
    side:       Option<&str>,   // "BUY" | "SELL"
    start_time: Option<i64>,    // 开始时间(含,ms)
    end_time:   Option<i64>,    // 结束时间(不含,ms)
    cursor:     Option<&str>,   // 分页游标
    limit:      Option<i32>,    // 最大 100,默认 20
) -> Result<TradesResponse, SdkError>;

type TradesResponse = PagedListResponse<TradeRecord>;

struct TradeRecord {
    trade_id:   String,    // TAKER 行和链上分配前的 MAKER 行返回空串
    order_id:   String,
    market_id:  String,
    token_id:   String,
    side:       OrderSide, // Buy / Sell / Unknown
    size:       String,    // 成交代币数
    amount:     String,    // 成交金额
    price:      String,
    fee:        String,
    role:       Role,      // Maker / Taker / Unknown
    tx_hash:    String,
    created_at: String,      // Unix ms(字符串)
}
```

`trade_id` 在 TAKER 行，以及在链上 trade-id 分配机制之前的历史 MAKER 行中为 `None`。

## 9. 条件代币 [sdk-api-reference-9-conditional-tokens]

模块： `okx_outcomes_sdk::models::position::*`（与 positions 共用）。API 位于 `okx_outcomes_sdk::api::positions`。

三者均为需要 EIP-712 签名的写操作。每个请求体的外层形状一致： `{ action, nonce, signature }`，只是 `action` 不同。每个均返回 `TxHashResponse { tx_hash: String }`。

### 9.1 拆分 `split` [sdk-api-reference-9-conditional-tokens-9-1-split]

将一个市场的等额 xp 拆分为等量的 YES + NO 代币（`merge` 的逆操作）。

- **端点：** `POST /api/v5/predictions/positions/split`
- **认证：** 必需 + EIP-712 签名

```rust
pub async fn split(&self, req: &SplitRequest) -> Result<TxHashResponse, SdkError>;

struct SplitRequest { action: SplitAction, nonce: i64, signature: SignatureWrapper }
struct SplitAction {
    action_type: String, // "predictionSplit"
    market_id:   String,
    size:        String, // 最小单位数量
}
```

### 9.2 合并 `merge` [sdk-api-reference-9-conditional-tokens-9-2-merge]

将等量的 YES + NO 代币合并（回到 xp）。

- **端点：** `POST /api/v5/predictions/positions/merge`
- **认证：** 必需 + EIP-712 签名

```rust
pub async fn merge(&self, req: &MergeRequest) -> Result<TxHashResponse, SdkError>;

struct MergeRequest { action: MergeAction, nonce: i64, signature: SignatureWrapper }
struct MergeAction {
    action_type: String, // "predictionMerge"
    market_id:   String,
    size:        String,
}
```

### 9.3 赎回 `redeem` [sdk-api-reference-9-conditional-tokens-9-3-redeem]

在市场结算后，赎回调用方的全部胜出代币余额。没有 `size` 字段；服务端会赎回调用方持有的全部数量。

- **端点：** `POST /api/v5/predictions/positions/redeem`
- **认证：** 必需 + EIP-712 签名

```rust
pub async fn redeem(&self, req: &RedeemRequest) -> Result<TxHashResponse, SdkError>;

struct RedeemRequest { action: RedeemAction, nonce: i64, signature: SignatureWrapper }
struct RedeemAction {
    action_type: String, // "predictionRedeem"
    market_id:   String,
}
```

市场尚未裁定时返回 `Api { code: 51020, ... }`。

## 10. 市场数据 [sdk-api-reference-10-market-data]

模块： `okx_outcomes_sdk::models::price::*`。API 位于 `okx_outcomes_sdk::api::prices`。

这些调用访问 OKX 的市场数据 API `https://www.okx.com/api/v5/market/*` —— 与预测市场 API 同主机但路径前缀和响应信封都不同。市场数据信封把 `code` 包成 JSON 字符串，因此 `SdkError::Api { code }` 中的 `code` 是解析后的整数值。

### 10.1 获取行情 `get_ticker` [sdk-api-reference-10-market-data-10-1-get_ticker]

单个 instrument 的最新报价。`inst_id` 是市场的 `yes_outcome.asset_id`。

- **端点：** `GET /api/v5/market/ticker`
- **认证：** 必需

```rust
pub async fn get_ticker(&self, inst_id: &str) -> Result<Ticker, SdkError>;

pub struct Ticker {
    inst_type:  String,
    inst_id:    String,
    last:       String, // 最新成交价
    last_sz:    String, // 最新成交数量
    ask_px:     String, // 盘口最优卖价
    ask_sz:     String, // 盘口最优卖单数量
    bid_px:     String, // 盘口最优买价
    bid_sz:     String, // 盘口最优买单数量
    open24h:    String, // 24 小时开盘价
    high24h:    String, // 24 小时最高价
    low24h:     String, // 24 小时最低价
    vol24h:     String, // 24 小时成交量(基础币)
    vol_ccy24h: String, // 24 小时成交量(报价币)
    sod_utc0:   String, // UTC 0  开盘价
    sod_utc8:   String, // UTC+8 开盘价
    ts:         String, // 更新时间戳(Unix ms 十进制字符串)
}
```

服务端返回 1 元素数组；SDK 会拆包。当 inst ID 未知时返回 `Api { code: -1, message: "ticker not found" }`。

### 10.2 获取K线 `get_candles` [sdk-api-reference-10-market-data-10-2-get_candles]

K 线历史。

- **端点：** `GET /api/v5/market/candles`
- **认证：** 必需

```rust
pub async fn get_candles(
    &self,
    inst_id: &str,
    bar:     Option<&str>,   // "1m" / "5m" / "15m" / "30m" / "1H" / "4H" / "1D" / ... ;默认 "1m"
    after:   Option<&str>,   // 返回时间戳在该值**之前**的 K 线(ms)
    before:  Option<&str>,   // 返回时间戳在该值**之后**的 K 线(ms)
    limit:   Option<i32>,    // 最大 300,默认 100
) -> Result<Vec<Candle>, SdkError>;

pub struct Candle(pub Vec<String>);

impl Candle {
    pub fn ts(&self)        -> &str;   // 索引 0:开盘时间(Unix ms 字符串)
    pub fn open(&self)      -> &str;   // 索引 1:开盘价
    pub fn high(&self)      -> &str;   // 索引 2:最高价
    pub fn low(&self)       -> &str;   // 索引 3:最低价
    pub fn close(&self)     -> &str;   // 索引 4:收盘价
    pub fn vol(&self)       -> &str;   // 索引 5:成交量(合约张数)
    // 索引 6:以计价币计的成交量(无 helper)
    // 索引 7:以报价币计的成交量(无 helper)
    pub fn confirmed(&self) -> bool;   // 索引 8:为 "1" 时返回 true(K 线已收盘)
}
```

### 10.3 获取深度 `get_pm_books` [sdk-api-reference-10-market-data-10-3-get_pm_books]

预测市场盘口深度快照。

- **端点：** `GET /api/v5/market/pm-books`
- **认证：** 必需
- **限频：** 40 次 / 2 秒

```rust
pub async fn get_pm_books(
    &self,
    inst_id: &str,             // YES 结果资产 ID
    sz:      Option<i32>,      // 单边深度档位数;最大 400(双边合计最多 800)。
                               // 不传时默认 1(仅 BBO)。
) -> Result<PmBookDepth, SdkError>;

pub struct PmBookDepth {
    asks:   Vec<Vec<String>>,  // 卖盘档位,按价格升序。每条为 [price, size, order_count]。
    bids:   Vec<Vec<String>>,  // 买盘档位,按价格降序。每条为 [price, size, order_count]。
    ts:     String,            // 快照时间戳(Unix ms 十进制字符串)
    seq_id: i64,               // 盘口版本序列;对多数调用方不透明,
                               // 仅为了与 API 响应对齐而暴露
}
```

服务端返回 1 元素的 `data` 数组；SDK 会拆包。响应为空时返回 `Api { code: -1, message: "pm-books snapshot not found" }`。

## 11. WebSocket [sdk-api-reference-11-websocket]

模块： `okx_outcomes_sdk::ws::*`。需要 `websocket` Cargo feature。

**连接模型**

Open API 对公共频道和私有频道使用同一个端点： `wss://<host>/ws/v5/business`。公共频道可匿名使用。私有频道在 WS 握手之后需要一次性的 `op: "login"`。

主机：

```rust
pub mod ws::endpoints {
    pub const DEFAULT_WS_HOST: &str = "wss://ws.okx.com:8443";
    pub const EU_WS_HOST:      &str = "wss://wseea.okx.com";
    pub const US_WS_HOST:      &str = "wss://wsus.okx.com";
    pub const BUSINESS_PATH:   &str = "/ws/v5/business";
}
```

`PredictionsWsClient` 默认使用 `DEFAULT_WS_HOST`；可通过 `PredictionsWsClient::with_host(...)` 或 `PREDICTIONS_WS_HOST` 环境变量覆盖。

生命周期与韧性：

- 25 秒心跳保活（OKX 要求 < 30 秒）。
- 自动重连，指数退避（3 秒 -> 6 秒 -> 12 秒 -> 最高 30 秒封顶）。
- 重连时，若已存储凭据，客户端会重放登录，并重新订阅断线时仍处于活跃状态的每一个频道。
- 每次状态切换都会触发 `connection_state_callback("public" | "private", connected: bool)`。

**公共 API**

```rust
pub struct PredictionsWsClient { /* ... */ }

impl PredictionsWsClient {
    pub fn new() -> Self;
    pub fn with_host(host: &str) -> Self;

    pub async fn connect(&self, path: &str) -> Result<(), SdkError>;
    pub async fn login(&self, creds: &ApiCredentials) -> Result<(), SdkError>;
    pub async fn subscribe(&self, channel: &str, params: Vec<HashMap<String, String>>) -> Result<(), SdkError>;
    pub async fn unsubscribe(&self, channel: &str, params: Vec<HashMap<String, String>>) -> Result<(), SdkError>;
    pub async fn disconnect(&self);

    pub fn set_on_data(&self, callback: WsDataCallback);
    pub fn set_on_connection_state(&self, callback: WsConnectionStateCallback);
}

pub type WsDataCallback             = Arc<dyn Fn(&WsMessage) + Send + Sync>;
pub type WsConnectionStateCallback  = Arc<dyn Fn(&str, bool) + Send + Sync>;
```

`subscribe` 是幂等的：使用相同的 `(channel, params)` 对调用两次不会重复订阅，也不会在重放列表中产生重复条目。

登录签名（由 `login` 内部处理）：SDK 计算 `sign = Base64(HMAC-SHA256(secret_key, timestamp + "GET" + "/users/self/verify"))`，并发送：

```json
{"op": "login", "args": [{"apiKey": "...", "passphrase": "...", "timestamp": "...", "sign": "..."}]}
```

`login` 返回的 future 只有在服务端响应后才会 resolve：

- `{"event":"login","code":"0"}` -> `Ok(())`。
- `{"event":"error","code":"600xx",...}` -> `Err(SdkError::WebSocket { message: "Login rejected: [60xxx] ..." })`。
- 30 秒内无响应 -> `Err(SdkError::WebSocket { message: "Login timed out (30s)" })`。

**消息分发**

每个传入的 JSON 帧都会被解析一次成 `WsMessage` 枚举，然后交给 `on_data`。消费方永远看不到原始 JSON。

```rust
pub enum WsMessage {
    Event {
        event:   String,
        channel: Option<String>,
        inst_id: Option<String>,
        msg:     Option<String>,
    },
    Prices(Vec<WsPriceTick>),
    Books { data: Vec<WsPmBookData>, action: String }, // action = "snapshot" | "update"
    Trades(Vec<WsPmTrade>),
    Tickers(Vec<WsPmTicker>),
    Game(Vec<WsGameStatus>),
    EventStatus(Vec<WsEventStatus>),
    Candle(Vec<Candle>),                   // 类型化包装,9 列 OHLCV 数组
    Orders(Vec<WsOrder>),
    Positions(Vec<WsPosition>),
    UserTrades(Vec<WsUserTrade>),
    Balance(Vec<WsBalance>),
    Pnl(Vec<WsPnl>),
    Unknown { channel: String, raw: serde_json::Value },
}
```

`WsMessage::Event` 承载 `event:"subscribe"|"unsubscribe"|"login"|"error"` 等确认消息，与任何数据频道无关。

**公共频道**

### 11.1 `prediction-market-prices` [sdk-api-reference-11-websocket-11-1-prediction-market-prices]

按市场的价格 tick。

- **订阅参数：** `[{"instId": "<asset_id>"}]`（每个市场一条）。
- **消息变体：** `WsMessage::Prices(Vec<WsPriceTick>)`。

```rust
struct WsPriceTick {
    yes_asset_id:     String,
    last_trade_price: Option<String>, // 首次成交前为 None
    best_bid:         Option<String>, // 没有买单时为 None
    best_ask:         Option<String>, // 没有卖单时为 None
    timestamp:        String,         // Unix ms 十进制字符串
    probability:      String,         // 基点 * 100,例如 "6500" = 65.00%
    market_volume:    String,
    event_volume:     String,
    event_id:         String,
}
```

### 11.2 `pm-books` [sdk-api-reference-11-websocket-11-2-pm-books]

盘口快照和增量更新。

- **订阅参数：** `[{"instId": "<asset_id>"}]`。
- **消息变体：** `WsMessage::Books { data, action }`，其中 `action` 为 `"snapshot"`（订阅 / 重连时的全量快照）或 `"update"`（增量 delta）。

```rust
struct WsPmBookData {
    asks:        Vec<Vec<String>>, // [[price, size, ...], ...]
    bids:        Vec<Vec<String>>, // [[price, size, ...], ...]
    ts:          String,
    checksum:    Option<i64>,      // 规范化盘口的 CRC32 完整性校验
    seq_id:      Option<i64>,      // 单调序列;出现间隙 = 丢失,应重置
    prev_seq_id: Option<i64>,      // 首个快照为 -1
}
```

当 `prev_seq_id` 与上一帧的 `seq_id` 不匹配时，丢弃本地盘口，等待下一个 `snapshot`。

### 11.3 `pm-trades` [sdk-api-reference-11-websocket-11-3-pm-trades]

公开成交回报。

- **订阅参数：** `[{"instId": "<asset_id>"}]`。
- **消息变体：** `WsMessage::Trades(Vec<WsPmTrade>)`。

```rust
struct WsPmTrade {
    inst_id:  String,
    trade_id: Option<String>, // 单笔推送:Some;聚合推送:None
    f_id:     Option<String>, // 聚合推送:首个 trade id;单笔:None
    l_id:     Option<String>, // 聚合推送:最后一个 trade id;单笔:None
    px:       String,         // 价格
    sz:       String,         // 数量
    side:     String,         // "buy" / "sell"(taker 方)
    ts:       String,
}
```

通过 `trade_id` 与 （`f_id`，`l_id`） 中哪一对为 `Some` 来区分单笔推送和窗口聚合推送。

### 11.4 `pm-tickers` [sdk-api-reference-11-websocket-11-4-pm-tickers]

OKX 风格的按 instrument ticker 推送。

- **订阅参数：** `[{"instId": "<asset_id>"}]`。
- **消息变体：** `WsMessage::Tickers(Vec<WsPmTicker>)`。

```rust
struct WsPmTicker {
    inst_type: String, inst_id: String,
    last:       String, last_sz: String,
    ask_px:     String, ask_sz:  String,
    bid_px:     String, bid_sz:  String,
    open24h:    String, high24h: String, low24h: String,
    vol24h:     String, vol_ccy24h: String,
    sod_utc0:   String, sod_utc8: String,
    ts:         String,
}
```

### 11.5 `pm-event-status` [sdk-api-reference-11-websocket-11-5-pm-event-status]

事件结算推送。

- **订阅参数：** `[{"instId": "event-<event_id>"}]`。SDK **不会**自动在 WS 路径上加 `event-` 前缀；请显式传入。
- **消息变体：** `WsMessage::EventStatus(Vec<WsEventStatus>)`。

```rust
struct WsEventStatus {
    event_id:       String,
    status:         String,  // 例如 "resolved"
    market_id:      String,  // 胜出市场 ID
    outcome_option: String,  // "yes" / "no" / "others" / 球队名 / "draw"
    timestamp:      String,
}
```

### 11.6 `pm-candle*` [sdk-api-reference-11-websocket-11-6-pm-candle]

K 线流。频道名编码了 bar： `pm-candle1m`、`pm-candle5m`、`pm-candle1H`、`pm-candle1D` 等等。

- **订阅参数：** `[{"instId": "<asset_id>"}]`。
- **消息变体：** `WsMessage::Candle(Vec<Candle>)`，每个 `Candle` 是 9 列 OHLCV 数组的类型化包装。访问方法： `ts()`， `open()`， `high()`， `low()`， `close()`， `vol()`， `vol_ccy()`， `vol_ccy_quote()`， `confirmed()`。

**私有频道（需要 `login`）**

订阅时传空参数（服务端将订阅范围限定到已登录账户）。

### 11.7 `pm-order` [sdk-api-reference-11-websocket-11-7-pm-order]

订单状态变更。

- **订阅参数：** `[]`。
- **消息变体：** `WsMessage::Orders(Vec<WsOrder>)`。

```rust
struct WsOrder {
    order_id:        String,
    market_id:       String,
    status:          OrderStatus,       // Active / Filled / PartiallyFilled / PlaceFailed /
                                        // CancelFailed / Cancelled / Expired / Unknown
    side:            OrderSide,         // Buy / Sell / Unknown
    // 以下字段都依赖 status —— 见 spec 的 status → 必备字段表。
    // 用 Option 建模,缺失键反序列化为 None。
    client_order_id: Option<String>,
    asset_id:        Option<String>,    // YES 或 NO 的 assetId
    direction:       Option<Direction>, // Yes / No / Unknown —— 该订单走的结果方向
    filled_size:     Option<String>,
    order_size:      Option<String>,    // serde alias = "size"
    avg_price:       Option<String>,
    amount:          Option<String>,    // BUY = 花费, SELL = 收到(xp)
    limit_price:     Option<String>,    // serde alias = "price"
    fail_message:    Option<String>,    // 仅 PLACE_FAILED / CANCEL_FAILED 出现
    odds_type:       Option<OddsType>,
    tx_hash:         Option<String>,    // serde rename = "txHash"
    trade_id:        Option<String>,
}
```

### 11.8 `pm-position` [sdk-api-reference-11-websocket-11-8-pm-position]

持仓更新。

- **订阅参数：** `[]`。
- **消息变体：** `WsMessage::Positions(Vec<WsPosition>)`。

该频道有两种 payload 变体；`WsPosition` 用单一扁平 struct 表示，
变体相关字段都放在 `Option` 里。基于 `status` 分支判断（可用
`PositionStatus::is_position_snapshot()` / `is_failed()`）以决定
哪些字段是有意义的。

```rust
struct WsPosition {
    // 两个变体共有
    market_id: String,
    status:    PositionStatus,    // Fill / FillFailed / Redeem / RedeemFailed /
                                  // Split / SplitFailed / Merge / MergeFailed /
                                  // Deposit / DepositFailed / Withdraw / WithdrawFailed / Unknown
    amount:    String,            // 变体 1: 持仓 `remain`(REDEEM 时为 "0")
                                  // 变体 2: split/merge/deposit/withdraw 数量
    odds_type: Option<OddsType>,

    // 变体 1(FILL / REDEEM / *_FAILED)—— 完整持仓快照
    id:                          Option<String>,
    token_id:                    Option<String>,
    asset_id:                    Option<String>,
    timestamp:                   Option<String>,
    un_realized_pnl:             Option<String>,
    un_realized_pnl_percentage:  Option<String>,
    value:                       Option<String>,
    avg_price:                   Option<String>,
    trade_id:                    Option<String>,

    // 变体 2(SPLIT / MERGE / DEPOSIT / WITHDRAW / *_FAILED)
    tx_hash: Option<String>,           // serde rename = "txHash"
    ext:     Option<WsPositionExt>,    // 仅 DEPOSIT 时填充
}

struct WsPositionExt {
    to_tx_hash: String,                // serde rename = "toTxHash"
}
```

### 11.9 `pm-user-trade` [sdk-api-reference-11-websocket-11-9-pm-user-trade]

用户自身的成交流。

- **订阅参数：** `[]`。
- **消息变体：** `WsMessage::UserTrades(Vec<WsUserTrade>)`。

```rust
struct WsUserTrade {
    order_id:        String,
    client_order_id: String,    // 容错默认空串;通常都有值
    market_id:       String,
    token_id:        String,
    asset_id:        String,    // yesAssetId 或 noAssetId
    side:            OrderSide, // Buy / Sell / Unknown
    size:            String,
    price:           String,
    txhash:          String,
    timestamp:       String,
    trade_id:        String,    // 交易 ID
}
```

### 11.10 `pm-balance` [sdk-api-reference-11-websocket-11-10-pm-balance]

余额变更。

- **订阅参数：** `[]`。
- **消息变体：** `WsMessage::Balance(Vec<WsBalance>)`。

```rust
struct WsBalance {
    wallet_address: String,
    available:      String,
    total:          String,
    frozen:         String,
    token_id:       String,                  // 链上 Point token id
    change_type:    BalanceChangeType,       // Place / Cancel / Fill / Split / Merge /
                                              // Redeem / Deposit / Withdraw / Unknown
    change_amount:  Option<String>,          // spec:可能为 null
    update_time:    String,
    odds_type:      Option<OddsType>,
}
```

### 11.11 `pm-pnl` [sdk-api-reference-11-websocket-11-11-pm-pnl]

浮动盈亏流 —— 推送**两种 payload**；用 serde `untagged` enum 建模，
根据存在的字段自动选择正确变体。

- **订阅参数：** `[]`。
- **消息变体：** `WsMessage::Pnl(Vec<WsPnl>)`。

```rust
enum WsPnl {
    Overview(WsPnlOverview),     // portfolioValue + 各周期汇总
    Timeseries(WsPnlTimeseries), // 包含 high/low/current 的图表点
}

struct WsPnlOverview {
    portfolio_value: String,                 // xp 余额 + 持仓市值
    periods:         Vec<WsPnlPeriodSummary>,
}

struct WsPnlPeriodSummary {
    period:      String, // "1D" / "1W" / "1M" / "6M" / "1Y"
    period_pnl:  String,
    pnl_percent: String,
}

struct WsPnlTimeseries {
    period:      String,         // "0"=1D / "1"=1W / "2"=1M / "3"=6M / "4"=1Y
    interval:    String,         // ms: 600000 / 1800000 / 3600000 / 86400000
    points:      Vec<WsPnlPoint>,
    current_pnl: String,
    high:        String,
    low:         String,
}
```

**WS 错误码**

WS 层错误通过 `WsMessage::Event { event: "error", msg, .. }` 暴露；登录相关的错误则通过 `login()` 返回的 `Err` 暴露。常见错误码：

| 错误码 | 含义 |
| --- | --- |
| `60004` | 登录时间戳无效（时钟漂移、已过期）。 |
| `60005` | API key 无效。 |
| `60006` | 时间戳已过期（30 秒窗口）。 |
| `60007` | 签名无效。 |
| `60009` | 登录失败（通用）。 |
| `60011` | 该私有频道需要登录。 |
| `60012` | 无效的 `op` 值。 |
| `60018` | 订阅失败（频道名或参数错误）。 |

## 12. 签名 [sdk-api-reference-12-signing]

模块： `okx_outcomes_sdk::signing::*`。需要 `signing` Cargo feature。

任何写操作的完整流水线：构造一个类型化的 `Action`，用你的 `k256::ecdsa::SigningKey` 通过 `sign_to_wrapper` 签名，然后把得到的 `SignatureWrapper` 放进请求体。

```rust
pub fn parse_private_key(hex_key: &str) -> Result<SigningKey, String>;
pub fn now_millis() -> u64;

pub fn sign_to_wrapper(
    action:        &Action,
    nonce:         u64,
    expires_after: Option<u64>,
    key:           &SigningKey,
) -> Result<SignatureWrapper, String>;
```

Action 构造函数：

```rust
pub fn action_place_order(orders: Vec<OrderRequest>) -> Action;
pub fn action_cancel(cancels: Vec<CancelRequest>) -> Action;
pub fn action_cancel_all(asset_ids: Vec<String>, market_type: &str) -> Action;
pub fn action_prediction_split (market_id: &str, size: &str) -> Action;
pub fn action_prediction_merge (market_id: &str, size: &str) -> Action;
pub fn action_prediction_redeem(market_id: &str) -> Action;
```

类型化输入：

```rust
struct OrderRequest {
    asset_id:        String,
    side:            SigningOrderSide,  // Buy / Sell(下单侧小写 wire)
    market_type:     String,            // "prediction"
    client_order_id: Option<String>,    // 34 字符客户端订单 ID
    price:           String,
    reduce_only:     bool,
    size:            String,
    size_type:       SizeType,          // Base(默认)/ Quote
    order_type:      OrderType,
}

enum OrderType {
    Limit(LimitTif),
}
enum LimitTif {
    Plain(String),                                  // "gtc" | "ioc" | "fok" | "alo"
    Gtd { expiry_time: u64 },                       // good-til-date,ms
}

struct CancelRequest {
    asset_id:    String,
    market_type: String,                            // "prediction"
    target:      CancelTarget,
}
enum CancelTarget { Oid(String), ClientOrderId(String) }
```

通信侧的对应类型（`OrderItem`、`CancelItem`）分别实现 `TryFrom<&OrderRequest>` 和 `From<&CancelRequest>`，以保证 JSON 请求体和签名字节都由同一份源 struct 构造。

客户端订单 ID：

```rust
pub fn generate_client_order_id_default() -> Result<String, String>;
pub fn generate_client_order_id(region: Region, env: Env) -> Result<String, String>;
pub fn validate_client_order_id(s: &str) -> bool;
pub fn parse_client_order_id_prefix(client_order_id: Option<&str>) -> ClientOrderIdPrefix;
pub fn register_client_order_id_context(region: Region, env: Env);
```

客户端订单 ID 是 34 字符的十六进制字符串，形如 `0x{region}{env}{30 位十六进制随机数}`。`generate_client_order_id_default()` 读取已注册的全局上下文（默认是 HK / PROD）。若需要不同的上下文，在启动时注册一次即可覆盖。

低阶 helper：

```rust
pub fn signer_address(key: &SigningKey) -> String;
pub fn ecrecover(signing_hash: &str, signature: &str) -> Result<String, String>;
pub fn sign_action(...) -> Result<String, String>;        // 返回 "0x..." 十六进制
pub fn sign_action_full(...) -> Result<(String, String, String, u8), String>; // (txhash, r, s, v)
pub fn sign_action_debug(...) -> Result<SigningDebug, String>; // 返回所有中间哈希值
```

正常流程请使用 `sign_to_wrapper`。低阶函数仅用于调试，或用于需要访问 txhash 的调用方（例如显示"在浏览器中查看"链接）。

## 13. 通用类型 [sdk-api-reference-13-common-types]

模块： `okx_outcomes_sdk::models::common::*`。

```rust
struct Pagination {
    next_cursor: Option<String>, // 最后一页时为 None
    has_more:    bool,
    page_size:   i32,            // 当前页条目数
}

struct EcdsaSignature {
    r: String, // 十六进制,带 0x 前缀
    s: String, // 十六进制,带 0x 前缀
    v: u8,     // recovery id:0 或 1
}

struct SignatureWrapper {
    // 序列化为 { "Ecdsa": { r, s, v } }
    ecdsa: EcdsaSignature,
}
```

SDK 透明地包装了两种 API 信封：

- 预测市场 REST： `{ "code": <int>, "message": "...", "data": <T> }`，其中 `code == 0` 表示成功。
- OKX 市场数据： `{ "code": "<int>", "msg": "...", "data": <T> }`（注意 code 是字符串）。`code == "0"` 表示成功。
