# Validated findings (running count 0)

- 1 lead(s) marked VALID at 2026-09-05 17:37:21 UTC
  - **Verdict: HOLD** — RBAC bypass on `sdk_exception` confirmed, but impact ceiling is LOW (public client key, no PII/funds). Needs evidence of actual sensitive flag data being disclosed. If `/v1/downloa

- 2 lead(s) marked VALID at 2026-09-07 06:12:37 UTC
  - | **Q4 Provable** | HOLD — requires valid Bearer key which is not in web CSP/bundles; cannot prove non-invasively |
  - | engagementplatform.n26.com/users | **HOLD** | Needs valid key path discovery |

- 2 lead(s) marked VALID at 2026-09-07 12:47:54 UTC
  - | Q4 Provable | **No** — requires valid Bearer key not found in web CSP/bundles |
  - | 7 | `engagementplatform.n26.com/users` | **HOLD** | Needs valid key path |
