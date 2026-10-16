# AES-SEC-001: Secure C and C++ Coding Rules

Status: Adopted
Owner: AES
Scope: Catalyst, Atarix, AEMS, JAG, firmware, emulator, tooling, and native-code repositories

## Purpose

This standard defines the baseline secure-coding rules for C and C++ code in the Atarix Engineering Standard ecosystem.

The goal is not to make unsafe languages safe by wishful thinking. The goal is to make dangerous operations visible, reviewed, tested, and mechanically enforced wherever practical.

## Governing Principle

Native code must be written as if all external input is hostile until validated.

Memory safety, integer safety, lifetime safety, parser safety, and trust-boundary safety are engineering requirements, not optional style preferences.

## Rule Levels

The project secure-coding profile uses four levels:

- MUST: violation blocks merge unless waived.
- SHOULD: expected default; deviation requires an engineering reason.
- MAY: allowed practice or optional improvement.
- BANNED: prohibited outside explicitly approved low-level modules.

## Required Repository Behavior

Each repository that contains or may contain C or C++ code MUST inherit this standard.

Each such repository MUST maintain a local secure-coding profile that states:

- which native-code languages and standards are allowed;
- which compiler warnings are required;
- which sanitizers are run in tests;
- which static-analysis tools are used;
- where unsafe code is allowed;
- how rule exceptions are documented.

Repositories that do not currently contain native code SHOULD still include an adoption marker stating that any future C/C++ contribution must comply before acceptance.

### Governed Project Scopes

A repository MAY declare governed project scopes in its repository manifest, as defined by AES-003 and authorized by Catylist `docs/adr/ADR-0004-governed-project-scopes.md`. A repository that declares no scopes is governed by this section exactly as written above.

When a repository declares governed project scopes, the obligations of this section apply per governed unit:

- each declared scope that contains or may contain C or C++ code MUST inherit this standard for the files it owns;
- each such scope MUST maintain its own local secure-coding profile, stating every item listed above, under `<scope>/docs/engineering/`;
- each such scope MUST maintain its own waiver log, and its own review-disposition ledger where AES-SEC-001 findings are dispositioned, under `<scope>/docs/engineering/`;
- the repository-level remainder, meaning every governed file outside all declared scopes, remains subject to this section at repository level, with the repository-level profile, waiver log, and review-disposition ledger;
- a scope to which this standard does not apply MUST be declared not applicable in the manifest, with its reason, using the AES-003 not-applicable form. It MUST NOT be silently omitted, and the declaration MUST be revisited when the scope's languages or surfaces change.

Scope-level adoption does not weaken any requirement of this standard. A scope profile MAY be stricter than this standard or than the repository-level profile. It MUST NOT relax this standard.

A scope's profile, waiver log, and review-disposition ledger satisfy this section only for files owned by that scope, and only when AES-SEC-001 evaluation runs with that scope as its scan root. Until enforcement tooling supports scoped scanning, a repository that declares scopes MUST keep its repository-wide AES-SEC-001 evaluation and repository-level evidence, and scope-level evidence is supplementary.

## Banned Interfaces

The following interfaces are BANNED in ordinary application, firmware, parser, emulator, and tooling code:

```text
gets
strcpy
strcat
sprintf
vsprintf
scanf("%s", ...)
sscanf("%s", ...)
atoi
atol
atoll
tmpnam
mktemp
system
popen
```

Dangerous primitives are allowed only inside reviewed low-level wrappers or platform modules:

```text
memcpy
memmove
strncpy
snprintf
malloc
calloc
realloc
free
new
delete
reinterpret_cast
const_cast
```

Use of a dangerous primitive outside an approved wrapper requires a waiver.

## Buffer and String Rules

- Every buffer crossing a trust boundary MUST carry an explicit length.
- Every parser entry point MUST accept bounded input.
- Serialized length fields MUST be validated against the actual remaining input.
- Strings from external sources MUST NOT be trusted as valid, terminated, encoded, or semantically acceptable until checked.
- `strncpy` MUST NOT be used as a general-purpose safe string copy.
- Truncation from `snprintf` or equivalent APIs MUST be detected when truncation affects correctness or security.

Preferred C parser shape:

```c
int parse_packet(const uint8_t *data, size_t len);
```

Preferred C++ parser shapes:

```cpp
std::span<const std::byte>
std::string_view
```

## Integer and Size Rules

- Integer values used for allocation, indexing, copy lengths, object counts, offsets, or serialized sizes MUST be range-checked before use.
- Multiplication used to compute allocation sizes MUST be checked for overflow before allocation.
- Addition used to compute sizes, offsets, or pointer movement MUST be checked for overflow before use.
- Signed values MUST NOT be converted to unsigned sizes until non-negativity and upper bounds are proven.
- `int` SHOULD NOT be used for sizes, offsets, object counts, or allocation lengths.
- Decompressed, decoded, or expanded output MUST have explicit maximum bounds.

Required C allocation pattern:

```c
if (count > SIZE_MAX / sizeof(struct item)) {
    return ERR_TOO_LARGE;
}

size_t bytes = count * sizeof(struct item);
```

## C++ Ownership and Lifetime Rules

- Raw owning pointers are BANNED in ordinary C++ code.
- Direct `new` and `delete` are BANNED outside resource wrappers, allocators, and low-level containers.
- RAII MUST be used for memory, file handles, sockets, locks, descriptors, temporary state, and other owned resources.
- `std::unique_ptr` is the default dynamic ownership type.
- `std::shared_ptr` MAY be used only when shared ownership is real and documented by design.
- Non-owning pointers and references MUST NOT outlive the object they reference.
- References or pointers to local objects MUST NOT be returned.
- Async callbacks, threads, and lambdas MUST document captured-object lifetime when capturing by reference or raw pointer.
- Iterators, references, and pointers MUST be assumed invalid after container operations that can invalidate them.

Preferred C++ ownership hierarchy:

```text
Owned dynamic object        -> std::unique_ptr<T>
Shared ownership            -> std::shared_ptr<T>, only when truly shared
Non-owning nullable view     -> T*
Non-owning non-null view     -> T& or not_null<T*>
Dynamic contiguous storage   -> std::vector<T>
Fixed-size storage           -> std::array<T, N>
Byte or string view          -> std::span / std::string_view
```

## Unsafe Code Containment

Unsafe code MUST be isolated behind safe interfaces.

Approved unsafe zones may include:

```text
src/platform/
src/abi/
src/unsafe/
src/alloc/
src/hardware/
src/registers/
```

Unsafe modules MUST document their local invariants. A public function exported from an unsafe module SHOULD expose a safe interface rather than leaking raw lifetime, size, or aliasing requirements to callers.

## Trust-Boundary Rules

- Shell execution with untrusted input is BANNED.
- Path construction from untrusted input MUST enforce canonicalization and containment.
- Temporary-file creation MUST avoid predictable names and race-prone patterns.
- File, socket, and device permissions MUST be explicit where privilege or confidentiality is involved.
- TOCTOU-sensitive checks MUST be avoided or tied to stable handles such as file descriptors.
- Error returns from security-relevant calls MUST NOT be ignored.

## Parser and Fuzzing Rules

Every parser for external input SHOULD have a fuzz harness before it is considered security-complete.

Fuzz targets include:

- network packets;
- binary files;
- archives and compressed data;
- images, audio, video, fonts, and firmware blobs;
- emulator media formats;
- configuration files;
- command-line arguments;
- serialization and deserialization code.

A parser MUST fail safely on malformed input. It MUST NOT crash, hang, recurse without bound, allocate without bound, or read/write out of bounds.

## Required Tooling Baseline

C and C++ repositories SHOULD build with a warning profile equivalent to:

```text
-Wall
-Wextra
-Wpedantic
-Wconversion
-Wsign-conversion
-Wshadow
-Wformat=2
-Wformat-security
-Wnull-dereference
-Wdouble-promotion
-Wimplicit-fallthrough
```

C repositories SHOULD also consider:

```text
-Wstrict-prototypes
-Wmissing-prototypes
-Wold-style-definition
-Wvla
```

C++ repositories SHOULD also consider:

```text
-Wnon-virtual-dtor
-Wold-style-cast
-Woverloaded-virtual
-Wuseless-cast
-Wzero-as-null-pointer-constant
```

Test builds SHOULD run AddressSanitizer and UndefinedBehaviorSanitizer where the platform supports them:

```text
-fsanitize=address,undefined
-fno-omit-frame-pointer
```

ThreadSanitizer, MemorySanitizer, LeakSanitizer, static analysis, and fuzzing SHOULD be added as the repository matures.

## CI Enforcement

Native-code repositories SHOULD eventually enforce this sequence:

1. format check;
2. compiler warnings;
3. static analysis;
4. unit tests;
5. sanitizer tests;
6. fuzz smoke tests for parsers;
7. release build with platform hardening flags.

Existing repositories MAY use a ratchet strategy:

- measure existing violations;
- baseline legacy violations;
- block new violations;
- fix highest-risk legacy violations first;
- require waivers for remaining exceptions;
- remove waivers over time.

## Waiver Process

A waiver is required for any MUST or BANNED rule violation.

Each waiver MUST state:

- the rule being waived;
- the repository, the governed project scope when the affected file belongs to one, and the file path;
- why the exception exists;
- the invariant that makes the code safe enough;
- what tests, sanitizer runs, fuzzing, or review evidence covers it;
- the owner;
- the review date.

A waiver for a file owned by a governed project scope MUST be recorded in that scope's waiver log. A waiver recorded in one scope never covers files in another scope or in the repository-level remainder, and a repository-level waiver never covers files owned by a declared scope.

Waivers are not permanent design permission. They are tracked engineering debt.

## Minimum Local Profile

Each adopting repository, and each adopting governed project scope, SHOULD include this minimum local policy:

```text
This repository inherits AES-SEC-001.

No new C or C++ code may be merged unless it:
- avoids banned APIs;
- carries explicit lengths for external buffers;
- checks allocation and copy-size arithmetic;
- uses RAII/smart ownership in C++;
- isolates unsafe code behind reviewed interfaces;
- compiles cleanly under the repository warning profile;
- has sanitizer coverage when supported;
- has fuzz coverage for external parsers when applicable;
- records waivers for all approved exceptions.
```

## References

- SEI CERT C Coding Standard
- SEI CERT C++ Coding Standard
- MITRE CWE Top 25 Most Dangerous Software Weaknesses
- CISA/NSA memory-safety guidance

## Related Documents

- Catylist `docs/adr/ADR-0004-governed-project-scopes.md`
- `docs/standards/AES-003-repository-manifest-standard.md`
- `docs/standards/AES-003-manifest-fields.md`

## Review Cadence

This standard SHOULD be reviewed whenever the ecosystem adds a new native-code platform, compiler, sanitizer, target architecture, parser, emulator subsystem, firmware target, or release process.

## Revision History

- 2026-10-16: Scope-aware adoption. A declared governed project scope satisfies the profile, waiver, and review-disposition obligations for the files it owns; the repository-level remainder stays covered at repository level. Authority: Catylist ADR-0004.
