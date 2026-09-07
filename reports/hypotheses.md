# Hypotheses (ranked)

## RANKED HYPOTHESES 2026-09-02 21:37:48 UTC

## RANKED HYPOTHESES 2026-09-02 23:32:55 UTC

## RANKED HYPOTHESES 2026-09-03 01:25:44 UTC

## RANKED HYPOTHESES 2026-09-03 06:35:49 UTC

## RANKED HYPOTHESES 2026-09-03 11:42:06 UTC

## RANKED HYPOTHESES 2026-09-03 15:57:16 UTC
- [62] app.n26.com/graphql: GraphQL WAF bypass on app.n26.com (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://app.n26.com/build/js/client.*.js (extract Statsig SDK key from JS bundle to unlock flags.n26.com testing), then GET https://flags.n26.com/v1/
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403 responses (not 404). WAF actively blocks POST. Bypass exploration warranted.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. Client-side SDK key extraction from app bundle is viable path.
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with /health endpoint. API enumeration needed.

## RANKED HYPOTHESES 2026-09-03 18:56:04 UTC
- [70] app.n26.com/graphql: GraphQL WAF bypass via GET query param smuggling (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://spc.n26.com/api/v1 (enumerate payment API versioned endpoints) with Referer: https://app.n26.com and app.n26.com cookies
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403 responses (not 404). WAF actively blocks POST. Bypass exploration warranted.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. Client-side SDK key extraction from app bundle is viable path.
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with /health endpoint. API enumeration needed.

## RANKED HYPOTHESES 2026-09-03 21:34:05 UTC
- [65] app.n26.com/graphql: GraphQL introspection via GET query param bypassing WAF (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://spc.n26.com/api/v1 (enumerate payment API versioned endpoints) with Referer: https://app.n26.com and app.n26.com cookies
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403 responses (not 404). WAF actively blocks POST. Bypass exploration warranted.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. Client-side SDK key extraction from app bundle is viable path.
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with /health endpoint. API enumeration needed.

## RANKED HYPOTHESES 2026-09-03 23:34:58 UTC
- [75] spc.n26.com: Payment service versioned API (spc.n26.com/api/v1) exposes BOLA/IDOR on financial operations (from art/lead_nemotron3.txt)
- [70] app.n26.com/graphql: GraphQL WAF bypass via GET query param smuggling (from art/lead_bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://spc.n26.com/api/v1 with headers `Referer: https://app.n26.com` and cookies `n26.csrf`, `num26UniqueDeviceToken`, `n26.graphql_form_payload` f
- NEXT(hypotheses-bigpickle.txt): PROBE: Fetch app.n26.com main page, extract all JS bundle URLs, search for Statsig SDK key (`client_key`, `sdk_key`, `statsig` patterns), then use extracted key
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403 responses (not 404). WAF actively blocks POST. GET query param returns connection reset — not a 
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. All /v1/* endpoints return 403/401. Client-side SDK key path (client.*.js
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with confirmed versioned API endpoints (/api/v1, /v1/transactions, /v1/payments, /v1/tokens all HTTP 200)
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403 responses. WAF actively blocks POST. Bypass exploration warranted.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. Client-side SDK key extraction is viable path.
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with /health endpoint. API enumeration needed.

## RANKED HYPOTHESES 2026-09-04 01:25:41 UTC
- [80] spc.n26.com: Payment service BOLA/IDOR on versioned financial endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://app.n26.com/graphql with `Content-Type: application/x-www-form-urlencoded`, body `query={__typename}`, cookies `n26.csrf`, `num26UniqueDevic
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403 responses (not 404). WAF actively blocks POST application/json. GET query param returns connecti
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. All /v1/* endpoints return 403/401. Client-side SDK key path (client.*.js
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with confirmed versioned API endpoints (/api/v1, /v1/transactions, /v1/payments, /v1/tokens all HTTP 200)

## RANKED HYPOTHESES 2026-09-04 06:05:57 UTC
- [80] spc.n26.com: Payment service BOLA/IDOR on versioned financial endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://app.n26.com/graphql with `Content-Type: application/x-www-form-urlencoded`, body `query={__typename}`, cookies `n26.csrf`, `num26UniqueDevic
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. All /v1/* endpoints return 403/401. Client-side SDK key path (client.*.js
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with confirmed versioned API endpoints (/api/v1, /v1/transactions, /v1/payments, /v1/tokens all HTTP 200)

## RANKED HYPOTHESES 2026-09-04 11:25:42 UTC
- [82] spc.n26.com/v1/transactions: Payment service BOLA/IDOR on spc.n26.com versioned endpoints (from art/lead_bigpickle.txt)
- [80] spc.n26.com: Payment service BOLA/IDOR on versioned financial endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://spc.n26.com/v1/transactions with headers `Referer: https://app.n26.com` and `Origin: https://app.n26.com` — check for transaction data, error
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://app.n26.com/graphql with `Content-Type: application/x-www-form-urlencoded`, body `query={__typename}`, cookies `n26.csrf`, `num26UniqueDevic
- LEARN: ACCEPTED IDOR @ spc.n26.com: Versioned payment API endpoints confirmed (HTTP 200). CSP allows cross-origin access from app.n26.com. Financial endpoints are high
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL WAF blocks POST application/json. Alternative Content-Type or HTTP method may bypass WAF rules.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig service with RBAC. Client-side SDK key extraction from actual bundle paths (not assumed) is viable path.
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. All /v1/* endpoints return 403/401. Client-side SDK key path (client.*.js
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with confirmed versioned API endpoints (/api/v1, /v1/transactions, /v1/payments, /v1/tokens all HTTP 200)

## RANKED HYPOTHESES 2026-09-04 15:15:36 UTC
- [80] spc.n26.com: Payment service BOLA/IDOR on versioned financial endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://app.n26.com/graphql with `Content-Type: application/x-www-form-urlencoded`, body `query={__typename}`, cookies `n26.csrf`, `num26UniqueDevic
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. All /v1/* endpoints return 403/401. Client-side SDK key path (client.*.js
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with confirmed versioned API endpoints (/api/v1, /v1/transactions, /v1/payments, /v1/tokens all HTTP 200)
- LEARN: ACCEPTED IDOR @ spc.n26.com: Versioned payment API endpoints confirmed (HTTP 200). CSP allows cross-origin access from app.n26.com. Financial endpoints are high
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL WAF blocks POST application/json. Alternative Content-Type or HTTP method may bypass WAF rules.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig service with RBAC. Client-side SDK key extraction from actual bundle paths (not assumed) is viable path.

## RANKED HYPOTHESES 2026-09-04 18:31:05 UTC
- [80] spc.n26.com: Payment service BOLA/IDOR on versioned financial endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://app.n26.com/graphql with `Content-Type: application/x-www-form-urlencoded`, body `query={__typename}`, cookies `n26.csrf`, `num26UniqueDevic
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. All /v1/* endpoints return 403/401. Client-side SDK key path (client.*.js
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector
- LEARN: ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with confirmed versioned API endpoints (/api/v1, /v1/transactions, /v1/payments, /v1/tokens all HTTP 200)
- LEARN: ACCEPTED IDOR @ spc.n26.com: Versioned payment API endpoints confirmed (HTTP 200). CSP allows cross-origin access from app.n26.com. Financial endpoints are high
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL WAF blocks POST application/json. Alternative Content-Type or HTTP method may bypass WAF rules
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig service with RBAC. Client-side SDK key extraction from actual bundle paths (not assumed) is viable path

## RANKED HYPOTHESES 2026-09-04 21:04:10 UTC
- [80] spc.n26.com: Payment service BOLA/IDOR on versioned financial endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://app.n26.com/graphql with `Content-Type: application/x-www-form-urlencoded`, body `query={__typename}`, cookies `n26.csrf`, `num26UniqueDevic
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. All /v1/* endpoints return 403/401. Client-side SDK key path (client.*.js
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with confirmed versioned API endpoints (/api/v1, /v1/transactions, /v1/payments, /v1/tokens all HTTP 200)
- LEARN: ACCEPTED IDOR @ spc.n26.com: Versioned payment API endpoints confirmed (HTTP 200). CSP allows cross-origin access from app.n26.com. Financial endpoints are high
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL WAF blocks POST application/json. Alternative Content-Type or HTTP method may bypass WAF rules.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig service with RBAC. Client-side SDK key extraction from actual bundle paths (not assumed) is viable path.

## RANKED HYPOTHESES 2026-09-04 23:07:41 UTC
- [80] spc.n26.com: Payment service BOLA/IDOR on versioned financial endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://app.n26.com/graphql with `Content-Type: application/x-www-form-urlencoded`, body `query={__typename}`, cookies `n26.csrf`, `num26UniqueDevic
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. All /v1/* endpoints return 403/401. Client-side SDK key path (client.*.js
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with confirmed versioned API endpoints (/api/v1, /v1/transactions, /v1/payments, /v1/tokens all HTTP 200)
- LEARN: ACCEPTED IDOR @ spc.n26.com: Versioned payment API endpoints confirmed (HTTP 200). CSP allows cross-origin access from app.n26.com. Financial endpoints are high
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL WAF blocks POST application/json. Alternative Content-Type or HTTP method may bypass WAF rules.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig service with RBAC. Client-side SDK key extraction from actual bundle paths (not assumed) is viable path.

## RANKED HYPOTHESES 2026-09-05 01:09:42 UTC
- [80] spc.n26.com: Payment service BOLA/IDOR on versioned financial endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://app.n26.com/graphql with `Content-Type: application/x-www-form-urlencoded`, body `query={__typename}`, cookies `n26.csrf`, `num26UniqueDevic
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. All /v1/* endpoints return 403/401. Client-side SDK key path (client.*.js
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with confirmed versioned API endpoints (/api/v1, /v1/transactions, /v1/payments, /v1/tokens all HTTP 200)
- LEARN: ACCEPTED IDOR @ spc.n26.com: Versioned payment API endpoints confirmed (HTTP 200). CSP allows cross-origin access from app.n26.com. Financial endpoints are high
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL WAF blocks POST application/json. Alternative Content-Type or HTTP method may bypass WAF rules.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig service with RBAC. Client-side SDK key extraction from actual bundle paths (not assumed) is viable path.

## RANKED HYPOTHESES 2026-09-05 05:49:45 UTC
- [80] spc.n26.com: Payment service BOLA/IDOR on versioned financial endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://app.n26.com/graphql with `Content-Type: application/x-www-form-urlencoded`, body `query={__typename}`, cookies `n26.csrf`, `num26UniqueDevic
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. All /v1/* endpoints return 403/401. Client-side SDK key path (client.*.js
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with confirmed versioned API endpoints (/api/v1, /v1/transactions, /v1/payments, /v1/tokens all HTTP 200)
- LEARN: ACCEPTED IDOR @ spc.n26.com: Versioned payment API endpoints confirmed (HTTP 200). CSP allows cross-origin access from app.n26.com. Financial endpoints are high
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL WAF blocks POST application/json. Alternative Content-Type or HTTP method may bypass WAF rules.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig service with RBAC. Client-side SDK key extraction from actual bundle paths (not assumed) is viable path.

## RANKED HYPOTHESES 2026-09-05 09:51:31 UTC
- [82] spc.n26.com/v1/transactions: Payment service BOLA/IDOR on spc.n26.com versioned endpoints (from art/lead_bigpickle.txt)
- [80] spc.n26.com: Payment service BOLA/IDOR on versioned financial endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://spc.n26.com/v1/transactions with headers `Referer: https://app.n26.com` and `Origin: https://app.n26.com` — check for transaction data, error
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://app.n26.com/graphql with `Content-Type: application/x-www-form-urlencoded`, body `query={__typename}`, cookies `n26.csrf`, `num26UniqueDevic
- LEARN: ACCEPTED IDOR @ spc.n26.com: Versioned payment API endpoints confirmed (HTTP 200). CSP allows cross-origin access from app.n26.com. Financial endpoints are high
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL WAF blocks POST application/json. Alternative Content-Type or HTTP method may bypass WAF rules.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig service with RBAC. Client-side SDK key extraction from actual bundle paths (not assumed) is viable path.
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. All /v1/* endpoints return 403/401. Client-side SDK key path (client.*.js
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with confirmed versioned API endpoints (/api/v1, /v1/transactions, /v1/payments, /v1/tokens all HTTP 200)
- LEARN: ACCEPTED IDOR @ spc.n26.com: Versioned payment API endpoints confirmed (HTTP 200). CSP allows cross-origin access from app.n26.com. Financial endpoints are high
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL WAF blocks POST application/json. Alternative Content-Type or HTTP method may bypass WAF rules.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig service with RBAC. Client-side SDK key extraction from actual bundle paths (not assumed) is viable path.

## RANKED HYPOTHESES 2026-09-05 13:19:06 UTC
- [82] spc.n26.com: Payment service BOLA/IDOR on versioned financial endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://app.n26.com/graphql with `Content-Type: application/x-www-form-urlencoded`, body `query={__typename}`, cookies `n26.csrf`, `num26UniqueDevic
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels, not a payment API
- LEARN: REJECTED AUTH  @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain all 403
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted; /v1/sdk_exception bypasses RBAC and accepts it
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. All /v1/* endpoints return 403/401. Client-side SDK key path (client.*.js
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with confirmed versioned API endpoints (/api/v1, /v1/transactions, /v1/payments, /v1/tokens all HTTP 200)
- LEARN: ACCEPTED IDOR @ spc.n26.com: Versioned payment API endpoints confirmed (HTTP 200). CSP allows cross-origin access from app.n26.com. Financial endpoints are high
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL WAF blocks POST application/json. Alternative Content-Type or HTTP method may bypass WAF rules.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig service with RBAC. Client-side SDK key extraction from actual bundle paths (not assumed) is viable path.

## RANKED HYPOTHESES 2026-09-05 16:16:16 UTC
- [85] flags.n26.com: Statsig feature flag enumeration via extracted client SDK key and RBAC bypass (from art/lead_nemotron3.txt)
- [82] spc.n26.com/v1/transactions: Payment service BOLA/IDOR on spc.n26.com versioned endpoints (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://spc.n26.com/v1/transactions with headers `Referer: https://app.n26.com` and `Origin: https://app.n26.com` — check for transaction data, error
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://flags.n26.com/v1/get_configs with `Content-Type: application/json`, body `{"client_key":"<extracted_client_key_from_bundle>"}` — enumerate f
- LEARN: ACCEPTED IDOR @ spc.n26.com: Versioned payment API endpoints confirmed (HTTP 200). CSP allows cross-origin access from app.n26.com. Financial endpoints are high
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL WAF blocks POST application/json. Alternative Content-Type or HTTP method may bypass WAF rules.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig service with RBAC. Client-side SDK key extraction from actual bundle paths (not assumed) is viable path.
- LEARN: ACCEPTED IDOR @ spc.n26.com: Versioned payment API endpoints confirmed (HTTP 200). CSP allows cross-origin access from app.n26.com. Financial endpoints are high
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL WAF blocks POST application/json. Alternative Content-Type or HTTP method may bypass WAF rules.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig service with RBAC. Client-side SDK key extraction from actual bundle paths (not assumed) is viable path.
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels, not a payment API
- LEARN: REJECTED AUTH  @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain all 403
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted; /v1/sdk_exception bypasses RBAC and accepts it
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: /v1/download_config_specs is a 2nd RBAC-exempt route (app-layer 401 across all GET key-delivery variants); sdk_exception rem
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded in any app bundle → no s
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes (get_configs, eva
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted from app bundle; `/v1/sdk_exception` bypasses RBAC and accepts it
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. All `/v1/*` endpoints return 403/401. Client-side SDK key path (client.*.
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.

## RANKED HYPOTHESES 2026-09-05 18:31:44 UTC
- [85] flags.n26.com: Statsig feature flag enumeration via extracted client SDK key and RBAC bypass (from art/lead_bigpickle.txt)
- [25] app.n26.com/graphql: GraphQL schema exposure via alternative transport on app.n26.com (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: POST https://flags.n26.com/v1/get_configs with `Content-Type: application/json`, body `{"client_key":"<extracted_client_key_from_bundle>"}` — enumerate f
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://flags.n26.com/v1/get_configs with `Content-Type: application/json`, body `{"client_key":"<extracted_client_key_from_bundle>"}` — enumerate f
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted from app bundle; `/v1/sdk_exception` bypasses RBAC and accepts it
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. All `/v1/*` endpoints return 403/401. Client-side SDK key path (client.*.
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST /v1/initialize + canonical SDK payload + public client key returns 200 with full flag/config disclosure. Prior POST-401
- LEARN: REJECTED MISCONFIG @ flags.n26.com/v1/download_config_specs: canonical POST returns 401 — key-invalid at app layer, not payload-shaped.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary now = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403.
- LEARN: REJECTED AUTH @ app.n26.com: GraphQL WAF-blocked across every tested transport.
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted from app bundle; `/v1/sdk_exception` bypasses RBAC and accepts it
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: `/v1/download_config_specs` is a 2nd RBAC-exempt route (app-layer 401 across all GET key-delivery variants); sdk_exception r
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded in any app bundle → no s
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes (get_configs, eva
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. All `/v1/*` endpoints return 403/401. Client-side SDK key path (client.*.

## RANKED HYPOTHESES 2026-09-05 20:45:52 UTC
- [85] flags.n26.com/v1/initialize: Statsig feature flag enumeration via extracted client SDK key and RBAC bypass on `/v1/initialize` (from art/lead_nemotron3.txt)
- [35] flags.n26.com/v1/evaluate: Envoy RBAC method+payload bypass extends beyond /v1/initialize (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: POST https://flags.n26.com/v1/evaluate with headers `k: client-OulnFN2GtdJJRAKPLjRLBgckZMUhF5zxtzPGciWbxO0`, `STATSIG-API-KEY: client-OulnFN2GtdJJRAKPLjR
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://flags.n26.com/v1/initialize with `Content-Type: application/json`, headers `STATSIG-API-KEY: client-OulnFN2GtdJJRAKPLjRLBgckZMUhF5zxtzPGciWb
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST /v1/initialize 200-with-data is Statatsig DESIGN — public client key unchanged across bundle rotation, browser SDK send
- LEARN: REJECTED MISCONFIG @ app.n26.com: numeric Statsig IDs (3526595..4173610755) absent from all 11 bundles — IDs server-assigned, name↔ID mapping not passively reco
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC is method+shape dependent (GET initialize 403 vs POST 200), but the 401 on download_config_specs proves app-layer
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted from app bundle; `/v1/sdk_exception` bypasses RBAC and accepts it
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST `/v1/initialize` + canonical SDK payload + public client key returns 200 with full flag/config disclosure. Prior POST-4
- LEARN: REJECTED MISCONFIG @ flags.n26.com/v1/download_config_specs: canonical POST returns 401 — key-invalid at app layer, not payload-shaped
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary now = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded in any app bundle → no s
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes (get_configs, eva
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 

## RANKED HYPOTHESES 2026-09-05 22:38:32 UTC
- [85] flags.n26.com/v1/initialize: Statsig feature flag enumeration via extracted client SDK key and RBAC bypass on `/v1/initialize` (from art/lead_nemotron3.txt)
- [40] flags.n26.com/v1/evaluate: Envoy RBAC method+payload bypass extends beyond /v1/initialize (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: POST https://flags.n26.com/v1/evaluate with headers `k: client-OulnFN2GtdJJRAKPLjRLBgckZMUhF5zxtzPGciWbxO0`, `STATSIG-API-KEY: client-OulnFN2GtdJJRAKPLjR
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://flags.n26.com/v1/initialize with `Content-Type: application/json`, headers `STATSIG-API-KEY: client-OulnFN2GtdJJRAKPLjRLBgckZMUhF5zxtzPGciWb
- LEARN: REJECTED AUTH @ support.n26.com/graphql: OPTIONS 204 (not 403) looked like alternate WAF boundary, but bare-GET stalls identically on app+support (25s, 0B) — sh
- LEARN: REJECTED MISCONFIG @ cdn.number26.de: `/` and `/?list-type=2` both 403 `AccessDenied` (S3 origin behind CloudFront) — private bucket, object-only; no listing/mi
- LEARN: ACCEPTED MISCONFIG @ n26.com: marketing on Envoy+CloudFront with wildcard script-src/connect-src/img-src; `cookie.n26.com` surfaced as new 404 leaf — hosts low-
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted from app bundle; `/v1/sdk_exception` bypasses RBAC and accepts it
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST `/v1/initialize` + canonical SDK payload + public client key returns 200 with full flag/config disclosure. Prior POST-4
- LEARN: REJECTED MISCONFIG @ flags.n26.com/v1/download_config_specs: canonical POST returns 401 — key-invalid at app layer, not payload-shaped
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary now = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded in any app bundle → no s
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes (get_configs, eva
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 

## RANKED HYPOTHESES 2026-09-06 00:18:36 UTC
- [85] flags.n26.com/v1/initialize: Statsig feature flag/config disclosure via method-dependent RBAC bypass on `/v1/initialize` (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://flags.n26.com/v1/initialize with `Content-Type: application/json`, headers `STATSIG-API-KEY: client-OulnFN2GtdJJRAKPLjRLBgckZMUhF5zxtzPGciWb
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted from app bundle; `/v1/sdk_exception` bypasses RBAC and accepts it
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST `/v1/initialize` + canonical SDK payload + public client key returns 200 with full flag/config disclosure. Prior POST-4
- LEARN: REJECTED MISCONFIG @ flags.n26.com/v1/download_config_specs: canonical POST returns 401 — key-invalid at app layer, not payload-shaped
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary now = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded in any app bundle → no s
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes (get_configs, eva
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: REJECTED AUTH @ support.n26.com/graphql: OPTIONS 204 (not 403) looked like alternate WAF boundary, but bare-GET stalls identically on app+support (25s, 0B) — sh
- LEARN: REJECTED MISCONFIG @ cdn.number26.de: `/` and `/?list-type=2` both 403 `AccessDenied` (S3 origin behind CloudFront) — private bucket, object-only; no listing/mi
- LEARN: ACCEPTED MISCONFIG @ n26.com: marketing on Envoy+CloudFront with wildcard script-src/connect-src/img-src; `cookie.n26.com` surfaced as new 404 leaf — hosts low-

## RANKED HYPOTHESES 2026-09-06 04:48:10 UTC
- [85] flags.n26.com/v1/initialize: Statsig feature flag/config disclosure via method-dependent RBAC bypass on `/v1/initialize` (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://flags.n26.com/v1/initialize with `Content-Type: application/json`, headers `STATSIG-API-KEY: client-OulnFN2GtdJJRAKPLjRLBgckZMUhF5zxtzPGciWb
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted from app bundle; `/v1/sdk_exception` bypasses RBAC and accepts it
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST `/v1/initialize` + canonical SDK payload + public client key returns 200 with full flag/config disclosure. Prior POST-4
- LEARN: REJECTED MISCONFIG @ flags.n26.com/v1/download_config_specs: canonical POST returns 401 — key-invalid at app layer, not payload-shaped
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary now = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded in any app bundle → no s
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes (get_configs, eva
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: REJECTED AUTH @ support.n26.com/graphql: OPTIONS 204 (not 403) looked like alternate WAF boundary, but bare-GET stalls identically on app+support (25s, 0B) — sh
- LEARN: REJECTED MISCONFIG @ cdn.number26.de: `/` and `/?list-type=2` both 403 `AccessDenied` (S3 origin behind CloudFront) — private bucket, object-only; no listing/mi
- LEARN: ACCEPTED MISCONFIG @ n26.com: marketing on Envoy+CloudFront with wildcard script-src/connect-src/img-src; `cookie.n26.com` surfaced as new 404 leaf — hosts low-

## RANKED HYPOTHESES 2026-09-06 09:11:41 UTC
- [85] flags.n26.com/v1/initialize: Statsig feature flag/config disclosure via method-dependent RBAC bypass on `/v1/initialize` (from art/lead_nemotron3.txt)
- [40] flags.n26.com/v1/evaluate: Envoy RBAC method+payload bypass extends to `/v1/evaluate` and `/v1/get_configs` (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: POST https://flags.n26.com/v1/initialize with `Content-Type: application/json`, headers `STATSIG-API-KEY: client-OulnFN2GtdJJRAKPLjRLBgckZMUhF5zxtzPGciWb
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://flags.n26.com/v1/initialize with `Content-Type: application/json`, headers `STATSIG-API-KEY: client-OulnFN2GtdJJRAKPLjRLBgckZMUhF5zxtzPGciWb
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted from app bundle; `/v1/sdk_exception` bypasses RBAC and accepts it
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST `/v1/initialize` + canonical SDK payload + public client key returns 200 with full flag/config disclosure. Prior POST-4
- LEARN: REJECTED MISCONFIG @ flags.n26.com/v1/download_config_specs: canonical POST returns 401 — key-invalid at app layer, not payload-shaped
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary now = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded in any app bundle → no s
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes (get_configs, eva
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: REJECTED AUTH @ support.n26.com/graphql: OPTIONS 204 (not 403) looked like alternate WAF boundary, but bare-GET stalls identically on app+support (25s, 0B) — sh
- LEARN: REJECTED MISCONFIG @ cdn.number26.de: `/` and `/?list-type=2` both 403 `AccessDenied` (S3 origin behind CloudFront) — private bucket, object-only; no listing/mi
- LEARN: ACCEPTED MISCONFIG @ n26.com: marketing on Envoy+CloudFront with wildcard script-src/connect-src/img-src; `cookie.n26.com` surfaced as new 404 leaf — hosts low-
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted from app bundle; `/v1/sdk_exception` bypasses RBAC and accepts it
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST `/v1/initialize` + canonical SDK payload + public client key returns 200 with full flag/config disclosure. Prior POST-4
- LEARN: REJECTED MISCONFIG @ flags.n26.com/v1/download_config_specs: canonical POST returns 401 — key-invalid at app layer, not payload-shaped
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary now = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded in any app bundle → no s
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes (get_configs, eva
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: REJECTED AUTH @ support.n26.com/graphql: OPTIONS 204 (not 403) looked like alternate WAF boundary, but bare-GET stalls identically on app+support (25s, 0B) — sh
- LEARN: REJECTED MISCONFIG @ cdn.number26.de: `/` and `/?list-type=2` both 403 `AccessDenied` (S3 origin behind CloudFront) — private bucket, object-only; no listing/mi
- LEARN: ACCEPTED MISCONFIG @ n26.com: marketing on Envoy+CloudFront with wildcard script-src/connect-src/img-src; `cookie.n26.com` surfaced as new 404 leaf — hosts low-
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted from app bundle; `/v1/sdk_exception` bypasses RBAC and accepts it
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST `/v1/initialize` + canonical SDK payload + public client key returns 200 with full flag/config disclosure. Prior POST-4
- LEARN: REJECTED MISCONFIG @ flags.n26.com/v1/download_config_specs: canonical POST returns 401 — key-invalid at app layer, not payload-shaped
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary now = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded in any app bundle → no s
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes (get_configs, eva
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: REJECTED AUTH @ support.n26.com/graphql: OPTIONS 204 (not 403) looked like alternate WAF boundary, but bare-GET stalls identically on app+support (25s, 0B) — sh
- LEARN: REJECTED MISCONFIG @ cdn.number26.de: `/` and `/?list-type=2` both 403 `AccessDenied` (S3 origin behind CloudFront) — private bucket, object-only; no listing/mi
- LEARN: ACCEPTED MISCONFIG @ n26.com: marketing on Envoy+CloudFront with wildcard script-src/connect-src/img-src; `cookie.n26.com` surfaced as new 404 leaf — hosts low-

## RANKED HYPOTHESES 2026-09-06 13:02:23 UTC
- [85] flags.n26.com/v1/initialize: Statsig feature flag/config disclosure via method-dependent RBAC bypass on `/v1/initialize` (from art/lead_nemotron3.txt)
- [85] flags.n26.com/v1/initialize: Statsig feature flag/config disclosure on /v1/initialize is by-design client SDK behavior, not reportable RBAC bypass (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: escalate to program coordinator — request scoped test account or LivePerson/app session to enable AUTH_HELPED GraphQL WS/APQ testing (the only remaining 
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://flags.n26.com/v1/initialize with `Content-Type: application/json`, headers `STATSIG-API-KEY: client-OulnFN2GtdJJRAKPLjRLBgckZMUhF5zxtzPGciWb
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted from app bundle; `/v1/sdk_exception` bypasses RBAC and accepts it
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST `/v1/initialize` + canonical SDK payload + public client key returns 200 with full flag/config disclosure — reclassifie
- LEARN: REJECTED MISCONFIG @ flags.n26.com/v1/download_config_specs: canonical POST returns 401 — key-invalid at app layer, not payload-shaped
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded → no server-key escalati
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes return 403
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses; WAF blocks all POST Content-Types; GET query param connection reset
- LEARN: REJECTED AUTH @ support.n26.com/graphql: OPTIONS 204, bare-GET stalls identically to app.n26.com (25s, 0B) — shared Envoy/WAF tarpit; GraphQL transport class cl
- LEARN: REJECTED MISCONFIG @ cdn.number26.de: `/` and `/?list-type=2` both 403 AccessDenied (S3+CloudFront) — private bucket, object-only; no listing/misconfig
- LEARN: ACCEPTED MISCONFIG @ n26.com: marketing on Envoy+CloudFront with wildcard CSP; `cookie.n26.com` 404 leaf — low-logic static content, INFO ceiling only
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted from app bundle; `/v1/sdk_exception` bypasses RBAC and accepts it
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST `/v1/initialize` + canonical SDK payload + public client key returns 200 with full flag/config disclosure. Prior POST-4
- LEARN: REJECTED MISCONFIG @ flags.n26.com/v1/download_config_specs: canonical POST returns 401 — key-invalid at app layer, not payload-shaped
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary now = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded in any app bundle → no s
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes (get_configs, eva
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses (not 404). WAF actively blocks POST application/json. GET query param returns 
- LEARN: REJECTED AUTH @ support.n26.com/graphql: OPTIONS 204 (not 403) looked like alternate WAF boundary, but bare-GET stalls identically on app+support (25s, 0B) — sh
- LEARN: REJECTED MISCONFIG @ cdn.number26.de: `/` and `/?list-type=2` both 403 `AccessDenied` (S3 origin behind CloudFront) — private bucket, object-only; no listing/mi
- LEARN: ACCEPTED MISCONFIG @ n26.com: marketing on Envoy+CloudFront with wildcard script-src/connect-src/img-src; `cookie.n26.com` surfaced as new 404 leaf — hosts low-

## RANKED HYPOTHESES 2026-09-06 16:04:46 UTC
- [85] flags.n26.com/v1/initialize: Statsig feature flag/config disclosure via method-dependent RBAC bypass on `/v1/initialize` (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: escalate to program coordinator — request scoped test account or LivePerson/app session to enable AUTH_HELPED GraphQL WS/APQ testing (the only remaining 
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://flags.n26.com/v1/initialize with `Content-Type: application/json`, headers `STATSIG-API-KEY: client-OulnFN2GtdJJRAKPLjRLBgckZMUhF5zxtzPGciWb
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST /v1/initialize 200-with-data is Statsig DESIGN — public client key unchanged across bundle rotation, browser SDK sends 
- LEARN: REJECTED MISCONFIG @ app.n26.com: numeric Statsig IDs (3526595..4173610755) absent from all 11 bundles — IDs server-assigned, name↔ID mapping not passively reco
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC is method+shape dependent (GET initialize 403 vs POST 200), but the 401 on download_config_specs proves app-layer
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
- LEARN: REJECTED AUTH @ support.n26.com/graphql: OPTIONS 204, bare-GET stalls identically to app.n26.com (25s, 0B) — shared Envoy/WAF tarpit; GraphQL transport class cl
- LEARN: REJECTED MISCONFIG @ cdn.number26.de: `/` and `/?list-type=2` both 403 `AccessDenied` (S3+CloudFront) — private bucket, object-only; no listing/misconfig
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: client SDK key extracted from app bundle; `/v1/sdk_exception` bypasses RBAC and accepts it
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST `/v1/initialize` + canonical SDK payload + public client key returns 200 with full flag/config disclosure — reclassifie
- LEARN: REJECTED MISCONFIG @ flags.n26.com/v1/download_config_specs: canonical POST returns 401 — key-invalid at app layer, not payload-shaped
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded → no server-key escalati
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes return 403
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses; WAF blocks all POST Content-Types; GET query param connection reset
- LEARN: ACCEPTED MISCONFIG @ n26.com: marketing on Envoy+CloudFront with wildcard CSP; `cookie.n26.com` 404 leaf — low-logic static content, INFO ceiling only

## RANKED HYPOTHESES 2026-09-06 18:03:15 UTC
- [75] flags.n26.com/v1/sdk_exception: Statsig SDK exception endpoint accepts public client key without RBAC (from art/lead_nemotron3.txt)
- [38] authentication-service.eks.core-production.keyless.technology: Authentication service endpoint enumeration and OIDC/WebAuthn flow discovery (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: HEAD+GET+OPTIONS https://authentication-service.eks.core-production.keyless.technology/ — passive endpoint enumeration on unprobed EKS auth service; HEAD
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://authentication-service.eks.core-production.keyless.technology/ (HEAD first) — confirm live service, enumerate endpoints; then GET /.well-know
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST /v1/initialize 200-with-data is Statsig DESIGN — public client key unchanged across bundle rotation, browser SDK sends 
- LEARN: REJECTED MISCONFIG @ app.n26.com: numeric Statsig IDs (3526595..4173610755) absent from all 11 bundles — IDs server-assigned, name↔ID mapping not passively reco
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded → no server-key escalati
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes return 403
- LEARN: REJECTED AUTH @ support.n26.com/graphql: OPTIONS 204, bare-GET stalls identically to app.n26.com (25s, 0B) — shared Envoy/WAF tarpit; GraphQL transport class cl
- LEARN: REJECTED MISCONFIG @ cdn.number26.de: `/` and `/?list-type=2` both 403 AccessDenied (S3+CloudFront) — private bucket, object-only; no listing/misconfig
- LEARN: ACCEPTED MISCONFIG @ n26.com: marketing on Envoy+CloudFront with wildcard CSP; `cookie.n26.com` 404 leaf — low-logic static content, INFO ceiling only
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST `/v1/initialize` + canonical SDK payload + public client key returns 200 with full flag/config disclosure — reclassifie
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded → no server-key escalati
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes return 403
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
- LEARN: REJECTED AUTH @ support.n26.com/graphql: OPTIONS 204, bare-GET stalls identically to app.n26.com (25s, 0B) — shared Envoy/WAF tarpit; GraphQL transport class cl
- LEARN: REJECTED MISCONFIG @ cdn.number26.de: `/` and `/?list-type=2` both 403 `AccessDenied` (S3+CloudFront) — private bucket, object-only; no listing/misconfig
- LEARN: ACCEPTED MISCONFIG @ n26.com: marketing on Envoy+CloudFront with wildcard CSP; `cookie.n26.com` 404 leaf — low-logic static content, INFO ceiling only
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses; WAF blocks all POST Content-Types; GET query param connection reset
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector

## RANKED HYPOTHESES 2026-09-06 20:22:28 UTC
- [75] flags.n26.com/v1/sdk_exception: Statsig SDK exception endpoint accepts public client key without RBAC (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: HEAD https://authentication-service.eks.core-production.keyless.technology/ → GET / → GET /.well-known/openid-configuration → GET /health → GET /api/swag
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST `/v1/initialize` + canonical SDK payload + public client key returns 200 with full flag/config disclosure — reclassifie
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded → no server-key escalati
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes return 403
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
- LEARN: REJECTED AUTH @ support.n26.com/graphql: OPTIONS 204, bare-GET stalls identically to app.n26.com (25s, 0B) — shared Envoy/WAF tarpit; GraphQL transport class cl
- LEARN: REJECTED MISCONFIG @ cdn.number26.de: `/` and `/?list-type=2` both 403 `AccessDenied` (S3+CloudFront) — private bucket, object-only; no listing/misconfig
- LEARN: ACCEPTED MISCONFIG @ n26.com: marketing on Envoy+CloudFront with wildcard CSP; `cookie.n26.com` 404 leaf — low-logic static content, INFO ceiling only
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses; WAF blocks all POST Content-Types; GET query param connection reset
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector

## RANKED HYPOTHESES 2026-09-06 22:12:26 UTC
- [65] authentication-service.eks.core-production.keyless.technology: Keyless auth service uses non-standard API paths requiring discovery (from art/lead_nemotron3.txt)
- [25] authentication-service.eks.core-production.keyless.technology/v1/auth/n26: WS routes subprotocol-gated on Keyless auth ingress (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: ask program coordinator (a) whether vendor-in-flow assets (`keyless.technology`, N26 tenant slug `n26`, exact routes `wss://…/v1/auth/n26`, `/v1/enroll/n
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://authentication-service.eks.core-production.keyless.technology/api/v1 → GET /v1 → GET /keyless → GET /auth → GET /api → GET /webauthn → GET /p
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST `/v1/initialize` + canonical SDK payload + public client key returns 200 with full flag/config disclosure — reclassifie
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403.
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded → no server-key escalati
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes return 403.
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API.
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspect body structure.
- LEARN: REJECTED AUTH @ support.n26.com/graphql: OPTIONS 204, bare-GET stalls identically to app.n26.com (25s, 0B) — shared Envoy/WAF tarpit; GraphQL transport class cl
- LEARN: REJECTED MISCONFIG @ cdn.number26.de: `/` and `/?list-type=2` both 403 `AccessDenied` (S3+CloudFront) — private bucket, object-only; no listing/misconfig.
- LEARN: ACCEPTED MISCONFIG @ n26.com: marketing on Envoy+CloudFront with wildcard CSP; `cookie.n26.com` 404 leaf — low-logic static content, INFO ceiling only.
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses; WAF blocks all POST Content-Types; GET query param connection reset.
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ authentication-service.eks.core-production.keyless.technology: Service LIVE (HTTP/2 404), Istio/Envoy, version `authentication-service-2/v2

## RANKED HYPOTHESES 2026-09-07 00:04:50 UTC
- [65] authentication-service.eks.core-production.keyless.technology: Keyless auth service uses non-standard API paths requiring discovery (from art/lead_nemotron3.txt)
- [42] engagementplatform.n26.com: engagementplatform Bearer API key recoverable from a public N26 client asset (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: one consolidated coordinator ask — (a) scope ruling for `keyless.technology` vendor asset, (b) a whitelisted/integration API key or scoped test identity 
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://authentication-service.eks.core-production.keyless.technology/api/v1 → GET /v1 → GET /keyless → GET /auth → GET /api → GET /webauthn → GET /p
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST `/v1/initialize` + canonical SDK payload + public client key returns 200 with full flag/config disclosure — reclassifie
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403.
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded → no server-key escalati
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes return 403.
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API.
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure.
- LEARN: REJECTED AUTH @ support.n26.com/graphql: OPTIONS 204, bare-GET stalls identically to app.n26.com (25s, 0B) — shared Envoy/WAF tarpit; GraphQL transport class cl
- LEARN: REJECTED MISCONFIG @ cdn.number26.de: `/` and `/?list-type=2` both 403 `AccessDenied` (S3+CloudFront) — private bucket, object-only; no listing/misconfig.
- LEARN: ACCEPTED MISCONFIG @ n26.com: marketing on Envoy+CloudFront with wildcard CSP; `cookie.n26.com` 404 leaf — low-logic static content, INFO ceiling only.
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses; WAF blocks all POST Content-Types; GET query param connection reset.
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ authentication-service.eks.core-production.keyless.technology: Service LIVE (HTTP/2 404), Istio/Envoy, version `authentication-service-2/v2

## RANKED HYPOTHESES 2026-09-07 03:27:00 UTC
- [65] authentication-service.eks.core-production.keyless.technology: Keyless auth service uses non-standard API paths requiring discovery (from art/lead_nemotron3.txt)
- [25] app.n26.com/graphql: GraphQL schema via WebSocket/persisted-query upgrade on app.n26.com (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: escalate to program coordinator — request scoped test account or LivePerson/app session to enable AUTH_HELPED GraphQL WS/APQ testing (the only remaining 
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://authentication-service.eks.core-production.keyless.technology/api/v2 → GET /v2 → GET /internal → GET /private → GET /admin → GET /actuator/he
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST /v1/initialize 200-with-data is Statsig DESIGN — public client key unchanged across bundle rotation, browser SDK sends 
- LEARN: REJECTED MISCONFIG @ app.n26.com: numeric Statsig IDs (3526595..4173610755) absent from all 11 bundles — IDs server-assigned, name↔ID mapping not passively reco
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC is method+shape dependent (GET initialize 403 vs POST 200), but the 401 on download_config_specs proves app-layer
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST /v1/initialize 200-with-data is Statsig DESIGN — public client key unchanged across bundle rotation, browser SDK sends 
- LEARN: REJECTED MISCONFIG @ app.n26.com: numeric Statsig IDs (3526595..4173610755) absent from all 11 bundles — IDs server-assigned, name↔ID mapping not passively reco
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded → no server-key escalati
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes return 403
- LEARN: REJECTED AUTH @ support.n26.com/graphql: OPTIONS 204, bare-GET stalls identically to app.n26.com (25s, 0B) — shared Envoy/WAF tarpit; GraphQL transport class cl
- LEARN: REJECTED MISCONFIG @ cdn.number26.de: `/` and `/?list-type=2` both 403 AccessDenied (S3+CloudFront) — private bucket, object-only; no listing/misconfig
- LEARN: ACCEPTED MISCONFIG @ n26.com: marketing on Envoy+CloudFront with wildcard CSP; `cookie.n26.com` 404 leaf — low-logic static content, INFO ceiling only
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector
- LEARN: ACCEPTED AUTH @ engagementplatform.n26.com: /users returns 401 key-gated (not 403/tarpit) — distinct live boundary; web-side Bearer key embedding negative acros
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary stable — initialize(POST+body)→200 by-design (INFO), sdk_exception GET→403, download_config_specs→401, all GET
- LEARN: ACCEPTED AUTH @ app.n26.com/graphql: tarpit/WAF-closed unchanged across all anonymous transports; no differential ingress appears.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST `/v1/initialize` + canonical SDK payload + public client key returns 200 with full flag/config disclosure — reclassifie
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403.
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded → no server-key escalati
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes return 403.
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API.
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure.
- LEARN: REJECTED AUTH @ support.n26.com/graphql: OPTIONS 204, bare-GET stalls identically to app.n26.com (25s, 0B) — shared Envoy/WAF tarpit; GraphQL transport class cl
- LEARN: REJECTED MISCONFIG @ cdn.number26.de: `/` and `/?list-type=2` both 403 `AccessDenied` (S3+CloudFront) — private bucket, object-only; no listing/misconfig.
- LEARN: ACCEPTED MISCONFIG @ n26.com: marketing on Envoy+CloudFront with wildcard CSP; `cookie.n26.com` 404 leaf — low-logic static content, INFO ceiling only.
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses; WAF blocks all POST Content-Types; GET query param connection reset.
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ authentication-service.eks.core-production.keyless.technology: Service LIVE (HTTP/2 404), Istio/Envoy, version `authentication-service-2/v2

## RANKED HYPOTHESES 2026-09-07 08:42:53 UTC
- [65] authentication-service.eks.core-production.keyless.technology: Keyless auth service uses non-standard API paths requiring discovery (from art/lead_nemotron3.txt)
- [65] authentication-service.eks.core-production.keyless.technology: Keyless auth service non-standard route discovery (survivor, scope-pending) (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: escalate consolidated coordinator ask — (a) written scope ruling on `keyless.technology` vendor-in-flow asset (tenant slug `n26`, `wss://…/v1/auth/n26`, 
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://authentication-service.eks.core-production.keyless.technology/api/v2 → GET /v2 → GET /internal → GET /private → GET /admin → GET /actuator/he
- LEARN: ACCEPTED AUTH @ engagementplatform.n26.com: /users returns 401 key-gated (not 403/tarpit) — distinct live boundary; web-side Bearer key embedding negative acros
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary stable — initialize(POST+body)→200 by-design (INFO), sdk_exception GET→403, download_config_specs→401, all GET
- LEARN: ACCEPTED AUTH @ app.n26.com/graphql: tarpit/WAF-closed unchanged across all anonymous transports; no differential ingress appears.
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: POST `/v1/initialize` + canonical SDK payload + public client key returns 200 with full flag/config disclosure — reclassifie
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: RBAC boundary = initialize(POST+body)→200+data, sdk_exception→202, download_config_specs→401, all GET config→403.
- LEARN: REJECTED MISCONFIG @ flags.n26.com server-key path: full bundle sweep found ONE key (public client key); no server/secret keys embedded → no server-key escalati
- LEARN: ACCEPTED MISCONFIG @ flags.n26.com: Envoy RBAC route map fully enumerated — only sdk_exception/download_config_specs bypass; all config routes return 403.
- LEARN: REJECTED IDOR @ spc.n26.com: versioned endpoints are 1x1 GIF tracking pixels (len=43), not a payment API.
- LEARN: REJECTED AUTH @ app.n26.com: WAF normalizes Content-Type; urlencoded/text/plain/multipart all 403 — WAF inspects body structure.
- LEARN: REJECTED AUTH @ support.n26.com/graphql: OPTIONS 204, bare-GET stalls identically to app.n26.com (25s, 0B) — shared Envoy/WAF tarpit; GraphQL transport class cl
- LEARN: REJECTED MISCONFIG @ cdn.number26.de: `/` and `/?list-type=2` both 403 `AccessDenied` (S3+CloudFront) — private bucket, object-only; no listing/misconfig.
- LEARN: ACCEPTED MISCONFIG @ n26.com: marketing on Envoy+CloudFront with wildcard CSP; `cookie.n26.com` 404 leaf — low-logic static content, INFO ceiling only.
- LEARN: ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403/timeout responses; WAF blocks all POST Content-Types; GET query param connection reset.
- LEARN: REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- LEARN: ACCEPTED MISCONFIG @ authentication-service.eks.core-production.keyless.technology: Service LIVE (HTTP/2 404), Istio/Envoy, version `authentication-service-2/v2
- LEARN: ACCEPTED AUTH @ engagementplatform.n26.com: /users returns 401 key-gated (not 403/tarpit) — distinct live boundary; web-side Bearer key embedding negative acros
