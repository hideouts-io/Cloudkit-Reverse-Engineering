# Evidence and methods

## Collection and context

The principal synthesis derives from a read-oriented local CloudKit audit collected November 19, 2026 on one arm64 host running macOS 27.2 (26B123). The source report records Xcode 27.2 (27B52) and cktool 1.0.23001. The audit classified 241 primary operations and recorded 836 native invocations in an execution ledger. The README also draws on separate historical, reference-VM, binary-trace, and Buddy-validation investigations. The retained reports identified below are private source material, not public downloads; readers cannot independently verify all source claims from this publication alone.

## Source index

S1–S10 are relative to the retained comprehensive audit collection. S11–S17 identify separate research collections; the paths are relative to each named collection rather than the comprehensive audit.

| ID | Retained source | Role |
| --- | --- | --- |
| S1 | `REPORT.md` | Audit synthesis, scope, observations and limitations. |
| S2 | `02-local/schema-and-counts.json` | Local metadata table and row counts. |
| S3 | `02-local/container-and-endpoint-summary.json` | Cached container and configuration-field summary. |
| S4 | `03-signatures/service-map/summary.json` and `03-signatures/` | Current owner mapping, signature checks and entitlement survey. |
| S5 | `08-review/runtime-review.md`, `08-review/runtime-analysis.json`, and `08-review/operational-followup/operational-followup.md` | Selected log window, status-tool results, and bounded follow-up. |
| S6 | `07-history/prior-evidence-review.md` and `07-history/mounted-snapshot-supplement/summary.json` | Retained historical analysis and same-host snapshot comparison. |
| S7 | `02-local/independent-plist-parser-comparison.json` | Comparison of typed parser outputs. |
| S8 | `05-remote/access-result.json` | Remote administration access limitation. |
| S9 | `execution-ledger.json`, `MANIFEST.json`, `MANIFEST.verification.json` | Invocation ledger and evidence inventory and verification. |
| S10 | `06-research/endpoint-attribution.md` and `06-research/research.md` | Endpoint attribution and third-party research assessment. |
| S11 | Comparative audit, personal-run release `REPORT.md` | Historical cache comparison and compatibility-overlay trace. |
| S12 | Comparative audit, reference-VM release `REPORT.md` | Fresh signed-out, same-build guest and subject comparison. |
| S13 | Encryption binary trace, `cloudkit-generated-metadata-current-route-findings.md` and `layer-trace-findings-readable.md` | Installed-code metadata-companion constructor and downstream PCS decision. |
| S14 | Encryption binary trace, `three-service-native-findings-readable.md` | Service-specific option-construction traces. |
| S15 | Encryption binary trace, `public-source-five-case-comparison.md` | Bounded comparison with pinned Apple-published Security source. |
| S16 | Buddy validation, `FINDINGS.md` | Separate native daemon acquisition-creator validation experiment. |
| S17 | November 18 follow-up, `03-runtime/step3-findings.md` and `04-history/step4-findings.md` | Earlier runtime window and cache history. |

The related official references are [Apple's CloudKit design guide](https://developer.apple.com/icloud/cloudkit/designing/), [Apple's enterprise network reference](https://support.apple.com/en-us/101555), and [Apple's cktool overview](https://developer.apple.com/icloud/ck-tool/). The local observations are not Apple statements, and the official documents do not independently confirm this host's state.

## Acquisition and verification limits

The primary metadata analysis used SQLite's online backup API with the source in read-only/query-only mode. A separate sequential copy of live database, WAL, and SHM files is non-atomic and was preserved with that qualification. Even read-only access can involve read locks or shared-memory bookkeeping; the audit did not claim zero filesystem footprint. Two complementary operational stores were logically backed up and passed SQLite checks, but their internal rows are not counts of remote records or transfers. [S1]

A second, reviewed parser was compared with Python's plist parser on typed output from the configuration blob. The report describes 12,617 input bytes and 1,500 objects and matching results. The comparison reduces the risk of one parser's interpretation being mistaken for the data; it does not establish that the configuration was used by live traffic or that it represents all possible configuration. Requested operating-system address/data memory limits were rejected, so an OS-enforced memory ceiling is not claimed. [S1, S7]

The manifest recorded 2,688 artifacts and approximately 148.7 MB, with hashes rechecked in the original audit. Local manifests were unsigned: agreement with recorded digests supports consistency with those lists, not independent authenticity, a pristine chain of custody, or a vendor baseline. [S1, S9]

Two status diagnostics timed out, and selected socket-inspection commands returned partial or empty output. Network and service visibility is incomplete. The absence of a printed listener or result is not proof that no listener or traffic existed. Access to remote administration was unavailable because a management token was missing and the user reported no developer access. [S1, S8]

## Publication and redaction

Only carefully selected aggregate counts, technical interpretations, product names, and public documentation links are included here. The repository excludes raw logs and database rows, account and device identifiers, push tokens, credentials, private local paths, detailed event timestamps, and raw diagnostic artifacts. Counts retain useful denominators and context, but readers cannot use this release to inspect individual records or independently reconstruct the complete dataset. Distinctive combinations of build and aggregate event characteristics could still be correlated by someone who already holds the same logs. The local source collection remains separate and is not part of these files or their chart metadata.

The figures show independently scoped populations; they should not be added together. A configuration-field occurrence is not a network request, a log message is not necessarily a retry, and a cached identity is not necessarily a unique process or remote account.
