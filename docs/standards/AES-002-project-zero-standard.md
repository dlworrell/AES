# AES-002 Project Zero Standard

Status: Draft
Owner: AES
Version: 0.1.0

## Purpose

Define the normative engineering standard for preparing a repository for sustained engineering work through Project Zero.

Project Zero establishes the minimum engineering baseline required before a repository enters normal implementation. The objective is to ensure repositories are organized, traceable, measurable, and suitable for automation.

## Scope

This standard applies to every repository governed by the Catalyst engineering ecosystem.

Project Zero applies regardless of repository type, including governance repositories, standards repositories, automation repositories, template repositories, product repositories, documentation repositories, and validation repositories.

Project Zero concludes when a repository satisfies the Engineering Ready requirements defined by AES and has documented any accepted deferrals.

## Definitions

### Project Zero

The engineering process that prepares a repository for sustained engineering work by establishing organization, traceability, metadata, validation, and automation readiness.

### Engineering Ready

The lifecycle state indicating that a repository has successfully completed Project Zero or has documented accepted deferrals for unfinished requirements.

### Repository Inventory

A complete accounting of the repository's files, directories, artifacts, and engineering assets.

### Documentation Inventory

A classified inventory of documentation artifacts, including ownership, status, location, and traceability.

### Repository Manifest

A machine-readable description of repository metadata, structure, lifecycle, dependencies, and engineering state.

### Engineering Evidence

Objective information demonstrating that a repository satisfies one or more Project Zero requirements.

### Traceability

The ability to relate engineering artifacts through documented relationships.

### Accepted Deferral

A documented exception that permits Project Zero to conclude while explicitly identifying incomplete work, rationale, ownership, and planned resolution.

### Certification

The documented determination that a repository satisfies the applicable Project Zero completion criteria and Engineering Ready requirements.

## Normative Requirements

The key words "shall", "shall not", "should", "should not", and "may" are to be interpreted as described in RFC 2119.

### Repository Requirements

- Every Catalyst-governed repository shall complete Project Zero before entering normal implementation, or shall document accepted deferrals.
- Every repository shall identify its authoritative owner.
- Every repository shall declare its engineering lifecycle state.
- Every repository shall maintain a machine-readable repository manifest conforming to AES-003.
- Every repository shall maintain an inventory of engineering artifacts.
- Every repository shall maintain an inventory of documentation artifacts.

### Documentation Requirements

- Every authoritative engineering document shall have a unique identifier.
- Every authoritative engineering document shall declare its owner, status, version, and lifecycle state.
- Documentation shall be traceable to the issues, specifications, implementations, or evidence that justify its existence.

### Traceability Requirements

- Engineering artifacts shall be traceable through documented relationships.
- Repository changes shall be traceable through version control history.
- Accepted deferrals shall identify the deferred requirement, rationale, owner, and intended resolution.

### Automation Requirements

- Repository state should be machine-readable whenever practical.
- Repository metadata should support automated inspection by AEMS.
- Manual engineering activities should be reduced through automation where practical.

### Validation Requirements

- Project Zero completion shall be supported by objective engineering evidence.
- Engineering Ready claims shall be supported by documented evidence or accepted deferrals.
- Compliance with this standard should be independently verifiable from repository state.

## Project Zero Workflow

Project Zero follows this workflow:

1. Observe repository state.
2. Inventory repository contents.
3. Classify engineering artifacts.
4. Organize artifacts according to applicable standards.
5. Apply required metadata.
6. Validate repository state.
7. Document accepted deferrals.
8. Record certification evidence.
9. Transition to Engineering Ready when allowed by AES-001.

The workflow shall preserve evidence sufficient to explain the repository state and any accepted deferrals.

## Evidence Requirements

Project Zero completion shall be supported by objective engineering evidence.

Evidence may include:

- repository inventory,
- documentation inventory,
- repository manifest,
- metadata validation results,
- documentation validation results,
- traceability records,
- issue records,
- commit history,
- review records,
- automation reports,
- certification reports,
- and accepted deferrals.

Engineering evidence should be objective, traceable, reproducible, version controlled, reviewable, and machine-readable where practical.

Evidence shall remain in, or be traceable to, the repository that produced it.

AEMS should automatically discover, collect, validate, and report Project Zero evidence wherever practical.

## Completion Criteria

Project Zero is complete when applicable requirements are satisfied or accepted deferrals are documented.

A repository shall demonstrate:

- repository inventory,
- documentation inventory,
- repository manifest,
- ownership,
- lifecycle state,
- traceability,
- validation results,
- engineering evidence,
- and accepted deferrals, if any.

A repository may be declared Engineering Ready when the required Project Zero evidence exists or accepted deferrals have been approved.

## Assessment

Assessment compares repository evidence against this standard.

Assessment reports should identify the repository, applicable standards, assessment date, findings, accepted deferrals, and evidence references.

AEMS should automate assessment where practical. Engineering judgment remains with the repository owner or designated engineering authority.
