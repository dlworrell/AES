---
id: CAN-120
title: Engineering Report Standard
status: draft
lifecycle: proposed
type: standard
owner: AES
version: 0.1.0
---

# CAN-120 — Engineering Report Standard

## 1. Purpose

This standard defines the canonical machine-readable and human-readable representation of an engineering assessment at a specific repository state. It applies to Project Zero, AEMS, EDT, and governed repositories that produce repository-health, verification, remediation, release, or certification reports.

An Engineering Report is not merely a rendered document. It is the authoritative assessment record from which presentations, dashboards, findings, remediation plans, and certification packages may be generated.

## 2. Authority boundaries

- AES owns this report standard and its required semantics.
- AEMS owns assessment policy and compliance decisions.
- Project Zero owns repository-lifecycle assessment execution.
- EDT owns transformation of canonical report data into durable knowledge products.
- Governed repositories own their source evidence and repository-specific extensions.

EDT shall not decide whether a repository passes. Project Zero and AEMS shall not maintain independent format-specific report implementations when EDT can render the canonical report model.

## 3. Required properties

Every Engineering Report shall be:

1. **Point-in-time:** bound to a repository, commit, assessment time, tool versions, and policy profile.
2. **Evidence-backed:** every material finding and decision references one or more evidence objects.
3. **Reproducible:** the report records sufficient inputs and versions to repeat the assessment.
4. **Format-neutral:** facts are represented once in a canonical model and may be rendered into multiple formats.
5. **Extensible:** project-specific sections may be added without weakening required fields.
6. **Comparable:** successive reports support deterministic change and trend analysis.
7. **Immutable after issuance:** corrections create a superseding report rather than rewriting an issued record.

## 4. Canonical report identity

Each report shall contain:

- report identifier;
- schema version;
- repository identity and canonical URL;
- assessed commit or immutable source revision;
- branch or ref, when applicable;
- assessment mode;
- assessment profile and profile version;
- start and completion timestamps in UTC;
- producing tool and version;
- governing standards and policy set;
- prior report identifier, when available;
- report status: draft, issued, superseded, or withdrawn.

Recommended identifier form:

```text
ERPT-<repository-id>-<UTC-date>-<sequence>
```

## 5. Required report sections

### 5.1 Executive summary

The executive summary shall state:

- assessment purpose;
- repository readiness or lifecycle state;
- final disposition;
- finding counts by severity;
- material changes since the prior report;
- required next actions;
- certification recommendation, when requested.

### 5.2 Scope and limitations

The report shall identify what was inspected, what was excluded, unavailable evidence, tool limitations, incomplete checks, and any assumptions. A report shall not imply coverage beyond its declared scope.

### 5.3 Repository identity and inventory

The report shall reference the repository manifest and summarize relevant source files, languages, build systems, workflows, dependencies, documents, generated artifacts, and external inputs.

### 5.4 Assessment results

Results shall be grouped by governed domain, including as applicable:

- identity and purpose;
- governance;
- documentation;
- architecture and specifications;
- build and reproducibility;
- implementation quality;
- verification and coverage;
- security and supply chain;
- release and operations;
- traceability;
- accessibility;
- evidence and provenance.

### 5.5 Findings

Every finding shall include:

- stable finding identifier;
- title and description;
- severity;
- confidence;
- affected domain and artifact;
- governing requirement;
- evidence references;
- detected state;
- required or recommended state;
- remediation guidance;
- automation eligibility;
- disposition and lifecycle state.

### 5.6 Remediation record

The report shall distinguish:

- proposed remediation;
- automatically applied remediation;
- skipped remediation;
- remediation requiring human judgment;
- verification performed after remediation.

No automatic remediation shall be reported as successful without post-change verification evidence.

### 5.7 Traceability

The canonical report shall support links among:

```text
Requirement → Assessment check → Finding → Evidence → Remediation → Verification → Decision
```

### 5.8 Decision and certification

When certification is requested, the report shall state one of:

- certified;
- conditionally certified;
- remediation required;
- assessment incomplete;
- certification denied.

The decision shall identify the authority, applicable criteria, unresolved exceptions, expiration or review date, and evidence package.

## 6. Severity model

The standard severity levels are:

- **Critical:** immediate unacceptable risk or invalidates certification.
- **High:** material requirement failure that blocks normal readiness or certification.
- **Medium:** significant weakness requiring planned remediation.
- **Low:** limited impact defect or maintainability concern.
- **Informational:** observation, opportunity, or contextual record.

Severity and confidence are independent. Tools shall not inflate severity to compensate for uncertain detection.

## 7. Canonical data package

A complete report package should contain:

```text
engineering-report/
├── report.json
├── executive-summary.md
├── findings.json
├── remediation-plan.json
├── traceability.json
├── inventory.json
├── metrics.json
├── provenance.json
├── certification.json
└── evidence/
```

`report.json` is the canonical report object. Other JSON files may be projections for operational convenience. Rendered Markdown, HTML, PDF, EPUB, SARIF, CSV, and dashboard views are derived products.

## 8. Historical comparison

A comparison report shall identify:

- new findings;
- resolved findings;
- reopened findings;
- changed severity or confidence;
- changed scope or policy;
- evidence added, removed, or invalidated;
- metric trends;
- certification-state changes.

A changed policy profile shall be reported explicitly so that a score change is not mistaken for a repository regression.

## 9. Scoring

Composite health scores may be included, but they shall never replace findings or certification criteria. Every score shall declare:

- component metrics;
- weights;
- normalization method;
- missing-data handling;
- policy version;
- whether the score is comparable to prior reports.

## 10. Rendering requirements

EDT renderers shall preserve canonical identifiers, evidence links, severity, scope, and decision semantics across output formats. Presentation changes must not alter the underlying assessment meaning.

At minimum, Project Zero and AEMS shall support:

- JSON for machine processing;
- Markdown for repository and GitHub presentation.

Archival PDF, HTML, SARIF, CSV, EPUB, and other outputs may be produced by EDT profiles.

## 11. Failure behavior

A failed assessment shall still produce the largest truthful report package possible. The report shall distinguish engine failure from repository noncompliance and identify the last completed stage.

## 12. Acceptance criteria

An implementation conforms to CAN-120 when it:

1. produces a canonical point-in-time report object;
2. links material claims to evidence;
3. declares scope and limitations;
4. preserves required finding semantics;
5. supports deterministic rendering into at least JSON and Markdown;
6. records remediation and post-remediation verification separately;
7. supports supersession rather than silent mutation;
8. can be compared with a prior report without relying on presentation text.
