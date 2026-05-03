# Trust Registry Operations

This runbook defines how to safely operate the public `SkillsAuth/trust-registry`.

## Branch and Repo Hardening

Apply these controls in GitHub settings:

1. Protect `main`
   - Require pull requests before merge.
   - Require at least 2 approvals.
   - Require status checks to pass (must include `Policy-only guard / Reject non-policy files`).
   - Require linear history.
   - Restrict who can push.
2. Require signed commits (if available in your org policy).
3. Enforce org-wide 2FA and hardware keys for admins.
4. Enable Dependabot and security alerts.
5. Configure CODEOWNERS for all trust files.

## Public/private split CI guard (ADR-007)

The workflow `.github/workflows/policy-only-guard.yml` enforces ADR-007
mechanically: it fails any pull request that adds files outside the
allow-list (policy artifacts, schemas, documentation, license,
codeowners, .gitignore, and the guard itself). Branch protection must
mark the `Policy-only guard` check as a required status check so the
guard cannot be bypassed by merging without it running.

If a legitimate new policy file class needs to be added, update the
allow-list inside the workflow in the same PR, and document the change
in `CHANGELOG.md`.

## Update Workflow

1. Create branch from latest `main`.
2. Update policy files (`trusted-identities.json`, `rekor-policy.json`, `revocations.json`).
3. Validate JSON against schemas.
4. Update `CHANGELOG.md`.
5. Open PR and require CODEOWNERS approvals.
6. Merge only after checks pass.
7. Create signed release tag (`trust-vN`) and publish release notes.

## Emergency Revocation Procedure

Use this when a signer or digest must be blocked immediately.

1. Add digest entry to `revocations.json`:
   - `manifestDigest` (sha256 hex)
   - `reason`
   - `revokedAt`
   - `revokedBy`
   - optional `notes`
2. Update `updatedAt` timestamp.
3. Update `CHANGELOG.md`.
4. Open emergency PR and request expedited review.
5. Tag and release (for pinned consumer updates).
6. Notify downstream verifier owners to pull the new pinned tag/commit.

## Rollback Procedure

1. Identify last known-good trust release tag.
2. Re-pin consumers to that tag/commit.
3. Revert problematic trust-registry commit via PR.
4. Publish follow-up release tag and incident note.

## Client Pinning Rule (Mandatory)

Never consume mutable `main` in strict mode.

Use one of:

- Pinned git tag (recommended operationally).
- Exact commit SHA (strongest immutability).
- Pinned file SHA-256 allowlist in verifier release.

