# AES-003 Repository Manifest Standard

Status: Draft
Owner: AES
Version: 0.3.0 (proposed)

## Purpose

Define the machine-readable manifest used to describe repositories governed by the Catalyst engineering ecosystem.

## Scope

This standard applies to repositories governed by Catalyst.

Each governed repository should maintain a repository manifest that records identity, ownership, lifecycle state, dependencies, engineering state, and metadata.

The manifest is intended for use by AEMS and related repository automation.

## Definitions

### Repository Manifest

A machine-readable file that describes a repository's identity, owner, lifecycle state, dependencies, engineering metadata, and automation state.

### Manifest Version

The version of the repository manifest format used by a repository.

### Repository Identifier

A stable identifier for a repository within the Catalyst ecosystem.

### Repository Owner

The authoritative owner responsible for the repository and its engineering state.

### Lifecycle State

The repository lifecycle state defined by AES-001.

### Manifest Consumer

A tool, report, workflow, or engineering process that reads the repository manifest.

### Manifest Producer

A human or tool that creates or updates the repository manifest.

## Manifest Structure

A repository manifest should be stored at the repository root unless another location is explicitly documented.

The standard manifest filename is `aes-manifest.yaml`.

The manifest should be YAML-first for human maintenance and compatible with schema-based validation.

The manifest is organized into sections for:

- manifest metadata,
- repository identity,
- ownership,
- lifecycle state,
- standards conformance,
- dependencies,
- Project Zero state,
- automation state,
- evidence references,
- optionally, governed project scopes,
- and, optionally, licensing decisions.

AEMS should treat the manifest as the primary machine-readable entry point for repository inspection.

## Standard Applicability

Wherever a manifest lists the standards that govern a repository or a governed project scope, each listed standard is either applicable or explicitly not applicable.

- An applicable standard should reference its adoption evidence, such as the local profile, waiver log, and review-disposition ledger the standard requires.
- A standard that does not apply must be declared not applicable together with a reason. A governing standard is never silently omitted.
- A not-applicable declaration is reviewable evidence. It must be revisited when the languages, surfaces, or content of the governed unit change.

Field names for this form are defined in the AES-003 manifest field reference.

## Governed Project Scopes

A repository may declare governed project scopes, as authorized by Catylist `docs/adr/ADR-0004-governed-project-scopes.md`. A governed project scope is a top-level directory that owns its own automation logic and its own standards-adoption evidence, while the repository keeps exactly one primary class.

The `scopes` declaration is optional. A manifest without it is valid, and the repository root remains the single governed unit, exactly as before this declaration existed.

When scopes are declared:

- each scope must declare an identifier, path, owner, primary class, lifecycle state, and governing standards;
- each scope must declare its dependencies on other scopes, such as a data dependency on another scope's output, and declared dependencies must not be cyclic;
- each scope path must be a distinct top-level directory; nested scopes are not permitted;
- every governed file belongs to exactly one scope or to the repository-level remainder;
- a scope's primary class must not grant authority beyond the repository's primary class;
- scope-level adoption evidence, such as the profile, waiver log, and review-disposition ledger, must live under `<scope>/docs/engineering/`;
- the manifest, the root `README.md`, root workflows, and repository-wide configuration remain repository-level obligations.

Retiring a scope, for example after extraction into its own repository, removes its entry from `scopes` and should record the successor repository in the change that removes it.

## Licensing Decisions (proposed)

A repository may record its licensing decisions in an optional `licensing` declaration: the default rights holder, copyright text, and SPDX license expression for the repository, plus path overrides for content with a different rights holder or license, such as third-party data or generated reports.

The declaration records decisions; it does not apply them. Per Catylist `docs/adr/ADR-0007-governed-check-in-and-shared-automation.md` §2.4, licenses are applied with SPDX expressions and the REUSE Specification (file headers, `.license` sidecars, `REUSE.toml`). When a repository has adopted REUSE, the manifest declaration and the REUSE annotations must agree, and AEMS reports any disagreement.

When `licensing` is declared:

- every license value must be a valid SPDX license expression;
- every `LicenseRef-` identifier used must have its text in `LICENSES/<identifier>.txt`;
- each override path must be relative to the repository root, and override paths must be unique;
- when overrides overlap, the most specific path applies;
- content owned by a third party must be marked as third-party, so that the repository owner is never presented as granting a license to it.

Per ADR-0007 §2.5, this declaration is proposed for report-only use in pilot scopes. Its normative form is to be confirmed from pilot evidence before AES adopts it.

## Planned Fields

A Catylist governance back-pointer from the manifest to the Catylist inventory is planned under dlworrell/Catylist#18. This version does not define it, and producers should not introduce ad hoc fields for it.

## Related Documents

- Catylist `docs/adr/ADR-0004-governed-project-scopes.md`
- Catylist `docs/adr/ADR-0007-governed-check-in-and-shared-automation.md`
- [REUSE Specification](https://reuse.software/spec/) and [SPDX License List](https://spdx.org/licenses/)
- `docs/standards/AES-003-manifest-fields.md`
- `docs/standards/AES-003-validation.md`
- `standards/AES-SEC-001-secure-c-cpp-coding-rules.md`

## Revision History

- 0.3.0 (proposed): Adds the optional `licensing` declaration for repository default and path-override licensing decisions (Catylist ADR-0007 §2.4). Backward compatible: manifests without `licensing` are unchanged.
- 0.2.0: Added the optional `scopes` declaration and the explicit not-applicable form for governing standards (Catylist ADR-0004). Backward compatible: manifests without `scopes` are unchanged.
- 0.1.0: Initial draft.
