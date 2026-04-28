# Pinned Consumption Guidance

Use trust-registry files by immutable reference only.

## Recommended

- Pin by release tag: `trust-v1`
- Or pin by commit SHA (strongest)

## Avoid

- Do not consume `https://raw.githubusercontent.com/SkillsAuth/trust-registry/main/...` in strict verification paths.

## Example URLs (tag pin)

- `https://raw.githubusercontent.com/SkillsAuth/trust-registry/trust-v1/trusted-identities.json`
- `https://raw.githubusercontent.com/SkillsAuth/trust-registry/trust-v1/rekor-policy.json`
- `https://raw.githubusercontent.com/SkillsAuth/trust-registry/trust-v1/revocations.json`

## Example URLs (commit pin)

- `https://raw.githubusercontent.com/SkillsAuth/trust-registry/<commit-sha>/trusted-identities.json`
- `https://raw.githubusercontent.com/SkillsAuth/trust-registry/<commit-sha>/rekor-policy.json`
- `https://raw.githubusercontent.com/SkillsAuth/trust-registry/<commit-sha>/revocations.json`

## Consumer Verification Steps

1. Fetch pinned files.
2. Validate against `schemas/*.schema.json`.
3. Optionally compare fetched file SHA-256 to an allowlist in your verifier build.
4. Apply policy to Sigstore verification result before trust decision.

