# AES-BLD-001: Native Build, Toolchain, and Distribution Parity

Status: Draft
Version: 0.1.0
Owner: AES
Authority: Catylist `docs/adr/ADR-003-authority-chain.md`
Implemented by: AEMS
Scope: Project-owned repositories that contain, build, or are expected to contain C or C++ code

## Purpose

This standard establishes one authoritative native-development contract while
preserving an independent GNU portability and distribution path.

The ecosystem uses:

- CMake and CTest as the canonical developer build and test-registration
  frontend;
- Clang/LLVM as the canonical diagnostics, compilation-database,
  static-analysis, and sanitizer evidence toolchain;
- GCC with GNU Autoconf, Automake, Libtool, and GNU Make as a first-class
  portability and source-distribution path.

Authority does not mean that one frontend generates or invokes the other.
CMake and GNU Autotools MUST remain independently usable. Their agreement is
established by an explicit parity gate.

## Requirement Levels

- MUST: required for conformance unless an approved waiver applies.
- SHOULD: expected default; deviations require a documented engineering reason.
- MAY: optional practice.

## Terms

### Canonical developer path

The CMake, CTest, and Clang path used for day-to-day development, compilation
database generation, static analysis, and sanitizer evidence.

### GNU portability path

The Autoconf, Automake, Libtool, GNU Make, and GCC path used to prove that the
source tree is not coupled to CMake or Clang and can produce a portable source
distribution.

### Build parity

Agreement between build frontends about production sources, generated
configuration, public compile definitions, feature switches, and target
semantics.

### Test parity

Registration and execution of the same normative test programs and fixtures
through CTest and Automake's test harness.

### Install parity

Equivalent runtime and development payloads when each frontend installs into
an empty staging prefix. Byte-identical compiled binaries are not required.

### Toolchain profile

A repository-local, version-controlled declaration of supported language
standards, build frontends, compilers, tool version policy, feature mappings,
install expectations, and approved exceptions.

## Applicability

Repositories with active C or C++ code MUST conform.

Project-owned repositories in which native code is planned MUST carry a
pre-adoption marker and MUST adopt the current AEMS template before the first
native production source is merged.

Documentation-only, governance-only, hardware-only, and third-party reference
repositories MAY declare the standard not applicable. A repository that builds
host utilities, generators, firmware support tools, or native extensions is
not documentation-only for this purpose.

Vendored and third-party source MUST be excluded from project warning,
clang-tidy, and parity ownership unless the repository explicitly maintains
that source as a project-owned fork.

## Authority Boundary

### AES-BLD-001-R001: Normative authority

AES owns the requirements in this document. AEMS MAY implement scanners,
reusable workflows, evidence schemas, and rollout reporting, but MUST NOT
silently strengthen or weaken these requirements.

### AES-BLD-001-R002: Repository implementation

Each applicable repository MUST own its source lists, target definitions,
tests, installation rules, package metadata, and local toolchain profile.

### AES-BLD-001-R003: Independent frontends

CMake MUST NOT invoke Autoconf, Automake, Libtool, or a generated `configure`
script to perform its build. The GNU path MUST NOT invoke CMake to perform its
build. Shared scripts MAY generate common source metadata, version data, or
test fixtures only when they do not delegate one frontend to the other.

### AES-BLD-001-R004: No hidden third build

CI wrapper scripts MAY orchestrate commands but MUST NOT contain a separate,
unreported compilation or installation implementation that bypasses both
frontends.

## Repository Toolchain Profile

### AES-BLD-001-R010: Required profile

Each applicable repository MUST maintain
`docs/engineering/AES-BLD-001-toolchain-profile.md`.

The profile MUST declare:

- supported C and C++ language standards;
- canonical CMake and Clang commands;
- GNU bootstrap, configure, build, test, install, and distribution commands;
- supported compiler families and minimum-version policy;
- the Clang and clang-tidy version relationship;
- target and feature-option mappings between frontends;
- install payload and package metadata expectations;
- cross-compilation and test-execution limitations;
- declared parity exclusions;
- waiver locations and owners.

### AES-BLD-001-R011: Version policy

The repository MUST declare its minimum CMake version in `CMakeLists.txt` and
`CMakePresets.json`. It MUST record the exact CMake, Clang, clang-tidy, GCC,
Autoconf, Automake, Libtool, and GNU Make versions used for compliance runs.

AEMS reference workflows MUST pin or otherwise resolve reproducible major tool
versions. Floating unrecorded tool versions are not acceptable evidence.

The initial distributed profile SHOULD support CMake 3.20 or newer. A
repository MAY require a newer version when a used feature requires it.

## Canonical CMake and Clang Path

### AES-BLD-001-R020: CMake project

An applicable repository MUST provide a root `CMakeLists.txt` that:

- declares project languages and language-standard requirements explicitly;
- supports out-of-source builds;
- expresses production targets and their usage requirements with
  target-scoped commands;
- does not inject project warnings into third-party targets;
- provides install rules for release artifacts;
- does not rely on undeclared developer-machine state.

### AES-BLD-001-R021: Presets

The repository MUST check in `CMakePresets.json` and MUST ignore
`CMakeUserPresets.json`.

The checked-in presets MUST provide stable configure, build, and test entry
points for:

- a Clang developer build;
- a GCC portability build;
- a Clang sanitizer build where supported.

Preset names MAY vary when the local toolchain profile maps them explicitly.
CTest presets MUST treat an unexpectedly empty test set as an error.

### AES-BLD-001-R022: CTest

Normative tests MUST be registered with CTest. The CMake path MUST provide one
documented command sequence that configures, builds, and runs all normative
tests from a clean out-of-source directory.

### AES-BLD-001-R023: Compilation database

The canonical Clang configuration MUST enable
`CMAKE_EXPORT_COMPILE_COMMANDS`. The resulting `compile_commands.json` MUST be
the authoritative input for clang-tidy and other compile-command-aware
analysis. It is generated evidence and MUST NOT be committed.

### AES-BLD-001-R024: Clang diagnostics

Project-owned code MUST compile with the repository's AES-SEC-001 warning
profile under Clang. CI MUST treat enabled project-owned warnings as errors.

The compliance run MUST execute clang-tidy with the checked-in `.clang-tidy`
configuration against the canonical compilation database. The Clang compiler
and clang-tidy SHOULD use the same LLVM major version.

### AES-BLD-001-R025: Sanitizers

Where the host and target support them, the canonical Clang path MUST build and
run normative tests with AddressSanitizer and UndefinedBehaviorSanitizer.
Unsupported targets MUST provide host tests, emulator evidence, or an approved
waiver with compensating checks.

### AES-BLD-001-R026: CMake installation

The CMake path MUST install into a relocatable staging prefix using
`cmake --install`. Install destinations MUST be relative to the selected
prefix and SHOULD use `GNUInstallDirs`.

Library repositories MUST install public headers, libraries, version metadata,
and a downstream-consumable package description. A `pkg-config` file is
required on platforms where `pkg-config` is supported. A CMake package
configuration SHOULD also be installed.

## GNU Autotools and GNU Make Path

### AES-BLD-001-R030: Autotools source

An applicable repository MUST provide:

- `configure.ac`;
- one or more Automake input files;
- an `m4/` directory when local macros are required;
- a `build-aux/` directory or configured auxiliary-directory equivalent;
- Libtool initialization when library targets require portable library
  construction.

Generated cache directories and generated Makefiles MUST NOT be committed.
Generated `configure` and supporting release files MAY be absent from normal
development branches, but MUST be present in source distributions intended for
users who should not need Autotools installed.

### AES-BLD-001-R031: Bootstrap and out-of-tree build

The development-tree GNU path MUST bootstrap with `autoreconf -fvi` or the
repository-documented equivalent. It MUST support an out-of-tree configure and
build such as:

```sh
mkdir build-autotools
cd build-autotools
../configure
make
make check
```

In-source build success does not replace this requirement.

### AES-BLD-001-R032: GNU compiler path

The GNU portability path MUST build and test with GCC and GNU Make. It MUST use
the same required language standard and an equivalent project-owned warning
policy as the CMake path.

### AES-BLD-001-R033: Compiler interchange

The GNU path MUST also configure, build, and test with `CC=clang` and, where
C++ applies, `CXX=clang++`. The CMake path MUST configure, build, and test with
GCC. This cross-matrix prevents build-frontend and compiler-family assumptions
from being conflated.

### AES-BLD-001-R034: Automake tests

Normative test executables and fixtures MUST be represented in Automake's test
harness and executed by `make check`. A test MAY have frontend-specific
launcher code, but its production behavior, assertions, fixtures, and pass/fail
criteria MUST remain equivalent to the CTest registration.

### AES-BLD-001-R035: Installation and uninstall

The GNU path MUST install into an empty staging root using `DESTDIR`, and
library repositories MUST provide the same public runtime and development
payload required of the CMake path.

`make uninstall` MUST remove files installed by the GNU path from the staging
root without removing unrelated files.

### AES-BLD-001-R036: Source distribution

Release-capable repositories MUST pass `make distcheck`. The resulting source
archive MUST build and test in a separate directory and MUST include the files
needed by a user to run `./configure`, `make`, and `make check` without first
installing Autoconf, Automake, or Libtool.

Repositories that never publish source distributions MAY waive only the
archive-production portion. They remain subject to GNU build, test, install,
and parity checks.

## Parity Contract

### AES-BLD-001-R040: Production-source parity

Both frontends MUST compile the same project-owned production sources for the
same logical target. Frontend-only adapters MUST be declared and MUST NOT
contain production behavior.

### AES-BLD-001-R041: Target and option parity

The local toolchain profile MUST map logical targets and user-visible feature
options between CMake and Autotools. Defaults that affect public behavior,
security, ABI, optional dependencies, or installed artifacts MUST agree.

### AES-BLD-001-R042: Test parity

Both frontends MUST execute the same normative test inventory. CI MUST fail
when a normative test is registered in only one frontend unless the local
profile records an approved platform or frontend-specific exclusion.

### AES-BLD-001-R043: Install-manifest parity

CI MUST install each frontend into a separate empty staging root and compare
normalized manifests.

The comparison MUST cover:

- installed path sets;
- public header contents;
- library and executable basenames;
- version and ABI-facing library metadata where supported;
- `pkg-config` package name, version, compile flags, and link flags;
- required licenses, notices, and public documentation;
- downstream consumer build and execution against each staged prefix.

Compiled artifacts are not required to be byte-identical. Frontend-specific
metadata, such as a CMake package file with no GNU analogue, MAY differ only
when declared in the local profile and when the common consumer contract
remains satisfied.

### AES-BLD-001-R044: ABI and symbol parity

Library repositories MUST compare exported public symbol sets and ABI-facing
library names or versions where the platform provides stable inspection tools.
Differences require a failure or explicit waiver.

### AES-BLD-001-R045: Consumer smoke tests

Library repositories MUST maintain a minimal consumer outside the library
target. CI MUST compile, link, and run that consumer against each staged
installation without using source-tree include or library paths.

## Required CI Evidence

### AES-BLD-001-R050: Required jobs

Applicable repositories MUST produce evidence for:

1. CMake, CTest, and Clang;
2. CMake, CTest, and GCC;
3. Autotools, GNU Make, and GCC;
4. Autotools, GNU Make, and Clang;
5. clang-tidy using the CMake compilation database;
6. Clang AddressSanitizer and UndefinedBehaviorSanitizer where supported;
7. staged install, consumer smoke, and parity comparison;
8. `make distcheck` for release-capable repositories.

AppleClang SHOULD be exercised on macOS for repositories claiming macOS
support. Cross-compiled targets MUST separately distinguish compile/link
evidence from executable test evidence.

### AES-BLD-001-R051: Evidence bundle

The compliance artifact MUST include:

- exact tool versions and runner identity;
- configure commands and selected options;
- CTest and Automake test results;
- clang-tidy and sanitizer results;
- both normalized install manifests;
- parity comparison output;
- consumer-smoke results;
- source-distribution result when applicable;
- active waivers and their expiry dates.

AEMS MUST define the machine-readable evidence schema. Human-readable logs
alone are insufficient once that schema is adopted.

## Reproducibility and Hygiene

### AES-BLD-001-R060: Clean-tree operation

Both build paths MUST work from a clean checkout without modifying tracked
files. Generated files MUST remain confined to ignored build, staging, or
bootstrap paths.

### AES-BLD-001-R061: Offline build boundary

After declared dependencies are installed or vendored, configure, build, test,
and install steps SHOULD be executable without network access. Any network
dependency MUST be explicit, checksummed or version-pinned, and documented in
the local profile.

### AES-BLD-001-R062: No developer-only success

Local success is not compliance evidence unless the same command sequence is
reproducible in CI or an equivalent controlled environment.

## AEMS Enforcement Mapping

AEMS SHALL implement this standard in stages:

| Stage | Enforcement |
|---|---|
| Inventory | Detect applicability, local profile, and declared build frontends |
| Structure | Validate required CMake, preset, Autotools, and profile files |
| Execution | Run the required compiler/frontend matrix |
| Parity | Compare tests, staged installations, package metadata, symbols, and consumers |
| Distribution | Execute and retain `make distcheck` evidence |
| Ecosystem | Aggregate adoption, failures, waivers, and evidence freshness |

AEMS checks MUST reference the requirement identifiers in this document.

## Waivers

A waiver MUST:

- identify the affected requirement;
- explain the technical constraint;
- name an owner and reviewer;
- record compensating validation;
- include an expiry or reassessment date;
- be stored in the governed repository and included in AEMS evidence.

Tool absence on a developer workstation is not a waiver for required CI
evidence. Unsupported sanitizer, symbol-inspection, or execution behavior on a
target platform may be waived when an alternate host, emulator, or static
validation path is documented.

## Guidance

- Keep source and test implementation shared; duplicate only frontend
  declarations.
- Prefer a small script that compares declared sources and tests over
  generating one build frontend from the other.
- Keep warning and sanitizer flags target-scoped.
- Use stable preset names so local commands and CI share the same interface.
- Treat `make distcheck` as a release-quality check, not a substitute for the
  canonical CMake/Clang path.
- Run parity on every pull request once a repository reaches enforcement
  status; reporting-only rollout is acceptable while legacy gaps are baselined.

## Rationale

CMake provides a strong cross-platform developer workflow and a compilation
database that integrates directly with Clang tooling. GNU Autotools remains a
valuable independent portability and source-distribution system, particularly
for Unix-like and GNU environments. Exercising both catches assumptions that a
single frontend or compiler family can hide.

The parity gate is the integration point. It compares observable contracts
rather than demanding identical build-system internals or byte-identical
binaries.

## References

- CMake Presets:
  <https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html>
- CMake compilation database:
  <https://cmake.org/cmake/help/latest/variable/CMAKE_EXPORT_COMPILE_COMMANDS.html>
- CTest:
  <https://cmake.org/cmake/help/latest/manual/ctest.1.html>
- CMake installation:
  <https://cmake.org/cmake/help/latest/command/install.html>
- Clang documentation: <https://clang.llvm.org/docs/>
- clang-tidy: <https://clang.llvm.org/extra/clang-tidy/>
- AddressSanitizer:
  <https://clang.llvm.org/docs/AddressSanitizer.html>
- UndefinedBehaviorSanitizer:
  <https://clang.llvm.org/docs/UndefinedBehaviorSanitizer.html>
- GNU Autoconf manual:
  <https://www.gnu.org/software/autoconf/manual/>
- GNU Automake manual:
  <https://www.gnu.org/software/automake/manual/>
- GNU Libtool manual:
  <https://www.gnu.org/software/libtool/manual/>
- GNU Make manual:
  <https://www.gnu.org/software/make/manual/>

## Related Documents

- `standards/AES-DEV-001-development-principles-and-check-in-discipline.md`
- `standards/AES-SEC-001-secure-c-cpp-coding-rules.md`
- `docs/architecture/normative-standards-model.md`

## Revision History

- 0.1.0: Initial CMake/Clang authority and GNU build-parity draft.
