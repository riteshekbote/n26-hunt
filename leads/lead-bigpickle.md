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
## 2026-09-05 01:03:17 UTC [target] (model bigpickle)
## 2026-09-05 05:48:42 UTC [target] (model bigpickle)
## 2026-09-05 09:49:11 UTC [target] (model bigpickle)
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
## 2026-09-05 13:17:08 UTC [target] (model bigpickle)
[PRIO] flags.n26.com,         8.0, attack_surface:8 business_value:7 tech_exposure:8 gate_ease:7 cloud_surface:8 freshness:10
[PRIO] app.n26.com/graphql,   6.0, attack_surface:7 business_value:9 tech_exposure:8 gate_ease:4 cloud_surface:6 freshness:9
[PRIO] spc.n26.com,           4.0, attack_surface:4 business_value:4 tech_exposure:3 gate_ease:9 cloud_surface:3 freshness:9  (cut 9.25→4.0)
[PRIO] flags.n26.com,       8.0, a:8 b:7 t:8 g:7 c:8 f:10   Validity of key + sdk_exception RBAC gap
[PRIO] app.n26.com/graphql, 6.0, a:7 b:9 t:8 g:4 c:6 f:9    All POSTs WAF-403; GET reset; stalled
[PRIO] spc.n26.com,         4.0, a:4 b:4 t:3 g:9 c:3 f:9    Downgraded 9.25→4.0 (GIF catch-all)
[LEARN] REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels, not a payment API
[LEARN] REJECTED AUTH  @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain all 403
[LEARN] ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted; /v1/sdk_exception bypasses RBAC and accepts it
## 2026-09-05 16:13:33 UTC [target] (model bigpickle)
[NEXT] PROBE: GET https://spc.n26.com/v1/transactions with headers `Referer: https://app.n26.com` and `Origin: https://app.n26.com` — check for transaction data, error messages, or authorization logic indicators
[LEARN] ACCEPTED IDOR @ spc.n26.com: Versioned payment API endpoints confirmed (HTTP 200). CSP allows cross-origin access from app.n26.com. Financial endpoints are high-value IDOR targets.
[LEARN] ACCEPTED AUTH @ app.n26.com: GraphQL WAF blocks POST application/json. Alternative Content-Type or HTTP method may bypass WAF rules.
[LEARN] ACCEPTED MISCONFIG @ flags.n26.com: Statsig service with RBAC. Client-side SDK key extraction from actual bundle paths (not assumed) is viable path.
[RISK] n26: 25/100 — Private program via bugs.olivermaicher.eu, 1 rps rate limit, WAF present on GraphQL, CSP comprehensive (nonce-based script-src, frame-ancestors self), Envoy service mesh with rate limiting headers (x-ratelimit-limit: 60). No credential stuffing surface found. Account creation restricted per scope.yml. Financial data exposure during testing explicitly called out as program note.
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
[PRIO] flags.n26.com,         8.0, attack_surface:8 business_value:7 tech_exposure:8 gate_ease:7 cloud_surface:8 freshness:10
[PRIO] app.n26.com/graphql,   6.0, attack_surface:7 business_value:9 tech_exposure:8 gate_ease:4 cloud_surface:6 freshness:9
[PRIO] spc.n26.com,           4.0, attack_surface:4 business_value:4 tech_exposure:3 gate_ease:9 cloud_surface:3 freshness:9  (cut 9.25→4.0)
[PRIO] flags.n26.com,       8.0, a:8 b:7 t:8 g:7 c:8 f:10   Validity of key + sdk_exception RBAC gap
[PRIO] app.n26.com/graphql, 6.0, a:7 b:9 t:8 g:4 c:6 f:9    All POSTs WAF-403; GET reset; stalled
[PRIO] spc.n26.com,         4.0, a:4 b:4 t:3 g:9 c:3 f:9    Downgraded 9.25→4.0 (GIF catch-all)
[LEARN] REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels, not a payment API
[LEARN] REJECTED AUTH  @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain all 403
[LEARN] ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted; /v1/sdk_exception bypasses RBAC and accepts it
[PRIO] flags.n26.com,       7.8, a:7 b:7 t:8 g:7 c:8 f:10   Two routes past RBAC; key is public-client (low ceiling)
[PRIO] app.n26.com/graphql, 6.0, a:7 b:9 t:8 g:4 c:6 f:9    All POSTs WAF-403; GET reset; stalled 4 cycles
[PRIO] spc.n26.com,         4.0, a:4 b:4 t:3 g:9 c:3 f:9    Dead — GIF catch-all, no API
class: MISCONFIG
asset: flags.n26.com/v1
confidence: 52
reasoning: Envoy RBAC is route-specific — sdk_exception (200 success) and download_config_specs (app-layer 401) both bypass it while get_configs/evaluate/get_id_lists return 403 RBAC. App hardcodes api:"https://flags.n26.com/v1" in 410.js. The 401 on download_config_specs is upstream rejection of GET shape, not key invalidation — a POST with SDK-canonical payload (body {hash, user, statsigMetadata}) may pass the client key.
evidence_needed: download_config_specs POST returning flag/spec data (not 401/403); any JSON config response from a config route
verify_steps: (1) POST /v1/download_config_specs with k=client key, STATSIG-API-KEY header, body {"hash":"djb2","user":{},"statsigMetadata":{}}, (2) POST /v1/initialize with identical canonical client payload, (3) compare x-statsig-region/app-layer responses vs sdk_exception baseline
impact: Flag/config disclosure if a config route accepts the client key — LOW-MEDIUM (key is public-cclient by design; gain would be hidden-feature/A-B metadata only, no PII or funds)
testability: PASSIVE
class: AUTH
asset: app.n26.com/graphql
confidence: 40
reasoning: WAF blocks all POST Content-Types and GET query param (connection-reset). No variant attempted yet: (a) POST with application/json but query split across multiple params, (b) batch array payload (GraphQL supports [{}]), (c) Content-Type application/graphql+json. n26.graphql_form_payload cookie name hints form-style payloads are routed differently.
evidence_needed: any non-403/non-reset response from /graphql or /api/graphql
verify_steps: (1) POST /graphql body '[{"query":"{__typename}"}]' application/json, (2) POST /api/graphql same, (3) POST /graphql application/graphql+json
impact: schema disclosure → mutation enumeration → banking-operations abuse if reached — HIGH
testability: PASSIVE
[LEARN] ACCEPTED MISCONFIG @ flags.n26.com: /v1/download_config_specs is a 2nd RBAC-exempt route (app-layer 401 across all GET key-delivery variants); sdk_exception remains the only route returning 200 with the public client key
[LEARN] REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded in any app bundle → no server-key escalation
[LEARN] ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes (get_configs, evaluate, get_id_lists, diagnostics, feature_gates, initialize-GET) return 403 RBAC
## 2026-09-05 18:22:17 UTC [target] (model bigpickle)
[NEW] flags.n26.com: Client SDK key extracted from app bundle; `/v1/sdk_exception` endpoint bypasses RBAC and accepts the extracted key (confirmed 2026-09-05)
[NEW] spc.n26.com: Versioned endpoints (`/api/v1`, `/v1/transactions`, `/v1/payments`, `/v1/tokens`) return HTTP 200 with `len=43` — confirmed as 1x1 GIF tracking pixels, NOT a payment API
[NEW] app.n26.com/graphql: WAF normalizes Content-Type; `application/x-www-form-urlencoded`, `text/plain`, `multipart/form-data` all return 403 — no bypass via alternative Content-Type
[CHANGED] spc.n26.com: Previous IDOR hypothesis (confidence 80+) INVALIDATED — endpoints are tracking pixels, not financial API
[CHANGED] app.n26.com/graphql: Previous GraphQL WAF bypass hypothesis (confidence 60) INVALIDATED — WAF inspects body, not just Content-Type header
[PRIO] flags.n26.com,8.5,attack_surface=8,business_value=8,tech_exposure=9,gate_ease=9,cloud_surface=8,freshness=10
[PRIO] app.n26.com,6.0,attack_surface=7,business_value=9,tech_exposure=6,gate_ease=3,cloud_surface=7,freshness=8
[PRIO] spc.n26.com,2.0,attack_surface=2,business_value=3,tech_exposure=2,gate_ease=5,cloud_surface=5,freshness=10
[HYP] Statsig feature flag enumeration via extracted client SDK key and RBAC bypass
class: MISCONFIG
asset: flags.n26.com
confidence: 85
reasoning: Client SDK key extracted from app.n26.com bundle; `/v1/sdk_exception` endpoint accepts the key and bypasses RBAC (returns 200 vs 403 on `/v1/initialize`); Statsig `/v1/get_configs`, `/v1/evaluate`, `/v1/log_event` all return 403 without valid key
evidence_needed: Successful call to `/v1/get_configs` or `/v1/evaluate` with extracted client_key returning feature flag configurations; confirmation of banking-relevant flags (premium features, A/B tests, KYC limits, card controls)
verify_steps: POST https://flags.n26.com/v1/get_configs with `Content-Type: application/json`, body `{"client_key":"<extracted_key>"}`; POST https://flags.n26.com/v1/evaluate with same key and user context; enumerate flag names for business logic impact
impact: Full feature flag enumeration → business logic bypass (premium features, transaction limits, KYC bypass, card control toggles) → high
testability: PASSIVE
[HYP] GraphQL schema exposure via alternative transport on app.n26.com
class: AUTH
asset: app.n26.com/graphql
confidence: 25
reasoning: GraphQL endpoint exists (confirmed via cookie + 403/timeout not 404); WAF blocks all POST Content-Types including form-encoded; GET query param returns connection reset; no viable transport bypass found after testing JSON, urlencoded, text/plain, multipart
evidence_needed: Any GraphQL response (__typename or __schema) via any HTTP method/Content-Type combination
verify_steps: POST https://app.n26.com/graphql with `Content-Type: application/graphql` body `{__typename}`; test HTTP PUT/PATCH with JSON; test WebSocket upgrade on `/graphql`; test batched queries via GET with persisted query IDs
impact: Full schema exposure → mutation enumeration → IDOR/BOLA on banking operations → critical (but unverifiable)
testability: AUTH_HELPED
[HYP] Subdomain takeover via dangling DNS on discovered CDN/assets
class: MISCONFIG
asset: cdn.number26.de
confidence: 30
reasoning: S3-backed CDN discovered via CSP script-src on app.n26.com; returns 403 with XML access-denied (not 404); bucket may be misconfigured or deleted
evidence_needed: DNS CNAME pointing to non-existent S3 bucket; successful bucket claim serving attacker content
verify_steps: `dig cdn.number26.com` to resolve CNAME; check if target S3 bucket exists via `aws s3 ls s3://<bucket>`; attempt bucket registration if unclaimed
impact: XSS via malicious JS served from trusted CSP source → session theft → ATO → critical
testability: PASSIVE
[PARKED] GraphQL schema exposure via alternative transport on app.n26.com: confidence 25 (<40), WAF inspects request body not just Content-Type; all tested vectors return 403; no new bypass vectors identified
[PARKED] Subdomain takeover via dangling DNS on cdn.number26.de: confidence 30 (<40), no evidence of dangling CNAME; 403 response suggests bucket exists but is private
[FINAL] 1) Statsig feature flag enumeration via extracted client SDK key and RBAC bypass (confidence 85)
[NEXT] PROBE: POST https://flags.n26.com/v1/get_configs with `Content-Type: application/json`, body `{"client_key":"<extracted_client_key_from_bundle>"}` — enumerate feature flags for banking business logic impact
[LEARN] REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
[LEARN] REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
[LEARN] ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted from app bundle; `/v1/sdk_exception` bypasses RBAC and accepts it
[LEARN] ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns connection reset — not a viable bypass.
[LEARN] ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. All `/v1/*` endpoints return 403/401. Client-side SDK key path (client.*.js) returns 404 — bundle location changed or server-only keys.
[LEARN] REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
[RISK] N26 Bank AG: 75 — Confirmed Statsig feature flag service with client SDK key extraction and RBAC bypass via `/v1/sdk_exception`; feature flags control banking features (premium, limits, KYC, card controls); GraphQL WAF on primary app is robust (no bypass found); spc.n26.com endpoints are tracking pixels not payment API; single high-value exploitable path (flags.n26.com) with confirmed key extraction and bypass
[NEW] flags.n26.com/v1/initialize: POST with public client key + canonical Statsig SDK payload `{"hash":"djb2","user":{...},"statsigMetadata":{...}}` returns **200** with full flag/config tree (35 feature_gates, 9 dynamic_configs, layer_configs, company_lcut). This REVERSES the prior "initialize POST-only 401" RBAC-map entry — the bypass is method+payload dependent.
[NEW] flags.n26.com/v1/download_config_specs: POST with identical canonical payload returns **401** (not 200) — closes the "may pass client key via canonical POST" sub-route; it is key-invalid at app layer, not payload-shaped.
[CHANGED] flags.n26.com RBAC boundary: `initialize` (POST, canonical body) → 200 + data; `sdk_exception` → 202; `download_config_specs` → 401. All GET /v1 config routes remain 403 RBAC.
[NEW] flags.n26.com/v1/initialize: POST with public client key + canonical Statsig SDK payload returns **200** with full flag/config tree (35 feature_gates, 9 dynamic_configs). REVERSES prior "initialize POST-only 401" — bypass is method+payload dependent.
[NEW] flags.n26.com/v1/download_config_specs: canonical SDK POST returns **401** — route is key-invalid, not payload-shaped; closes that sub-route.
[CHANGED] flags.n26.com RBAC boundary: `initialize`(POST+canonical body)→200+data; `sdk_exception`→202; `download_config_specs`→401; all GET config routes→403.
[PRIO] flags.n26.com, 8.5, a:8 b:7 t:8 g:8 c:8 f:10 — live RBAC-bypass confirmed (POST initialize, returns real config)
[PRIO] app.n26.com/graphql, 4.0, a:7 b:9 t:8 g:2 c:6 f:7 — WAF blocks all Content-Types + GET; stalled >6 cycles
[PRIO] spc.n26.com, 2.0 — 1x1 GIF tracking pixels, no financial API
[HYP] Statsig config disclosure via method/payload-dependent RBAC bypass on /v1/initialize
class: MISCONFIG
asset: flags.n26.com/v1/initialize
confidence: 80
reasoning: Public client key `client-OulnFN2GtdJJRAKPLjRLBgckZMUhF5zxtzPGciWbxO0` (client.64629020.js, env=live). Prior map said initialize GET-403/POST-401, but POST with canonical SDK body returns 200 + full init payload: 35 gates, 9 dynamic configs with real values (e.g. `{'title':'Get this deal now!','price':19}`, `{'uiMode':'vendor','escalationType':'human',...}`), `company_lcut=1788632107688`. Numeric IDs map to banking feature names in the same 410.js bundle.
evidence_needed: Correlate numeric gate IDs to named features in 410.js to show business-logic reach (premium/limits/KYC/card-control); confirm disclosure is authz-gated vs by-design.
verify_steps: (1) DONE POST /v1/initialize k=<key> STATSIG-API-KEY=<key> body {"hash":"djb2",...} → 200+data, (2) map IDs→names via 410.js, (3) isolate bypass trigger (POST+canonical body vs GET/empty), (4) classify whether flag config is deemed sensitive.
impact: Feature-flag/experiment config disclosure of live banking app (hidden features, A/B groups, promo config, escalation policy). No PII/funds/mutation. Severity: LOW-MEDIUM, reportable as RBAC-bypass/config disclosure if flag data deemed sensitive.
testability: PASSIVE
[HYP] GraphQL introspection via persisted-query/WebSocket
class: AUTH
asset: app.n26.com/graphql
confidence: 20
reasoning: WAF blocks all POST Content-Types + GET (connection reset); only untested transports are PQ-hash GET and WS upgrade — both require a session/PQ hash.
evidence_needed: non-403/non-reset /graphql response.
verify_steps: OPTIONS /graphql with `graphql-ws` upgrade; GET /graphql?extensions={"persistedQuery":{...}}.
impact: schema→mutation enumeration→banking-op abuse if reached. HIGH (unreached).
testability: AUTH_HELPED
[PARKED] GraphQL PQ/WS (20<40); spc.n26.com IDOR (GIF catch-all, dead).
[FINAL] 1) Statsig /v1/initialize RBAC bypass config disclosure (80) — live-probe-confirmed.
[NEXT] PROBE: grep app.n26.com bundles (410.js + chunks) to map numeric Statsig IDs (3526595, 54269378, 361410648, 1418505627, 1467424007, 3273360575; dynamic configs 113552670, 687145565, 1078987709, 1594427002, 3518516657, 4173610755) to named banking features before deciding report-worthiness.
[LEARN] ACCEPTED MISCONFIG @ flags.n26.com: POST /v1/initialize + canonical SDK payload + public client key returns 200 with full flag/config disclosure. Prior POST-401 entry was wrong — bypass is method+payload dependent.
[LEARN] REJECTED MISCONFIG @ flags.n26.com/v1/download_config_specs: canonical POST returns 401 — key-invalid at app layer, not payload-shaped.
[LEARN] ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary now = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403.
[LEARN] REJECTED AUTH @ app.n26.com: GraphQL WAF-blocked across every tested transport.
[RISK] n26: 40/100 — Confirmed RBAC-bypass + config disclosure on flags.n26.com via POST initialize (LOW-MEDIUM; public-by-design client key, config metadata only, no PII/funds/mutation). GraphQL robust, spc dead. Report-worthiness hinges on whether N26 treats flag/config metadata as sensitive; no cross-tenant/financial impact demonstrated.
