# Rollback Traceability

## Identify the current release and previous known-good version

```bash
# Sort semantic version tags newest first
git tag --sort=-v:refname

# Inspect candidate releases
git show --no-patch --format=fuller v1.1.1
git show --no-patch --format=fuller v1.1.0

# Resolve exact commit SHAs for an incident record
git rev-parse v1.1.1^{commit}
git rev-parse v1.1.0^{commit}
```

Do not assume the highest version tag is currently deployed. Confirm the production version against the deployment platform's record, including environment, artifact ID, timestamp, and full commit SHA.

## Roll back to the last known-good release

```bash
# Confirm v1.1.0 is an approved, known-good release first
git checkout --detach v1.1.0

# Build/redeploy the artifact from this exact commit using the team's
# approved deployment process, then run health checks and record the result.
```

A detached checkout selects the source revision; it does not by itself alter production. Deployment must be performed through the approved release procedure and verified with health checks.

## Why this is reproducible

Consistent annotated semantic tags identify precise Git commits, and each annotation records why the tag exists. The release map and notes add the human context needed to distinguish a source commit from an actual deployment and to select a validated rollback target. This is materially safer than the legacy labels `release_2`, `latest-good`, or `stable-build`, which do not by themselves specify a version contract or prove what was deployed.
