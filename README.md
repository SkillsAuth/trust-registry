# SkillsAuth Trust Registry (Template)

This folder is a production-ready template for the public trust registry repository at:

- `https://github.com/SkillsAuth/trust-registry`

It is intentionally minimal and contains only verification policy artifacts.

## Purpose

The trust registry is a public, read-only policy source for consumers verifying SkillsAuth Sigstore bundles.

- Private signing remains in `SkillsAuth/skillsai` (private repo).
- Public policy and revocation metadata live in `SkillsAuth/trust-registry` (public repo).
- Verifiers should pin policy by immutable release tag or commit SHA, never mutable `main`.

## Files

- `trusted-identities.json` - Allowed OIDC signer identities and constraints.
- `rekor-policy.json` - Verification requirements and failure behavior.
- `revocations.json` - Revoked manifest digest denylist with audit metadata.
- `schemas/*.schema.json` - JSON schemas for strict validation.
- `CHANGELOG.md` - Human-readable history for policy/revocation updates.
- `OPERATIONS.md` - Update, release, emergency rollback, and governance runbook.
- `CODEOWNERS.example` - Suggested CODEOWNERS policy for hardening.

## Security Non-Goals

Do not store in trust-registry:

- Private keys, CI secrets, OIDC tokens.
- Internal infrastructure details.
- Mutable "latest trusted signer" claims without immutable release references.

## Minimal Publisher Model (Current)

Current trusted signer (Tier 0) should be constrained to this workflow identity:

- `https://github.com/SkillsAuth/skillsai/.github/workflows/sign-skill-manifests.yml@refs/heads/main`

OIDC issuer:

- `https://token.actions.githubusercontent.com`

## Consumer Guidance

1. Fetch pinned trust policy (release tag or commit SHA).
2. Validate with included JSON schemas.
3. Verify Sigstore bundle and cert chain.
4. Enforce identity constraints from `trusted-identities.json`.
5. Enforce revocation denylist from `revocations.json`.
6. Apply failure behavior from `rekor-policy.json`.

