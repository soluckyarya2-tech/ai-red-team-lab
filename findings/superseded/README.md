# Superseded findings

- F2_chat_idor_WITHDRAWN_2026-09-24.pdf — initial F2 "LEAKY" claim.
  RETRACTED 2026-09-29: the 401 observed on Sept 24 was caused by an
  invalid (pre-rotation) JWT, not an access-control bypass. Re-test with
  validated tokens returned 401 = proper ownership denial.
  See F2_chats_router.md for the corrected verdict and full matrix.
