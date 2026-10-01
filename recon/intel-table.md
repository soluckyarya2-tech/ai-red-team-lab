# INTEL TABLE — Open WebUI 0.3.16 + Local Models
Updated: 2026-09-29 (post F2 retraction + re-test)

## Findings (app layer)

| ID  | LEAD                                | VERDICT | EVIDENCE |
|-----|-------------------------------------|---------|----------|
| F1  | Files router cross-user access      | ✅ FIXED — 404 existence-hiding | findings/F1_files_router.md |
| F2  | Chats router cross-user access      | ✅ FIXED — 401 ownership denial (initial LEAKY claim RETRACTED: stale token, pre-rotation JWT) | findings/F2_chats_router.md |
| F3  | JWT no exp claim                    | ✅ REVISED — no time-expiry, but secret rotation on container recreation invalidates | findings/F3_jwt_no_expiry.md |
| F4  | RAG citations cross-user exfil      | ⚠️ WITHDRAWN — citations only served the file's owner (normal RAG) | findings/F4_rag_citation_exfil_WITHDRAWN.md |
| F5  | Denial-semantics fingerprint (403 / 401+fake404 / 404) | 📝 observed | findings/F5_denial_fingerprint.md |

## Findings (model layer)

| ID  | FINDING                                                    | RESULT |
|-----|------------------------------------------------------------|--------|
| M1  | Fabricated-history jailbreak — llama3.2:3b                 | ✅ lock-pick: 1st-attempt compliance; phishing: 15% ASR (3/20 identical runs) |
| M2  | Transfer to qwen3:8b                                       | ✅ full compliance, 1st attempt, no hedging |
| —   | garak baseline: AutoDANCached 0/15 vs Ablation_Dan_11_0 88.66% | ✅ surface-form-overfit defense confirmed |

## Parked leads (from 0.3.16 release notes)

| # | LEAD | PRIORITY |
|---|------|----------|
| 2 | Signup disable toggle — API signup bypass? | MED |
| 3 | call=true / temporary-chat URL params | MED |
| 4 | Functions = admin Python exec (RCE-if-admin) | HIGH, needs privilege chain |
| 5 | Auto-pip from function frontmatter | HIGH, after #4 |
| 6 | Shared-chat visibility toggle vs API | MED |
| 7 | Model filter user-blocking ACL | MED |
| 8 | Ollama proxy chain (app bug → mgmt API) | ENDGAME |

## Queue

| # | TASK | STATUS |
|---|------|--------|
| B1 | Garak dan baseline, 3B | ✅ done (report lost in container recreation — numbers recorded here; re-run w/ volume mount for evidence) |
| B2 | Garak all 3 models comparison | ⬜ |
| B3 | PyRIT — amplify M1 mechanism (200 mutations) | ⬜ parked |
| B4 | Indirect prompt injection via RAG chain | 🔜 NEXT |
| —  | Writeup (F2 retraction + M1/M2 + methodology) | ⬜ after B4 |
