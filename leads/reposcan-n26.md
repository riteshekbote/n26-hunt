## REPOSCAN 2026-09-03 15:50:22 UTC
[HYP] Hardcoded Basic Auth credential `android:secret` in PSD2 MFA Challenge script
class: SECRET
asset: n26/psd2-tpp-docs/doc/assets/bash/pin_encryption_and_initiating_transaction.sh:26
confidence: 65
reasoning: The script contains a hardcoded Base64-encoded Basic Auth header `YW5kcm9pZDpzZWNyZXQ=` which decodes to `android:secret`. This credential is used in a `curl` call to `https://$PISP_HOST/api/mfa/challenge` (default host `pisp.tech26.de`). If this is a real static credential accepted by the MFA challenge endpoint (rather than a per-session token), it could allow unauthorized triggering of out-of-band MFA challenges for any user. The script is published as a TPP example in the open-banking documentation repo and hits what appears to be the real PISP endpoint. The credential may be a well-known sandbox/example value, but if it is accepted by the production endpoint it constitutes a leaked platform credential.
impact: Medium — potential for unauthorized MFA challenge initiation against N26 users if the credential is live
verify_steps: (1) Passively check if `pisp.tech26.de/api/mfa/challenge` accepts `Authorization: Basic YW5kcm9pZDpzZWNyZXQ=` by observing public API docs or error responses. (2) Confirm whether the endpoint uses a static app-level credential or per-user tokens.
[HYP] Internal S3 staging bucket name disclosure
class: MISCONFIG
asset: n26/N26AndroidSamples/credit/src/main/res/raw/credit_drafts.json:8,30
confidence: 50
reasoning: Mock credit data in the Android sample app references `https://s3.eu-central-1.amazonaws.com/consumercredit-staging/` for image assets. This reveals the internal AWS S3 bucket naming convention (`consumercredit-staging`) for N26's consumer credit staging environment. If the bucket lacks proper access controls, the naming pattern could aid enumeration or direct access. This is mock data in a sample app, so the actual bucket may or may not still exist.
impact: Low — informational disclosure of internal bucket naming; actual exploitation depends on bucket ACL configuration
verify_steps: (1) Passively verify if `s3://consumercredit-staging` is publicly listable or accessible via `aws s3 ls s3://consumercredit-staging/ --no-sign-request`. (2) No further active testing required.
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-03 19:05:48 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-03 21:44:42 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-03 23:46:39 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-04 02:54:55 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-04 07:43:45 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-04 12:30:01 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-04 16:35:52 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-04 19:08:05 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-04 21:34:43 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-04 23:17:42 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-05 01:05:53 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-05 05:50:28 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-05 09:57:06 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-05 13:17:46 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-05 16:08:48 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-05 18:24:32 UTC
[HYP] Hardcoded Base64-Coded OAuth Credential in Sample Script
class: SECRET
asset: psd2-tpp-docs/doc/assets/bash/pin_encryption_and_initiating_transaction.sh:26
confidence: 85
reasoning: The script contains a hardcoded `Authorization:Basic YW5kcm9pZDpzZWNyZXQ=` header which decodes to `android:secret` — a plaintext client ID/secret pair used for MFA challenge API calls. This is a real OAuth2 Basic auth credential embedded in a public documentation sample. If the same credential is valid against the live `pisp.tech26.de` or `xs2a.tech26.de` endpoints, it could allow unauthorized MFA challenge initiation.
impact: Medium — credential may still be valid against production/staging PISP endpoint
verify_steps: 
[HYP] Internal S3 Bucket Name and Staging Environment URL Leaked
class: MISCONFIG
asset: N26AndroidSamples/credit/src/main/res/raw/credit_drafts.json:8,30
confidence: 75
reasoning: Two URLs expose the internal AWS S3 bucket name `consumercredit-staging` under `eu-central-1.amazonaws.com`. This reveals: (1) the exact S3 bucket naming convention for the consumer credit service, (2) that a staging environment exists at this path. While the S3 bucket currently returns 404 (access denied or deleted), the bucket name is useful for targeted enumeration or social engineering. The hardcoded staging data also includes loan amounts, repayment schedules, and UUIDs.
impact: Low — bucket appears non-publicly accessible (404), but internal naming convention is exposed
verify_steps: 
[HYP] Sandbox OAuth Client ID and IBAN Exposed in Postman Collection
class: SECRET
asset: psd2-tpp-docs/doc/assets/postman/XS2A_N26_Sandbox.postman_environment.json
confidence: 70
reasoning: The Postman environment file contains hardcoded sandbox credentials: `dedicated_aisp_client_id=w6uP8Tcg6K2QR905Rms8iXTlksL6OD1KOWBxTK7wxPI`, `auth_code=w6uP8Tcg6K2QR905Rms8iXTlksL6OD1KOWBxTK`, sandbox IBAN `DE15100110012627633320`, and a sandbox `user_id`/`consent_id` UUID. The collection also references OAuth client IDs `PSDDE-BAFIN-000001` and `PSDES-BDE-3DFD12`. While labeled "sandbox," if the sandbox OAuth tokens are reusable or if the same client_id works in production, this could allow unauthorized API access.
impact: Low — explicitly sandboxed environment, but shared sandbox credentials may be reusable
verify_steps: 
[HYP] Internal API Hostname Exposed in Public Documentation Script
class: MISCONFIG
asset: psd2-tpp-docs/doc/assets/bash/pin_encryption_and_initiating_transaction.sh:8
confidence: 65
reasoning: The script defaults `PISP_HOST` to `pisp.tech26.de` — an internal N26 Payment Initiation Service Provider hostname. This reveals the existence and naming convention of internal banking API infrastructure. Combined with the hardcoded credential (`android:secret`), this provides a clear attack surface.
impact: Low — hostname alone is not exploitable, but combined with the credential finding increases attack surface
verify_steps: 
[HYP] Slack Bot Token Passed via URL Query Parameter
class: MISCONFIG
asset: bob/Sources/Bob/Core/Slack/SlackClient.swift:31
confidence: 55
reasoning: The bob bot passes the Slack API token as a URL query parameter (`token=...`) to `https://slack.com/api/rtm.start`. While this is the deprecated Slack RTM API pattern (now superseded by Socket Mode), tokens in URL query strings can appear in server logs, proxy logs, and browser history. The token is stored as a `private let` in memory, so it's not hardcoded — but the传输 pattern is insecure.
impact: Low — token is not hardcoded (loaded from config), but URL-based transmission is an insecure pattern
verify_steps: 
TARGET_ORG not configured for n26; skipping public-org deep scan.
