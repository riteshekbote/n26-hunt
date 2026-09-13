# Validated findings (running count 0)

- 1 lead(s) marked VALID at 2026-09-05 17:37:21 UTC
  - **Verdict: HOLD** — RBAC bypass on `sdk_exception` confirmed, but impact ceiling is LOW (public client key, no PII/funds). Needs evidence of actual sensitive flag data being disclosed. If `/v1/downloa

- 2 lead(s) marked VALID at 2026-09-07 06:12:37 UTC
  - | **Q4 Provable** | HOLD — requires valid Bearer key which is not in web CSP/bundles; cannot prove non-invasively |
  - | engagementplatform.n26.com/users | **HOLD** | Needs valid key path discovery |

- 2 lead(s) marked VALID at 2026-09-07 12:47:54 UTC
  - | Q4 Provable | **No** — requires valid Bearer key not found in web CSP/bundles |
  - | 7 | `engagementplatform.n26.com/users` | **HOLD** | Needs valid key path |

- 4 lead(s) marked VALID at 2026-09-13 06:48:50 UTC
  - | Q5 Novel | **NO** — POST /v1/initialize returning data when given a valid public client key is the standard Statsig SDK flow. Not a bypass. |
  - | Q5 Novel | **NO** — `/v1/initialize` accepting a valid public client key via POST is the documented Statsig client SDK flow |
  - | Q4 Provable | **NO** — Requires valid Bearer key not found in web CSP/bundles. 10 web bundles searched, no token. CORS is open but no web-accessible key path identified. Server/mobile-only API. |
  - **Verdict: HOLD** — Distinct auth boundary confirmed (401 key-gated, not tarpit). CORS-open suggests server/mobile integration. But no valid Bearer key path found in web bundles or CSP. Needs valid ke
