# affiliate — OKX V5 API endpoints

Supported per site (✓ = available, ✗ = not on that site). The `doc` column is a REFERENCE path (valid for most sites) — the exact per-site/per-lang path can differ, so read `index.json` `sites.<site>.doc.<lang>` for the authoritative path, then open `docs/<site>/<lang>/<that>`. Frontmatter carries method/path/rest/ws.

| method | path | name | doc (ref) | okex | us | my | turkey |
|---|---|---|---|:-:|:-:|:-:|:-:|
| GET | /api/v5/affiliate/co-inviter/list | Get co-inviter link list | api/rest/affiliate/coInviterList.md | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/affiliate/invitee/detail | Get the invitee's detail | api/rest/affiliate/inviteeDetail.md | ✓ | ✓ | ✓ | ✓ |
| GET | /api/v5/affiliate/invitee/list | Get invitee list | api/rest/affiliate/inviteeList.md | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/affiliate/link/list | Get link list | api/rest/affiliate/linkList.md | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/affiliate/performance/summary | Get performance summary | api/rest/affiliate/performanceSummary.md | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/affiliate/sub-affiliate/list | Get sub-affiliate list | api/rest/affiliate/subAffiliateList.md | ✓ | ✗ | ✓ | ✗ |
| GET | /api/v5/affiliate/tvb/invitee/detail | Get TVB invitee detail | api/rest/affiliate/tvbInviteeDetail.md | ✗ | ✗ | ✓ | ✗ |
| GET | /api/v5/affiliate/tvb/invitee/list | Get TVB invitee list | api/rest/affiliate/tvbInviteeList.md | ✗ | ✗ | ✓ | ✗ |
| GET | /api/v5/affiliate/tvb/link/list | Get TVB link list | api/rest/affiliate/tvbLinkList.md | ✗ | ✗ | ✓ | ✗ |
| GET | /api/v5/affiliate/tvb/performance/summary | Get TVB performance summary | api/rest/affiliate/tvbPerformanceSummary.md | ✗ | ✗ | ✓ | ✗ |
| GET | /api/v5/affiliate/tvb/tier-breakdown | Get TVB bonus breakdown by fee tier | api/rest/affiliate/tvbTierBreakdown.md | ✗ | ✗ | ✓ | ✗ |
