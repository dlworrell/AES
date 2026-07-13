---
id: CAN-130
title: Evidence and Provenance Model
status: draft
lifecycle: proposed
type: standard
owner: AES
version: 0.1.0
---

# CAN-130 — Evidence and Provenance Model

## 1. Purpose

This standard defines how engineering evidence is identified, acquired, hashed, attributed, related, retained, invalidated, and referenced across the Catalyst ecosystem.

Evidence supports findings, remediation decisions, verification claims, engineering reports, releases, and certifications. A claim without traceable evidence is an assertion, not an engineering result.

## 2. Authority boundaries

- AES defines the required evidence semantics.
- AEMS defines and enforces policy for required evidence.
- Project Zero collects repository-lifecycle evidence and uses it in assessments.
- EDT extracts, normalizes, transforms, and renders evidence while preserving provenance.
- Governed repositories remain authoritative for their original source artifacts.

Evidence storage does not transfer ownership of the source artifact.

## 3. Evidence principles

Engineering evidence shall be:

1. **Identifiable:** assigned a stable identifier within its evidence package.
2. ** attributable:** linked to its source, producer, collection method, and time.
3. **Integrity-protected:** hashed using a declared algorithm.
4. **Contextualized:** associated with the repository state, tool version, and policy under which it was collected.
5. **Minimally transformed:** original evidence is retained when practical; transformations are recorded as derived evidence.
6. **Immutable after issuance:** a changed object becomes a new evidence object.
7. **Reviewable:** a human or tool can follow provenance back to the source.
8. **Scoped:** limitations, truncation, sampling, and unavailable data are explicit.

## 4. Evidence classes

Evidence may be classified as:

- **Source evidence:** repository files, commits, tags, manifests, specifications, ADRs, and configuration.
- **Execution evidence:** build logs, test logs, workflow runs, coverage, benchmarks, traces, and simulator output.
- **Review evidence:** approvals, review comments, waivers, issue decisions, and sign-offs.
- **Environmental evidence:** toolchain versions, runner image, operating system, hardware, environment variables, and dependency lock state.
- **External evidence:** standards, publications, vendor documentation, third-party attestations, and external datasets.
- **Derived evidence:** normalized records, indexes, graphs, summaries, extracted tables, and transformed documents.
- **Certification evidence:** the bounded package used to justify a certification decision.

## 5. Canonical evidence object

Every evidence object shall support the following fields:

- evidence identifier;
- evidence class and media type;
- title or concise description;
- source locator;
- source authority;
- repository and immutable revision, when applicable;
- producer or collector;
- collection method;
- collection timestamp in UTC;
- byte length or record count;
- cryptographic digest and digest algorithm;
- transformation history;
- parent evidence identifiers for derived evidence;
- applicable scope;
- confidentiality and retention classification;
- availability state;
- limitations or known defects.

Recommended identifier form:

```text
EV-<repository-id>-<UTC-date>-<sequence>
```

Identifiers are references, not substitutes for hashes. A digest proves content identity; an identifier preserves semantic continuity within the engineering record.

## 6. Source locators

A source locator shall be specific enough to retrieve or verify the original object. Examples include:

- Git repository URL, commit SHA, and path;
- GitHub workflow run, job, and artifact identifiers;
- issue or pull-request URL and comment identifier;
- document identifier, edition, page, and section;
- immutable object-store URI;
- DOI, ISBN, standards identifier, or archival catalog identifier.

Mutable branch names, filenames, or web pages alone are insufficient for certification evidence unless accompanied by an immutable snapshot or digest.

## 7. Integrity and hashing

SHA-256 is the baseline digest algorithm unless a governing security standard requires a stronger or different algorithm.

For a directory or package, the evidence record shall define whether the digest covers:

- an archive byte stream;
- a canonical manifest of member paths and member hashes;
- a Merkle tree;
- another explicitly documented canonicalization.

Two packages shall not be claimed identical unless the canonicalization method is also identical.

## 8. Transformation provenance

Every transformation shall record:

- input evidence identifiers and hashes;
- transformation tool and version;
- command, profile, or configuration;
- transformation time;
- output evidence identifier and hash;
- warnings, dropped content, normalization, OCR confidence, or lossy operations.

EDT shall preserve the distinction between original evidence and derived document products. OCR text, summaries, translated text, and rendered PDF are derived evidence and shall not silently replace the source scan or source document.

## 9. Evidence relationships

The model shall support at least:

- `derived_from`;
- `verifies`;
- `contradicts`;
- `supersedes`;
- `duplicates`;
- `supports_finding`;
- `supports_decision`;
- `produced_by`;
- `collected_during`;
- `invalidated_by`.

Relationships shall be directional and shall identify both objects explicitly.

## 10. Findings and evidence

Every Critical or High finding shall reference direct evidence sufficient to reproduce or independently verify the finding. Medium and lower findings should do the same unless the report clearly states that the item is advisory.

A finding shall distinguish:

- evidence of the observed condition;
- evidence of the governing requirement;
- evidence that remediation changed the condition;
- evidence that post-remediation verification passed.

## 11. Evidence package structure

A standard evidence package should contain:

```text
evidence-package/
├── manifest.json
├── relationships.json
├── checksums.sha256
├── sources/
├── execution/
├── reviews/
├── environment/
├── derived/
└── certification/
```

The manifest is canonical for package membership and metadata. Reports may reference package members by evidence identifier without embedding every object.

## 12. Retention and confidentiality

Evidence policy shall classify objects as public, internal, confidential, restricted, or secret-bearing.

Secrets, personal data, credentials, private keys, regulated information, and unnecessary proprietary content shall not be copied into routine evidence packages. Where such material proves a finding, the evidence record should use redaction, secure references, or attestations while retaining enough metadata to support review.

Retention periods shall account for release support, certification duration, legal obligations, reproducibility, storage cost, and source availability.

## 13. Invalidation and supersession

Evidence may become invalid because:

- the source was corrupted or misidentified;
- collection was incomplete;
- the tool was defective;
- the environment was not the declared environment;
- a later authoritative source contradicts it;
- required provenance is missing.

Invalidation shall not delete the record. It shall mark the evidence unavailable for specified claims, identify the reason, and link to replacement or corrective evidence.

## 14. Reproducibility

An evidence-backed claim is reproducible when an independent operator can identify the source revision, obtain the required inputs, recreate the declared environment within stated tolerances, execute the collection or verification method, and compare the result using declared acceptance criteria.

Where exact reproduction is impossible, the report shall state the limitation and distinguish repeatability, reproducibility, and independent corroboration.

## 15. EDT requirements

EDT shall:

- retain source locators and hashes through extraction and rendering;
- emit provenance records for all derived objects;
- expose lossy transformations and confidence measures;
- preserve document identifiers, citations, page mappings, and structural relationships when available;
- support rendering evidence references into human-readable reports without changing canonical identifiers.

## 16. Project Zero and AEMS requirements

Project Zero and AEMS shall:

- collect evidence before issuing findings where practical;
- record tool, policy, repository, and environment versions;
- preserve evidence packages even when assessment execution fails;
- prevent certification when required evidence is unavailable or invalid;
- distinguish repository failure, policy failure, evidence failure, and engine failure.

## 17. Acceptance criteria

An implementation conforms to CAN-130 when it:

1. assigns stable identifiers and cryptographic hashes;
2. records source authority, location, collection method, and time;
3. distinguishes original and derived evidence;
4. records transformation provenance;
5. supports evidence-to-finding and evidence-to-decision traceability;
6. preserves invalidated and superseded records;
7. provides a canonical package manifest;
8. handles confidentiality and retention explicitly;
9. supports independent verification of material claims.
