# Tag History Audit

## Scope and evidence

This audit is based on the repository's checked-in `docs/deployment-history.md`, `docs/incident-log.md`, `docs/release-notes-old.md`, and `CHANGELOG.md`. Those files establish the operational failure modes below. The legacy tag names are retained as historical evidence; they must not be treated as verified release identifiers until the Git refs and deployment records are reconciled.

## Findings

### 1. `release_2` is not a semantic version and was an unsafe rollback target

**Evidence:** `docs/incident-log.md` says the 2026-04-22 production `502` incident was rolled back to `release_2`, which pointed to an older unsupported commit. It also records that `release_2` is lightweight and has no annotation.

**Team impact:** The name does not say whether this is version 2, a second release, or a branch-specific build. Without an annotated message and a corresponding deployment record, operators cannot infer what the commit contains or why it was considered safe.

**Risk:** A failed release can be rolled back to an unsupported build, prolonging an outage and making incident review difficult.

### 2. `latest-good` is a mutable-sounding label with no version identity

**Evidence:** `docs/deployment-history.md` says production was deployed from `latest-good` on 2026-05-04. The incident log says the hotfix had no matching semantic version or release notes; `docs/release-notes-old.md` only says “good for rollback.”

**Team impact:** “Latest” and “good” express an opinion, not a release identity or the change set. A team cannot tell whether two deployment records using the same name refer to the same known-good artifact.

**Risk:** A rollback target can be misidentified during an incident, and the deployed source cannot be reliably audited from the label alone.

### 3. `stable-build` has no linked deployment record

**Evidence:** The deployment history records a missing deployment record for the release tagged `stable-build` on 2026-03-15. The old notes only say “stable build from branch.”

**Team impact:** The tag does not identify a numeric release, and the notes do not identify the exact commit's changes or target environment.

**Risk:** Auditors cannot connect a purported release to a production deployment, and responders cannot determine whether it is an approved rollback candidate.

### 4. `v1.4.2` is referenced by assumption rather than confirmed deployment evidence

**Evidence:** On 2026-04-10, `docs/deployment-history.md` says a staging promotion was assumed to match `v1.4.2`, while the production tag was undocumented. The changelog repeats this as an assumption.

**Team impact:** A staging version and the production version are not interchangeable evidence. The record does not prove which commit was deployed to production.

**Risk:** An incident responder may compare or roll back against the wrong commit, and the deployment audit trail contains an unverified assertion.

### 5. `v2-final-FINAL` conveys informal status, not an immutable release contract

**Evidence:** `docs/release-notes-old.md` lists `v2-final-FINAL` with “production-ready? maybe”.

**Team impact:** The repeated “final” marker is ambiguous and the question mark explicitly signals uncertainty. It also does not communicate whether this is a candidate from a production perspective or which supported release preceded it.

**Risk:** Humans and automation cannot consistently distinguish a candidate from a production release; deployment gates and rollback instructions become guesswork.

### 6. `version-1.0` and bare `1.5.0` break the proposed naming convention

**Evidence:** Both names appear in `docs/release-notes-old.md`; one uses a textual prefix and the other omits the `v` prefix.

**Team impact:** Mixed formats make tag filtering and release instructions inconsistent. Version sorting and scripts that expect `vMAJOR.MINOR.PATCH` can omit or mishandle these labels.

**Risk:** A release can be missed during inventory, ordering, automation, or recovery. Their actual compatibility meaning also cannot be inferred without mapping each tag to its commit.

### 7. `patch-new` does not say which release it patches

**Evidence:** `docs/release-notes-old.md` describes `patch-new` only as “new patch release.”

**Team impact:** There is no numeric patch sequence or stated base release, so the record cannot establish whether this fixes `v1.4.2`, `1.5.0`, or another line.

**Risk:** A team may select a fix from the wrong release line or be unable to prove which known-good version should precede the patch.

### 8. Production state is not consistently recorded

**Evidence:** The deployment history and changelog both say the version running on 2026-03-31 was unknown. The 2026-05-04 record names `latest-good`, but does not provide a commit hash or artifact identifier.

**Team impact:** The tag name alone cannot answer “what commit and artifact are in production?”

**Risk:** Incident response, compliance review, and repeatable recovery are compromised. Each production deployment must record the immutable commit SHA, version tag, environment, and deployment timestamp.

## Remediation

Adopt the policy in [VERSIONING.md](VERSIONING.md), use annotated `vMAJOR.MINOR.PATCH` release tags, map each tag to an immutable commit SHA, and maintain release notes and deployment evidence together. Do not silently relabel historical deployments as verified: reconcile old references against actual tag refs and CI/CD/deployment records first.
