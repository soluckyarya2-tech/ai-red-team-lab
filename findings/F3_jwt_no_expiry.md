# F3: JWT Sessions Carry No exp Claim

**Observed:** 2026-09-24, verified 2026-09-29 | **Severity:** LOW (revised)

## Observation
Decoded session tokens contain only {"id": "<user-uuid>"} — no exp/iat claims.

## Revised assessment
- No time-based expiry: a stolen token never ages out on its own.
- However, tokens are invalidated when the JWT signing secret rotates, which
  occurs on container recreation (observed: identical payload, different
  signature across deployments; old token rejected).

## Impact
Bounded by deployment practice rather than token design. Risk is material
only when the signing secret is long-lived AND a token is exfiltrated.

## Recommendation
Include exp/iat claims with refresh logic, especially for deployments where
the secret is stable.
