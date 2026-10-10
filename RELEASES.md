# Release Notes

> **Important evidence note:** The legacy repository records do not prove that the example semantic versions below were historically deployed to production. These are structured source-release notes for the proposed tag sequence. Do not describe them as production releases until each tag's target commit is checked and deployment evidence is available.

## v1.0.0

- **Date:** 2026-06-06 (Git commit date, UTC)
- **Summary:** Establishes the baseline repository snapshot.
- **Added:** Initial repository scaffold and assignment structure, according to the commit message.
- **Changed:** No changes claimed beyond the baseline commit.
- **Fixed:** None claimed.
- **Rollback target:** None in this proposed release sequence. If this version fails, there is no earlier tagged known-good version in the sequence. Select a separately validated deployment revision if one exists.
- **Deployment status:** Not verified in the supplied records.

## v1.1.0

- **Date:** 2026-06-06 (Git commit date, UTC)
- **Summary:** Source snapshot containing the dummy checkout service, according to the commit message. Validate the diff and tests before calling it releasable.
- **Added:** Checkout service source snapshot.
- **Changed:** No specific behavior change is claimed without confirming the full commit diff.
- **Fixed:** None claimed.
- **Rollback target:** `v1.0.0`, only if it has been validated as a deployable, known-good revision. Otherwise use the last independently verified production deployment.
- **Deployment status:** Not verified in the supplied records.

## v1.1.1

- **Date:** 2026-06-06 (Git commit date, UTC)
- **Summary:** Source snapshot associated with the historical release-notes/changelog addition. This documentation change alone should not be treated as a shipped application patch.
- **Added:** Legacy release-note and changelog records.
- **Changed:** Release-history documentation.
- **Fixed:** No application fix is evidenced by the commit message.
- **Rollback target:** `v1.1.0`, only if it is confirmed as deployable and known-good; otherwise use the last independently verified production deployment.
- **Deployment status:** Not verified in the supplied records.

## Legacy records requiring reconciliation

The old note `v1.4.2` says payment token caching was added and checkout timeouts were fixed, but the deployment history explicitly says the production tag for 2026-04-10 is undocumented. The old entry `1.5.0` mentions a checkout button fix and audit logging, but the missing `v` prefix and absent verified commit mapping prevent treating it as a reliable release record. Entries for `version-1.0`, `release_2`, `v2-final-FINAL`, `stable-build`, `latest-good`, and `patch-new` also lack the information required to build a trustworthy release sequence. Preserve these as historical notes until commit and deployment evidence has been reconciled.
