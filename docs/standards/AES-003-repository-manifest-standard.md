# AES-003 Repository Manifest Standard

Status: Draft
Owner: AES
Version: 0.1.0

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
