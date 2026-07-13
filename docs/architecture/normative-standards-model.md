# AES Normative Standards Model

Status: Draft
Owner: AES
Authority: Catylist
Implemented by: AEMS

## Purpose

AES is the normative engineering-standards layer in the Catalyst authority chain:

```text
Catylist -> AES -> AEMS -> governed repositories
```

Catylist defines program authority. AES translates that authority into versioned engineering obligations. AEMS consumes those obligations to generate checks, collect evidence, report conformance, and enforce ratcheted policy.

## Repository Boundary

AES owns declarative standards material:

- normative standards;
- standards profiles;
- schemas for standards and profiles;
- standard registries and indexes;
- document templates;
- lifecycle and revision records;
- informative rationale and examples.

Executable scanners, repository repair tools, CI generators, and compliance engines belong in AEMS.

Project-specific architecture, implementation, tests, and local evidence belong in the governed project repository.

## Normative Document Requirements

Every stable AES standard shall declare:

- identifier;
- title;
- status;
- semantic version or revision;
- owner;
- Catylist authority reference;
- scope and applicability;
- normative requirements;
- required evidence;
- waiver rules;
- AEMS enforcement mapping;
- dependencies and related standards;
- revision history.

Normative language shall use clearly defined requirement levels such as MUST, SHOULD, MAY, and BANNED.

## Standards Families

AES standards may be organized into families such as:

- architecture;
- development;
- documentation;
- coding and language;
- testing and verification;
- security;
- CI/CD;
- traceability;
- observability and diagnostics;
- release engineering;
- repository structure.

Numbering and family names shall be governed by an AES registry rather than inferred from directory layout alone.

## Profiles

Profiles group standards for common repository classes. A governed repository should normally select one or more profiles instead of enumerating every standard independently.

Example profile classes include:

- documentation/governance repository;
- C library;
- firmware;
- RTL/FPGA;
- operating system;
- application;
- research or exploratory work.

A profile shall declare included standards, allowed extensions, required evidence, and any repository-class-specific defaults.

## Machine-Readable Metadata

AES shall maintain a versioned schema for standard and profile metadata. AEMS shall parse this metadata directly rather than hard-code standard identities or evidence expectations.

Minimum metadata includes:

```yaml
id: AES-EXAMPLE-001
title: Example Standard
status: draft
version: 0.1.0
owner: AES
authority: Catylist
implemented_by:
  - AEMS
applies_to:
  - repository
required_evidence: []
waiver_policy: explicit
```

## Traceability

Every executable AEMS rule shall trace to one or more AES requirements. Every AES standard shall trace to its Catylist authority or governance rationale.

The required direction is:

```text
Catylist governance
        -> AES standard
        -> AEMS rule
        -> repository evidence
```

## Prohibitions

AES shall not:

- contain project-specific implementation policy unless clearly marked as an informative example;
- embed executable enforcement as the normative source of truth;
- redefine Catylist repository governance;
- permit AEMS behavior to become authoritative merely because it exists in code.

## Adoption Sequence

1. Normalize existing AES-000, AES-DEV-001, and AES-SEC-001 metadata and structure.
2. Define the standards metadata schema and lifecycle vocabulary.
3. Define the standards registry and profile model.
4. Map AEMS rules to normative requirement identifiers.
5. Ratchet downstream repositories only after evidence generation is stable.
