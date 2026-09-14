## REPOSCAN MANUAL AUDIT 2026-09-14 01:12:00 UTC

[HYP] Hardcoded Basic Auth Credential `android:secret` in PSD2 MFA Challenge Script
class: SECRET
asset: n26/psd2-tpp-docs/doc/assets/bash/pin_encryption_and_initiating_transaction.sh:26
confidence: 75
reasoning: Line 26 contains a hardcoded Base64-encoded Basic Auth header `YW5kcm9pZDpzZWNyZXQ=` which decodes to `android:secret`. This credential is used in a curl POST to `https://$PISP_HOST/api/mfa/challenge` (defaults to `pisp.tech26.de`). The credential is a static app-level OAuth client ID/secret pair, not a per-user token. If accepted by the live MFA challenge endpoint, it could allow unauthorized out-of-band MFA challenge initiation for any user. The credential may be a well-known sandbox/demo value per PSD2 documentation conventions, but it targets a real PISP endpoint that is confirmed reachable (returns 404 rather than DNS failure).
impact: Medium — potential unauthorized MFA challenge trigger if credential is live on production PISP
verify_steps: (1) Confirm whether `pisp.tech26.de` is a sandbox-only or production host by checking HTTP response headers/body on error paths. (2) Check N26 PSD2 developer docs to verify if `android:secret` is documented as a sandbox-only credential. (3) Test if the endpoint accepts the credential by observing error response differences with/without the header.

[HYP] Internal S3 Bucket Name `consumercredit-staging` Disclosed in Android Sample Data
class: MISCONFIG
asset: n26/N26AndroidSamples/credit/src/main/res/raw/credit_drafts.json:8,30
confidence: 65
reasoning: Two mock credit entries reference `https://s3.eu-central-1.amazonaws.com/consumercredit-staging/` for image assets. This discloses the internal AWS S3 bucket naming convention (`consumercredit-staging`) for N26's consumer credit staging environment in eu-central-1. The bucket naming pattern reveals internal infrastructure naming conventions that could aid enumeration if staging buckets are recreated or if similar patterns apply to other services.
impact: Low — bucket appears non-public/deleted; informational naming convention leak only
verify_steps: (1) Verify if `s3://consumercredit-staging` is publicly listable or accessible via `aws s3 ls s3://consumercredit-staging/ --no-sign-request`. (2) No further action needed unless staging bucket is recreated.

[HYP] Internal PSD2 API Hostnames Exposed in Public Documentation
class: MISCONFIG
asset: n26/psd2-tpp-docs/doc/sandbox.md, n26/psd2-tpp-docs/doc/assets/bash/pin_encryption_and_initiating_transaction.sh
confidence: 70
reasoning: Multiple files expose internal PSD2 API hostnames: `xs2a.tech26.de`, `pisp.tech26.de`, `aisp.tech26.de`. These are real endpoints (confirmed reachable via HTTP 404/401 responses). The hostnames reveal N26's internal PSD2 API naming convention and infrastructure. Combined with the hardcoded `android:secret` credential, this provides a complete attack surface for the MFA challenge flow.
impact: Low — hostnames alone are not directly exploitable; combined with credential findings increases attack surface
verify_steps: (1) Already confirmed hostnames resolve and respond (HTTP 404/401). (2) Check if hosts serve other endpoints beyond documented paths.

[HYP] Sandbox OAuth Client IDs and Internal Hostname in Postman Environment
class: MISCONFIG
asset: n26/psd2-tpp-docs/doc/assets/postman/XS2A_N26_Sandbox.postman_environment.json
confidence: 55
reasoning: The Postman environment file contains sandbox credentials: `dedicated_aisp_client_id=w6uP8Tcg6K2QR905Rms8iXTlksL6OD1KOWBxTK7wxPI`, `auth_code=w6uP8Tcg6K2QR905Rms8iXTlksL6OD1KOWBxTK`, sandbox IBAN `DE15100110012627633320`, and internal hostname `xs2a.tech26.de` (confirmed live — returns HTTP 401). The collection also references OAuth client IDs `PSDDE-BAFIN-000001` and `PSDES-BDE-3DFD12` as code_challenge values. While explicitly labeled "sandbox," these values reveal the internal PSD2 API hostname and OAuth registration format.
impact: Informational — sandbox-only documentation values; `xs2a.tech26.de` is a real endpoint (returns 401)
verify_steps: (1) Already confirmed `xs2a.tech26.de` is live (returns 401 for unauthenticated requests). (2) Verify the client_id values are sandbox-scoped only.

[HYP] Slack Bot Token Transmitted via URL Query Parameter
class: MISCONFIG
asset: n26/bob/Sources/Bob/Core/Slack/SlackClient.swift:31
confidence: 60
reasoning: The bob Slack bot passes the API token as a URL query parameter (`token=...`) to the deprecated Slack RTM API endpoint `https://slack.com/api/rtm.start`. Tokens in URL query strings appear in server logs, proxy logs, browser history, and HTTP Referer headers. The token is loaded from config at runtime (not hardcoded in source), but the transmission pattern is an insecure practice per OWASP. The Slack RTM API is deprecated (superseded by Socket Mode). Bob is an archived internal N26 tool used for TravisCI/GitHub communication.
impact: Low — token is not hardcoded (loaded from config at runtime); insecure transmission pattern only; bot is archived
verify_steps: (1) Verify no token values are committed in the repo's config files or .env. (2) Confirm the bot is no longer actively deployed (repo is archived since 2020). (3) If still deployed, rotate the Slack bot token and migrate to Socket Mode.

[HYP] Internal Artifactory Registry URL Disclosed in Public npm Lockfile
class: MISCONFIG
asset: n26/express-simple-locale/package-lock.json:1518,2429,3391,3571,7527
confidence: 45
reasoning: The package-lock.json contains resolved URLs pointing to an internal N26 Artifactory instance at `artifactory.cd-tech26.de:443` (e.g., `https://artifactory.cd-tech26.de:443/artifactory/api/npm/npm/lodash/-/lodash-4.17.21.tgz`). This reveals the internal artifact registry hostname and that it hosts N26's npm packages. The endpoint is not publicly reachable (connection refused / timeout), suggesting it is VPN-internal only. This is a standard npm lockfile entry, not a credential.
impact: Informational — internal hostname disclosure; Artifactory is not publicly accessible
verify_steps: (1) Already confirmed Artifactory is not publicly reachable (connection timeout). (2) No further action needed.

[HYP] Hardcoded Staging URL in Android Sample Configuration
class: MISCONFIG
asset: n26/N26AndroidSamples/buildsystem/configurations.gradle
confidence: 40
reasoning: The Android sample app contains a hardcoded staging endpoint URL: `https://www.definitelynotn26-staging.de/api/`. While clearly a placeholder/example URL, it reveals the naming convention for N26 staging environments (staging subdomain pattern). The URL is used as `N26_FAKE_API_URL` in build configurations.
impact: Informational — staging URL naming convention disclosure; URL is clearly a placeholder
verify_steps: (1) Confirm the URL is not a real endpoint (appears to be a joke/placeholder). (2) No further action needed.
