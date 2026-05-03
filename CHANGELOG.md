# Trust Registry Changelog

## 2026-05-03 (Fulcio OIDC — reusable workflow SAN)

- Corrected Tier 0 **`subjectAlternativeName`** and **`workflow`** for
  `skillsauth-trust-svc-tier0-main` to
  **`SkillsAuth/trust-svc`** **`.github/workflows/sign-execute.yml`** (not
  `sign.yml`). Public Fulcio issues certificates for the **called** reusable
  workflow job; the thin `sign.yml` dispatch file is not the URI SAN.
- Bumped policy **`version`** to **`1.2`**.

## 2026-05-02 (Trust Platform v2 - Phase 1 cutover, ADR-007 + ADR-008)

- Added active Tier 0 signer `skillsauth-trust-svc-tier0-main` for
  `SkillsAuth/trust-svc` (canonical **`sign-execute.yml`** URI SAN recorded under
  **2026-05-03**).
  This is the new SkillsAuth org-owned production signing identity.
- Marked legacy `beinghimansh-tier0-main` as `status: revoked`. New
  signatures from the personal-account workflow are rejected by
  trust-svc. Existing pre-revokedAt bundles remain verifiable for audit.
- Wiped + re-signed the 8 production v1 `SkillManifest` bundles under
  ArtifactManifest v2 (per ADR-008). The pre-cutover bundles plus their
  original Rekor log indexes are archived in
  `SkillsAuth/trust-svc/migrations/v1-archive/2026-05-02.json`.
- Bumped `version` to `1.1`. Consumers should pin a release tag, not
  `main`.

## 2026-04-28

- Initial trust-registry template created in the private implementation repo.
- Added strict schemas:
  - `schemas/trusted-identities.schema.json`
  - `schemas/rekor-policy.schema.json`
  - `schemas/revocations.schema.json`
- Added initial policy artifacts:
  - `trusted-identities.json` (SkillsAuth Tier 0 signer identity)
  - `rekor-policy.json` (fail-closed defaults for identity and issuer mismatch)
  - `revocations.json` (empty denylist scaffold)
- Added operational and governance docs:
  - `OPERATIONS.md`
  - `CODEOWNERS.example`

