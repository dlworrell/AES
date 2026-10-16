# AES-DEV-001: Development Principles and Check-In Discipline

Status: Draft
Owner: AES
Scope: Catalyst ecosystem project repositories

## Purpose

This standard defines how project work enters the Catalyst ecosystem.

The goal is not merely to produce working code. The goal is to produce systems that are understandable, observable, auditable, recoverable, secure, and maintainable over long periods of time.

This standard applies to project-owned repositories including architecture, documentation, tooling, firmware, FPGA, operating-system, application, and governance repositories.

Project-specific documents may extend this standard. They must not weaken it without an explicit waiver or ADR.

## Engineering Traditions

The ecosystem borrows discipline from several traditions:

- practical hardware and firmware engineering;
- workstation-class architecture design;
- curl-style small-patch development and review discipline;
- OpenBSD-style security posture;
- documentation-first project governance.

Individual projects may add their own lineage. For example, ATARIX also draws from Vega816, BB816, Sun/NuBus/UPA, curl, and OpenBSD.

## Development Order

Project development follows this order:

1. Architecture first.
2. Specification second.
3. Implementation third.

No major externally visible component should exist without a corresponding specification or design note.

This applies to:

- hardware interfaces;
- FPGA modules;
- firmware interfaces;
- operating-system interfaces;
- protocol formats;
- file formats;
- APIs;
- command-line behavior;
- automation behavior;
- governance behavior.

Exploratory work is allowed, but it must be clearly marked as exploratory and must not be treated as stable architecture until the relevant design record exists.

## Documentation First

Documentation is part of the system.

A change that introduces or materially changes behavior must update the relevant documentation in the same change series. Deferring documentation to a later unspecified change is not acceptable for stable behavior.

Required documentation may include:

- architecture overview;
- component specification;
- protocol specification;
- register map;
- memory map;
- packet, record, or descriptor format;
- user-facing behavior;
- failure-mode description;
- recovery behavior;
- security model;
- ADR.

Documentation-only changes are acceptable and encouraged when they clarify intent, provenance, or design constraints.

## Documentation Authority

Each Catalyst project-owned repository must declare where its authoritative development, specification, ADR, and interface documents live.

Documentation authority may be:

- `local`: the repository owns its own governing documents.
- `delegated`: another Catalyst project repository owns the governing documents.
- `transitional`: authority is temporarily centralized while documents are being split into their final repositories.
- `external`: a repository is outside Catalyst governance and is used only as an external/reference source.

Delegated or transitional authority must name the authority repository and the relevant document paths.

A repository is not noncompliant merely because its authoritative documents live elsewhere, provided the delegation is explicit and traceable.

External/reference repositories must not be rewritten merely to satisfy Catalyst policy.

AEMS should report local, delegated, transitional, and external documentation authority separately.

## Version Everything

Interfaces are versioned when compatibility matters.

Examples:

- protocol versions;
- descriptor versions;
- schema versions;
- file format versions;
- public API versions;
- workflow or automation contract versions.

Existing interfaces must not be silently changed. A change to an existing interface must do one of the following:

- preserve compatibility;
- add a new version;
- document the compatibility break and migration path;
- explicitly mark the old interface as deprecated.

## Small Changes

Prefer:

- small commits;
- small pull requests;
- small reviews;
- small testable units.

Large architectural changes must be decomposed into reviewable units.

A commit should normally represent one logical change. Mechanical refactors, generated updates, documentation changes, and behavior changes should not be mixed without an explicit rationale.

## curl-Style Check-In Discipline

Project changes should be easy to review, easy to test, easy to revert, and useful to future `git bisect` work.

Commit subjects should normally use an area prefix:

```text
area: imperative summary
```

Examples:

```text
discovery: validate image length before CRC copy
mailbox: add v1 CRC mismatch fixture
aems: show review findings in aggregate report
repo-templates: add secure-cxx profile template
```

A change should include, or explicitly justify the absence of:

- tests;
- updated documentation;
- interface version impact;
- recovery or failure-mode impact;
- security or trust-boundary impact.

Reviewers should reject clever code that is hard to reason about when a clearer design is available.

## Default-Branch Integrity

Repository history is evidence. Reviewed commits, their authorship, and their
signatures must reach the default branch unchanged and stay there.

Authority: `dlworrell/Catylist` `docs/adr/ADR-0006-default-branch-protection-and-signed-commits.md`.

These requirements apply to every repository that Catylist places in
default-branch protection scope. External/reference repositories are outside
them. In this section, MUST means required for conformance unless an approved
waiver applies.

### AES-DEV-001-R001: Verified commits

Every commit that lands on a default branch MUST be signed and reported as
verified by the hosting platform (GitHub "verified"). This applies equally to
maintainers, AI coding agents, and automation.

### AES-DEV-001-R002: No history rewriting

Default-branch history MUST NOT be rewritten. Force pushes and other
non-fast-forward updates to a default branch, and deletion of a default
branch, are not permitted.

### AES-DEV-001-R003: Pull requests only

Changes MUST reach a default branch only through a pull request. Direct pushes
to a default branch are not permitted.

### AES-DEV-001-R004: Merge commits only

Pull requests MUST be integrated by merge commit. Squash and rebase merges MUST
NOT be used, so that the reviewed commits, their authorship, and their
signatures reach the default branch unchanged.

### AES-DEV-001-R005: Automation follows the same rules

Automation MUST NOT push directly to a default branch. Workflows that produce
commits MUST open a pull request instead, and the commits they produce MUST be
verified, for example by creating them through the GitHub API rather than by a
plain `git push` from a runner.

### AES-DEV-001-R006: Existing history is preserved

Existing unsigned commits MUST NOT be rewritten to satisfy R001. These
requirements apply to new changes.

### AES-DEV-001-R007: Immutable release tags

Release tags matching `v*` MUST NOT be deleted, moved, or force-updated once
created.

A repository that cannot yet meet R001 MAY meet the other requirements first,
provided the exception is recorded as a waiver in that repository with an
expiry date.

## Observable Systems

Every subsystem should expose enough state for diagnosis and recovery.

Subsystem specifications should define, as applicable:

- status;
- counters;
- fault history;
- health metrics;
- recovery information;
- diagnostic visibility;
- debug hooks or trace points.

Debuggability is a first-class design goal.

A subsystem that can fail silently is incomplete.

## Security by Design

Projects should use the following posture:

- secure by default;
- least privilege;
- privilege separation where appropriate;
- capability or authority revocation where appropriate;
- explicit trust boundaries;
- small trusted computing bases;
- cryptography by reuse, not invention.

Security-sensitive specifications must identify trust boundaries and the authorities that cross them.

## Cryptographic Standards

Projects should avoid custom cryptography.

Preferred primitives, when a project needs these categories, are:

- Ed25519 for signatures;
- ChaCha20-Poly1305 for authenticated encryption;
- BLAKE2b and SHA-256 for hashing.

A project may use other well-reviewed primitives when required by interoperability, platform constraints, or an ADR.

Custom cryptography requires an ADR and a specific review plan.

Cryptographic code should be reused from established implementations wherever possible.

## Recovery First

Subsystems should define:

- failure modes;
- recovery behavior;
- reset behavior;
- diagnostic visibility;
- expected operator, supervisor, or automation action.

Systems should fail visibly and recover deliberately.

## Architecture Decision Records

Major decisions must be documented as ADRs.

An ADR should record:

- context;
- decision;
- alternatives considered;
- consequences;
- status;
- date.

Examples of ADR-worthy decisions include:

- choice of hardware platform;
- choice of protocol structure;
- choice of authority model;
- choice of repository architecture;
- choice of build system;
- irreversible compatibility decisions;
- major security or recovery tradeoffs.

## Authority Models

Projects that define authority, privilege, access, execution domains, credentials, capabilities, or trust zones must document the authority model.

For ATARIX specifically:

- rings provide execution authority;
- capabilities provide resource authority;
- neither mechanism is sufficient alone.

Other projects may use different authority models, but the model must be explicit when authority boundaries exist.

## Acceptance Expectations

A significant project change should be rejected or sent back for design work when it lacks:

- a specification for new externally visible behavior;
- a versioning answer for an interface change;
- tests or a test rationale;
- an observability answer for a new subsystem;
- a recovery answer for a fault-prone subsystem;
- a trust-boundary answer for a security-sensitive path;
- an ADR for a major architecture decision.

## AEMS Enforcement Direction

AEMS should first report evidence, not block aggressively.

Initial checks should detect:

- this standard or local project profile;
- architecture/specification directories;
- ADR directories;
- versioned interface specifications;
- observability language;
- recovery or failure-mode language;
- trust-boundary or authority-model language;
- check-in discipline guidance;
- default-branch protection, merge settings, release-tag protection, and commit verification state (AES-DEV-001-R001 to R007).

AEMS enforcement should ratchet:

1. detect;
2. report;
3. baseline gaps;
4. require evidence for new major work;
5. block repeated or unreviewed violations.

## Design Principle

Prefer systems that are understandable, observable, auditable, recoverable, and secure over systems that are merely clever.

## Revision History

- 2026-10-16: Added default-branch integrity requirements AES-DEV-001-R001 to
  R007 per Catylist ADR-0006.
