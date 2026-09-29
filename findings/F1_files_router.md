# F1: Files Router Cross-User Access — FIXED

**Target:** Open WebUI v0.3.16 | **Date:** 2026-09-24
**Vector tested:** GET /api/v1/files/{victim_file_id}[/content|/data] with
attacker (test2) Bearer token. Attacker token validity proven via
GET /api/v1/files/ → 200 [] (authenticated, user-scoped listing).

## Results
- /files/{id}/content → 404 "We could not find what you're looking for :/"
- /files/{id}/data    → 404 generic (route absent)
- /files/ (listing)   → 200 [] — victim's file absent from attacker's scope
- Control (victim's own token, own file): 200 + full content

## Verdict: FIXED
Denial style: existence-hiding (404, not 403) — no metadata leak, no
enumeration oracle. Consistent with the cross-user file fix noted in the
0.3.16 changelog.

## Limitations
Tested with one victim file ID on one instance; file-derived routes
(knowledge collections) were not exhaustively enumerated.
