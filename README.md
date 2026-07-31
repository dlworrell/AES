# AES

AES is the Atarix Engineering Standard.

This repository is the canonical engineering-standards authority for the
Catalyst ecosystem. It contains the engineering creed, standards, decision
records, research notes, case studies, and templates that guide how projects
are designed, documented, reviewed, secured, verified, and maintained.

## Purpose

AES exists to preserve engineering knowledge so future maintainers can
understand what was built, why it was built, how it must be validated, and how
it should evolve.

## Pillar I

First, Observe. Then, Understand. Then, Improve.

Observation precedes understanding. Understanding precedes improvement.

## Authority chain

- Catylist defines program governance, repository relationships, and authority
  boundaries.
- AES defines engineering obligations, standards, and required evidence.
- AEMS manages the Catalyst project and verifies or enforces AES requirements.
- Project repositories implement systems and maintain project-specific
  specifications, ADRs, tests, and evidence.
- Just-a-Geek-LLC owns company and public-facing organizational material.

The policy dependency direction is:

```text
Catylist -> AES -> AEMS -> governed repositories
```

AES must not redefine Catylist governance. AEMS must not redefine AES
requirements. Downstream repositories may extend AES locally but may not weaken
an AES requirement without an explicit waiver or ADR permitted by the
governing standard.

## Repository role

AES owns:

- engineering principles and development discipline;
- secure coding requirements;
- documentation, testing, build, versioning, CI/CD, observability,
  optimization, and release standards;
- standard-level evidence requirements;
- standard templates and normative terminology;
- engineering-standard ADRs and revision history.

AES does not own:

- Catalyst program governance;
- AEMS scanner or project-management implementations;
- project-specific architecture and implementation specifications;
- company or public-facing content.

## Active core standards

- [AES-DEV-001 — Development Principles and Check-In Discipline](standards/AES-DEV-001-development-principles-and-check-in-discipline.md)
  is currently `draft` and is already used through project-local profiles and
  AEMS evidence reporting.
- [AES-BLD-001 — Native Build, Toolchain, and Distribution Parity](standards/AES-BLD-001-native-build-toolchain-and-distribution-parity.md)
  is `draft` and defines CMake/CTest with Clang as the canonical developer and
  analysis path, GNU Autotools with GCC as the independent portability and
  source-distribution path, and CI parity between them.
- [AES-SEC-001 — Secure C and C++ Coding Rules](standards/AES-SEC-001-secure-c-cpp-coding-rules.md)
  is `adopted` and is evaluated through AEMS adoption and security scans.
- [AES-SEC-002 — Cross-Language, Secret, and Encrypted-Storage Boundaries](standards/AES-SEC-002-cross-language-secret-storage-boundaries.md)
  is `adopted` and defines required boundaries for safe-language/native ABIs,
  secret lifecycles, encrypted local state, platform hardening, and
  repository-data hygiene.

`aes-manifest.yaml` is the machine-readable source for repository role,
standard status, governing relationships, and compliance expectations.

## Current structure

- `creed/`: foundational philosophy;
- `standards/`: engineering standards and practices;
- `adr/`: engineering-standard decision records;
- `research/`: investigations and proposed methods;
- `case-studies/`: operational lessons;
- `templates/`: document and rule templates;
- `references/`: external influences and source material.

## Current status

AES is in active foundation work. Downstream enforcement has begun: AEMS now
maintains repository inventories and executes AES-DEV-001 and AES-SEC-001
reporting and adoption gates, including for `MayaUSD2017Bridge`.

The immediate objective is to stabilize the core standards, expand
machine-readable requirement metadata, provide a consistent standard structure,
and ratchet downstream enforcement without rewriting externally governed
upstream repositories.

## Core idea

Technology is temporary. Engineering knowledge endures.
