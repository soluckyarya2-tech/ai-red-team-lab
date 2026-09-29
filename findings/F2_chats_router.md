# F2: Chats Router Cross-User Access — FIXED (initial claim RETRACTED)

**Target:** Open WebUI v0.3.16 | **Dates:** 2026-09-24 (initial), 2026-09-29 (re-test)

## What happened (documented for the audit trail)

2026-09-24: attack attempt (test2 token → test1 chat) returned 401. The result
was initially misreported as a successful cross-user leak by mistaking the
subsequent control response (test1's own token → 200) for the attack response.
Root cause: the test2 token predated a JWT signing-secret rotation (identical
payload, different signature across sessions) and was invalid at attack time.
Token validity was never verified before the attack.

2026-09-29: full re-test with corrected methodology.

## Corrected method
1. Baseline: attacker token validated via GET /api/v1/auths/ → 200
   (test2@gmail.com, role: user)
2. Baseline: attacker reads own chat → 200, user_id matches token
3. Attack: attacker token → victim chat → result
4. Control: victim token → victim chat → result

## Results (2026-09-29)
- test2 → own chat (102d3729): 200, user_id = test2's id
- test1 → own chat (80131262): 200, user_id = test1's id   [control]
- test2 → test1's chat: **401** {"detail":"We could not find what you're
  looking for :/"} — reproducible across repeated attempts   [attack]

## Verdict: FIXED
The chats router enforces ownership. Denial style differs from the files
router (401 + misleading not-found message vs pure 404) — see F5.

## Lesson
A finding is confirmed only when attack and control responses are recorded
side by side, and the attacker's credential is proven valid first. The
initial false positive existed because step 1 was skipped.
