# N26 Public Repository Source Code Audit Findings
## Date: 2026-09-04
## Auditor: opencode/mimo-v2.5-free

## Scope
- **Organization**: N26 (github.com/n26)
- **Repos Audited**: n26/bob, n26/express-simple-locale, n26/N26AndroidSamples, n26/bob-vapor-template, n26/psd2-tpp-docs, n26/flowkit-ios

---

## Findings

### [HYP] Hardcoded Basic Auth Credential in PSD2 MFA Challenge Script
- **Class**: SECRET
- **Asset**: n26/psd2-tpp-docs/doc/assets/bash/pin_encryption_and_initiating_transaction.sh:26
- **Confidence**: 65
- **Reasoning**: The script contains a hardcoded Base64-encoded Basic Auth header `YW5kcm9pZDpzZWNyZXQ=` which decodes to `android:secret`. This credential is used in a `curl` call to `https://$PISP_HOST/api/mfa/challenge` (default host `pisp.tech26.de`). If this is a real static credential accepted by the MFA challenge endpoint (rather than a per-session token), it could allow unauthorized triggering of out-of-band MFA challenges for any user. The script is published as a TPP example in the open-banking documentation repo and hits what appears to be the real PISP endpoint. The credential may be a well-known sandbox/example value, but if it is accepted by the production endpoint it constitutes a leaked platform credential.
- **Impact**: Medium — potential for unauthorized MFA challenge initiation against N26 users if the credential is live
- **Verify Steps**: (1) Passively check if `pisp.tech26.de/api/mfa/challenge` accepts `Authorization: Basic YW5kcm9pZDpzZWNyZXQ=` by observing public API docs or error responses. (2) Confirm whether the endpoint uses a static app-level credential or per-user tokens.

---

### [HYP] Internal S3 Staging Bucket Name Disclosure
- **Class**: MISCONFIG
- **Asset**: n26/N26AndroidSamples/credit/src/main/res/raw/credit_drafts.json:8,30
- **Confidence**: 50
- **Reasoning**: Mock credit data in the Android sample app references `https://s3.eu-central-1.amazonaws.com/consumercredit-staging/` for image assets. This reveals the internal AWS S3 bucket naming convention (`consumercredit-staging`) for N26's consumer credit staging environment. If the bucket lacks proper access controls, the naming pattern could aid enumeration or direct access. This is mock data in a sample app, so the actual bucket may or may not still exist.
- **Impact**: Low — informational disclosure of internal bucket naming; actual exploitation depends on bucket ACL configuration
- **Verify Steps**: (1) Passively verify if `s3://consumercredit-staging` is publicly listable or accessible via `aws s3 ls s3://consumercredit-staging/ --no-sign-request`. (2) No further active testing required.

---

### [HYP] Postman Sandbox Environment with Client IDs and Auth Codes
- **Class**: SECRET
- **Asset**: n26/psd2-tpp-docs/doc/assets/postman/XS2A_N26_Sandbox.postman_environment.json
- **Confidence**: 40
- **Reasoning**: The Postman environment file contains sandbox credentials including `auth_code` (`w6uP8Tcg6K2QR905Rms8iXTlksL6OD1KOWBxTK`), `dedicated_aisp_client_id` (`w6uP8Tcg6K2QR905Rms8iXTlksL6OD1KOWBxTK7wxPI`), and `user_id` (`f3f51978-20fb-4bd9-91d9-7cc2a1fd9618`). While these are explicitly labeled as sandbox values and the environment is named `XS2A_N26_Sandbox`, they could potentially be reused or similar patterns used in production. The `redirect_uri` points to `https://app.n26.com/login` which is the production login page.
- **Impact**: Low — sandbox credentials only; however, if the same client_id pattern is used in production or if sandbox/production share infrastructure, this could be leveraged
- **Verify Steps**: (1) Confirm these are sandbox-only values by checking if they work against production endpoints (do NOT actively test). (2) Check if the client_id pattern follows a predictable sequence.

---

## Summary

| Finding | Class | Confidence | Impact |
|---------|-------|------------|--------|
| Hardcoded Basic Auth in PSD2 script | SECRET | 65% | Medium |
| S3 staging bucket name disclosure | MISCONFIG | 50% | Low |
| Postman sandbox credentials | SECRET | 40% | Low |

## Notes

- The N26 GitHub organization has only 6 public repositories, all of which are documentation, sample code, or open-source tools.
- No production secrets (AWS keys, API keys, private keys, etc.) were found in any repository.
- The `bob` and `bob-vapor-template` repos contain template placeholders (`<slack-token>`, `<token>`) rather than actual secrets.
- The `flowkit-ios` repo contains only iOS library code with no embedded credentials.
- The `express-simple-locale` repo is a simple npm package with no sensitive data.
