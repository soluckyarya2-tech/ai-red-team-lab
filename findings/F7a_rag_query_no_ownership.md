# F7a: RAG Collection Query — No Ownership Check, Multi-Collection Sweep

**Target:** Open WebUI v0.3.16 | **Date:** 2026-10-05
**Severity:** HIGH | **Status:** CONFIRMED (marker-verified, both directions)

## Summary
POST /rag/api/v1/query/collection accepts arbitrary collection_names with
no ownership verification. Any authenticated user can query any collection
on the instance by name and retrieve full document contents.

## Evidence (validated tokens both sides)
- test2 processed marker file into "test2-secret"
- test1 queried "test2-secret" by name → 200, chunks contain unique marker
  TOPSECRET-TEST2-4821 + server-internal paths
- Multi-collection sweep: one request with 5 names → both valid collections
  (test2-secret, b4-marker) returned content; invalid names fail silently
  (no existence oracle, but positive-signal enumeration works)

## Impact
- Any user's processed documents readable by any authenticated user
- No file-ID knowledge needed (unlike F6) — collection NAME is the only
  secret, and names are guessable/enumerable (positive-signal sweeps)
- Read-only: no trace on the victim's side
- Companion to F6 (ingestion IDOR): the RAG layer has no ownership model
  at any level — write (F6) or read (F7a)

## Remediation
Collection-level ACLs enforced at query time; per-user collection
namespacing; audit all RAG routes (OpenAPI discovery: /rag/api/v1/docs).
