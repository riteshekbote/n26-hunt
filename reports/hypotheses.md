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
