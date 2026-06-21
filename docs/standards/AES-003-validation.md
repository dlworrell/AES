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

## Validation Results

Validation should produce a repeatable result that can be preserved as engineering evidence.

AEMS should report missing, malformed, stale, or contradictory manifest data.

## Compatibility

Manifest changes should preserve compatibility where practical.

Breaking manifest changes should require a new manifest version and migration guidance.
