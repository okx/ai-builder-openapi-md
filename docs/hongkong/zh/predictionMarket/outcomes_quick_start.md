---
site: hongkong
lang: zh
name: 快速开始
type: guide
---

# 快速开始 [quick-start]

本指南将帮助你完成 Outcomes API 的最小接入流程，并提交第一笔订单。

## 前置条件 [quick-start-prerequisites]

开始前，请确保你已经具备以下条件：

| 条件       | 说明                             |
| ---------- | -------------------------------- |
| OKX 账户   | 已开通预测市场相关能力           |
| 可用余额   | 用于提交订单                     |
| OKX App    | 用于创建 API Key 和授权 Agent |
| 开发环境   | 本文示例使用 Rust SDK            |
| Agent 私钥 | 用于对下单、撤单等写操作进行签名 |

<aside class="notice">
API Key 和 Agent 私钥是两类不同凭证。API Key 用于 REST 请求鉴权；Agent 私钥用于交易类写操作签名。
</aside>

## 第 1 步：设置 API Key [quick-start-step-1-set-up-an-api-key]

在对任何请求进行签名之前，您需先通过 [OKX API 管理页面](https://www.okx.com/account/my-api) 或 APP 创建 API Key。创建成功后，您将获得以下 3 项必须妥善保管的凭证：

| 凭证       | 说明                     |
| ---------- | ------------------------ |
| APIKey     | 公开标识                 |
| SecretKey  | 用于请求签名             |
| Passphrase | 您自行设置的额外安全凭证 |

其中，APIKey 与 SecretKey 由平台随机生成并提供；Passphrase 由您自行设置，用于增强 API 访问的安全性。

### APIKey 权限 [quick-start-step-1-set-up-an-api-key-apikey-permissions]

对于预测市场，需要选中以下所有权限：

| 权限          | 说明                                 |
| ------------- | ------------------------------------ |
| 读取（Read）  | 查询账单、历史记录等只读操作         |
| 交易（Trade） | 下单、撤单、转账、调整配置等写入操作 |

### 创建步骤 [quick-start-step-1-set-up-an-api-key-creation-steps]

| 步骤 1：APP 预测市场页面点选 "View" | 步骤 2：点击右上角菜单 |
| :---: | :---: |
| <img src="images/outcomes-quickstart-1.jpeg" alt="APP 预测市场页面点选 View" style="width: 100%; max-width: 300px;" /> | <img src="images/outcomes-quickstart-2.jpeg" alt="点击右上角菜单" style="width: 100%; max-width: 300px;" /> |

| 步骤 3：进入生成 API Key 页面 | 步骤 4：生成 API Key |
| :---: | :---: |
| <img src="images/outcomes-quickstart-3.jpeg" alt="进入生成 API Key 页面" style="width: 100%; max-width: 300px;" /> | <img src="images/outcomes-quickstart-4.jpeg" alt="生成 API Key" style="width: 100%; max-width: 300px;" /> |

| 步骤 5：生成后可查看详情 | 步骤 6：详情页面 |
| :---: | :---: |
| <img src="images/outcomes-quickstart-5.jpeg" alt="生成后可查看详情" style="width: 100%; max-width: 300px;" /> | <img src="images/outcomes-quickstart-6.jpeg" alt="详情页面" style="width: 100%; max-width: 300px;" /> |

所有 REST 接口都需要 OKX API 凭证。使用 `with_credentials` 构造客户端（详见第 5 步）。

## 第 2 步：设置 Agent [quick-start-step-2-set-up-an-agent]

您的主密钥保持安全——创建一个代表您签署交易的 Agent 密钥。

1. 生成一个新的以太坊密钥对（任何密钥生成工具）。保存 **Agent 私钥** 和 **Agent 地址**。
2. 在 **OKX App** 中授权该 Agent——Agent 授权必须通过 App 完成，不能通过 API。

### APP 授权 Agent 步骤 [quick-start-step-2-set-up-an-agent-app-agent-authorization-steps]

| 步骤 1：输入待授权的 Agent 地址 | 步骤 2：授权成功 |
| :---: | :---: |
| <img src="images/outcomes-quickstart-7.jpeg" alt="输入待授权的 Agent 地址" style="width: 100%; max-width: 300px;" /> | <img src="images/outcomes-quickstart-8.jpeg" alt="授权成功" style="width: 100%; max-width: 300px;" /> |

授权完成后，后续所有写 API 调用使用 Agent 私钥，读 API 调用不需要私钥。

## 第 3 步：添加 SDK [quick-start-step-3-add-the-sdk]

**安装**

在 `Cargo.toml` 中加入本 SDK 与 Tokio 运行时：

> Cargo.toml 依赖配置

```toml
[dependencies]
okx-outcomes-sdk = { git = "https://github.com/okx/outcomes-sdk.git", features = ["signing", "websocket"] }
tokio = { version = "1", features = ["full"] }
```

这就是完整的依赖列表。`tokio-tungstenite`、`k256`、`sha3`、`futures-util` 等都会通过 SDK 的 feature 间接引入。**不要** 自行添加这些依赖，否则可能与 SDK 构建时使用的版本不一致。

**Feature 开关**

| Feature     | 启用内容                                                                           | 何时启用                                                                                           |
| ----------- | ---------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| _default_   | 仅 REST 客户端（事件、市场、订单、持仓、余额、成交、价格、体育数据）。                       | 只读集成。                                                                                         |
| `signing`   | EIP-712 + ECDSA 动作签名辅助函数。                                                 | 任何写操作：`place_order`、`cancel_order`、`cancel_all`、`split`、`merge`、`redeem`、`heartbeat`。 |
| `websocket` | 基于 tokio-tungstenite 的 `PredictionsWsClient`，以及类型化的 `WsMessage` 解析器。 | 实时价格、成交、订单簿，以及用户订单 / 持仓 / 余额流。                                             |

## 第 4 步：配置环境变量 [quick-start-step-4-configure-environment-variables]

建议使用环境变量保存凭证：

> 环境变量配置

```shell
export PREDICTIONS_API_KEY="your-api-key"
export PREDICTIONS_API_SECRET="your-secret-key"
export PREDICTIONS_API_PASSPHRASE="your-passphrase"
export PREDICTIONS_AGENT_PRIVATE_KEY="0x..."
```

<aside class="warning">
生产环境中建议使用密钥管理服务，不要将 <code>.env</code> 文件提交到代码仓库。
</aside>

## 第 5 步：初始化 Client [quick-start-step-5-initialize-the-client]

使用 API Key、Secret Key 和 Passphrase 初始化 SDK Client：

> 初始化 Client 示例

```rust
use okx_outcomes_sdk::{ApiCredentials, OutcomesSdkClient};

let creds = ApiCredentials {
    api_key:    std::env::var("PREDICTIONS_API_KEY")?,
    secret_key: std::env::var("PREDICTIONS_API_SECRET")?,
    passphrase: std::env::var("PREDICTIONS_API_PASSPHRASE")?,
};

let client = OutcomesSdkClient::with_credentials(creds);
```

SDK 会在本地完成 REST 请求签名，并自动写入所需的鉴权请求头。

<aside class="notice">
<code>secret_key</code> 不会被发送到网络上。SDK 会按 OKX 的 REST 鉴权规范，在本地用 HMAC-SHA256 对每个请求签名：<code>OK-ACCESS-SIGN = Base64(HMAC-SHA256(secret_key, timestamp + METHOD + path + body))</code>。
</aside>

<aside class="notice">
下单 / 撤单 / split / merge / redeem 还需要一把用于 EIP-712 签名的<b>链上私钥</b>（即 Agent 私钥）。这把私钥与你的 REST API Key 互相独立。
</aside>

## 第 6 步：查询可交易市场 [quick-start-step-6-query-tradable-markets]

`get_events` 返回分页的事件列表。所有筛选参数都是可选的：

> 查询事件列表示例

```rust
let page = client
    .get_events(
        Some("active"),      // status: "active" | "resolved"
        None,                // tag
        None,                // league_id
        Some("volume_24h"),  // sort: "volume" | "volume_24h" | "ending_soon" | "newest"
        None,                // cursor（来自上一页）
        Some(20),            // page_size（最大 50）
    )
    .await?;

for event in &page.events {
    println!("{} - {} markets ({} points volume)",
        event.event_title, event.total_markets_count, event.volume);
}

// 分页：把 `page.pagination.next_cursor` 作为下一次调用的 `cursor` 传回去。
```

**其他读取方法**

| 方法                                                                   | 用途                             |
| ---------------------------------------------------------------------- | -------------------------------- |
| `client.search(keyword, cursor, page_size)`                            | 按关键字搜索事件与市场。         |
| `client.get_event(event_id)`                                           | 单个事件及其完整市场列表。       |
| `client.get_event_markets(event_id)`                                   | 单个事件下的全部市场（不分页）。 |
| `client.get_market(market_id)`                                         | 通过市场 ID 获取单个市场。       |
| `client.get_ticker(inst_id)`                                           | 单个市场标的的最新行情。         |
| `client.get_candles(...)`                                              | K 线历史。                       |
| `client.get_trades(...)`                                               | 最近的公开成交历史。             |

找到 `status: "active"` 的市场，记录：

- `yesOutcome.assetId`（例如 `"100049000"`）——用于下单
- `marketId`（例如 `"1"`）——用于持仓查询

<aside class="warning">
新创建的市场可能 <code>assetId: null</code>（尚未上链）。跳过这些——等待 <code>assetId</code> 填充后再操作。
</aside>

## 第 7 步：下限价单 [quick-start-step-7-place-a-limit-order]

以 0.55 xp 的价格在市场 `100049000` 上买入 10 个 YES 代币。需要 **两层认证**：API Key 请求头 + SDK 签名请求体。

> 下限价单完整示例

```rust
use okx_outcomes_sdk::{ApiCredentials, OutcomesSdkClient};
use okx_outcomes_sdk::models::order::{
    LimitOrderType, OrderItem, PlaceOrderAction, PlaceOrderRequest, SigningOrderSide, SizeType,
};
use okx_outcomes_sdk::signing::{
    action_place_order, generate_client_order_id_default, now_millis, parse_private_key,
    sign_to_wrapper, LimitTif, OrderRequest, OrderType,
};

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let creds = ApiCredentials {
        api_key:    std::env::var("PREDICTIONS_API_KEY")?,
        secret_key: std::env::var("PREDICTIONS_API_SECRET")?,
        passphrase: std::env::var("PREDICTIONS_API_PASSPHRASE")?,
    };
    let client = OutcomesSdkClient::with_credentials(creds);

    // 1. Load the on-chain signing key (hex, with or without 0x prefix).
    let key = parse_private_key(&std::env::var("PREDICTIONS_AGENT_PRIVATE_KEY")?)?;

    // 2. Build the typed order. `asset_id` is the outcome asset ID from Step 6.
    let order_request = OrderRequest {
        asset_id:         "100049000".into(),
        side:             SigningOrderSide::Buy,
        market_type:      "prediction".into(),
        client_order_id:  Some(generate_client_order_id_default()?),
        price:            "0.55".into(),
        reduce_only:      false,
        size:             "10".into(),
        size_type:        SizeType::Base, // omitted on the wire (default)
        order_type:       OrderType::Limit(LimitOrderType { tif: LimitTif::Gtc }),
    };

    // 3. Derive the wire-format OrderItem from the same OrderRequest so the
    //    JSON body cannot drift from the signed msgpack bytes.
    let order_item = OrderItem::try_from(&order_request)?;
    let action = action_place_order(vec![order_request]);

    // 4. Sign and assemble the SignatureWrapper expected by the wire format.
    let nonce = now_millis();
    let signature = sign_to_wrapper(&action, nonce, None, &key)?;

    // 5. Submit.
    let req = PlaceOrderRequest {
        action: PlaceOrderAction {
            action_type: "placeOrder".into(),
            grouping:    "na".into(),
            orders:      vec![order_item],
        },
        nonce: nonce as i64,
        signature,
    };
    let resp = client.place_order(&req).await?;
    println!("tx_hash: {}", resp.tx_hash);
    Ok(())
}
```

> 下单响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "txHash": "0x7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1a2b3c4d5e6f7a8b"
  }
}
```

## 第 8 步：查看订单 [quick-start-step-8-view-orders]

`list_orders` 在 `status = Some("open")` 时按页返回账户上所有非终态订单。把上一次响应中的 `next_cursor` 通过 `cursor` 参数传回，直到 `has_next` 为 `false`：

> 查询订单列表示例

```rust
use okx_outcomes_sdk::models::order::OrderRecord;

let mut cursor: Option<String> = None;
let mut all_open: Vec<OrderRecord> = Vec::new();

loop {
    let page = client
        .list_orders(
            None,             // market_id: None = 所有市场，Some(123) = 限定某个市场
            Some("open"),     // "open" = PENDING_PLACE / ACTIVE / PENDING_CANCEL；"closed" = FILLED / PARTIALLY_FILLED / CANCELLED / EXPIRED / FAILED
            cursor.as_deref(),
            Some(50),         // 每页最大条数
        )
        .await?;
    all_open.extend(page.list);
    if !page.has_next {
        break;
    }
    cursor = page.next_cursor;
}

println!("{} open orders", all_open.len());
for o in &all_open {
    println!(
        "#{} [{}] {} {}/{} @ {}  market={} asset={} cloid={:?}",
        o.id, o.status, o.side, o.filled_size, o.size, o.price,
        o.market_id, o.asset_id, o.client_order_id,
    );
}
```

每个 `OrderRecord` 暴露下单参数（`side`、`size`、`price`、`order_type`、`size_type`、`expiration`）、下单元数据（`market_id`、`asset_id`、`client_order_id`、`tx_hash`），以及当前生命周期状态（`status`、`filled_size`、`filled_amount`）。

<aside class="notice">
如果只想看挂单状态的订单，过滤 <code>o.status == "ACTIVE"</code> 即可，这会排除撮合引擎仍在处理以及处于撤单中的订单。
</aside>

提示：

- `market_id` 在 SDK 中是 `i64`，不是字符串；请用 `Some(123_456_789)` 传入。REST API 返回的 `marketId` 是字符串，SDK 内部会自动转换。
- `client_order_id`（下单时传入的 cloid）可以传给 `cancel_order`，以使用客户端 ID 撤单而不是服务端 ID。
- 记录 `id`（例如 `578840`）——撤单时需要用到。

## 第 9 步：撤销订单 [quick-start-step-9-cancel-an-order]

撤单流程与 `place_order` 一致，同样分三步：构造类型化的 `CancelRequest`，从中派生对应的 wire 格式 `CancelItem`，对动作签名，最后提交 wire 请求。可以通过订单的服务端 `oid`（`OrderRecord.id` 的返回值）或 `cloid`（下单时传入的 `client_order_id`）来定位目标订单。

下面的片段假设 `client` 和 `key` 已按下单一节构造完成。

> 撤销订单示例

```rust
use okx_outcomes_sdk::models::order::{CancelItem, CancelOrderAction, CancelOrderRequest};
use okx_outcomes_sdk::signing::{
    action_cancel, now_millis, sign_to_wrapper, CancelRequest, CancelTarget,
};

// 1. CancelTarget 必须选其一：
//    Oid   = 服务端分配的订单 ID（OrderRecord.id），十进制字符串。
//    Cloid = 客户端分配的订单 ID，带 0x 前缀的 hex 字符串。
let cancel_request = CancelRequest {
    asset_id:    "100049000".into(),
    market_type: "prediction".into(),
    target:      CancelTarget::Oid("578840".into()),
    // target:   CancelTarget::Cloid("0xabc...".into()),
};

// 2. 从同一份请求派生 wire 项，确保签名字节与 JSON 请求体不会发散。
let cancel_item = CancelItem::from(&cancel_request);
let action      = action_cancel(vec![cancel_request]);

// 3. 签名并提交。
let nonce     = now_millis();
let signature = sign_to_wrapper(&action, nonce, None, &key)?;

let resp = client
    .cancel_order(&CancelOrderRequest {
        action: CancelOrderAction {
            action_type: "cancel".into(),
            cancels:     vec![cancel_item],
        },
        nonce:     nonce as i64,
        signature,
    })
    .await?;
println!("cancel tx_hash: {}", resp.tx_hash);
```

<aside class="notice">
一个 <code>CancelOrderRequest</code> 可以携带多个 <code>CancelItem</code>：把所有需要撤销的 <code>CancelRequest</code> 都放进同一次 <code>action_cancel(vec![...])</code> 调用，它们就会共享一份签名和一笔链上交易。
</aside>

<aside class="notice">
撤单是异步的：<code>cancel_order</code> 在交易被接收后即返回，订单会随链上确认从 <code>PENDING_CANCEL</code> 进入 <code>CANCELLED</code>。如果需要等待终态，可轮询 <code>get_order(id)</code> 或订阅私有的 <code>pm-order</code> WebSocket 频道。
</aside>

## 第 10 步：查看余额、订单与持仓 [quick-start-step-10-view-balance-orders-and-positions]

> 查询余额与持仓示例

```rust
// 余额
let entries = client.get_balance().await?; // Vec<BalanceEntry>
for entry in &entries {
    println!("[{}] available={} total={}", entry.odds_type, entry.available, entry.balance);
}

// 持仓
let positions = client.get_positions(
    Some("open"), // "open" | "closed" | "settled"
    None,         // market_id
    None,         // cursor
    Some(50),     // limit
).await?;
println!("{} positions", positions.list.len());
```

订单查询方式见第 8 步。`get_positions` 同样通过 `cursor` + `limit` 分页。

## 完整流程回顾 [quick-start-full-flow-recap]

最小接入流程如下：

1. 在 App / 网页 中创建 API Key
2. 在 App 中授权 Agent
3. 安装 SDK
4. 配置环境变量
5. 初始化 Client
6. 查询 Event 和 Market
7. 获取 YES / NO outcome 的 assetId，使用 assetId 提交订单
8. 查询订单状态
9. 如有需要，撤销订单
10. 成交后查询持仓和余额

## 注意事项 [quick-start-notes]

| 主题       | 说明                                                                                                             |
| ---------- | ---------------------------------------------------------------------------------------------------------------- |
| 心跳       | 如果运行机器人，设置心跳以在机器人断线时自动撤销所有订单。每 < 5 分钟发送一次预签名的全部撤单，详情见 SDK 文档。 |
| 价格刻度   | 见下表。                                                                                                         |
| Nonce      | 使用当前毫秒时间戳。每个 Nonce 只能使用一次。                                                                    |
| 时间戳偏差 | `OK-ACCESS-TIMESTAMP` 与服务器时间相差不能超过 30 秒，否则返回错误码 50102。                                     |

**价格刻度规则**

| 价格区间         | 最小精度 | 示例             |
| ---------------- | -------- | ---------------- |
| 0.04 - 0.96      | 0.01     | 0.04, 0.55, 0.96 |
| < 0.04 或 > 0.96 | 0.001    | 0.039, 0.962     |

## 常见问题 [quick-start-faq]

| 问题 | 可能原因 | 处理方式 |
| --- | --- | --- |
| `assetId` 为 `null` | 市场尚未完成注册 | 等待 `assetId` 返回有效值后再下单 |
| 下单失败 | 市场不可交易、余额不足、签名错误或参数不合法 | 查看错误码和订单失败原因 |
| 签名失败 | Agent 私钥不正确，或签名内容与请求体不一致 | 确认 Agent 已授权，并使用 SDK 构造签名 |
| 查询不到订单 | 订单 ID 错误，或订单已经结束 | 使用订单列表接口确认订单状态 |
| 撤单后订单仍未取消 | 撤单异步处理中 | 等待状态从 `PENDING_CANCEL` 更新为 `CANCELLED` |
