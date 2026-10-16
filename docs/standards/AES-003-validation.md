# AES-003 Supporting Document: Manifest Validation Requirements

Status: Draft
Owner: AES
Parent: AES-003 Repository Manifest Standard

## Purpose

Define validation expectations for Catalyst repository manifests.

## Validation Requirements

A repository manifest should be validated for:

- file presence
- parseability
- manifest version
- repository identity
- repository ownership
- lifecycle state
- Project Zero state
- applicable standards
- dependency references
- evidence references
- standard applicability entries, including a reason for every not-applicable entry
- governed project scopes, when declared

## Governed Project Scope Validation

When a manifest declares `scopes`, validation should report:

- duplicate scope identifiers or paths;
- scope paths that are not top-level directories, do not exist, or are nested in another scope;
- missing required scope fields;
- a scope class broader in authority than the repository's primary class;
- not-applicable standard entries without a reason;
- scope evidence paths outside `<scope>/docs/engineering/`;
- dependencies on undeclared scopes, and dependency cycles;
- governed files claimed by more than one scope.

A manifest without `scopes` is validated exactly as before.

## Validation Results

Validation should produce a repeatable result that can be preserved as engineering evidence.

AEMS should report missing, malformed, stale, or contradictory manifest data.

## Compatibility

Manifest changes should preserve compatibility where practical.

Breaking manifest changes should require a new manifest version and migration guidance.
