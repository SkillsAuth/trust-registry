# Trust Registry Changelog

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

