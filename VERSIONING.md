# Release Versioning Policy

## Semantic Versioning

Production releases use `MAJOR.MINOR.PATCH` as described by Semantic Versioning, with a lowercase `v` prefix in the Git tag.

- **MAJOR**: increment when a change is incompatible with the supported public API or behavior. Example: `v1.4.2 -> v2.0.0` for a breaking checkout API change.
- **MINOR**: increment when backward-compatible functionality is added. Example: `v1.4.2 -> v1.5.0` for a new optional checkout capability.
- **PATCH**: increment for backward-compatible bug fixes. Example: `v1.4.2 -> v1.4.3` for a checkout timeout fix that does not change the public contract.

When incrementing MAJOR or MINOR, reset all lower-order components to zero. Released version numbers are never reused for a different commit.

## Tag naming format

The exact stable-release pattern is:

```text
vMAJOR.MINOR.PATCH
```

Examples: `v1.0.0`, `v1.1.0`, `v1.1.1`. Use lowercase `v`, decimal non-negative integers, and no spaces, underscores, or informal suffixes. Do not use names such as `latest-good`, `stable-build`, `release_2`, or `v2-final-FINAL` for production releases.

## Annotated versus lightweight tags

All release tags **must be annotated**. An annotated tag records a message, tagger identity, timestamp, and the target object, allowing a reviewer to see the purpose and release record. A lightweight tag is only a pointer and is not sufficient for an official release.

Create a tag on the exact release commit with a descriptive message:

```bash
git tag -a v1.1.0 <commit-sha> -m "Release 1.1.0: add payment token caching"
git show --no-patch --format=fuller v1.1.0
git push origin v1.1.0
```

The commit SHA must be reviewed before tagging. Do not move a published release tag to a different commit; publish a new version instead. Push tags explicitly and verify they are present on the remote.

## Pre-release rule

Release candidates and betas use a SemVer pre-release suffix, for example `v1.5.0-rc.1`, `v1.5.0-rc.2`, or `v1.5.0-beta.1`. Increment the numeric identifier for each successive candidate. Pre-release versions have lower precedence than the corresponding final release: `v1.5.0-rc.1 < v1.5.0-rc.2 < v1.5.0`. A pre-release is not a production release unless explicitly approved and recorded as such.

## Release approval and traceability

Before a stable tag is created, reviewers must agree on the version bump, CI/test status, release notes, rollback target, and intended deployment. For every deployment, record the environment, UTC timestamp, version tag, full commit SHA, artifact/build identifier, and deployment result in the deployment system or release map. Keep Git commit identity and actual deployment evidence separate: a tag proves the source commit, not by itself that a commit reached production.
