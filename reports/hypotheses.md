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
