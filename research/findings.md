# CloudKit local audit: evidence and interpretation

The November 19, 2026 investigation examined a single Mac’s local CloudKit-related state. It combined a consistent SQLite metadata backup, scoped diagnostic commands, entitlement and signature checks, a five-minute log review, and comparisons with retained same-host evidence. It did **not** enumerate remote CloudKit records, prove account compromise, establish a vulnerability, or certify that all account activity was legitimate. [S1]

This is a sanitized research synthesis. The source labels identify the retained private materials; they are not links to public copies. Account data, raw logs, identifiers, access tokens and local personal paths are not part of this publication. The reported aggregates can be inspected here, but independent readers cannot verify the unpublished raw evidence from this repository alone.

## 1. What the local metadata cache contained

The consistent metadata database backup contained **8 tables and 312 rows**. Within it were **86 cached identity rows representing 58 distinct base identifiers**, and **70 distinct cached container identifiers**. The cache also had 55 container-server rows labeled Production. [S1, S2]

| Table | Local rows |
| --- | ---: |
| AppBundle | 86 |
| AppContainerAccount | 96 |
| ContainerServerInfo | 55 |
| DSIDMap | 1 |
| Properties | 3 |
| PushTokens | 70 |
| ServerConfiguration | 1 |
| VerifiedPublicKey | 0 |
| **Total** | **312** |

These rows are metadata about the inspected client state; they are **not** counts of remote records, remote users, uploads, or transfers. A table name does not establish that every row contains a usable value, and a zero-row table is not evidence that no key exists anywhere else. The cache is a partial local view, not a complete inventory of containers or databases in the account.

The main database passed an integrity check; two complementary operational stores passed quick checks. Those checks address database structure, not whether the contents are complete, authentic, or benign. The supplemental stores held 2 and 6,290 internal rows respectively; those are also local storage counts, not remote record totals. A read-only SQLite online backup provided the consistent metadata view. A separate sequential copy of live database files was non-atomic and was retained with that qualification. [S1]

## 2. Identity mapping and executable checks

The audit mapped **46 of 58 cached base identifiers** to verified current owners; **12 remained unresolved**. Of the 46 mappings, 45 had an explicit signed application-identifier binding. Separately, **48 mapped owner executables** passed Apple-anchor verification, as did all eight initially selected component checks. These sets overlap and must not be summed. [S1, S4]

An application scan found **563 bundles**: entitlement extraction succeeded for **556**, failed for **7**, and returned **87 CloudKit/iCloud-related matches**. A relevant entitlement describes a declared capability; it does not show that an app used CloudKit during this audit. Entitlement extraction is not equivalent to strict executable signature verification. The failed extractions and unresolved mappings leave real coverage gaps rather than evidence of suspicious activity by themselves. [S1, S4]

## 3. The unusual string-valued capability

In the 86 cached identity rows, the legacy `com.apple.private.cloudkit.masquerade` entry appeared as Boolean `true` in **59**, string `"true"` in **1**, and was absent in **26**. These are row counts, not counts of independently installed machines or independent signed owners. [S1, S6]

Retained implementation evidence identified a shipped compatibility overlay that constructs a string-valued dictionary and passes its output through JSON serialization. This gives a concrete implementation path capable of explaining why the legacy key is represented as a string in a cache even when it is absent from a helper’s signed entitlement dictionary. It does **not** identify the historical writer, execution branch, or transaction for the specific cached row, and it does not establish that the representation is a default on independent installations. Earlier unresolved-serializer language was superseded by the newer producer trace. [S6]

This is an example of why an unfamiliar representation alone is not proof of tampering: the representation, its producer, and the exact historical write are distinct questions. No independent, comparably exercised installation was available, so the denominator for independent recurrence is **zero**. [S6]

## 4. Configuration is not traffic

The audit inspected **279 HTTP(S) URL occurrences** in configuration and container URL fields. All used HTTPS, and the five distinct hostnames matched documented Apple iCloud domain families. Occurrences can repeat across fields; they are not unique requests. [S1, S10]

The initial suffix-based classification was too narrow and incorrectly flagged an Apple-documented domain; a later source review corrected it. A configured endpoint whose name references China does not prove that the Mac used that route or where data was physically stored. Configured URLs show available configuration, not network connections or completed data transfers. Apple describes the relevant service domains in its [enterprise network reference](https://support.apple.com/en-us/101555). [S10]

The configuration blob was byte-identical in the November 13 and November 18 same-host captures. An independent parser comparison processed a 12,617-byte blob containing 1,500 objects and agreed with Python’s plist parser on the typed result. The comparison improves confidence in that parsing result; it does not validate every policy’s semantics or establish server behavior. Requested OS address/data memory ceilings were rejected before parsing, so the audit did not claim those ceilings were enforced. [S1, S7]

Network visibility remained incomplete: two selected open-file/socket requests returned nonzero with partial output, and a separate listener listing reflected only what that particular point-in-time command exposed. No complete absence of network activity, listeners, or interception follows from those observations. [S1, S5]

## 5. Runtime observations and their limits

A five-minute log export contained **13,856 records**: 6,632 Default, 4,646 Info, 891 Debug, 97 Error, 1 Fault, and 1,589 without a message type. The untyped group included 1,588 activity-creation records and one metadata record. These are log records, which may include repeated emissions and differ in severity and meaning; they are not a count of independent failures. [S5]

The review highlighted several bounded groups:

- **50 push-registration failure messages** occurred in one daemon burst spanning 517 microseconds. The messages did not supply a sufficient reason to determine a network, authorization, or server cause. They do not prove 50 network requests or retries.
- **11 account-info cache-write failures** included directory-creation, missing-path and permission-related errors. A later path/ownership/ACL follow-up did not establish their historical cause.
- **10 account-store connection or lookup errors** described an invalidated service connection. Their wording alone does not establish that a process crashed or that data was lost.
- **1 background-task request Fault** was logged. Its severity alone does not establish compromise or data loss.

All **23 CKKS views** reported `ready`, with no non-null per-view status errors, and Octagon reported `Ready` at collection time. Twenty-one views reported managed synchronization and initial sync finished; two were non-managed and reported initial sync not finished while remaining ready. These are status responses, not a cross-device end-to-end transfer test or proof of account-wide health. [S5]

Of ten reviewed status invocations, **seven passed, two timed out, and one failed**. The failed team query lacked a management token; it did not demonstrate that teams or data were absent. The two timed-out diagnostics had incomplete output. [S5]

## 6. History and evidence quality

Two already-mounted, read-only recent snapshots of the same host supplied metadata families; their checks passed and their table counts and full configuration blob matched the current logical copy. Some stored representations differed, but the compared typed inner capability objects were semantically equal after JSON parsing. These snapshots are same-host historical observations, not independent clean-install baselines. No backup was newly mounted or restored for the comparison. [S1, S6]

Retained manifests matched **95/95**, **39/39**, and **707/707** files in three reviewed groups, and additional selected original/copy and command-stream checks matched. Those unsigned local lists support consistency with the recorded digests; they do not independently prove acquisition authenticity, authorship, or that the host was pristine before collection. [S1, S6]

## 7. Questions that remain open

1. **Unresolved ownership:** Can the 12 remaining base identifiers be mapped using independently verifiable ownership evidence, without treating a name alone as proof?
2. **Historical attribution:** Is there an earlier, independently timed backup and suitable writer-correlated evidence that identifies when and how the unusual representation was written? Current implementation evidence does not answer that question.
3. **Independent recurrence:** Does a separately initialized, comparably exercised installation of the same build show the same representation? A repeated snapshot of the original host is not an independent reference.
4. **Operational impact:** Do the cache-write, account-store or push-registration messages correlate with a persistent, user-visible failure? A reproducible symptom and appropriately scoped diagnostics would be needed to connect them.
5. **Remote state:** What exists in an authorized developer’s permitted CloudKit containers? No management token or developer access was available for this audit, so remote teams, schemas, zones, shares, records and metrics were not enumerated. An access failure is not evidence of absence.

Resolving these questions requires additional evidence or specific authorization. Repeating the same local capture cannot by itself answer the independent-reference or earlier-history questions. [S1, S6]


The source labels above refer to the [evidence index](evidence-and-methods.md).
