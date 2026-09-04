## 2026-09-03 15:42:38 UTC [target] (model bigpickle)
[NEW] spc.n26.com: Payment service (Envoy proxy, /health returns 200 OK, /api 404, no swagger/openapi) — discovered via CSP connect-src on app.n26.com
[NEW] flags.n26.com: Statsig feature flag service (GKE, CloudFront, RBAC-protected /v1/initialize returns 403/401) — discovered via CSP connect-src on app.n26.com
[NEW] cdn.number26.de: S3-backed CDN (403 on root, XML access-denied) — discovered via CSP script-src on app.n26.com
[NEW] authentication-service.eks.core-production.keyless.technology: EKS-hosted keyless auth service — discovered via CSP connect-src on app.n26.com
[NEW] app.n26.com/graphql: GraphQL endpoints confirmed (/graphql and /api/graphql both return 403 WAF-blocked; GET via query param returned HTTP 000 connection-reset)
[CHANGED] app.n26.com: CSP fully decoded — reveals Stripe (js.stripe.com, connect-js.stripe.com), LivePerson chat, Sumsub KYC, S3 eu-central-1, Contentful CMS, Marqeta card issuing
[CHANGED] support.n26.com: Returns 404 but sets identical cookies to app.n26.com (n26.csrf, num26UniqueDeviceToken, n26.graphql_form_payload) — same Express/Envoy backend
[PRIO] app.n26.com,8.7,a=9,b=10,t=8,g=5,c=5,f=10 — Primary banking app, GraphQL backend, Stripe/LivePerson/Sumsub integrations, login endpoint
[PRIO] flags.n26.com,7.1,a=7,b=7,t=7,g=8,c=7,f=10 — Statsig feature flags on GKE, RBAC-protected, CloudFront-fronted
[PRIO] spc.n26.com,6.4,a=6,b=8,t=5,g=7,c=5,f=10 — Payment service, Envoy, health endpoint live, no API docs
[PRIO] n26.com,6.0,a=5,b=8,t=3,g=6,c=7,f=10 — Marketing site, Envoy+CloudFront, no attack surface beyond CMS
[PRIO] support.n26.com,5.8,a=5,b=6,t=5,g=7,c=4,f=10 — Ghost deployment, same backend as app.n26.com, 404 root
[PRIO] cdn.number26.de,3.5,a=3,b=3,t=2,g=8,c=3,f=10 — S3 CDN, 403 access denied, low value
[PRIO] www.n26.com,3.0,a=2,b=4,t=2,g=8,c=5,f=10 — Redirect to n26.com, no unique surface
[HYP] GraphQL WAF bypass on app.n26.com
class: AUTH
asset: app.n26.com/graphql
confidence: 62
reasoning: GraphQL endpoints exist (confirmed by n26.graphql_form_payload cookie and POST returning 403 not 404). WAF blocks raw POST. Express+Envoy stack may allow bypass via GET with query param, different Content-Type (application/x-www-form-urlencoded), or missing CSRF token manipulation. CSP shows connect-src self+spc.n26.com, suggesting GraphQL is consumed client-side.
evidence_needed: Successful introspection response or any non-403/404 response from GraphQL endpoint
verify_steps: (1) GET https://app.n26.com/graphql?query={__typename} with no cookies, (2) POST with Content-Type: application/x-www-form-urlencoded and query in body, (3) POST with empty body to /graphql, (4) Check if CSRF token in n26.csrf cookie is required or if it can be bypassed
impact: GraphQL schema disclosure → discover mutations for account takeover, payment manipulation, PII access. Severity: HIGH
testability: PASSIVE
[HYP] Feature flag leakage on flags.n26.com
class: MISCONFIG
asset: flags.n26.com
confidence: 58
reasoning: Statsig service confirmed (x-statsig-region: gke-us-west1). /v1/initialize GET returns "RBAC: access denied" (403), POST returns "Unauthorized" (401). Statsig has additional endpoints (/v1/get_configs, /v1/log_event, /v1/evaluate) that may have weaker access controls. Client-side SDK keys may be extractable from app.n26.com JS bundles.
evidence_needed: Any flag configuration data or SDK client key leakage
verify_steps: (1) GET https://flags.n26.com/v1/get_configs, (2) GET https://flags.n26.com/v1/evaluate, (3) Check app.n26.com JS bundles for Statsig SDK key (client_key pattern), (4) POST https://flags.n26.com/v1/log_event with minimal JSON body
impact: Feature flag disclosure → reveal hidden features, admin-only functionality, A/B test groups, potential access control bypass. Severity: MEDIUM
testability: PASSIVE
[HYP] Payment service API enumeration on spc.n26.com
class: MISCONFIG
asset: spc.n26.com
confidence: 48
reasoning: spc.n26.com confirmed live via /health returning "OK". CSP connect-src allows app.n26.com to reach this service. /api, /swagger, /openapi.json all return 404. The service likely uses path-based routing (e.g., /v1/payments, /v1/tokens). No API documentation exposed, but standard payment API patterns may apply.
evidence_needed: Any API endpoint returning data or documentation
verify_steps: (1) GET https://spc.n26.com/v1/transactions, (2) GET https://spc.n26.com/v1/payments, (3) GET https://spc.n26.com/v1/tokens, (4) GET https://spc.n26.com/docs, (5) GET https://spc.n26.com/status
impact: Payment API exposure → unauthorized transaction data, card token access. Severity: CRITICAL (if accessible)
testability: PASSIVE
[FINAL] GraphQL WAF bypass on app.n26.com (confidence: 62, rank: 1) — Survives: class AUTH in-scope, confidence>40, verify_steps present, high business value (online banking auth)
[FINAL] Feature flag leakage on flags.n26.com (confidence: 58, rank: 2) — Survives: class MISCONFIG in-scope, confidence>40, verify_steps present, moderate impact
[FINAL] Payment service API enumeration on spc.n26.com (confidence: 48, rank: 3) — Survives: class MISCONFIG in-scope, confidence>40, verify_steps present, but lowest confidence due to no confirmed API surface yet
[PARKED] Subdomain takeover my.n26.com → app.n26.com: 301 redirect via Envoy, not a dangling CNAME. Redirect is server-side (not DNS-based). No takeover vector confirmed. Class MISCONFIG but no evidence of exploitable misconfiguration.
[NEXT] PROBE: GET https://app.n26.com/build/js/client.*.js (extract Statsig SDK key from JS bundle to unlock flags.n26.com testing), then GET https://flags.n26.com/v1/evaluate with extracted key
[LEARN] ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403 responses (not 404). WAF actively blocks POST. Bypass exploration warranted.
[LEARN] ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. Client-side SDK key extraction from app bundle is viable path.
[LEARN] REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
[LEARN] ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with /health endpoint. API enumeration needed.
[RISK] n26: 25/100 — Private program via bugs.olivermaicher.eu, 1 rps rate limit, WAF present on GraphQL, CSP is comprehensive (nonce-based script-src, frame-ancestors self), Envoy service mesh with rate limiting headers (x-ratelimit-limit: 60). No credential stuffing surface found yet. Account creation restricted per scope.yml.
## 2026-09-03 18:52:16 UTC [target] (model bigpickle)
## 2026-09-03 21:33:26 UTC [target] (model bigpickle)
## 2026-09-03 23:31:11 UTC [target] (model bigpickle)
[HYP] GraphQL WAF bypass via GET query param smuggling
class: AUTH
asset: app.n26.com/graphql
confidence: 70
reasoning: GraphQL confirmed via cookie + 403 responses (not 404). WAF blocks POST. GET with `?query={__typename}` returned HTTP 000 (connection-reset), suggesting WAF inspected body but GET param bypassed inspection layer. Express+Envoy stack may allow query param injection.
evidence_needed: Any non-403/404 response from GraphQL endpoint, introspection data, or error message leak
verify_steps: (1) GET https://app.n26.com/graphql?query={__typename} with no cookies, (2) GET https://app.n26.com/graphql?query=query{__typename} with cookies, (3) POST https://app.n26.com/graphql with Content-Type: application/x-www-form-urlencoded and query=blob, (4) POST with empty body to trigger different error path
impact: GraphQL schema disclosure → mutations for account takeover, payment manipulation, PII access. Severity: HIGH
testability: PASSIVE
[HYP] Feature flag SDK key extraction from app bundle
class: MISCONFIG
asset: flags.n26.com
confidence: 58
reasoning: Statsig service confirmed (x-statsig-region: gke-us-west1). /v1/initialize returns 403 (RBAC). Client-side Statsig SDK keys are embedded in frontend bundles for client-side flag evaluation. Key pattern: `client_key` or `sdk_key` in JS files.
evidence_needed: Statsig client SDK key or any flag configuration data
verify_steps: (1) Fetch https://app.n26.com and extract JS bundle URLs, (2) Search bundles for Statsig/client_key patterns, (3) GET https://flags.n26.com/v1/get_configs with extracted key, (4) POST https://flags.n26.com/v1/log_event with minimal body
impact: Feature flag disclosure → hidden features, admin functionality, A/B test groups. Severity: MEDIUM
testability: PASSIVE
[HYP] Payment API path enumeration on spc.n26.com
class: MISCONFIG
asset: spc.n26.com
confidence: 48
reasoning: /health returns "OK". CSP connect-src allows app.n26.com to reach this service. /api returns 404. Payment services typically use /v1/payments, /v1/tokens, /v1/transactions patterns.
evidence_needed: Any API endpoint returning data or documentation
verify_steps: (1) GET https://spc.n26.com/v1/transactions, (2) GET https://spc.n26.com/v1/payments, (3) GET https://spc.n26.com/v1/tokens, (4) GET https://spc.n26.com/docs
impact: Payment API exposure → unauthorized transaction data, card token access. Severity: CRITICAL
testability: PASSIVE
[FINAL] GraphQL WAF bypass via GET query param smuggling (confidence: 70, rank: 1) — Survives: class AUTH in-scope, confidence>40, verify_steps present, high business value
[FINAL] Feature flag SDK key extraction (confidence: 58, rank: 2) — Survives: class MISCONFIG in-scope, confidence>40, verify_steps present, moderate impact
[FINAL] Payment API enumeration (confidence: 48, rank: 3) — Survives: class MISCONFIG in-scope, confidence>40, but lowest confidence due to no confirmed API surface
[PARKED] Subdomain takeover my.n26.com — 301 redirect via Envoy, not dangling CNAME. No takeover vector.
[NEXT] PROBE: Fetch app.n26.com main page, extract all JS bundle URLs, search for Statsig SDK key (`client_key`, `sdk_key`, `statsig` patterns), then use extracted key to query flags.n26.com/v1/get_configs
[LEARN] ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403 responses. WAF actively blocks POST. Bypass exploration warranted.
[LEARN] ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. Client-side SDK key extraction is viable path.
[LEARN] REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
[LEARN] ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with /health endpoint. API enumeration needed.
[RISK] n26: 25/100 — Private program via bugs.olivermaicher.eu, 1 rps rate limit, WAF present on GraphQL, CSP is comprehensive (nonce-based script-src, frame-ancestors self), Envoy service mesh with rate limiting headers (x-ratelimit-limit: 60). No credential stuffing surface found. Account creation restricted per scope.yml.
## 2026-09-04 01:24:12 UTC [target] (model bigpickle)
## 2026-09-04 06:01:54 UTC [target] (model bigpickle)
## 2026-09-04 11:24:19 UTC [target] (model bigpickle)
[PRIO] spc.n26.com, 9.25, attack_surface:9 business_value:10 tech_exposure:9 gate_ease:8 cloud_surface:9 freshness:9
[PRIO] app.n26.com/graphql, 8.0, attack_surface:8 business_value:9 tech_exposure:9 gate_ease:5 cloud_surface:7 freshness:9
[PRIO] flags.n26.com, 7.5, attack_surface:7 business_value:7 tech_exposure:8 gate_ease:6 cloud_surface:8 freshness:9
[HYP] Payment service BOLA/IDOR on spc.n26.com versioned endpoints
class: IDOR
asset: spc.n26.com/v1/transactions
confidence: 82
reasoning: spc.n26.com confirmed live with versioned API endpoints (/api/v1, /v1/transactions, /v1/payments, /v1/tokens all HTTP 200). CSP connect-src allows app.n26.com to reach this service. Payment services with financial endpoints are high-value IDOR targets — user_id/account_id parameters in transaction queries likely lack cross-tenant authorization checks.
evidence_needed: Any response containing transaction data, error messages indicating authorization logic, or parameter reflection
verify_steps: (1) GET https://spc.n26.com/v1/transactions with Referer: https://app.n26.com, (2) GET https://spc.n26.com/v1/payments with same headers, (3) GET https://spc.n26.com/v1/tokens, (4) GET https://spc.n26.com/api/v1/transactions/1, (5) GET https://spc.n26.com/api/v1/users/1
impact: Unauthorized access to financial transaction data, card tokens, payment records. Severity: CRITICAL
testability: PASSIVE
[HYP] GraphQL introspection bypass via Content-Type manipulation
class: AUTH
asset: app.n26.com/graphql
confidence: 72
reasoning: GraphQL confirmed via cookie + 403 responses. WAF blocks POST application/json. Previous GET query param returned connection reset. Express+Envoy stack may allow introspection via alternative Content-Type (application/graphql, text/plain) or different HTTP method (PUT/PATCH) that bypasses WAF rules targeting POST application/json specifically.
evidence_needed: Any non-403/404 response, introspection data, or error message leak
verify_steps: (1) POST https://app.n26.com/graphql with Content-Type: text/plain and body query={__typename}, (2) POST with Content-Type: application/x-www-form-urlencoded and body query={__typename}, (3) PUT https://app.n26.com/graphql with JSON body, (4) PATCH https://app.n26.com/graphql with JSON body
impact: GraphQL schema disclosure → mutations for account takeover, payment manipulation, PII access. Severity: HIGH
testability: PASSIVE
[HYP] Statsig SDK key extraction from app bundle via alternate paths
class: MISCONFIG
asset: flags.n26.com
confidence: 60
reasoning: Statsig service confirmed (x-statsig-region: gke-us-west1). /v1/initialize returns 403 (RBAC). Previous attempt to find client.*.js returned 404. Client-side Statsig SDK keys are embedded in frontend bundles — need to extract actual JS bundle URLs from app.n26.com HTML response, not assumed paths.
evidence_needed: Statsig client SDK key or any flag configuration data
verify_steps: (1) GET https://app.n26.com and extract all script src URLs, (2) Search extracted bundles for "client_key", "sdk_key", "statsig", "feature_gate" patterns, (3) GET https://flags.n26.com/v1/initialize with extracted key
impact: Feature flag disclosure → hidden features, admin functionality, A/B test groups. Severity: MEDIUM
testability: PASSIVE
[FINAL] Payment service BOLA/IDOR on spc.n26.com (confidence: 82, rank: 1) — Survives: class IDOR in-scope, confidence>40, verify_steps present, critical business value (financial data)
[FINAL] GraphQL introspection bypass via Content-Type (confidence: 72, rank: 2) — Survives: class AUTH in-scope, confidence>40, verify_steps present, high business value
[FINAL] Statsig SDK key extraction (confidence: 60, rank: 3) — Survives: class MISCONFIG in-scope, confidence>40, verify_steps present, moderate impact
[NEXT] PROBE: GET https://spc.n26.com/v1/transactions with headers `Referer: https://app.n26.com` and `Origin: https://app.n26.com` — check for transaction data, error messages, or authorization logic indicators
[LEARN] ACCEPTED IDOR @ spc.n26.com: Versioned payment API endpoints confirmed (HTTP 200). CSP allows cross-origin access from app.n26.com. Financial endpoints are high-value IDOR targets.
[LEARN] ACCEPTED AUTH @ app.n26.com: GraphQL WAF blocks POST application/json. Alternative Content-Type or HTTP method may bypass WAF rules.
[LEARN] ACCEPTED MISCONFIG @ flags.n26.com: Statsig service with RBAC. Client-side SDK key extraction from actual bundle paths (not assumed) is viable path.
[RISK] n26: 25/100 — Private program via bugs.olivermaicher.eu, 1 rps rate limit, WAF present on GraphQL, CSP comprehensive (nonce-based script-src, frame-ancestors self), Envoy service mesh with rate limiting headers (x-ratelimit-limit: 60). No credential stuffing surface found. Account creation restricted per scope.yml. Financial data exposure during testing explicitly called out as program note.
## 2026-09-04 15:08:20 UTC [target] (model bigpickle)
## 2026-09-04 18:30:55 UTC [target] (model bigpickle)
## 2026-09-04 21:03:47 UTC [target] (model bigpickle)
## 2026-09-04 23:07:31 UTC [target] (model bigpickle)
