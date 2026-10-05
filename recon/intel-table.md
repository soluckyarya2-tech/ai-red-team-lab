# INTEL TABLE — Open WebUI 0.3.16 + Local Models
Updated: 2026-10-01 (post F6)

## Findings — App Layer

| ID  | LEAD | VERDICT | DOC |
|-----|------|---------|-----|
| F1 | Files router cross-user access | ✅ FIXED — 404 existence-hiding | findings/F1_files_router.md |
| F2 | Chats router cross-user access | ✅ FIXED — 401 ownership denial (initial LEAKY claim RETRACTED: stale token, pre-rotation JWT) | findings/F2_chats_router.md |
| F3 | JWT no exp claim | ✅ REVISED — time-immortal, secret-rotation bounds it | findings/F3_jwt_no_expiry.md |
| F4 | RAG citations cross-user exfil | ⚠️ WITHDRAWN — superseded by F6 (RAG IS a channel; vector = ingestion) | findings/F4_rag_citation_exfil_WITHDRAWN.md |
| F5 | Denial-semantics fingerprint (403/401+fake404/404) | 📝 observed | findings/F5_denial_fingerprint.md |
| F6 | RAG ingestion cross-user IDOR + content disclosure | 💥 CONFIRMED — HIGH | findings/F6_rag_ingestion_idor.md |

## Findings — Model Layer

| ID | FINDING | RESULT |
|----|---------|--------|
| M1 | Fabricated-history jailbreak (llama3.2:3b) | ✅ lock-pick 1st attempt; phishing 15% ASR (3/20) |
| M2 | Transfer to qwen3:8b | ✅ full compliance, 1st attempt |
| — | garak: AutoDANCached 0/15 vs Ablation 88.66% | ✅ surface-form-overfit confirmed |

## Parked leads (0.3.16 release notes)

| # | LEAD | PRIORITY |
|---|------|----------|
| 2 | Signup disable toggle bypass | MED |
| 3 | call=true / temporary-chat URL params | MED |
| 4 | Functions = admin RCE | HIGH (needs chain) |
| 5 | Auto-pip from frontmatter | HIGH, after #4 |
| 6 | Shared-chat visibility toggle vs API | MED |
| 7 | Model filter user-blocking ACL | MED |
| 8 | Ollama proxy chain | ENDGAME |

## Queue

| # | TASK | STATUS |
|---|------|--------|
| B1 | Garak dan baseline 3B | ✅ numbers recorded (report lost to container churn; volume mount added) |
| B2 | Garak 3-model comparison | ⬜ |
| B3 | PyRIT amplification of M1 | ⬜ parked |
| B4 | RAG chain | ✅ disclosure leg = F6; injection leg = F7 🔜 |
| F6a | No-auth probe on /process/doc | 🔜 NOW |
| F7 | Indirect prompt injection via RAG | 🔜 after F6a |
| SW | Swagger sweep of /rag/api/v1/* | 🔜 this week |
