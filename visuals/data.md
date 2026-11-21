# Chart data and definitions

The chart data is a selective, sanitized transcription of the original private audit. The chart populations differ and must not be combined into a common total. For source provenance and collection limits see [evidence and methods](../research/evidence-and-methods.md).

## Local metadata cache

[Figure](cache-tables.svg) · [CSV](cache-tables.csv) · Source S2

| Table | Rows |
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

These are rows in eight tables in a consistent local SQLite backup, not remote record or user counts. PushTokens is a table name; no token values are published. A zero count only describes this table in this backup.

## Application entitlement survey

[Figure](app-inventory.svg) · [CSV](app-inventory.csv) · Sources S1 and S4

| Measure | Count | Population |
| --- | ---: | --- |
| Bundles discovered | 563 | Discovered bundles |
| Entitlement extractions succeeded | 556 | 563 discovered bundles |
| Entitlement extractions failed | 7 | 563 discovered bundles |
| Relevant CloudKit/iCloud entitlement matches | 87 | 556 successful extractions |

The 87 matches are a subset of the successful extractions, not an additional population. Matching does not prove use of CloudKit, and extraction alone is not strict signature verification.

## Current owner mapping

[Figure](owner-mapping.svg) · [CSV](owner-mapping.csv) · Source S4

| Measure | Count | Population |
| --- | ---: | --- |
| Mapped to verified current owners | 46 | 58 cached base identifiers |
| Unresolved | 12 | 58 cached base identifiers |
| Explicit signed application-identifier binding | 45 | 46 mapped base identifiers |

The binding count is a subset of mapped identifiers. Unresolved identifiers are a coverage limit, not proof of an untrusted owner.

## Scope diagram

[Figure](coverage-boundary.svg) is a conceptual explanation of the audit boundary, not a measured network map. The audit did not enumerate remote CloudKit resources; local data cannot determine their presence or absence.
