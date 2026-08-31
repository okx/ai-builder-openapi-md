---
name: okx-v5-api
description: >-
  REST-first lookup for OKX V5 API definitions - a lightweight router over a
  Markdown corpus that ships INSIDE this skill. Resolves which REST endpoints
  exist on which site (okex/Global, us, my, turkey), the correct REST base URL
  per site, whether a given REST endpoint is supported there, and the
  per-endpoint params/response/rate limits. WebSocket docs may be included as
  supplemental Markdown and can be searched when needed, but the deterministic
  routing/index flow is REST endpoint path based. Use this whenever you need an
  OKX API's documented definition while writing or reviewing code. All data is
  bundled with the skill - no setup, no clone, no network.
license: MIT
metadata:
  author: okx
  version: "0.2.0"
  homepage: "https://www.okx.com"
  expected_data_major: 0

---
# OKX V5 API endpoint lookup

A **router** over the OKX V5 API docs. The site table, the routing `index/`, and
the per-endpoint bodies all ship **inside this skill's own directory** — the same
directory this `SKILL.md` lives in. The primary lookup flow is for REST endpoints:
given a REST endpoint, it resolves the site's REST base URL/domain, whether that
endpoint is **supported** on the target site, and which doc file holds the
params/response/rate-limit details.

WebSocket Markdown can exist in the corpus as supplemental documentation. Search
it when needed, but do not assume WebSocket docs have the same deterministic REST
endpoint-path routing/index.

Everything is **local and self-contained** — no network at query time, and
**nothing to set up**. `npx skills add` installs `SKILL.md` **together with** its
data (`sites.json`, `index.json`, `index/`, `docs/`), all under this skill's
directory.

Sites: `okex` (Global), `us`, `my` (**EEA**, not Malaysia),
`turkey`. Langs: `en`, `zh`.

## Path resolution — `<skill_dir>`

Throughout this document, **`<skill_dir>`** means **the directory that contains
this `SKILL.md`**. All data is addressed relative to it:

- `<skill_dir>/sites.json` — site table (REST/WS domains per site)
- `<skill_dir>/index.json` — authoritative endpoint index
- `<skill_dir>/index/<module>.md` — per-module routing tables (+ `modules.md` map)
- `<skill_dir>/docs/<site>/<lang>/...` — per-endpoint bodies

There is no config file and no `docs_path`. If, for some reason,
`<skill_dir>/sites.json` is not present, the skill install is incomplete —
reinstall with `npx skills add` (do not try to fetch data from the network).

## When to use

- You need an OKX V5 API endpoint's definition (path, method, params, response,
  rate limit, REST base URL).
- You are **writing** or **reviewing** code that calls the OKX API and need to
  confirm an endpoint's path, availability, domain, or parameters — look it up
  here instead of relying on memory.
- Especially when the target site is **not** OKX Global — `us`,
  `my` (EEA), `turkey` each have their own REST domains and their own set of
  supported REST endpoints. Never assume Global's URLs or coverage apply.
- You need WebSocket docs only as supplemental Markdown search. Use `sites.json`
  for the site's `ws` domain, then search/read the relevant Markdown; do not use
  REST endpoint routing as a promise of WebSocket coverage.

Your **result** is the endpoint's documented definition — this skill helps you
*find and read* it. Making the call or writing the code around it stays with the
caller; the protocol below simply stops once the definition is in hand.

## Preflight

Do this before touching any doc — routing and bodies alike. Everything here is
offline (no network) and reads only from `<skill_dir>` (see **Path resolution**).

1. **Confirm the data is present.** Check that `<skill_dir>/sites.json` exists. It
   ships with the skill, so it should always be there; if it is missing, the
   install is incomplete — tell the user to reinstall via `npx skills add` and
   stop. Never fetch data from the network.
2. **Offline major-compat check (local only).** Compare `<skill_dir>/sites.json`
   `version` **major** against this file's `metadata.expected_data_major`. Because
   the data ships *with* the skill they should always match; if they somehow
   differ, warn that the install looks inconsistent and suggest reinstalling the
   skill. Never touches the network.

## Query protocol

**Answer from the docs, never from memory.** Every part of a definition (path,
params, response, domain, `supported`) must come from the files below. If
something isn't in the docs, or you're unsure, search the docs — do **not** guess
or fill in from training data (it may be outdated or a hallucination).

Everything reads from `<skill_dir>` (run **Preflight** first). Read the small
slices below, in order:

1. **REST domain — `<skill_dir>/sites.json` (authoritative).** Look up the site's
   `rest` base URL there — always read the file, never rely on a remembered
   domain. `sites.json` also carries `ws` domains for supplemental WebSocket docs.
   Domains are site-specific (the values here are
   **illustrative only**; `sites.json` is the source of truth): `us`→`us.okx.com`,
   `my`/EEA→`eea.okx.com`, `turkey`→`tr.okx.com`, `okex`→`www.okx.com`.
   Portfolio-margin/broker endpoints use `rest_pap` / `ws_pap` where present.
2. **Find the endpoint — `<skill_dir>/index/<module>.md`.** The module is the
   **2nd path segment**: `/api/v5/<module>/...` (e.g. `/api/v5/trade/order` →
   `trade`). Unsure which modules exist? Open `<skill_dir>/index/modules.md`
   first. The matching row gives the endpoint `name`, its relative `doc` path, a
   ✓/✗ per site, and a `method` column to disambiguate GET vs POST on the same
   path.
   - **Don't know the path yet?** Start from the need: open
     `<skill_dir>/index/modules.md` to pick the likely module, **or**
     keyword-search the corpus:
     `rg -i "<keyword>" <skill_dir>/index/ <skill_dir>/docs/<site>/<lang>/`.
     Search the `zh` bodies for Chinese concepts (e.g. 划转/定投/子账户). Then
     use the matching endpoint's row as above.
3. **Supported check.** If the site's column is **✗**, the endpoint is **not**
   available there — report it as **unsupported** for that site and stop; there
   is no definition to return.
4. **Read the body.** The module table's `doc` is a **reference** path (right for
   most sites, but the exact per-site/per-lang path can differ — do not blindly
   compose it). For the exact path, look the endpoint up in
   `<skill_dir>/index.json`: find the entry by `method` + `path`, take
   `sites.<site>.doc.<lang>`, and read `<skill_dir>/docs/<site>/<lang>/<that>`.
   (`index.json` is pretty-printed, so search it **with surrounding context** —
   e.g. `rg -A 30 '"/api/v5/trade/order"' <skill_dir>/index.json`, or a `jq`
   lookup — because the `sites.<site>.doc` values sit a dozen-odd lines *below*
   the matched `path`; the same `path` appears once per method, so match on
   `method` + `path`.) For normal single-endpoint files, the doc's YAML
   frontmatter is authoritative for `method`/`path`/`rest`/`ws`.
   - **Multi-endpoint docs.** If the doc frontmatter has `multi_endpoint: true`,
     or if `index.json` shows several REST endpoints sharing the same `doc` path,
     do **not** read the whole file for one endpoint. First use the generated
     `Endpoint sections` table near the top of the file, or run
     `rg -n -F "<search key>" <skill_dir>/docs/<site>/<lang>/<that>`.
     Then read only the matching endpoint section and nearby shared definitions
     until the next endpoint heading. In these files, keep the target
     `method`/`path` from `index.json` or the matched section; file frontmatter
     `method`/`path` names only the first endpoint in that Markdown file.

## Example — place-order call for the US site

Goal: a place-order REST call targeting **OKX US**.

1. **Domain (`<skill_dir>/sites.json`).** `id: us` → `rest: https://us.okx.com`,
   `ws: wss://wsus.okx.com:8443`. Use that, not the global URL.
2. **Find it (`<skill_dir>/index/trade.md`).** Place order is
   `POST /api/v5/trade/order`, module `trade`. The row:
   `| POST | /api/v5/trade/order | POST / Place order | api/rest/trade/placeOrder.md | ✓ | ✓ | ✓ | ✓ |`
   (columns: `method | path | name | doc | okex | us | my | turkey`)
3. **Supported?** The `us` column is `✓` — proceed. (If it were `✗`, US doesn't
   support it — report unsupported, no definition to return.)
4. **Read the body.** Look up `POST /api/v5/trade/order` in
   `<skill_dir>/index.json` → `sites.us.doc.en` (=`api/rest/trade/placeOrder.md`),
   then open `<skill_dir>/docs/us/en/api/rest/trade/placeOrder.md` for params,
   request example, response schema, and rate limits. (For `zh` you'd take
   `sites.us.doc.zh`, which here differs — `api/rest/trade/spot/placeOrder.md` —
   so don't reuse the `en` path.)

Result — the resolved **definition** for this endpoint on US: method `POST`,
path `/api/v5/trade/order`, base URL `https://us.okx.com`, supported on `us` ✓,
with params / request-response schema / rate limits in the doc opened above.
That documented definition is the result you're after.

## Version check (optional, user-triggered)

- **Offline (default).** Report the local `version` (`<skill_dir>/sites.json`)
  and whether it and this file's `metadata.expected_data_major` **majors** match.
  No network.
- **Online (opt-in).** Only if the user asks to "check for updates" **and**
  network is available: compare the local `version` to the latest published one;
  if behind, advise reinstalling the skill (`npx skills add`) to get the newer
  bundled data. This is the **sole** networked action.

Semver: **MAJOR** = breaking layout/schema change (skill must update);
**MINOR** = endpoints added/changed; **PATCH** = corrections.

## Notes

- **Self-contained.** Routing (`sites.json`, `index/`) AND bodies (`docs/`) ship
  inside this skill's directory. There is no separate docs repo to clone and no
  `docs_path` to configure — resolve everything relative to `<skill_dir>`.
- **`my` = EEA**, not Malaysia.
- **WebSocket docs.** WebSocket Markdown is supplemental search corpus. This
  skill's deterministic routing protocol is REST endpoint path based.
