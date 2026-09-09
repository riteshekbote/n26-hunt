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
## REPOSCAN 2026-09-05 20:42:43 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-05 22:38:44 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-06 00:13:25 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-06 04:47:27 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-06 09:08:57 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-06 13:05:45 UTC
[HYP] Hardcoded Basic Auth credentials for MFA challenge endpoint
class: SECRET
asset: n26/psd2-tpp-docs/doc/assets/bash/pin_encryption_and_initiating_transaction.sh:26
confidence: 35
reasoning: Base64-encoded "android:secret" hardcoded in Authorization:Basic header for POST to /api/mfa/challenge on pisp.tech26.de. However this lives in a sandbox/documentation repo (PSD2 TPP docs) for third-party developers and uses template variables ($CLIENT_KEY_FILE, $USER_NAME, $PASSWORD) for real credentials. The "android:secret" is a well-known OAuth2 client credential pair for Android demo clients, not a production secret.
impact: INFO (sandbox documentation artifact, not production)
verify_steps: 1) Confirm pisp.tech26.de is a sandbox-only host 2) Verify this credential is documented as a demo value in N26 PSD2 developer docs 3) Check if this credential grants any real access
[HYP] Internal Artifactory registry URL exposed in public npm lockfile
class: MISCONFIG
asset: n26/express-simple-locale/package-lock.json:1518
confidence: 30
reasoning: package-lock.json contains resolved URLs pointing to internal N26 Artifactory instance at artifactory.cd-tech26.de (e.g., https://artifactory.cd-tech26.de:443/artifactory/api/npm/npm/lodash/-/lodash-4.17.21.tgz). Reveals internal artifact registry hostname. However this is a public open-source npm package repo — the Artifactory URL is a development-time dependency source, not a credential.
impact: INFO (internal hostname disclosure via public repo)
verify_steps: 1) Confirm artifactory.cd-tech26.de resolves 2) Check if it's publicly accessible or VPN-only 3) Assess if hostname alone is actionable
[HYP] S3 staging bucket name disclosed in sample data
class: MISCONFIG
asset: n26/N26AndroidSamples/credit/src/main/res/raw/credit_drafts.json:8
confidence: 20
reasoning: Mock JSON data references S3 bucket "consumercredit-staging" in eu-central-1 (https://s3.eu-central-1.amazonaws.com/consumercredit-staging/...). Bucket is staging environment with only PNG images referenced. This is sample/demo data in a public Android sample app repo.
impact: INFO (staging bucket name disclosure, images only)
verify_steps: 1) Confirm bucket exists via HEAD request 2) Verify bucket is private (already confirmed via cdn.number26.de probes in inventory)
[HYP] Sandbox Postman environment with placeholder tokens
class: OTHER
asset: n26/psd2-tpp-docs/doc/assets/postman/XS2A_N26_Sandbox.postman_environment.json
confidence: 10
reasoning: Postman environment file contains UUIDs used as placeholder values for access_token, device_token, consent_id, etc. All identical dummy value "f3f51978-20fb-4bd9-91d9-7cc2a1fd9618". Also contains dedicated_aisp_client_id = "w6uP8Tcg6K2QR905Rms8iXTlksL6OD1KOWBxTK7wxPI" (the public Statsig client key already discovered in app bundle). All are sandbox/sample values for third-party PSD2 developer documentation.
impact: INFO (no real secrets, sandbox documentation)
verify_steps: 1) Confirm all tokens are placeholders 2) Verify dedicated_aisp_client_id matches public Statsig key already enumerated
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-06 15:58:51 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-06 18:11:30 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-06 20:29:21 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-06 22:18:56 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-07 00:05:52 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-07 04:51:19 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-07 09:56:42 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-07 15:33:47 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-07 19:25:24 UTC
class: OTHER
asset: express-simple-locale/package-lock.json
confidence: 15
reasoning: Resolved npm packages point to https://artifactory.cd-tech26.de:443/artifactory/api/npm/npm/ — an internal N26 Artifactory instance. This is a standard npm lockfile entry, not a credential. The URL is publicly observable from any `npm install` run against this package.
impact: Informational — internal infrastructure URL discoverable via lockfile
verify_steps: Run `npm install` on express-simple-locale and observe registry resolution
class: OTHER
asset: psd2-tpp-docs/doc/assets/postman/XS2A_N26_Sandbox.postman_environment.json
confidence: 5
reasoning: Contains `dedicated_aisp_client_id` = `w6uP8Tcg6K2QR905Rms8iXTlksL6OD1KOWBxTK7wxPI` and `auth_code`. All values in this file are identical placeholder UUIDs (`f3f51978-...`) and known sandbox test values. This is intentional TPP documentation — PSD2 client_ids are public by design (they identify the TPP, not a secret). The `auth_code` is a one-time-use code that has expired. This file is shipped as part of the PSD2 integration docs.
impact: Informational — sandbox-only public documentation values
verify_steps: Verify the client_id does not grant access to production endpoints by checking it is sandbox-scoped
class: OTHER
asset: psd2-tpp-docs/doc/sandbox.md
confidence: 10
reasoning: Sandbox test emails like `openbankingpsu@n26.com`, `test.account+...@n26.com` are documented as public test accounts for TPP developers. These are intentionally shared.
impact: None — public sandbox test accounts
verify_steps: N/A — documented in public TPP onboarding docs
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-07 22:15:53 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-08 00:28:53 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-08 05:10:59 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-08 09:51:39 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-08 14:08:04 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-08 17:59:24 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-08 20:51:45 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-08 23:11:35 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-09 01:17:26 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-09 06:04:54 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-09 11:32:03 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-09 15:19:09 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-09 18:41:57 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
