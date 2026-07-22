# OKX V5 API — Flat Markdown Docs

Machine-friendly, flattened Markdown of the OKX V5 REST/WebSocket API, generated
from the official documentation source. Built for AI agents and tooling to read
and to write/verify OKX API integration code across **multiple sites**.

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
sites.json                       # site -> REST/WS domains (+ version, generated_at)
index/                           # layered lookup tables (primary navigation)
  modules.md                     #   module list + per-site endpoint counts
  <module>.md                    #   one table per module: method|path|name|doc|per-site supported
index.json                       # full machine index (secondary): every endpoint, per-site "supported" + doc path (+ version, generated_at)
docs/<site>/<lang>/...md         # one self-contained Markdown file per endpoint
skills/okx-v5-api/               # pure-Markdown skill (SKILL.md only)
```

- **Sites**: `okex` (Global), `us`, `hongkong` (uses the global domain), `my`
  (EEA), `turkey`. Each site's docs are **flattened and self-contained** — the
  domain and the set of supported endpoints already reflect that site. Do not
  assume the Global site's endpoints or domain apply elsewhere.
- **Languages**: `en` and `zh`.
- Each Markdown file carries YAML frontmatter (`method`, `path`, `site`, `lang`,
  `rest`, `ws`, `module`) so tools can read the essentials without parsing prose.

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

1. Read `sites.json` to get the target site's `rest` (and `ws`) domain. Use that
   as the base URL — **never hardcode a single global domain**.
2. Derive the module from the endpoint path (2nd segment) and open
   `index/<module>.md`. Confirm the endpoint's column for your site is `✓`.
   **If it is `✗` (not supported), do not generate that call.**
3. From the matched row, read the `doc` path and open
   `docs/<site>/<lang>/<doc>`. Its YAML frontmatter carries `method`/`path`/
   `rest`/`ws`; the prose covers parameters, response, and rate limits.
   - The en/zh `doc` path can differ slightly for a few endpoints. If `<doc>` is
     absent for your site/lang, find the file by name under
     `docs/<site>/<lang>/`. `index.json` records both relative paths as
     `sites[id].doc.{en,zh}`; the full path is `<docs_dir>/<lang>/<doc>`, where
     `docs_dir` comes from `sites.json` (e.g. `docs/us` → `docs/us/en/<doc>`).

## Skill & install model

The `skills/okx-v5-api/` skill is **pure Markdown** — just `SKILL.md`, no Python,
no scripts, no runtime dependency, and **no bundled data**. It does not carry its
own copy of the routing layer. Instead it reads **everything** — `sites.json`,
`index/`, and the endpoint doc `docs/` — from **this repository, cloned by you**.

Point the skill at your clone once via a fixed config file at
`~/.okx-v5-api/config.json` with a `docs_path` field naming the checkout. The
skill then resolves both routing and bodies under that path:

1. **Route**: from `docs_path`, read `sites.json` + `index/` to find the site
   domain, per-site support, and the target `doc` path.
2. **Read the body**: open `docs_path/docs/<site>/<lang>/<doc>`.

If `~/.okx-v5-api/config.json` or the clone is missing, set up the clone and
point `docs_path` at it before using the skill.

## Provenance & disclaimer

Generated from the upstream OKX V5 API documentation source; see `version` and
`generated_at` in `sites.json` / `index.json` for the exact release. For the
authoritative, always-current reference, see the official OKX API documentation.
This mirror is provided to make the docs consumable by AI tooling; verify against
the official docs before relying on it in production.
