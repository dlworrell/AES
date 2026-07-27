# Secure C/C++ Profile

Status: Adopted
Repository: dlworrell/AES
Standard: AES-SEC-001
Role: Engineering standards authority

## Purpose

AES is the normative authority for `AES-SEC-001: Secure C and C++ Coding Rules`.
This local profile makes the authority repository subject to the same operational
adoption contract it defines for project-owned repositories.

The current repository is documentation-first. Its long-term plan permits
standalone C-based tooling, so the secure native baseline is adopted before
native implementation begins.

## Required Local Behavior

Any C or C++ code introduced here must:

- follow `standards/AES-SEC-001-secure-c-cpp-coding-rules.md`;
- avoid banned unsafe interfaces;
- carry explicit bounds across external buffer and parser boundaries;
- check allocation, length, and copy-size arithmetic;
- compile cleanly under the repository warning and Clang-Tidy profile;
- run sanitizer and fuzz controls when the code surface requires them; and
- record any approved exception in the local waiver log.

## Authority Boundary

This file records local adoption only. It does not replace, weaken, or fork the
normative AES standard.

## Ratchet Rule

The repository begins with a zero-violation baseline. New violations block merge
unless an explicit, reviewed waiver is recorded.
