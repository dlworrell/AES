# AES-001 Repository Lifecycle Standard

Status: Draft
Owner: AES
Version: 0.1.0

## Purpose

Define the lifecycle states used by AES-governed repositories.

## Scope

This standard applies to repositories that participate in the Catalyst engineering ecosystem and claim AES governance.

The lifecycle applies to standards repositories, automation repositories, template repositories, product repositories, documentation repositories, and validation repositories.

## Definitions

### Lifecycle State

A named repository condition that describes the repository's current engineering maturity.

### Engineering Ready

A lifecycle state indicating that a repository has enough structure, evidence, metadata, and automation to support normal implementation work.

### Project Zero

The initialization and cleanup phase that prepares a repository for sustained engineering work.

### Accepted Deferral

A documented exception that permits later work to proceed while explicitly recording unfinished lifecycle requirements.

## Lifecycle States

1. Idea
2. Program Planning
3. Project Creation
4. Project Zero
5. Engineering Ready
6. Implementation
7. Verification
8. Release
9. Maintenance
10. Archive

## Core Rule

A repository should not enter normal implementation work until it is Engineering Ready or has documented accepted deferrals.

## Ownership

AES owns the lifecycle standard.
AEMS implements lifecycle inspection and reporting.
repo_templates supports repository bootstrap.
Project repositories provide lifecycle evidence.
catylist tracks ecosystem-level lifecycle status.

## Related Documents

- CAT-001 Catalyst Engineering Ecosystem
- CAT-001 Engineering Lifecycle supporting document
