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
## REPOSCAN 2026-09-09 21:29:49 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-09 23:30:18 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-10 01:26:51 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-10 06:38:11 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-10 11:48:31 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-10 15:49:54 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-10 18:57:50 UTC
[HYP] Hardcoded Basic Auth Credential `android:secret` in PSD2 MFA Script
class: SECRET
asset: n26/psd2-tpp-docs/doc/assets/bash/pin_encryption_and_initiating_transaction.sh:26
confidence: 60
reasoning: Line 26 contains `Authorization:Basic YW5kcm9pZDpzZWNyZXQ=` (decodes to `android:secret`) hardcoded in a curl call to `https://$PISP_HOST/api/mfa/challenge`. The script is a TPP onboarding example but targets the real PISP endpoint `pisp.tech26.de` (confirmed live in passive recon). The credential is a static app-level OAuth client ID/secret pair, not a per-user token. If accepted by the production MFA challenge endpoint, it could allow unauthorized out-of-band MFA challenge initiation. The credential may be a well-known sandbox/demo value per PSD2 documentation conventions.
impact: Medium — potential unauthorized MFA challenge trigger if credential is live on production PISP
verify_steps: (1) Confirm `pisp.tech26.de/api/mfa/challenge` accepts `Basic YW5kcm9pZDpzZWNyZXQ=` passively by observing public API docs or error responses. (2) Check N26 PSD2 developer docs to verify if `android:secret` is documented as a sandbox-only credential.
[HYP] Internal S3 Bucket Name Disclosure in Android Sample Data
class: MISCONFIG
asset: n26/N26AndroidSamples/credit/src/main/res/raw/credit_drafts.json:8,30
confidence: 40
reasoning: Two mock credit entries reference `https://s3.eu-central-1.amazonaws.com/consumercredit-staging/` for image assets. This discloses the internal AWS S3 bucket naming convention (`consumercredit-staging`) for N26's consumer credit staging environment. The bucket currently returns 403/NoSuchBucket (confirmed via passive recon at cdn.number26.de), so it may be deleted or private. However, the naming pattern reveals internal infrastructure naming and could aid enumeration if the bucket is recreated or if similar naming conventions apply to other staging buckets.
impact: Low — bucket appears non-public/deleted; informational naming convention leak only
verify_steps: (1) Already confirmed bucket is non-public via cdn.number26.de probes. (2) No further action needed unless staging bucket is recreated.
[HYP] Sandbox OAuth Client ID and Internal Hostname Exposed in Postman Collection
class: MISCONFIG
asset: n26/psd2-tpp-docs/doc/assets/postman/XS2A_N26_Sandbox.postman_environment.json
confidence: 30
reasoning: Postman environment file contains sandbox credentials: `dedicated_aisp_client_id=w6uP8Tcg6K2QR905Rms8iXTlksL6OD1KOWBxTK7wxPI`, `auth_code`, sandbox IBAN `DE15100110012627633320`, and internal hostname `xs2a.tech26.de`. While explicitly labeled as sandbox, these values: (1) reveal internal PSD2 API hostname and OAuth client registration format, (2) the `auth_code` is a one-time-use value that has expired, (3) the `dedicated_aisp_client_id` matches the public Statsig client key already discovered in app bundles. All values are documented sandbox test artifacts for TPP developers.
impact: Informational — sandbox-only documentation values, no production secrets
verify_steps: (1) Confirm `xs2a.tech26.de` is sandbox-only. (2) Verify `dedicated_aisp_client_id` is a public client key, not a secret.
[HYP] Slack Token Transmitted via URL Query Parameter
class: MISCONFIG
asset: n26/bob/Sources/Bob/Core/Slack/SlackClient.swift:31
confidence: 35
reasoning: The Slack RTM client passes the API token as a URL query parameter (`token=...`) to `https://slack.com/api/rtm.start`. Tokens in URL query strings appear in server logs, proxy logs, and browser history. The token is loaded from config at runtime (not hardcoded), but the transmission pattern is an insecure practice per OWASP. This is the deprecated Slack RTM API pattern (superseded by Socket Mode).
impact: Low — token is not hardcoded (loaded from config); insecure transmission pattern only
verify_steps: (1) Verify token is not committed anywhere in the repo. (2) Confirm this is the deprecated RTM API pattern.
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-10 21:27:54 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-10 23:23:01 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-11 01:26:30 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-11 06:40:08 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-11 11:48:03 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-11 15:52:15 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-11 18:58:34 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-11 21:37:26 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-11 23:31:29 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-12 01:31:35 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-12 06:29:42 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-12 11:15:20 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-12 14:12:51 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-12 17:15:18 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-12 19:31:02 UTC
[HYP] Hardcoded Basic Auth Credential `android:secret` in PSD2 MFA Challenge Script
class: SECRET
asset: n26/psd2-tpp-docs/doc/assets/bash/pin_encryption_and_initiating_transaction.sh:26
confidence: 70
reasoning: Line 26 contains a hardcoded Base64-encoded Basic Auth header `YW5kcm9pZDpzZWNyZXQ=` which decodes to `android:secret`. This credential is used in a curl POST to `https://$PISP_HOST/api/mfa/challenge` (defaults to `pisp.tech26.de`). The credential is a static app-level OAuth client ID/secret pair, not a per-user token. If accepted by the live MFA challenge endpoint, it could allow unauthorized out-of-band MFA challenge initiation for any user. The credential may be a well-known sandbox/demo value per PSD2 documentation conventions, but it targets a real PISP endpoint that is confirmed reachable (returns 404 rather than DNS failure).
impact: Medium — potential unauthorized MFA challenge trigger if credential is live on production PISP
verify_steps: (1) Confirm whether `pisp.tech26.de` is a sandbox-only or production host by checking HTTP response headers/body on error paths. (2) Check N26 PSD2 developer docs to verify if `android:secret` is documented as a sandbox-only credential. (3) Test if the endpoint accepts the credential by observing error response differences with/without the header.
[HYP] Slack Bot Token Transmitted via URL Query Parameter
class: MISCONFIG
asset: n26/bob/Sources/Bob/Core/Slack/SlackClient.swift:31
confidence: 65
reasoning: The bob Slack bot passes the API token as a URL query parameter (`token=...`) to the deprecated Slack RTM API endpoint `https://slack.com/api/rtm.start`. Tokens in URL query strings appear in server logs, proxy logs, browser history, and HTTP Referer headers. The token is loaded from config at runtime (not hardcoded in source), but the transmission pattern is an insecure practice per OWASP. The Slack RTM API is deprecated (superseded by Socket Mode). Bob is an archived internal N26 tool used for TravisCI/GitHub communication.
impact: Low — token is not hardcoded (loaded from config at runtime); insecure transmission pattern only; bot is archived
verify_steps: (1) Verify no token values are committed in the repo's config files or .env. (2) Confirm the bot is no longer actively deployed (repo is archived since 2020). (3) If still deployed, rotate the Slack bot token and migrate to Socket Mode.
[HYP] Internal S3 Bucket Name `consumercredit-staging` Disclosed in Android Sample Data
class: MISCONFIG
asset: n26/N26AndroidSamples/credit/src/main/res/raw/credit_drafts.json:8,30
confidence: 55
reasoning: Two mock credit entries reference `https://s3.eu-central-1.amazonaws.com/consumercredit-staging/` for image assets. This discloses the internal AWS S3 bucket naming convention (`consumercredit-staging`) for N26's consumer credit staging environment in eu-central-1. The bucket currently returns 404 (passive verification confirmed), so it may be deleted or ACL-restricted. However, the naming pattern reveals internal infrastructure naming conventions that could aid enumeration if staging buckets are recreated or if similar patterns apply to other services.
impact: Low — bucket appears non-public/deleted (404 confirmed); informational naming convention leak only
verify_steps: (1) Already confirmed bucket is non-existent/non-public via HEAD request (404). (2) No further action needed unless staging bucket is recreated.
[HYP] Internal Artifactory Registry URL Disclosed in Public npm Lockfile
class: MISCONFIG
asset: n26/express-simple-locale/package-lock.json:1518,2429,3391,3571,7527
confidence: 40
reasoning: The package-lock.json contains resolved URLs pointing to an internal N26 Artifactory instance at `artifactory.cd-tech26.de:443` (e.g., `https://artifactory.cd-tech26.de:443/artifactory/api/npm/npm/lodash/-/lodash-4.17.21.tgz`). This reveals the internal artifact registry hostname and that it hosts N26's npm packages. The endpoint is not publicly reachable (connection refused / timeout), suggesting it is VPN-internal only. This is a standard npm lockfile entry, not a credential.
impact: Informational — internal hostname disclosure; Artifactory is not publicly accessible
verify_steps: (1) Already confirmed Artifactory is not publicly reachable (connection timeout). (2) No further action needed.
[HYP] Internal PSD2 API Hostname `pisp.tech26.de` Exposed in Public Documentation Script
class: MISCONFIG
asset: n26/psd2-tpp-docs/doc/assets/bash/pin_encryption_and_initiating_transaction.sh:8
confidence: 60
reasoning: The script defaults `PISP_HOST` to `pisp.tech26.de` — an internal N26 Payment Initiation Service Provider hostname. The endpoint is reachable (returns HTTP 404, not DNS failure), confirming it is a real host. Combined with the hardcoded `android:secret` credential finding above, this provides a complete attack surface for the MFA challenge flow. The hostname reveals N26's internal PSD2 API naming convention.
impact: Low — hostname alone is not directly exploitable; combined with the credential finding increases attack surface
verify_steps: (1) Already confirmed `pisp.tech26.de` resolves and responds (HTTP 404). (2) Check if the host serves other endpoints beyond `/api/mfa/challenge`.
[HYP] Sandbox OAuth Client IDs and Internal Hostname in Postman Environment
class: MISCONFIG
asset: n26/psd2-tpp-docs/doc/assets/postman/XS2A_N26_Sandbox.postman_environment.json
confidence: 35
reasoning: The Postman environment file contains sandbox credentials: `dedicated_aisp_client_id=w6uP8Tcg6K2QR905Rms8iXTlksL6OD1KOWBxTK7wxPI`, `auth_code=w6uP8Tcg6K2QR905Rms8iXTlksL6OD1KOWBxTK`, sandbox IBAN `DE15100110012627633320`, and internal hostname `xs2a.tech26.de` (confirmed live — returns HTTP 401). The collection also references OAuth client IDs `PSDDE-BAFIN-000001` and `PSDES-BDE-3DFD12` as code_challenge values. While explicitly labeled "sandbox," these values reveal the internal PSD2 API hostname and OAuth registration format. The `auth_code` is a one-time-use value that has expired. The `dedicated_aisp_client_id` matches a public Statsig client key already discovered in app bundles.
impact: Informational — sandbox-only documentation values; `xs2a.tech26.de` is a real endpoint (returns 401)
verify_steps: (1) Already confirmed `xs2a.tech26.de` is live (returns 401 for unauthenticated requests). (2) Verify the client_id values are sandbox-scoped only.
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-12 21:37:37 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-12 23:19:43 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-13 01:10:59 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-13 06:16:44 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-13 12:03:27 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-13 16:25:18 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-13 18:56:08 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-13 21:15:52 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-13 23:13:20 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-14 01:13:32 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-14 06:24:17 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-14 13:19:06 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-14 18:43:13 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-14 22:15:51 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-15 00:41:46 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-15 05:48:55 UTC
[HYP] <title>
class: SECRET|MISCONFIG|IDOR|SSRF|OTHER
asset: n26/<repo>/<path>
confidence: <0-100>
reasoning: <facts>
impact: <severity>
verify_steps: <passive confirmation steps>
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-15 11:10:28 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-15 15:35:56 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-15 19:11:24 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-15 22:18:03 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-16 00:29:36 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-16 05:13:08 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-16 10:01:33 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-16 15:01:14 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-16 18:56:22 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-16 21:54:29 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-17 00:04:19 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-17 04:58:56 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-17 09:55:36 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-17 14:42:20 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-17 18:48:42 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-17 21:55:20 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-17 23:55:29 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-18 03:11:22 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-18 08:35:22 UTC
[HYP] Hardcoded Sandbox Client ID and Auth Code in Postman Environment
class: MISCONFIG
asset: n26/psd2-tpp-docs/doc/assets/postman/XS2A_N26_Sandbox.postman_environment.json
confidence: 35
reasoning: The Postman environment file contains hardcoded `client_id` (`PSDDE-BAFIN-000001`), `auth_code` (`w6uP8Tcg6K2QR905Rms8iXTlksL6OD1KOWBxTK`), and a sample IBAN (`DE15100110012627633320`). However, these are explicitly documented sandbox/test values — the docs state these are sample PKCE flows with `code_verifier="foobar"` and the client_id is labeled as a sample TPP ID. The environment is named "XS2A_N26_Sandbox".
impact: Low — these are intentionally public sandbox test credentials, not production secrets
verify_steps: Confirm `xs2a.tech26.de/sandbox` is indeed a public sandbox (not production) by accessing it; verify `PSDDE-BAFIN-000001` is a documented sample client ID in the PSD2 docs
[HYP] Hardcoded Basic Auth Header in Example Script
class: SECRET
asset: n26/psd2-tpp-docs/doc/assets/bash/pin_encryption_and_initiating_transaction.sh:26
confidence: 30
reasoning: The bash script contains a hardcoded Base64-encoded Basic Auth header: `Authorization:Basic YW5kcm9pZDpzZWNyZXQ=` which decodes to `android:secret`. This is used in a sandbox example for MFA challenge flow. However, this appears to be a sample/example credential for the sandbox environment, not a production secret.
impact: Low — sandbox example credential, not production
verify_steps: Check if `pisp.tech26.de` is a public sandbox endpoint; verify `android:secret` is documented as a sample credential
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-18 13:08:13 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-18 17:08:38 UTC
[HYP] Hardcoded Basic Auth credential `android:secret` in PSD2 MFA challenge script
class: SECRET
asset: n26/psd2-tpp-docs/doc/assets/bash/pin_encryption_and_initiating_transaction.sh:26
confidence: 75
reasoning: Line 26 contains a hardcoded Base64-encoded Basic Auth header `YW5kcm9pZDpzZWNyZXQ=` which decodes to `android:secret`. This static credential is used in a curl POST to `https://$PISP_HOST/api/mfa/challenge` (default host `pisp.tech26.de`, confirmed live via HTTP 404). If the credential is accepted by the production MFA endpoint, it could allow unauthorized out-of-band MFA challenge initiation for any user.
impact: Medium — potential unauthorized MFA challenge trigger if credential is live on production PISP
verify_steps: (1) Passively confirm `pisp.tech26.de/api/mfa/challenge` accepts `Basic YW5kcm9pZDpzZWNyZXQ=` by observing public API docs or error responses. (2) Verify `android:secret` is documented as a sandbox-only credential in N26 PSD2 developer docs.
[HYP] Internal S3 staging bucket name disclosure
class: MISCONFIG
asset: n26/N26AndroidSamples/credit/src/main/res/raw/credit_drafts.json:8,30
confidence: 65
reasoning: Two mock credit entries reference `https://s3.eu-central-1.amazonaws.com/consumercredit-staging/` for image assets, exposing the internal AWS S3 bucket naming convention (`consumercredit-staging`) for N26's consumer credit staging environment. The bucket currently returns 403/NoSuchBucket (confirmed via passive recon), but the naming pattern reveals internal infrastructure naming that could aid enumeration if staging buckets are recreated.
impact: Low — bucket appears non-public/deleted; informational naming convention leak only
verify_steps: (1) Already confirmed bucket is non-public via cdn.number26.de probes. (2) No further action needed unless staging bucket is recreated.
[HYP] Internal PSD2 API hostnames exposed in public documentation
class: MISCONFIG
asset: n26/psd2-tpp-docs/doc/sandbox.md, n26/psd2-tpp-docs/doc/assets/bash/pin_encryption_and_initiating_transaction.sh
confidence: 70
reasoning: Multiple files expose internal PSD2 API hostnames (`xs2a.tech26.de`, `pisp.tech26.de`, `aisp.tech26.de`). These are real endpoints (confirmed reachable via HTTP 404/401 responses). Combined with the hardcoded `android:secret` credential, this provides a complete attack surface for the MFA challenge flow.
impact: Low — hostnames alone are not directly exploitable; combined with credential findings increases attack surface
verify_steps: (1) Already confirmed hostnames resolve and respond (HTTP 404/401). (2) Check if hosts serve other endpoints beyond documented paths.
[HYP] Slack bot token transmitted via URL query parameter
class: MISCONFIG
asset: n26/bob/Sources/Bob/Core/Slack/SlackClient.swift:31
confidence: 60
reasoning: The bob Slack bot passes the API token as a URL query parameter (`token=...`) to the deprecated Slack RTM API endpoint `https://slack.com/api/rtm.start`. Tokens in URL query strings appear in server logs, proxy logs, browser history, and HTTP Referer headers. The token is loaded from config at runtime (not hardcoded), but the transmission pattern is an insecure practice per OWASP.
impact: Low — token is not hardcoded (loaded from config at runtime); insecure transmission pattern only; bot is archived
verify_steps: (1) Verify no token values are committed in the repo's config files or .env. (2) Confirm the bot is no longer actively deployed (repo is archived since 2020).
[HYP] Internal Artifactory registry URL disclosed in public npm lockfile
class: MISCONFIG
asset: n26/express-simple-locale/package-lock.json:1518,2429,3391,3571,7527
confidence: 45
reasoning: The package-lock.json contains resolved URLs pointing to an internal N26 Artifactory instance at `artifactory.cd-tech26.de:443`. This reveals the internal artifact registry hostname. The endpoint is not publicly reachable (connection refused/timeout), suggesting it is VPN-internal only.
impact: Informational — internal hostname disclosure; Artifactory is not publicly accessible
verify_steps: (1) Already confirmed Artifactory is not publicly reachable (connection timeout). (2) No further action needed.
[HYP] Sandbox OAuth client IDs and internal hostname in Postman environment
class: MISCONFIG
asset: n26/psd2-tpp-docs/doc/assets/postman/XS2A_N26_Sandbox.postman_environment.json
confidence: 35
reasoning: The Postman environment file contains sandbox credentials: `dedicated_aisp_client_id=w6uP8Tcg6K2QR905Rms8iXTlksL6OD1KOWBxTK7wxPI`, `auth_code`, sandbox IBAN `DE15100110012627633320`, and internal hostname `xs2a.tech26.de` (confirmed live — returns HTTP 401). While explicitly labeled "sandbox," these values reveal the internal PSD2 API hostname and OAuth registration format.
impact: Informational — sandbox-only documentation values; `xs2a.tech26.de` is a real endpoint (returns 401)
verify_steps: (1) Already confirmed `xs2a.tech26.de` is live (returns 401 for unauthenticated requests). (2) Verify the client_id values are sandbox-scoped only.
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-18 19:41:36 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-18 22:16:52 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-19 12:00:00 UTC (manual fresh audit — all 5 N26 public repos)
[HYP] Hardcoded PKCE code_verifier defeats OAuth2 CSRF protection
class: SECRET
asset: n26/psd2-tpp-docs/doc/assets/postman/XS2A_N26_Sandbox.postman_collection.json:250-252
confidence: 80
reasoning: The Postman sandbox collection hardcodes code_verifier="foobar" (lines 250-252) paired with code_challenge=w6uP8Tcg6K2QR905Rms8iXTlksL6OD1KOWBxTK7wxPI (same value as dedicated_aisp_client_id, lines 99-100). PKCE relies on a random, unguessable code_verifier per authorization request; a static value eliminates the CSRF interception protection PKCE was designed to provide. Although labeled "sandbox," this collection is the official TPP onboarding reference — TPPs copying this pattern into production integrations will ship a broken PKCE flow. If N26's production xs2a.tech26.de accepts code_verifier="foobar" for any registered TPP, the authorization code interception protection is void.
impact: Medium — breaks PKCE security guarantee for any TPP following this sample; could enable authorization code interception if combined with other flaws
verify_steps: (1) Passively verify whether xs2a.tech26.de/sandbox/oauth2/token rejects requests with code_verifier="foobar" when code_challenge doesn't match. (2) Check if production xs2a endpoint enforces strict PKCE code_challenge verification. (3) Confirm this code_challenge is indeed the SHA-256 hash of "foobar" or a separate static value.
[HYP] Production N26 login URL in sandbox Postman environment
class: MISCONFIG
asset: n26/psd2-tpp-docs/doc/assets/postman/XS2A_N26_Sandbox.postman_environment.json:81-82
confidence: 55
reasoning: The sandbox Postman environment contains redirect_uri=https://app.n26.com/login — a production N26 web app URL — as the default OAuth2 redirect target for all sandbox authorization flows. If TPPs use this environment configuration against the production xs2a.tech26.de endpoint (instead of /sandbox), OAuth2 authorization codes and tokens could be redirected through the live N26 login page, potentially leaking tokens in browser history or enabling redirect-based token interception.
impact: Low — redirect_uri is likely restricted per-TPP in production; sandbox env is explicitly for sandbox use only; but it reveals the production redirect surface
verify_steps: (1) Confirm xs2a.tech26.de enforces registered redirect_uri per TPP (passive: check API error responses). (2) Verify app.n26.com/login handles OAuth2 callbacks safely (no token in URL fragment).
## REPOSCAN 2026-09-19 00:31:11 UTC
[HYP] Hardcoded PKCE code_verifier defeats OAuth2 CSRF protection
class: SECRET
asset: n26/psd2-tpp-docs/doc/assets/postman/XS2A_N26_Sandbox.postman_collection.json:250-252
confidence: 80
reasoning: The Postman sandbox collection hardcodes code_verifier="foobar" (lines 250-252) paired with code_challenge=w6uP8Tcg6K2QR905Rms8iXTlksL6OD1KOWBxTK7wxPI (the same value as dedicated_aisp_client_id, lines 99-100). PKCE relies on a random, unguessable code_verifier per authorization request; a static value eliminates the CSRF interception protection PKCE was designed to provide. Although labeled "sandbox," this collection is the official TPP onboarding reference — TPPs copying this pattern into production integrations will ship a broken PKCE flow. If N26's production xs2a.tech26.de accepts code_verifier="foobar" for any registered TPP, the authorization code interception protection is void.
impact: Medium — breaks PKCE security guarantee for any TPP following this sample; could enable authorization code interception if combined with other flaws
verify_steps: (1) Passively verify whether xs2a.tech26.de/sandbox/oauth2/token rejects requests with code_verifier="foobar" when code_challenge doesn't match. (2) Check if production xs2a endpoint enforces strict PKCE code_challenge verification. (3) Confirm this code_challenge is indeed the SHA-256 hash of "foobar" or a separate static value.
[HYP] Production N26 login URL in sandbox Postman environment
class: MISCONFIG
asset: n26/psd2-tpp-docs/doc/assets/postman/XS2A_N26_Sandbox.postman_environment.json:81-82
confidence: 55
reasoning: The sandbox Postman environment contains redirect_uri=https://app.n26.com/login — a production N26 web app URL — as the default OAuth2 redirect target for all sandbox authorization flows. If TPPs use this environment configuration against the production xs2a.tech26.de endpoint (instead of /sandbox), OAuth2 authorization codes and tokens could be redirected through the live N26 login page, potentially leaking tokens in browser history or enabling redirect-based token interception.
impact: Low — redirect_uri is likely restricted per-TPP in production; sandbox env is explicitly for sandbox use only; but it reveals the production redirect surface
verify_steps: (1) Confirm xs2a.tech26.de enforces registered redirect_uri per TPP (passive: check API error responses). (2) Verify app.n26.com/login handles OAuth2 callbacks safely (no token in URL fragment).
[HYP] Hardcoded Basic Auth credential android:secret in PSD2 MFA challenge script
class: SECRET
asset: n26/psd2-tpp-docs/doc/assets/bash/pin_encryption_and_initiating_transaction.sh:26
confidence: 75
reasoning: Line 26 contains hardcoded Base64-encoded header Authorization:Basic YW5kcm9pZDpzZWNyZXQ= (decodes to android:secret) in a curl POST to https://$PISP_HOST/api/mfa/challenge (default host pisp.tech26.de, confirmed live via HTTP 404). Static app-level OAuth credential, not per-user token. If accepted by production PISP, enables unauthorized MFA challenge initiation.
impact: Medium — potential unauthorized MFA challenge trigger if credential is live on production PISP
verify_steps: (1) Passively confirm whether pisp.tech26.de/api/mfa/challenge accepts Basic YW5kcm9pZDpzZWNyZXQ= by observing error response differences with/without header. (2) Verify android:secret is documented as sandbox-only in N26 PSD2 developer docs.
[HYP] Internal S3 bucket name consumercredit-staging disclosed in Android sample data
class: MISCONFIG
asset: n26/N26AndroidSamples/credit/src/main/res/raw/credit_drafts.json:8,30
confidence: 60
reasoning: Two mock credit entries reference https://s3.eu-central-1.amazonaws.com/consumercredit-staging/ for image assets. Exposes internal AWS S3 bucket naming convention for N26 consumer credit staging in eu-central-1. Bucket returns 403/NoSuchBucket (passive recon confirmed), but naming pattern aids enumeration.
impact: Low — bucket non-public/deleted (404/403 confirmed); informational naming convention leak only
verify_steps: (1) Already confirmed bucket non-public via HEAD request. (2) No further action needed unless staging bucket is recreated.
[HYP] Slack bot token transmitted via URL query parameter
class: MISCONFIG
asset: n26/bob/Sources/Bob/Core/Slack/SlackClient.swift:31
confidence: 65
reasoning: The bob Slack bot passes API token as URL query parameter (token=...) to deprecated Slack RTM endpoint https://slack.com/api/rtm.start. Tokens in query strings appear in server logs, proxy logs, browser history, and HTTP Referer headers. Token is loaded from config at runtime (not hardcoded), but transmission pattern is insecure per OWASP. Bob is archived since 2020.
impact: Low — token not hardcoded (loaded from config); insecure transmission pattern only; bot archived
verify_steps: (1) Verify no token values committed in repo config/.env. (2) Confirm bot no longer actively deployed. (3) If still deployed, rotate token and migrate to Socket Mode.
[HYP] Internal Artifactory registry URL disclosed in public npm lockfile
class: MISCONFIG
asset: n26/express-simple-locale/package-lock.json:1518,2429,3391,3571,7527
confidence: 45
reasoning: package-lock.json contains resolved URLs pointing to internal N26 Artifactory at artifactory.cd-tech26.de:443 (e.g., https://artifactory.cd-tech26.de:443/artifactory/api/npm/npm/lodash/-/lodash-4.17.21.tgz). Reveals internal artifact registry hostname. Not publicly reachable (connection timeout confirmed).
impact: Informational — internal hostname disclosure; Artifactory not publicly accessible
verify_steps: (1) Already confirmed Artifactory not publicly reachable (connection timeout). (2) No further action needed.
[HYP] Internal PSD2 API hostnames exposed in public documentation
class: MISCONFIG
asset: n26/psd2-tpp-docs/doc/sandbox.md, n26/psd2-tpp-docs/doc/assets/bash/pin_encryption_and_initiating_transaction.sh
confidence: 70
reasoning: Multiple files expose internal PSD2 API hostnames: xs2a.tech26.de, pisp.tech26.de, aisp.tech26.de. All confirmed reachable (HTTP 404/401). Hostnames reveal internal PSD2 API naming convention and infrastructure. Combined with android:secret credential, provides complete attack surface for MFA challenge flow.
impact: Low — hostnames alone not directly exploitable; combined with credential findings increases attack surface
verify_steps: (1) Already confirmed hostnames resolve and respond. (2) Check if hosts serve other endpoints beyond documented paths.
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-19 05:00:18 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-19 09:23:46 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-19 13:22:58 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-19 16:47:15 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-19 19:09:46 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-19 21:29:12 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-19 23:30:16 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-20 01:36:37 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-20 06:58:02 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-20 12:08:22 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-20 16:13:49 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-20 18:51:53 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-20 21:10:12 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-20 23:17:59 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-21 01:13:13 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-21 06:28:24 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-21 13:19:15 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-21 18:48:40 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-21 22:26:40 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-22 01:07:30 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-22 06:19:34 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-22 11:51:50 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-22 16:16:35 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-22 19:46:42 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-22 22:43:28 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-23 01:14:40 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-23 06:03:28 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-23 11:46:51 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-23 16:06:01 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-23 19:42:29 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-23 22:40:30 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-24 01:03:43 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-24 06:13:27 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-24 11:55:13 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-24 16:29:59 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-24 20:06:13 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-24 23:14:52 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-25 01:35:41 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-25 06:34:11 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-25 12:10:40 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-25 17:17:33 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
## REPOSCAN 2026-09-25 20:32:52 UTC
TARGET_ORG not configured for n26; skipping public-org deep scan.
