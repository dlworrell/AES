# CAN-001 – Engineering Constitution

Status: Draft

## Purpose

The Engineering Constitution defines the working rules for Project Zero and the wider Catalyst and Atarix engineering ecosystem.

It exists to keep the project coherent as it grows across repositories, tools, documents, hardware, software, and future contributors.

## Authority

AES is the canonical engineering record for the Catalyst and Atarix ecosystem.

Project repositories may contain implementation-specific detail, but engineering principles, documentation discipline, decision process, and traceability rules originate here unless explicitly superseded by a later AES document.

## First Principle

First, Observe. Then, Understand. Then, Improve.

Observation precedes understanding. Understanding precedes improvement.

No change should be made merely because a better idea is imagined. The current state must be observed first, the design intent must be understood next, and improvement must follow from evidence, rationale, and traceable need.

## Source of Truth

Git is the source of truth.

Chat, meetings, notes, and informal discussion may produce insight, but engineering state becomes authoritative only when committed to the appropriate repository.

## Documentation Discipline

Documentation is an engineering artifact.

The following rules apply unless a later AES standard narrows or extends them:

1. One document per commit.
2. One logical change per commit.
3. Preserve provenance, rationale, and traceability.
4. Prefer incremental correction over broad rewrite.
5. Do not discard prior reasoning without recording why it was superseded.

## Change Discipline

Changes must be small enough to review and specific enough to understand.

A change should answer:

- What changed?
- Why did it change?
- What prior state or decision does it depend on?
- What future work does it enable or constrain?

## Continuity

Project Zero is not a brainstorming exercise.

Brainstorming may occur when explicitly requested, but default work mode is execution from the current repository state.

Before proposing a new structure, process, or architecture, contributors must first read the relevant committed material and determine whether the existing structure already provides a path forward.

## Stewardship

Engineering work should be maintainable by people who were not present for the original discussion.

Every durable decision should leave enough context for a future maintainer to reconstruct the reason it exists.

## Relationship to Other Documents

This constitution provides the foundation for later AES and CAN documents, including intellectual lineage, glossary, templates, research notes, and engineering mindset material.

It does not replace detailed standards. It constrains them.
