# Activity Diagram: AD-UC-06 View carbon traceability record

| Field | Value |
|---|---|
| AD ID | AD-UC-06 |
| Use case | UC-06 View carbon traceability record |
| Feature | F-01 Carbon Traceability & Product Carbon Footprint Management |
| Primary actor (W0) | Sustainability & ESG Department |
| Must-slice trigger source | A supplier, production, or shipment record is created in its originating process |
| Lanes | EcoTrace system (single black-box lane) |
| Inputs (L3–L5) | Context Diagram; Use Case Diagram; W0 Table 1 and Table 2; W1 FR-01-07 and FR-01-08; W4 Prioritization; Architecture v1.1; SD-UC-06 |
| Trigger | A newly created supplier, production, or shipment record is made available to EcoTrace for traceability linkage. |
| Preconditions | The new record can be identified, and its applicable lineage references can be resolved for the nominal linkage path. |
| Success end state | EcoTrace has linked the record to its applicable upstream raw-material sources and downstream shipped batches in the traceability model, maintaining the raw-material-to-shipment chain. |
| Failure end state | Not specified by FR-01-07. Missing or invalid lineage references and later reconciliation behavior remain open questions. |
| FRs covered | **FR-01-07 (Must only)** — Maintain carbon traceability linkage |
| Excluded related FR | **FR-01-08 (Should)** — User selects a shipped product batch and views the full carbon traceability record |
| Business rules applied | Whenever supplier, production, and shipment records are created, EcoTrace links each record to its upstream raw-material sources and downstream shipped batches to maintain an unbroken traceability chain (FR-01-07). |
| Version / status | v1.0 · Draft for review |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph SYS["EcoTrace system"]
    start@{ shape: sm-circ }
    s1("Receive a newly created supplier, production, or shipment record for linkage (FR-01-07)")
    d1{"Which type of record was created?"}
    s2("Identify supplier record and its raw-material source references (FR-01-07)")
    s3("Identify production batch and consumed raw-material references (FR-01-07)")
    s4("Identify shipment record and its shipped production-batch references (FR-01-07)")
    m1{" "}
    s5("Retrieve the created record and its referenced lineage records (FR-01-07)")
    s6("Resolve applicable upstream raw-material source relationships (FR-01-07)")
    s7("Resolve applicable downstream shipped-batch relationships (FR-01-07)")
    s8("Build or update supplier-to-material-to-production-to-shipment relationships (FR-01-07)")
    s9("Persist the applicable traceability links in the traceability model (FR-01-07)")
    o1["Traceability chain [Linked / updated]"]
    stop@{ shape: fr-circ }

    start --> s1 --> d1
    d1 -->|"[Supplier record]"| s2 --> m1
    d1 -->|"[Production record]"| s3 --> m1
    d1 -->|"[Shipment record]"| s4 --> m1
    m1 --> s5 --> s6 --> s7 --> s8 --> s9 --> o1 --> stop
  end
```

Reading notes:

- This diagram models the **Must-only background linkage behavior** in FR-01-07. It does not model the user-facing selection and display behavior in FR-01-08 (Should).
- W0 names Sustainability & ESG Department as UC-06's primary actor. However, the Must trigger is the creation of a supplier, production, or shipment record. No user swimlane is shown because record creation occurs in its originating process; this activity begins when the created record is made available to EcoTrace.
- The phrase "receive a newly created record" is an abstract business-level trigger. It does **not** prescribe event-driven messaging, polling, or a particular integration protocol.
- d1 separates the three record types so EcoTrace can identify the relevant lineage references. m1 is an explicit merge: exactly one of the three record-type paths continues into common lineage processing.
- The EcoTrace system is one black-box lane. Record ingestion, the linkage engine, operational record storage, and the traceability graph shown in SD-UC-06 are not separate activity swimlanes.
- The nominal path assumes the applicable lineage references can be resolved. FR-01-07 does not define what happens when references are missing, invalid, or point to records that have not yet been created; no error, retry, or reconciliation branch is invented.
- The result object represents a logically updated traceability chain. It does not prescribe a physically separate graph database or a specific storage topology.

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action / trigger | Receive a newly created supplier, production, or shipment record for linkage | FR-01-07 trigger condition |
| d1 | Decision | Which type of record was created? | FR-01-07: supplier, production, and shipment records |
| s2 | Action | Identify supplier record and its raw-material source references | FR-01-07 |
| s3 | Action | Identify production batch and consumed raw-material references | FR-01-07 |
| s4 | Action | Identify shipment record and its shipped production-batch references | FR-01-07 |
| m1 | Merge | Rejoin the three mutually exclusive record-type paths | Control flow; no separate requirement |
| s5 | Action | Retrieve the created record and its referenced lineage records | FR-01-07 |
| s6 | Action | Resolve applicable upstream raw-material source relationships | FR-01-07 |
| s7 | Action | Resolve applicable downstream shipped-batch relationships | FR-01-07 |
| s8 | Action | Build or update supplier-to-material-to-production-to-shipment relationships | FR-01-07 |
| s9 | Action | Persist the applicable traceability links in the traceability model | FR-01-07; supporting BE work item: raw-material-to-shipment traceability graph |
| o1 | Object node | Traceability chain [Linked / updated] | Logical output of FR-01-07; not a formal status enumeration |

## 3. Branches and acceptance tests

Each outgoing branch from d1 is a mutually exclusive record type. The scenarios are nominal-path acceptance tests based on FR-01-07; they do not define missing-reference behavior.

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [Supplier record] | A newly created supplier record and its applicable raw-material source references can be identified | The record is made available to EcoTrace | EcoTrace identifies the supplier/material references, resolves the applicable lineage records, and updates the traceability relationships. |
| d1 [Production record] | A newly created production record identifies a production batch and its consumed raw-material references | The record is made available to EcoTrace | EcoTrace links the production batch to its applicable consumed raw-material sources and the corresponding traceability chain. |
| d1 [Shipment record] | A newly created shipment record identifies the shipment and its shipped production-batch references | The record is made available to EcoTrace | EcoTrace links the shipment to its shipped production batch(es) and the corresponding upstream raw-material sources. |

## 4. Object states used

| Object | State before activity | State after activity | Notes |
|---|---|---|---|
| Supplier / production / shipment record | Created | Linked in the traceability model | "Linked" describes the logical relationship established by this activity, not an approved record-status enum. |
| Traceability graph / model | Existing or initial model | Updated with applicable nodes and relationship edges | Logical model state only; FR-01-07 does not prescribe a separate graph database. |
| Raw-material-to-shipment chain | Existing or not yet fully linked | Linked for the applicable resolved references | The treatment of records whose upstream/downstream references do not yet exist is an open question. |

## 5. Screens touched

The Must behavior is automatic background linkage and does not require a user to open a screen or manually create the traceability links.

| Screen / actor action | Status |
|---|---|
| S-F01-06 — Carbon Traceability Record | Related viewer screen in the Screen List; not an action in this Must-only activity because its select-and-display behavior is FR-01-08 (Should). The Screen List currently labels S-F01-06 as Must and maps it to both FR-01-07 and FR-01-08; see Open Question 6. |
| Select a shipped product batch and view its full carbon traceability record | Excluded from this diagram: FR-01-08 (Should) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | Which operational applications publish or expose supplier, production, and shipment record-created information to EcoTrace? What is the approved integration mechanism? | Yes — for detailed integration design | Use a technology-neutral created-record trigger; do not assume event messaging or polling. |
| 2 | What are the canonical identifiers and reference fields for supplier records, raw-material lots, production batches, and shipment records? | Yes — for precise linkage rules and acceptance tests | Refer to generic record IDs and lineage references only. |
| 3 | When an upstream record is created before its downstream production or shipment records exist, how should the chain be completed later? Is incremental linking sufficient, or is reconciliation/backfill required? | Yes — for lifecycle completeness | Do not invent a pending state, retry, or backfill process. |
| 4 | What should EcoTrace do when an expected upstream or downstream reference is missing, invalid, or inconsistent? | Yes — for exception-path completeness | No reject, retry, pending, or exception branch is modeled because FR-01-07 does not specify one. |
| 5 | Is the traceability graph a separate store, or a logical graph/data model within the existing EcoTrace database? | No — for this logical activity; yes for physical design | Describe a traceability model without prescribing a database topology. |
| 6 | The Screen List labels S-F01-06 (Carbon Traceability Record) as Must and maps it to FR-01-07 and FR-01-08, but W1/W4 classify FR-01-08 as Should and FR-01-07 describes background linkage. Should the screen priority be changed, or is there a separate Must-level viewing requirement missing from W1? | Yes — for Screen List / W1 traceability sign-off | Do not add a viewer action to this Must-only diagram until the requirement baseline is clarified. |
| 7 | The metadata table in SD-UC-06 currently lists FR-01-08, while its introductory description and sequence flow model the FR-01-07 background linkage behavior. Should the SD metadata be corrected to FR-01-07, with FR-01-08 noted as the related Should behavior? | Yes — for AD/SD traceability consistency | This update changes AD-UC-06 only; SD-UC-06 is not silently modified. |
| 8 | Should UC-06 be split into (a) background traceability-linkage maintenance and (b) user-facing traceability-record viewing, or should the current UC keep both slices with separate priorities? | No — for the current Must slice; yes for use-case baseline cleanup | Keep the W0 UC-06 name and actor, while documenting FR-01-07 and FR-01-08 as separate priority slices. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Mermaid syntax / render | Static syntax reviewed; not independently rendered on mermaid.live | Draft | 2026-11-16 |
| Every decision has guarded outgoing flows | Pass (static): d1 has three mutually exclusive record-type guards | Draft | 2026-11-16 |
| No action has multiple incoming control flows without a merge | Pass (static): m1 explicitly merges the three record-type paths before s5 | Draft | 2026-11-16 |
| Every fork has a matching join | N/A — no fork/join | Draft | 2026-11-16 |
| Every system action cites an FR | Pass — s1 to s9 cite FR-01-07; m1 is a control-flow merge | Draft | 2026-11-16 |
| Every Must FR of UC-06 appears in the diagram | Pass — FR-01-07 included; FR-01-08 is Should and excluded | Draft | 2026-11-16 |
| Lane names follow the black-box activity convention | Pass — one EcoTrace system lane; no user-create lane or internal UI/API/database lanes | Draft | 2026-11-16 |
| Related sequence diagram uses the same trigger, record types, and linkage outcome | Flow-level alignment pass; SD-UC-06 metadata FR mismatch is recorded in Section 6 | Draft | 2026-11-16 |
| Screen List / W1 priority mismatch is recorded | Pass — S-F01-06 scope and priority are recorded in Section 6 | Draft | 2026-11-16 |
| Peer review / AI critic | Not yet performed | — | — |

---

*Template and notation follow the team's AD-UC-03 / AD-UC-10 / AD-UC-12 structure and Mermaid swimlane-beta convention. This document models only the FR-01-07 Must linkage behavior.*
