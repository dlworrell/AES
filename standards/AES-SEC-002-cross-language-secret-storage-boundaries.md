# AES-SEC-002: Cross-Language, Secret, and Encrypted-Storage Boundaries

Status: Draft  
Owner: AES  
Scope: Catalyst applications that cross a memory-safe/native ABI, manage
secrets, or persist confidential local state

## Purpose

This standard defines security obligations at boundaries where language safety,
memory ownership, concurrency isolation, cryptographic keys, encrypted storage,
platform toolchains, and repository data meet.

It extends
[AES-SEC-001](AES-SEC-001-secure-c-cpp-coding-rules.md). It does not replace
that standard or make memory-unsafe code safe by declaration.

## Rule Levels

- MUST: violation blocks acceptance unless waived.
- SHOULD: expected default; deviation requires a recorded engineering reason.
- MAY: permitted option.
- BANNED: prohibited outside an explicitly reviewed exception.

## Required Local Profile

A repository in scope MUST document:

- the languages, ABI, target platforms, and supported toolchains;
- the confidential data and secret material being protected;
- every safe-language/native-code entry point;
- ownership, lifetime, mutability, thread-affinity, and callback rules;
- key generation, storage, use, recovery, and destruction behavior;
- encrypted database, journal, temporary-file, backup, and restore boundaries;
- platform hardening and artifact-verification evidence;
- sanitizer, analyzer, fuzzing, dependency, and repository-hygiene gates; and
- any deferred ABI migration or approved waiver.

## ABI and Ownership Boundary

A memory-safe language's guarantees stop at an unsafe foreign-function
interface.

- Production FFI calls MUST be confined to a small reviewed bridge rather than
  distributed through UI, business, or orchestration code.
- New native object APIs MUST prefer opaque handles with explicit
  constructor/destructor functions.
- An existing versioned ABI MAY retain an exposed layout temporarily only when
  compatibility is documented and an opaque-handle migration is assigned to a
  new interface version.
- Every pointer parameter MUST state ownership, nullability, mutability, valid
  length, lifetime, and thread-affinity.
- Long-lived ownership of a heap pointer MUST NOT be transferred implicitly
  across the ABI.
- Borrowed data MUST be copied into native value types before its documented
  lifetime ends.
- Callback contracts MUST state whether invocation is synchronous, on which
  thread or executor it occurs, and whether the callback or context is
  retained.
- Constructors and destructors MUST form an explicit lifecycle pair. Failure
  paths MUST leave ownership unambiguous.
- Unsafe pointers and FFI handles MUST NOT be treated as inherently
  thread-safe or `Sendable`.
- An unchecked concurrency conformance requires a documented synchronization
  invariant and focused tests.

## C and C++ Boundary

Native code MUST inherit AES-SEC-001.

- External buffers carry explicit lengths and are validated before use.
- Reserved feature-test macros belong to the build configuration, not
  project-owned source files.
- `strncpy` and `strncat` MUST NOT be described as general safe replacements
  for unbounded string functions.
- A native function MUST clear only sensitive mutable storage it owns.
  Overwriting a borrowed `const` caller buffer is BANNED.
- Source policy SHOULD mechanically reject prohibited APIs and unexpected
  unsafe-zone expansion.
- Public error text MUST be bounded and MUST NOT include secret bytes,
  credentials, personal records, or raw provider responses.

## Swift Boundary

Swift 6 repositories in scope MUST:

- compile in Swift 6 language mode;
- enable complete strict-concurrency checking;
- place mutable shared state behind an actor or global-actor boundary;
- use `Sendable` value types across concurrency boundaries;
- isolate `UnsafePointer`, `UnsafeMutablePointer`, raw-buffer, opaque-pointer,
  unmanaged-reference, and C-callback code in the reviewed bridge; and
- copy borrowed C strings and rows before returning from a synchronous
  callback.

Views and presentation code MUST NOT call the C ABI directly. A checked-in
source gate SHOULD enforce this boundary.

The name of an internal implementation type MUST NOT be cited as a public API.
For example, a project may require a secure-memory abstraction only when that
abstraction is actually public, supported on every target, and its guarantees
are documented.

## Secret and Key Lifecycle

- Projects MUST reuse established cryptographic implementations and platform
  key stores rather than invent cryptography.
- Keys MUST cross APIs as explicit byte buffers and lengths, not
  null-terminated strings.
- Keys, passwords, tokens, and recovery material MUST NOT appear in logs,
  errors, telemetry, environment variables, command-line arguments, process
  listings, or repository content.
- Interactive secret input SHOULD disable terminal echo or use a platform
  secure-input control.
- UI fields that contain secrets SHOULD be marked privacy-sensitive when the
  platform supports it.
- A secret buffer has one documented owner. That owner SHOULD reduce its
  lifetime and clear mutable project-owned bytes after use with the strongest
  supported optimization-resistant primitive.
- Secret clearing MUST be described accurately. It does not by itself prove
  that runtime copies, swap, crash capture, core dumps, or compiler-created
  temporaries cannot contain a copy.
- Key-store accessibility, device migration, synchronization, backup, and
  locked-device behavior MUST be explicit.
- A project MUST NOT claim hardware-backed or secure-enclave storage unless the
  actual key type, API, and target provide that property.
- Recovery keys and device keys SHOULD be distinct when portable recovery and
  device-local access have different trust boundaries.

## Encrypted Storage

Confidential database protection includes more than the main database file.

- The main file, journals, WAL, shared-memory state, temporary tables and
  indexes, backups, exports, staging files, and crash artifacts MUST be
  included in the storage threat model.
- Temporary database state MUST remain encrypted or in memory. A project using
  SQLite/SQLCipher SHOULD enforce this in both the dependency build and each
  runtime connection where supported.
- Encrypted open MUST key the connection before protected data access and MUST
  fail closed without plaintext fallback.
- Integrity and authentication checks MUST run before a restored or migrated
  database becomes active.
- Encrypted files MUST use exclusive, race-resistant creation and explicit
  least-privilege permissions.
- Restore and migration MUST use a private, protected application or service
  container. Shared groups and public document directories require a specific
  design reason.
- A restore source SHOULD be opened read-only, bounded by size, checked against
  an exact supported schema, and preserved on failure.
- Replacement of active state MUST define rollback, sidecar cleanup, and
  recovery behavior.
- Storage encryption MUST NOT be represented as erasure of plaintext blocks or
  removal of data already present in repository history.

## Platform Hardening

Projects MUST select controls that belong to each target format and linker.
Flags from one object format MUST NOT be copied blindly to another platform.

For ELF release executables where supported, the baseline SHOULD include:

```text
-fstack-protector-strong
-D_FORTIFY_SOURCE=<supported level>
-fPIE -pie
-Wl,-z,relro,-z,now
-Wl,-z,noexecstack
```

CI SHOULD inspect the resulting ELF and require the intended PIE,
non-executable-stack, RELRO/NOW, canary, and fortify evidence. Compiler flags
alone are not artifact evidence.

Apple application targets SHOULD use:

- Apple-Clang stack protection and platform PIE defaults;
- Hardened Runtime where supported;
- App Sandbox for macOS and Mac Catalyst where the application model permits;
- least-privilege entitlements; and
- security-scoped user-selected file access instead of broad filesystem
  entitlements.

Apple CI SHOULD inspect Mach-O flags/symbols and resolved build settings. ELF
`-z` options MUST NOT be sent to the Darwin linker.

Process sandboxes such as namespaces, seccomp, chroot, containers, or service
sandboxes are deployment controls. A risk assessment SHOULD select them for
networked, privileged, multi-user, or hostile-input services; they are not a
portable substitute for memory and input safety.

## Analysis and Dynamic Verification

Repositories in scope SHOULD enforce:

1. formatting and warnings-as-errors;
2. source-boundary and banned-API scans;
3. static analysis;
4. unit, integrity, wrong-key, and failure-path tests;
5. AddressSanitizer and UndefinedBehaviorSanitizer for native tests;
6. fuzzing for external parsers and decoders;
7. Swift complete-concurrency compilation;
8. Thread Sanitizer for concurrent Swift and supported native tests;
9. hardened release construction and binary inspection; and
10. secret/private-data scans over source and production artifacts.

Tool unavailability on one target is not permission to disable a supported
gate on another target. Evidence MUST distinguish local, hosted, simulated,
signed-device, and production verification.

## Dependencies

- Security-sensitive dependencies MUST use immutable or exact versions with
  recorded provenance.
- CI MUST detect drift between platform-specific declarations of the same
  dependency.
- Known-vulnerability review MUST be automated where the repository host and
  package ecosystem support it.
- When automated dependency review is unavailable, the repository MUST record
  a repeatable advisory-review process before changing or releasing a pin.
- Third-party warnings MUST NOT weaken checks for project-owned code.

## Repository Data and History

- Repositories MUST classify personal catalogues, databases, screenshots,
  provider responses, exports, keys, certificates, signing profiles, and
  derived caches.
- Ignore rules and a tracked-file gate MUST prevent new confidential storage or
  secret material from entering ordinary source paths.
- Tests and CI MUST use synthetic data unless a separately approved private
  environment is explicitly required.
- Production builds SHOULD succeed with historical/private source directories
  physically absent.
- Production artifacts SHOULD be scanned for private symbols and stable data
  markers.

Rewriting Git history is a destructive migration, not a routine lint fix. It
requires:

- a verified external encrypted backup;
- a successful restore rehearsal;
- an exact reviewed target list;
- collaborator and deployment coordination;
- branch-protection and mirror planning;
- rollback instructions; and
- explicit authorization immediately before refs are rewritten.

Private repository visibility reduces exposure but does not remove this
requirement.

## Evidence

An adopting repository MUST record:

- the implementation commit;
- local and hosted tool results;
- target toolchain and dependency versions;
- artifact-inspection results;
- untested targets and compensating controls;
- deferred ABI or recovery work;
- explicit non-claims about hardware storage, secret erasure, and Git history;
  and
- links to the local threat model, operations runbook, and waivers.

## AEMS Enforcement Direction

AEMS SHOULD ratchet this draft in reporting mode before blocking:

1. detect a local cross-language/storage profile;
2. inventory FFI bridge files and direct-call escape paths;
3. report strict-concurrency and sanitizer settings;
4. report encrypted temp-store, dependency-pin, and repository-hygiene
   evidence;
5. inspect platform release-verification evidence;
6. baseline gaps; and
7. block new violations after the standard is adopted and repositories have a
   migration window.

AEMS owns detector implementation. AES owns these obligations. Catylist owns
the authority relationship between them.

## Waivers

A waiver is required for every MUST or BANNED violation. It MUST identify the
rule, repository and symbols, necessity, safety invariant, evidence, owner,
approval date, review date, removal condition, and affected release scope.

## References

- [Swift 6 data-race safety](https://www.swift.org/migration/documentation/swift-6-concurrency-migration-guide/enabledataracesafety/)
- [Apple Keychain accessibility](https://developer.apple.com/documentation/security/ksecattraccessiblewhenunlockedthisdeviceonly)
- [Apple App Sandbox](https://developer.apple.com/documentation/xcode/configuring-the-macos-app-sandbox)
- [Apple sanitizer diagnostics](https://developer.apple.com/documentation/xcode/diagnosing-memory-thread-and-crash-issues-early)
- [SQLite temporary files](https://sqlite.org/tempfiles.html)
- [SQLite `temp_store`](https://sqlite.org/pragma.html#pragma_temp_store)
- [SQLCipher design](https://www.zetetic.net/sqlcipher/design/)
- [Clang AddressSanitizer](https://clang.llvm.org/docs/AddressSanitizer.html)
- [Clang UndefinedBehaviorSanitizer](https://clang.llvm.org/docs/UndefinedBehaviorSanitizer.html)
- [GNU linker options](https://sourceware.org/binutils/docs/ld/Options.html)
- [GitHub dependency review](https://docs.github.com/code-security/supply-chain-security/understanding-your-software-supply-chain/about-dependency-review)

## Review Cadence

This standard SHOULD be reviewed when the ecosystem adds a language/ABI,
cryptographic provider, encrypted-store format, secret-recovery mechanism,
target object format, platform sandbox, compiler, sanitizer, or repository
data migration.
