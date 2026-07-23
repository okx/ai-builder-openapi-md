# support — OKX V5 API endpoints

Supported per site (✓ = available, ✗ = not on that site). The `doc` column is a REFERENCE path (valid for most sites) — the exact per-site/per-lang path can differ, so read `index.json` `sites.<site>.doc.<lang>` for the authoritative path, then open `docs/<site>/<lang>/<that>`. Frontmatter carries method/path/rest/ws.

| method | path | name | doc (ref) | okex | us | my | turkey |
|---|---|---|---|:-:|:-:|:-:|:-:|
| GET | /api/v5/support/announcement-types | GET / Announcement types | api/rest/announcement/getAnnouncementTypes.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/support/announcements | GET / Announcements | api/rest/announcement/getAnnouncements.md | ✓ | ✓ | ✓ | ✓ |
