# OKX V5 API — Flat Markdown Docs

Machine-friendly, flattened Markdown of the OKX V5 REST API, generated from the
official documentation source. Built for AI agents and tooling to read and to
write/verify OKX API integration code across **multiple sites**.

The primary lookup flow is for REST endpoints: endpoint path -> module index ->
site/language doc. WebSocket Markdown is included when present as supplemental
documentation and can be searched when needed, but it does not have the same
deterministic REST endpoint routing/index.

> Generated content — do not edit by hand. `sites.json` and `index.json` each
> carry a `version` (semver) and a `generated_at` timestamp so you can tell which
> release you are reading.

## Versioning

`sites.json` and `index.json` carry a `version` field using semver
`MAJOR.MINOR.PATCH` (a bare string, e.g. `0.1.0` — no `v` prefix):

- **MAJOR** — breaking change to the data format (layout/schema). Tooling that
  consumes these files may need updating.
- **MINOR** — endpoints added or changed.
- **PATCH** — documentation fixes.

`generated_at` records when the build was produced. Use `version` to check
compatibility and `generated_at` for freshness.

## What's here

```
sites.json                       # site -> REST and WS domains (+ version, generated_at)
index/                           # layered lookup tables (primary navigation)
  modules.md                     #   module list + per-site endpoint counts
  <module>.md                    #   one table per module: method|path|name|doc|per-site supported
index.json                       # full machine index (secondary): every endpoint, per-site "supported" + doc path (+ version, generated_at)
docs/<site>/<lang>/...md         # usually one endpoint per Markdown file
skills/okx-v5-api/               # pure-Markdown skill (SKILL.md only)
```

- **Sites**: `okex` (Global), `us`, `my` (EEA), `turkey` (Hong Kong is **not**
  covered). Each site's docs are **flattened and self-contained** — the
  domain and the set of supported endpoints already reflect that site. Do not
  assume the Global site's endpoints or domain apply elsewhere.
- **Languages**: `en` and `zh`.
- Each endpoint Markdown file carries YAML frontmatter (`method`, `path`, `site`,
  `lang`, `rest`, `ws`, `module`) so tools can read the essentials without
  parsing prose.
- Some multi-endpoint source pages are preserved as one Markdown file and marked
  `multi_endpoint: true`. Those files include an `Endpoint sections` table near
  the top so AI agents can locate a target `method path` and read only the
  matching section. In multi-endpoint files, the frontmatter `method`/`path`
  identifies the first endpoint only; use `index.json` plus the target section
  for the endpoint-specific method/path.

## Navigation (the `index/` layer)

`index/` is the primary way to find an endpoint. It is plain Markdown — no code,
no runtime, all local.

- `index/modules.md` lists every module with its total endpoint count and how
  many are supported per site — a quick overview and jump-off point.
- `index/<module>.md` is a small table for one module. The **module is the 2nd
  path segment** (`/api/v5/<module>/...`, e.g. `/api/v5/trade/order` → `trade`).
  Each row has `method | path | name | doc` plus a per-site `✓ / ✗` column.
- `index.json` remains available as the full machine index (per-site `doc` is a
  per-language `{ "en": ..., "zh": ... }` map), but for lookups prefer the
  `index/` tables — they are smaller and easier to scan.

## For AI agents

1. Read `sites.json` to get the target site's `rest` domain. Use that as the REST
   base URL - **never hardcode a single global domain**. `sites.json` also
   carries `ws` domains for supplemental WebSocket docs.
2. Derive the module from the endpoint path (2nd segment) and open
   `index/<module>.md`. Confirm the endpoint's column for your site is `✓`.
   **If it is `✗` (not supported), do not generate that call.**
3. The module table's `doc` is a reference path. For the exact per-site/per-lang
   path, look up the endpoint in `index.json` by `method` + `path`, take
   `sites.<site>.doc.<lang>`, and read `docs/<site>/<lang>/<that>` (or more
   generally `<docs_dir>/<lang>/<that>`, where `docs_dir` comes from
   `sites.json`, e.g. `docs/us` → `docs/us/en/<that>`). For normal
   single-endpoint files, YAML frontmatter carries the endpoint `method`/`path`/
   `rest`/`ws`; the prose covers parameters, response, and rate limits.
   - If the doc frontmatter has `multi_endpoint: true`, do not read the entire
     file for a single endpoint. Use the generated `Endpoint sections` table or
     `rg -n -F "<search key>" docs/<site>/<lang>/<that>` to locate the target
     section, then read only that section and nearby shared definitions. For
     these files, keep the target `method`/`path` from `index.json` or the
     matched section; the file frontmatter `method`/`path` names only the first
     endpoint in the file.

## Skill & install model

The `skills/okx-v5-api/` skill is **pure Markdown** — just `SKILL.md`, no Python,
no scripts, no runtime dependency, and **no bundled data**. It does not carry its
own copy of the routing layer. Instead it reads **everything** — `sites.json`,
`index/`, and the endpoint doc `docs/` — from **this repository, cloned by you**.

Installing the skill does **not** install this data repo. Point the skill at
your clone once via a fixed config file at `~/.okx-v5-api/config.json` with a
`docs_path` field naming the checkout. The skill then resolves both routing and
bodies under that path:

1. **Route**: from `docs_path`, read `sites.json` + `index/` to find the site
   domain, per-site support, and the target `doc` path.
2. **Read the body**: open `docs_path/docs/<site>/<lang>/<doc>`.

If `~/.okx-v5-api/config.json` or the clone is missing, set up the clone and
point `docs_path` at it before using the skill.

If you have not cloned/downloaded this docs repo yet:

```sh
git clone https://github.com/okx/ai-builder-openapi-md ~/.okx-v5-api-docs
mkdir -p ~/.okx-v5-api
printf '{"docs_path":"%s/.okx-v5-api-docs"}\n' "$HOME" > ~/.okx-v5-api/config.json
```

If you already cloned/downloaded this repo, run this from the repo root:

```sh
mkdir -p ~/.okx-v5-api
printf '{"docs_path":"%s"}\n' "$(pwd)" > ~/.okx-v5-api/config.json
```

## Provenance & disclaimer

Generated from the upstream OKX V5 API documentation source; see `version` and
`generated_at` in `sites.json` / `index.json` for the exact release. For the
authoritative, always-current reference, see the official OKX API documentation.
This mirror is provided to make the docs consumable by AI tooling; verify against
the official docs before relying on it in production.
