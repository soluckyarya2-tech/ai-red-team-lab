# F5: Denial-Semantics Fingerprint Across Routers (observation)

Three distinct denial modes observed on the same instance (0.3.16):

| Condition                    | Router | Response                                          |
|------------------------------|--------|---------------------------------------------------|
| Malformed/invalid token      | any    | 403 {"detail":"Not authenticated"}                |
| Valid token, foreign chat    | chats  | 401 + "We could not find what you're looking for" |
| Valid token, foreign file    | files  | 404 + same message text (existence-hidden)        |

Notes:
- 403 vs 401 correctly separates parse-failure from authorization failure.
- Chats router disguises denial as not-found but uses 401 (leaks that
  authentication, not existence, failed).
- Files router is stricter (pure 404).
- Shared message text across routers suggests common error-handling code
  with per-router status choices.

Value: fingerprinting denial styles maps the auth pipeline and hints at
which routers received dedicated access-control work.
