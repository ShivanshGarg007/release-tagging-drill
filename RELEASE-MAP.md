# Release Map

## Verified release references

The following three semantic release tags form the proposed clean sequence. Because this exercise repository has no reliable verified production mapping in the legacy records, these entries identify source snapshots and example deployment intent, not proof of historical production deployment. Dates below are Git commit dates in UTC. Confirm the tag objects and exact target commits after creating them locally.

| Version (tag) | Commit (short hash) | Date (UTC) | What Shipped / Deployment Record |
|---|---|---|---|
| `v1.0.0` | `4febcc6` | 2026-06-06 | Baseline snapshot: initial repository commit. Deployment status: not verified; no production deployment evidence supplied. |
| `v1.1.0` | `82535d9` | 2026-06-06 | Checkout service code snapshot. Deployment status: not verified; use as a release-sequence example, not a claim of production deployment. |
| `v1.1.1` | `86989b6` | 2026-06-06 | Historical documentation/changelog snapshot. Deployment status: not verified; use as a release-sequence example, not a claim of production deployment. |

> **Evidence caveat:** These commit messages and dates come from the repository history. This table is intentionally explicit that deployments are unverified. Before presenting these as actual production releases, inspect the complete commits and validate each version's shipped application changes; a release tag must not be used to retroactively invent a deployment record.

## Useful commands

```bash
# See semantic versions newest first and their target commits
git tag --sort=-v:refname
git show --no-patch --format=fuller v1.1.1

# Print an exact version-to-commit mapping
git for-each-ref refs/tags --sort=-v:refname \
  --format='%(refname:short) %(objectname:short) %(creatordate:iso8601) %(subject)'

# Record a real deployment separately: environment, UTC timestamp,
# full commit SHA, artifact/build ID, deployment result and operator.
```

## Rollback traceability demonstration

For a deliberate rollback from the newer release `v1.1.1` to the previous known-good release `v1.1.0`, first verify that `v1.1.0` passed checks and is confirmed by the deployment record:

```bash
git tag --sort=-v:refname
git show --no-patch --format=fuller v1.1.0
git checkout --detach v1.1.0
# Redeploy the artifact built from the resolved v1.1.0 commit,
# then verify health checks and record the incident/deployment.
```

A version tag resolves to a specific Git object and the annotation explains the intended release, while the map supplies a human-readable change/deployment record. In contrast to labels like `latest-good` or `release_2`, this gives responders a deterministic source revision to inspect and rebuild. A Git checkout alone does not redeploy production; the team must run its approved deployment procedure and verify the result.
