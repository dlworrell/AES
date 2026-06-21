# AES-001 Repository Lifecycle Standard

Status: Draft
Owner: AES
Version: 0.1.0

## Purpose

Define the lifecycle states used by AES-governed repositories.

## Scope

This standard applies to repositories that participate in the Catalyst engineering ecosystem and claim AES governance.

The lifecycle applies to standards repositories, automation repositories, template repositories, product repositories, documentation repositories, and validation repositories.

## Definitions

### Lifecycle State

A named repository condition that describes the repository's current engineering maturity.

### Engineering Ready

A lifecycle state indicating that a repository has enough structure, evidence, metadata, and automation to support normal implementation work.

### Project Zero

The initialization and cleanup phase that prepares a repository for sustained engineering work.

### Accepted Deferral

A documented exception that permits later work to proceed while explicitly recording unfinished lifecycle requirements.

## Lifecycle States

1. Idea
2. Program Planning
3. Project Creation
4. Project Zero
5. Engineering Ready
6. Implementation
7. Verification
8. Release
9. Maintenance
10. Archive

## Normative Requirements

- A repository shall declare its lifecycle state.
- A repository shall preserve evidence supporting its lifecycle state.
- Project Zero shall precede Engineering Ready.
- Engineering Ready shall require documented evidence or documented accepted deferrals.
- Lifecycle transitions should be traceable through issues, commits, reviews, or reports.
- AEMS implementations should inspect lifecycle state from repository data.
- Accepted deferrals shall identify the deferred requirement and the reason for deferral.

## Lifecycle Transition Rules

- Idea may transition to Program Planning when program review is needed.
- Program Planning may transition to Project Creation when ownership and scope are identified.
- Project Creation may transition to Project Zero when a repository exists and can be inspected.
- Project Zero may transition to Engineering Ready when required evidence exists or accepted deferrals are documented.
- Engineering Ready may transition to Implementation when normal engineering work is authorized.
- Implementation may transition to Verification when claims are ready to be checked.
- Verification may transition to Release when required evidence supports publication.
- Release may transition to Maintenance after the released baseline is accepted.
- Maintenance may transition to Archive when active support ends.
- A repository may return to an earlier lifecycle state when evidence shows that its current state is no longer valid.

## Compliance and Evidence

A repository demonstrates compliance with this standard by preserving evidence for its declared lifecycle state.

Acceptable evidence may include:

- repository manifests,
- Project Zero reports,
- issue records,
- commits,
- review records,
- validation reports,
- certification reports,
- release records,
- or documented accepted deferrals.

AEMS should use available repository evidence to determine whether the declared lifecycle state is consistent with the repository record.

When evidence is missing, the repository should be treated as incomplete for the affected lifecycle state unless an accepted deferral exists.

## Core Rule

A repository should not enter normal implementation work until it is Engineering Ready or has documented accepted deferrals.

## Ownership

AES owns the lifecycle standard.
AEMS implements lifecycle inspection and reporting.
repo_templates supports repository bootstrap.
Project repositories provide lifecycle evidence.
catylist tracks ecosystem-level lifecycle status.

## Related Documents

- CAT-001 Catalyst Engineering Ecosystem
- CAT-001 Engineering Lifecycle supporting document
