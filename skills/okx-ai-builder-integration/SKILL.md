---
name: okx-ai-builder-integration
description: >-
  Guide an AI assistant through integrating with the OKX AI Builder Program:
  how to apply for AI Builder, how to pick the right integration path (user
  type), how to read the reference demos, and how to help the user write their
  real integration code with correct AI Builder Code order attribution. Covers
  four user types — openapi-user (self account + local OpenAPI script), cli-user
  (OKX Trade CLI), mcp-user (OKX MCP), oauth-user (third-party server + OAuth
  + Fast API). The runnable demos live in a public GitHub repo; this
  skill routes to them and carries the rules. Use when the user wants to build
  or trade with OKX AI Builder, asks how to apply, which path fits them, or needs
  a worked OKX integration. 中文触发:AI Builder 接入 / OKX 下单归因 /
  AI Builder Code / 集成 demo / OAuth Broker / Fast API / OKX Trade CLI / OKX MCP。
license: MIT
metadata:
  author: okx
  version: "0.1.0"
  homepage: "https://www.okx.com"
  demo_repo_ref: "github-main"
---

# OKX AI Builder integration guide

Route an AI assistant to the right OKX AI Builder integration path and help the
user turn a reference demo into their **own** working integration.

**This skill carries knowledge, not code.** The runnable demos live in a public
GitHub repository (see §4); this skill tells you which one to read, how to read
it, what to copy into the user's project, and the rules that must hold. Do the
steps in order.

- Demos home (public): `https://github.com/okx/builder-integration-demo` (ref: `metadata.demo_repo_ref`, currently `github-main`)
- OKX API definitions (endpoints beyond a demo): see §7.

> If the linked repo does not yet contain `demos/openapi-user`, `demos/cli-user`,
> `demos/mcp-user`, and `demos/oauth-user`, its four-user-type content is not
> published yet — tell the user and do not fabricate demo contents.

## §1. Apply for AI Builder (do this first)

Everything else assumes the user has an **AI Builder Code** — the identifier OKX
maps to the order attribution field `tag`. The parameter NAME used to carry it
differs per surface (see §6); do not assume a single parameter name. Without a
Builder Code, order-producing calls cannot be attributed.

**How to apply** (from OKX's official guide; confirm current details there:
`https://www.okx.com/help/ai-builder-program-integration-guide`):

1. Sign in to OKX and go to the **AI Builder Program** page
   (`https://www.okx.com/agent-tradekit/builder`).
2. Click **Apply Now** and select **AI Builder** as the application type.
3. Enter the applicant and project information (project name, project link,
   project overview).
4. Read and accept the applicable Terms of Service, then submit the application.
5. Obtain your **Builder Code** and integration/testing materials from the
   **AI Builder Dashboard** (or your application approval notice).

**Status:** the Builder Code's activation state and when rebates start are
governed by the Dashboard status and the applicable terms. A Builder Code that is
not yet **Activated** can still be used for technical integration and testing;
only rebates depend on activation. Do not gate integration or testing on
activation.

Do not invent any application URL, form, or approval step beyond the above; for
anything more specific, point the user to the official guide. If the user already
has an AI Builder Code, proceed to §2.

## §2. Identify the user type

Pick the path **before** writing code. Four types:

| Type | Use when | Whose account | Demo |
|---|---|---|---|
| `openapi-user` | Strategy code runs on a machine/server the user controls and signs OKX OpenAPI directly | User's own | `demos/openapi-user` |
| `cli-user` | Trading is driven through the **OKX Trade CLI** (`okx` command) in a terminal or coding agent | User's own | `demos/cli-user` |
| `mcp-user` | Trading is driven through **OKX MCP** (ChatGPT app, Claude Desktop, app-connected MCP) | User's own | `demos/mcp-user` |
| `oauth-user` | A third-party service trades **end users'** accounts from its own server (OAuth + Fast API) | End users' | `demos/oauth-user` |

Authoritative routing table + full decision tree: `docs/USER_TYPES.md` in the demo repo (this table is a short index into it).

Disambiguation:
- `oauth-user` is only for a third-party service trading **end users'** accounts
  (it needs OAuth + Fast API permission + IP allowlist). Running on a server does
  **not** make it `oauth-user` — if it trades only the operator's own account, it
  is `openapi-user`, `cli-user`, or `mcp-user`.
- `cli-user` vs `mcp-user`: CLI = the `okx` command; MCP = an app calling OKX MCP tools.
- Not supported yet: third-party server + CLI, and third-party server + MCP.

## §3. Prerequisites — guide the user's per-type setup

Once the type is chosen, the user needs their type's setup done before the demo
can run. **Don't just describe it — take the user to the right place and walk
them through it**, opening the official guide for current details:
`https://www.okx.com/help/ai-builder-program-integration-guide`. Enforce the
gates below (they hold regardless of what the screens look like). This section is
account/surface **setup**; §5 covers turning the demo into the user's own code.

- **openapi-user** — Take the user to their own OKX account's API management page
  (`https://www.okx.com/account/my-api`) and walk them through creating an API key
  (key permissions + storage rules: see §6 — Least-privilege API keys + Security). **Gate:** no order at all —
  demo or live — until a key is configured, and keep the demo's
  `--confirm-live-order` gate on every live write.
- **cli-user** — In the user's terminal, walk them through installing the OKX
  Trade CLI (a current version that supports `--aiBuilderCode`; currently
  `npm i -g @okx_ai/okx-trade-cli` — confirm against the Agent Trade Kit install
  guide `https://www.okx.com/agent-tradekit`) and
  configuring an API-key profile or CLI OAuth locally, so `okx` is authorized to
  their account and the credentials stay in the user's environment. **Gate:** when
  you reach the demo in §4/§5, keep its version gate — that demo stops if the CLI
  is missing or too old.
- **mcp-user** — Walk the user through connecting and authorizing OKX **inside
  their MCP host app or OKX connector** (ChatGPT, Claude Desktop, …). The host or
  connector owns the credentials — never ask the user for API keys, secrets, or
  passphrases. **Gate:** the mcp-user demo defines a one-read-only-call connect
  check; if it fails, stop and have the user connect/authorize in the app. Keep
  it.
- **oauth-user** — Take the user into the AI Builder workbench
  (`https://www.okx.com/agent-tradekit/builder`) and open **Settings** to
  configure their OAuth & Fast API info: app name and OAuth logo (set during
  application, editable here), app URLs and the OAuth-exempt IP allowlist (up to
  10 IPs), and **enable Fast API** (this path uses it) with the user IP
  allowlist (up to 200 IPs). Obtain the `CLIENT_ID` / `CLIENT_SECRET` from the
  workbench, and register the **exact** `redirect_uri` (a mismatch fails
  authorization). Exact current steps + error handling live in the demo's
  `INTEGRATION_GUIDE.md` / `PITFALLS.md`. **Gate:** the OAuth flow can't run
  without `CLIENT_ID` / `CLIENT_SECRET` and a registered `redirect_uri`.

## §4. Get the demo (reading index)

Each demo folder has a consistent shape: `README.md` (entry) + an **artifact**
(code for openapi/oauth; `SKILL.md` for cli/mcp) + a `For real integration`
section. Two ways to get it — either works for any type:

- **View on GitHub** (no shell needed): open/fetch the file, e.g.
  `https://github.com/okx/builder-integration-demo/blob/github-main/demos/<type>/README.md`. For a fetch tool
  that needs raw content, use `https://github.com/okx/builder-integration-demo/raw/github-main/demos/<type>/…`.
- **Download** (shell available): clone at the pinned ref (`metadata.demo_repo_ref`, currently `github-main`).
  ```sh
  git clone --depth 1 --branch github-main https://github.com/okx/builder-integration-demo okx-ai-builder-demo
  # or just one demo (sparse):
  #   git clone --depth 1 --branch github-main --filter=blob:none --sparse https://github.com/okx/builder-integration-demo d \
  #     && cd d && git sparse-checkout set demos/<type>
  ```

Per type, read this, then go to §5:

| Type | Read | Get it |
|---|---|---|
| openapi-user | `demos/openapi-user/README.md` → the code | view or download |
| oauth-user | `demos/oauth-user/README.md` → `INTEGRATION_GUIDE.md` + `PITFALLS.md` → backend code | view or download |
| cli-user | `demos/cli-user/README.md` → `demos/cli-user/SKILL.md` (the artifact) | view or download |
| mcp-user | `demos/mcp-user/README.md` → `demos/mcp-user/SKILL.md` (the artifact) | view or download |

If the repo layout differs from what this skill describes, **trust the repo** and
tell the user this skill may be stale.

## §5. Use the demo (understand it, then produce the user's code)

**A demo is a verified *simple instance*, not a production implementation.** Your
job is to help the user build **their own** integration, not to ship the demo
as-is. Flow:

1. Read the demo's `README.md` — especially **What this demo shows** (scope) and
   **For real integration** (caveats).
2. Follow the demo's **Copy vs Adapt** section, which buckets **every** file as
   *copy verbatim* / *adapt* / *demo scaffolding — do not copy*. Buckets are
   per-demo — do not carry one demo's bucketing to another:
   - **Copy verbatim** the files that demo marks so (e.g. openapi-user's signing
     client `okx_openapi_client.py`). Do not re-implement HMAC signing by hand.
     Note: oauth-user has **no** copy-verbatim file — its `backend/okx_client.py`
     is **Adapt**, and its `MOCK`/`_mock_*` branches must be deleted first.
   - **Adapt** the example logic to the user's strategy/business.
   - **Do not copy** tests-as-reference, `.env.example`, `AGENTS.md`, or demo docs.
3. Put real deliverables in the **user's own project directory**. The only hard
   rule is: never write anything into **this guide skill's own install directory**.
   (For cli/mcp the deliverable is itself a skill file — see step 4.)
4. **cli-user/mcp-user**: the artifact is the demo's `SKILL.md`. Use it to
   configure the real `okx` CLI / OKX MCP for the user. When you hand the user
   that skill file (into their project, or wherever their agent loads skills):
   - replace **all** placeholders — `<AI_BUILDER_CODE>` with the user's real
     Builder Code, and profile placeholders like `<demo-profile>`/`<live-profile>`;
   - rewrite the demo's relative links (`../…`, `../../docs/…`) to absolute repo
     URLs, since they dangle once copied out of the repo;
   - give the skill's frontmatter `name` something unambiguous in the user's
     environment (the demo's bare `cli-user`/`mcp-user` collide easily).
5. **Verify before live.** Run in demo/simulated mode first: do the preflight
   reads and place one attributed test order. Confirm attribution **per surface**:
   for openapi-user/oauth-user (raw OpenAPI), check the returned order's `tag`
   equals the user's Builder Code; for cli-user/mcp-user, confirm the
   `--aiBuilderCode`/`aiBuilderCode` argument was actually submitted (the CLI/MCP
   response may not echo `tag` — do not claim response-level `tag` confirmation
   there). Only then discuss going live.

## §6. Integration rules (canonical for cross-surface rules)

**This section is normative and canonical for the cross-surface general rules
below.** Each demo's `README`/`PITFALLS`/`INTEGRATION_GUIDE` is canonical for
that demo's **per-path** caveats and code specifics — follow those for anything
file- or path-specific.

- **Attribution.** Attach AI Builder Code on every order-producing call **whose
  surface exposes it** — verify from the command's `--help` or the tool schema.
  Parameter name is per surface: `--ai-builder-code` (openapi-user script),
  `--aiBuilderCode` (cli-user CLI flag), `aiBuilderCode` (mcp-user MCP tool arg),
  `AI_BUILDER_CODE` env → order `tag` (oauth-user). **Never rename the OKX request
  field `tag`; never invent an alternate flag; never fall back to a raw `tag`
  argument on CLI/MCP.** If the selected surface cannot carry it, say so and stop
  rather than placing an unattributed order.
- **AI Builder Code format.** It is **not a secret** (do not lock it in a secret
  manager or refuse to put it in a CLI flag). Case-sensitive, 1–16 alphanumeric
  (`A–Z a–z 0–9`). Use the user's value; never invent one. Keep the demos'
  validation gate.
- **Least-privilege API keys.** Wherever the user supplies an OKX API key
  (openapi-user, and a cli-user on an API-key profile), enable **read + trade
  permissions only — never withdrawal**, and use a demo/simulated key first.
- **Security.** Never put `client_secret` / `secretKey` / `passphrase` in frontend
  code, logs, or committed files; real `.env` is local-only; production uses a
  secret manager. Default to simulated trading; the openapi client defaults
  `simulated=True` — pass `simulated=False` (and a live key) only for production.
- **Don't ship demo shortcuts.** e.g. the oauth demo's `MOCK` test switch spans
  three files — `backend/okx_client.py` (fakes order acceptance), `backend/app.py`
  (bypasses the OAuth config validation gate), and `frontend/index.html` (skips
  real OAuth + suppresses the live-order confirm) — strip **all** of it before
  production (see that demo's Copy vs Adapt notes).
- **Demo ≠ product.** Always read the demo's `For real integration` / `PITFALLS`
  before going live (backend key storage, error-code checks, other-site
  differences, rate limits).

## §7. Look up OKX API definitions

**Trigger rule:** for any OKX endpoint / parameter / site difference the demo does
**not** cover, verify it first — **never hand-write an endpoint path or field from
memory.** Use the first option available to you (do not run detection commands):

1. If you have the **`okx-v5-api`** skill, use it (local, structured, offline — best).
2. Otherwise, the public **OpenAPI Markdown repo** `https://github.com/okx/ai-builder-openapi-md`
   — same data. Prefer the layered index over the big JSON: read
   `skills/okx-v5-api/sites.json`, then `skills/okx-v5-api/index/modules.md` →
   `index/<module>.md` (module = 2nd path segment of `/api/v5/<module>/…`), then
   the endpoint's `skills/okx-v5-api/docs/<site>/<lang>/…`. (`index.json` is the
   full machine index — large; fetch only if needed.) For a raw-content fetch use
   the `raw.githubusercontent.com/okx/ai-builder-openapi-md/github-main/<path>` form
   (`github-main` is the public default branch).
   These paths assume the self-contained layout (data under `skills/okx-v5-api/`);
   if the published repo still has data at its root, use the root paths instead.
3. Otherwise, the live docs site `https://www.okx.com/docs-v5/`.
4. Otherwise, ask the user to paste the official endpoint docs — do not guess.

(1) and (2) are the **same data** in two forms — "no skill installed" ≠ "no data".
Installing `okx-v5-api` is optional and improves the experience; never require it
mid-task or block on it.

## §8. Self-check (routing fixture)

Sanity-check your routing with these four prompts. Each should resolve to the
listed type, artifact, attribution parameter, and prerequisite gate:

| User says… | Type | Artifact | Attribution param | Prerequisite gate |
|---|---|---|---|---|
| "I run my own bot on my VPS and sign OKX requests myself" | openapi-user | `demos/openapi-user/` code | `--ai-builder-code` → `tag` | API key (read+trade, no withdraw) configured |
| "I want my terminal/agent to trade my account via the `okx` command" | cli-user | `demos/cli-user/SKILL.md` | `--aiBuilderCode` | OKX Trade CLI installed + version OK |
| "My ChatGPT/Claude app trades my account through OKX MCP" | mcp-user | `demos/mcp-user/SKILL.md` | `aiBuilderCode` | OKX connected in the host (read-only call ok) |
| "My service trades my end users' accounts from my server" | oauth-user | `demos/oauth-user/` backend | `AI_BUILDER_CODE` env → `tag` | `CLIENT_ID`/`CLIENT_SECRET` from the workbench |

## Notes

- This skill is **reference mode**: demos are fetched/viewed from the public repo,
  not bundled here. `metadata.demo_repo_ref` pins the ref used for downloads (`github-main`).
- `my` = EEA (not Malaysia). Sites: `okex` (Global), `us`, `my`, `turkey`.
