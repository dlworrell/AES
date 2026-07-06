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
- check-in discipline guidance.

AEMS enforcement should ratchet:

1. detect;
2. report;
3. baseline gaps;
4. require evidence for new major work;
5. block repeated or unreviewed violations.

## Design Principle

Prefer systems that are understandable, observable, auditable, recoverable, and secure over systems that are merely clever.
