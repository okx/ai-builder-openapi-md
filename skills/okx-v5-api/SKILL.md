---
name: okx-v5-api
description: >-
  Look up OKX V5 REST/WS API definitions — an index/retriever over the official
  OpenAPI docs. Resolves which endpoints exist on which site (okex/Global, us,
  my, turkey), the correct base URL (rest/ws domain) per site, whether
  a given endpoint is supported there, and the per-endpoint params/response/rate
  limits. Use this whenever you need an OKX API's definition — e.g. while writing
  or reviewing code that calls the OKX API and you need to confirm an endpoint's
  path, availability, domain, or parameters — especially for a non-global site
  (US, EEA, Turkey; the EEA site id is `my`, meaning EEA, NOT
  Malaysia), where base URL and endpoint availability differ from OKX Global.
  Its result is the API's documented definition — finding and reading the docs;
  making the call or writing the code around it stays with the caller. Reads
  everything from a docs repo you set up once (see Setup) — do that before first use.
license: MIT
metadata:
  author: okx
  version: "0.1.1"
  homepage: "https://www.okx.com"
  expected_data_major: 0

---
# OKX V5 API endpoint lookup

A **router** over the OKX V5 API docs. It reads a **docs repo you set up once**
(see **Setup**): the site table, the routing `index/`, and the per-endpoint
bodies all live there. Given an endpoint, it resolves the site's base URL/domain,
whether that endpoint is **supported** on the target site, and which doc file
holds the params/response/rate-limit details.

Everything is **local** once the repo is cloned — no network at query time. The
skill installed by `npx skill add` is just this `SKILL.md`; all data comes from
the docs repo, so **the repo must be set up before any use — routing included.**

Sites: `okex` (Global), `us`, `my` (**EEA**, not Malaysia),
`turkey`. Langs: `en`, `zh`.

## When to use

- You need an OKX V5 API endpoint's definition (path, method, params, response,
  rate limit, base URL, WS URL).
- You are **writing** or **reviewing** code that calls the OKX API and need to
  confirm an endpoint's path, availability, domain, or parameters — look it up
  here instead of relying on memory.
- Especially when the target site is **not** OKX Global — `us`,
  `my` (EEA), `turkey` each have their own REST/WS domains and their own set of
  supported endpoints. Never assume Global's URLs or coverage apply.

Your **result** is the endpoint's documented definition — this skill helps you
*find and read* it. Making the call or writing the code around it stays with the
caller; the protocol below simply stops once the definition is in hand.

## Preflight

Do this before touching any doc — routing and bodies alike. Everything here is
offline (no network).

1. **Ensure the docs repo is set up.** Read the **fixed** config
   `~/.okx-v5-api/config.json` and take its `docs_path`. If the file is
   **missing**, **not valid JSON**, has an **empty/missing** `docs_path`,
   `docs_path` is **not absolute**, or that path has **no `sites.json`** inside
   (`<docs_path>/sites.json`) → the docs are not set up → show **Setup** and stop
   until fixed. Nothing (not even routing) works before this.
2. **Offline major-compat check (local only).** Compare `<docs_path>/sites.json`
   `version` **major** against this file's `metadata.expected_data_major`. If
   majors **differ**, warn that skill and docs data are out of sync — paths and
   layout may not match — and suggest updating whichever side is older
   (`npx skill` for the skill, `git pull` in the docs repo). Never touches the
   network.

## Setup

Show this **only when the docs are not set up**. The config path is **fixed** at
`~/.okx-v5-api/config.json` — not configurable, not an env var. Its content is
just `{"docs_path":"<absolute path to the docs repo>"}`. `docs_path` **must be
absolute**. Offer the user these three options:

**a) I already have the docs repo cloned.** Point the config at it:

```sh
mkdir -p ~/.okx-v5-api && printf '{"docs_path":"%s"}\n' "<abs path>" > ~/.okx-v5-api/config.json
```

**b) Clone + configure in one line** (uses the default location):

```sh
git clone <REPO_URL> ~/.okx-v5-api-docs && mkdir -p ~/.okx-v5-api && printf '{"docs_path":"%s/.okx-v5-api-docs"}\n' "$HOME" > ~/.okx-v5-api/config.json
```

> `<REPO_URL>` is a placeholder — the real docs repo URL is **TBD**; substitute
> it before running, or ask the user for the clone URL.

**c) Let me (the AI) set it up.** With your **explicit consent** (this modifies
your machine), I'll clone the docs repo to the default `~/.okx-v5-api-docs` and
write the config, then continue. I must ask for consent **before** running.

Notes:

- **Default docs location:** `~/.okx-v5-api-docs/` — writable, shared across
  projects (clone once, reuse everywhere), never a system dir. Clone elsewhere
  if you like, just point `docs_path` there. **Windows:** `~` = `%USERPROFILE%`.
- The one-liners assume a plain path/URL. If your `docs_path` or clone URL has
  quotes, backslashes, spaces, or shell metacharacters, don't paste them raw —
  write the config by hand as `{"docs_path":"<path>"}` with the value
  JSON-escaped, or let option (c) write it safely.

## Query protocol

**Answer from the docs, never from memory.** Every part of a definition (path,
params, response, domain, `supported`) must come from the files below. If
something isn't in the docs, or you're unsure, search the docs — do **not** guess
or fill in from training data (it may be outdated or a hallucination).

Everything reads from `docs_path` (run **Preflight** first). Read the small
slices below, in order:

1. **Domain — `<docs_path>/sites.json` (authoritative).** Look up the site's
   `rest` and `ws` base URLs there — always read the file, never rely on a
   remembered domain. Domains are site-specific (the values here are
   **illustrative only**; `sites.json` is the source of truth): `us`→`us.okx.com`,
   `my`/EEA→`eea.okx.com`, `turkey`→`tr.okx.com`, `okex`→`www.okx.com`.
   Portfolio-margin/broker endpoints use `rest_pap` / `ws_pap` where present.
2. **Find the endpoint — `<docs_path>/index/<module>.md`.** The module is the
   **2nd path segment**: `/api/v5/<module>/...` (e.g. `/api/v5/trade/order` →
   `trade`). Unsure which modules exist? Open `<docs_path>/index/modules.md`
   first. The matching row gives the endpoint `name`, its relative `doc` path, a
   ✓/✗ per site, and a `method` column to disambiguate GET vs POST on the same
   path.
   - **Don't know the path yet?** Start from the need: open
     `<docs_path>/index/modules.md` to pick the likely module, **or**
     keyword-search the corpus:
     `rg -i "<keyword>" <docs_path>/index/ <docs_path>/docs/<site>/<lang>/`.
     Search the `zh` bodies for Chinese concepts (e.g. 划转/定投/子账户). Then
     use the matching endpoint's row as above.
3. **Supported check.** If the site's column is **✗**, the endpoint is **not**
   available there — report it as **unsupported** for that site and stop; there
   is no definition to return.
4. **Read the body.** The module table's `doc` is a **reference** path (right for
   most sites, but the exact per-site/per-lang path can differ — do not blindly
   compose it). For the exact path, look the endpoint up in
   `<docs_path>/index.json`: find the entry by `method` + `path`, take
   `sites.<site>.doc.<lang>`, and read `<docs_path>/docs/<site>/<lang>/<that>`.
   (`index.json` is pretty-printed, so search it **with surrounding context** —
   e.g. `rg -A 30 '"/api/v5/trade/order"' <docs_path>/index.json`, or a `jq`
   lookup — because the `sites.<site>.doc` values sit a dozen-odd lines *below*
   the matched `path`; the same `path` appears once per method, so match on
   `method` + `path`.) The doc's YAML frontmatter is authoritative for
   `method`/`path`/`rest`/`ws`.

## Example — place-order call for the US site

Goal: a place-order REST call targeting **OKX US**.

1. **Domain (`<docs_path>/sites.json`).** `id: us` → `rest: https://us.okx.com`,
   `ws: wss://wsus.okx.com:8443`. Use that, not the global URL.
2. **Find it (`<docs_path>/index/trade.md`).** Place order is
   `POST /api/v5/trade/order`, module `trade`. The row:
   `| POST | /api/v5/trade/order | POST / Place order | api/rest/trade/placeOrder.md | ✓ | ✓ | ✓ | ✓ | ✓ |`
3. **Supported?** The `us` column is `✓` — proceed. (If it were `✗`, US doesn't
   support it — report unsupported, no definition to return.)
4. **Read the body.** Look up `POST /api/v5/trade/order` in
   `<docs_path>/index.json` → `sites.us.doc.en` (=`api/rest/trade/placeOrder.md`),
   then open `<docs_path>/docs/us/en/api/rest/trade/placeOrder.md` for params,
   request example, response schema, and rate limits. (For `zh` you'd take
   `sites.us.doc.zh`, which here differs — `api/rest/trade/spot/placeOrder.md` —
   so don't reuse the `en` path.)

Result — the resolved **definition** for this endpoint on US: method `POST`,
path `/api/v5/trade/order`, base URL `https://us.okx.com`, supported on `us` ✓,
with params / request-response schema / rate limits in the doc opened above.
That documented definition is the result you're after.

## Version check (optional, user-triggered)

- **Offline (default).** Report the local `version` (`<docs_path>/sites.json`)
  and whether it and this file's `metadata.expected_data_major` **majors** match.
  No network.
- **Online (opt-in).** Only if the user asks to "check for updates" **and**
  network is available: compare the local `version` to the latest published one;
  if behind, advise `git pull` in the docs repo and/or `npx skill` for the
  skill. This is the **sole** networked action.

Semver: **MAJOR** = breaking layout/schema change (skill must update);
**MINOR** = endpoints added/changed; **PATCH** = corrections.

## Notes

- **Duplication.** The cloned docs repo also contains a copy of this skill. That
  copy is **not** active — Claude Code runs the `npx skill add`-installed
  `SKILL.md`; the clone is used **only** for its data (`sites.json`, `index/`,
  `docs/`). Ignore the clone's `SKILL.md`.
- **`my` = EEA**, not Malaysia.
