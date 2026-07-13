# AES

AES is the Atarix Engineering Standard.

This repository is the canonical engineering-standards authority for the Catalyst ecosystem. It contains the engineering creed, standards, decision records, research notes, case studies, and templates that guide how projects are designed, documented, reviewed, secured, verified, and maintained.

## Purpose

AES exists to preserve engineering knowledge so future maintainers can understand what was built, why it was built, how it must be validated, and how it should evolve.

## Pillar I

First, Observe. Then, Understand. Then, Improve.

Observation precedes understanding. Understanding precedes improvement.

## Authority Chain

- Catylist defines program governance, repository relationships, and authority boundaries.
- AES defines engineering obligations, standards, and required evidence.
- AEMS manages the Catalyst project and verifies or enforces AES requirements.
- Project repositories implement systems and maintain project-specific specifications, ADRs, tests, and evidence.
- Just-a-Geek-LLC owns company and public-facing organizational material.

The policy dependency direction is:

```text
Catylist → AES → AEMS → governed repositories
```

AES must not redefine Catylist governance. AEMS must not redefine AES requirements. Downstream repositories may extend AES locally but may not weaken an AES requirement without an explicit waiver or ADR permitted by the governing standard.

## Repository Role

AES owns:

- engineering principles and development discipline;
- secure coding requirements;
- documentation, testing, build, versioning, CI/CD, observability, optimization, and release standards;
- standard-level evidence requirements;
- standard templates and normative terminology;
- engineering-standard ADRs and revision history.

AES does not own:

- Catalyst program governance;
- AEMS scanner or project-management implementations;
- project-specific architecture and implementation specifications;
- company or public-facing content.

## Current Structure

- `creed/`: foundational philosophy;
- `standards/`: engineering standards and practices;
- `adr/`: engineering-standard decision records;
- `research/`: investigations and proposed methods;
- `case-studies/`: operational lessons;
- `templates/`: document and rule templates;
- `references/`: external influences and source material.

## Current Status

AES is in active foundation work. The immediate objective is to stabilize the existing core standards, establish machine-readable standard metadata, and provide a consistent standard structure before adding further downstream enforcement.

## Core Idea

Technology is temporary. Engineering knowledge endures.
