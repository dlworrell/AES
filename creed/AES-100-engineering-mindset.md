# AES-100 – Engineering Mindset

Status: Draft

## Purpose

AES-100 defines the engineering mindset expected across AES, Project Zero, Catalyst, Atarix, and related work.

It describes how contributors should approach unknown systems, incomplete information, inherited decisions, and proposed improvements.

## Core Principle

First, Observe. Then, Understand. Then, Improve.

This is not a slogan. It is the operating discipline of the project.

## Observe

Observation means reading, inspecting, measuring, and recording the current state before changing it.

Observation includes:

- reading committed documents before proposing replacements
- inspecting repository state before creating new structure
- identifying what already exists
- distinguishing evidence from assumption
- preserving facts even when they are inconvenient

Observation prevents premature redesign.

## Understand

Understanding means reconstructing intent, constraint, context, and tradeoff.

Understanding includes:

- asking why a structure exists
- identifying what problem a decision solved
- tracing an idea to its source when possible
- distinguishing temporary scaffolding from durable architecture
- recognizing when an apparent flaw is actually a constraint response

Understanding prevents shallow correction.

## Improve

Improvement means making a justified change that leaves the system clearer, stronger, or more maintainable.

Improvement includes:

- committing one logical change at a time
- preserving rationale
- reducing ambiguity
- strengthening traceability
- improving future maintainability
- avoiding broad rewrites when narrow correction is sufficient

Improvement must be earned by observation and understanding.

## Engineering Conduct

A contributor working under AES should:

1. Read first.
2. Preserve context.
3. Prefer small, reviewable changes.
4. Make reasoning durable.
5. Respect existing structure until evidence shows it should change.
6. Treat documentation as engineering work.
7. Leave the project easier to understand than it was found.

## Anti-Patterns

The following behaviors work against AES discipline:

- redesigning before reading
- replacing terminology without recording rationale
- treating chat as the source of truth
- mixing unrelated changes in one commit
- erasing provenance
- creating speculative structure without committed need
- confusing personal preference with engineering improvement

## Standard of Work

AES work should be understandable by a future maintainer who was not present for the original discussion.

If that future maintainer cannot determine what changed, why it changed, and what it depends on, the work is incomplete.

## Relationship to CAN-001

CAN-001 defines the engineering constitution.

AES-100 defines the mindset needed to practice that constitution well.
