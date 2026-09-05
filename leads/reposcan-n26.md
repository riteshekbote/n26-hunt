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
