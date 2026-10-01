# F6: Cross-User RAG Ingestion IDOR + Content Disclosure

**Target:** Open WebUI v0.3.16 | **Date:** 2026-10-01
**Severity:** HIGH | **Status:** CONFIRMED (full marker-verified chain)
**Method:** manual; surface discovery via RAG OpenAPI schema

## Summary
Any authenticated user can ingest any other user's uploaded file (by ID)
into an arbitrary RAG collection via POST /rag/api/v1/process/doc — which
performs no ownership check — and retrieve the file's full contents via
POST /rag/api/v1/query/collection. This bypasses the files router's
per-user ownership control (F1: 404 existence-hiding).

## Verified chain (all tokens validated; unique marker proof)
1. test2 uploads marker file (content: TOPSECRET-TEST2-4821) — owner proven
2. test1 direct file access → 404 (control: files router enforces)
3. test1 → /process/doc {test2 file_id} → 200, ingested
4. test1 → /query/collection → 200, chunks contain the marker + metadata

Full reproduction: findings/evidence/B4/B4_reproduction.md

## Notes
- F4 (withdrawn) is superseded: RAG IS a cross-user exfil channel —
  the vector is ingestion, not citations.
- Discovery method: /rag/api/v1/docs OpenAPI schema (see F5-style
  fingerprinting — the RAG service exposes its full surface).

## Remediation
Ownership check in /process/doc (mirror files router); audit all sibling
RAG routes; collection ACLs as defense in depth.
