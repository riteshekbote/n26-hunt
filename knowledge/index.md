# Knowledge Base (seed)
- 2026-09-03 ACCEPTED AUTH @ app.n26.com: GraphQL confirmed via cookie + 403 responses (not 404). WAF actively blocks POST. Bypass exploration warranted.
- 2026-09-03 ACCEPTED MISCONFIG @ flags.n26.com: Statsig instance with RBAC, behind CloudFront+GKE. Client-side SDK key extraction from app bundle is viable path.
- 2026-09-03 REJECTED MISCONFIG @ my.n26.com: Server-side 301 redirect, not dangling DNS. No subdomain takeover vector.
- 2026-09-03 ACCEPTED MISCONFIG @ spc.n26.com: Live payment service with /health endpoint. API enumeration needed.
