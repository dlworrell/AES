# AES-003 Supporting Document: Manifest Field Requirements

Status: Draft
Owner: AES
Parent: AES-003 Repository Manifest Standard

## Purpose

Define the initial field set expected in a Catalyst repository manifest.

## Core Fields

A governed repository should provide fields for:

- manifest version
- repository identifier
- repository name
- repository owner
- repository role
- lifecycle state
- Project Zero state
- applicable AES standards
- primary branch
- issue tracker

These fields provide the minimum information needed for repository discovery, lifecycle reporting, and AEMS inspection.

## Supplemental Fields

A repository may also provide fields for:

- contacts
- labels
- dashboards
- release policy
- certification history
- related repositories
- documentation roots
- generated reports
- accepted deferrals

Supplemental fields should not contradict core repository identity, lifecycle, ownership, or standards information.

## Standard Applicability Entry

A manifest that lists the standards governing a repository or a governed project scope uses the following entry form for each standard:

| Field | Required | Meaning |
|---|---|---|
| `id` | yes | The standard identifier, for example `AES-SEC-001` |
| `applicability` | yes | `applicable` or `not-applicable` |
| `reason` | when `not-applicable` | Why the standard does not apply, for example the absence of a C or C++ surface |
| `evidence` | no | Map of adoption-evidence paths relative to the repository root, for example `profile`, `waiver_log`, and `review_dispositions` |

A not-applicable entry without a `reason` is invalid. Omitting a governing standard is not a substitute for a not-applicable entry.

## Governed Project Scopes

`scopes` is an optional top-level list. It is defined by AES-003 under Catylist `docs/adr/ADR-0004-governed-project-scopes.md`. A manifest without `scopes` is unchanged.

Each scope entry provides:

| Field | Required | Meaning |
|---|---|---|
| `id` | yes | Stable scope identifier, unique within the manifest |
| `path` | yes | The top-level directory the scope owns, with a trailing `/` |
| `owner` | yes | The accountable owner of the scope |
| `class` | yes | The scope's primary class, from the same vocabulary as the repository's primary class, and no broader in authority than it |
| `lifecycle` | yes | The scope's lifecycle state, from the same vocabulary as the repository's lifecycle state |
| `standards` | yes | Standard applicability entries for the scope; evidence paths must be under `<path>docs/engineering/` |
| `dependencies` | no | Declared dependencies on other scopes; omitted or empty means none |

Each dependency entry provides:

| Field | Required | Meaning |
|---|---|---|
| `scope` | yes | The `id` of the scope depended on |
| `kind` | yes | The kind of dependency, for example `data`, `build`, or `runtime` |
| `interface` | no | The documented interface of the other scope that the dependency uses |
| `description` | no | A short explanation of the dependency |

Example (informative):

```yaml
scopes:
  - id: native-core
    path: native-core/
    owner: dlworrell
    class: experimental
    lifecycle: active
    standards:
      - id: AES-SEC-001
        applicability: applicable
        evidence:
          profile: native-core/docs/engineering/SECURE-C-CXX.md
          waiver_log: native-core/docs/engineering/AES-SEC-001-waivers.md
          review_dispositions: native-core/docs/engineering/AES-SEC-001-review-dispositions.json
  - id: analysis-scripts
    path: analysis-scripts/
    owner: dlworrell
    class: experimental
    lifecycle: bootstrap
    standards:
      - id: AES-SEC-001
        applicability: not-applicable
        reason: Python-only scope with no C or C++ surface.
    dependencies:
      - scope: native-core
        kind: data
        interface: native-core/docs/output-format.md
        description: Reads the result files that native-core produces.
```

A Catylist governance back-pointer is planned under dlworrell/Catylist#18 and is not defined here.
