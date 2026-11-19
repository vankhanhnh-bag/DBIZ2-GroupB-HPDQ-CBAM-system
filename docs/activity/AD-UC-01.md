# Activity Diagram: AD-UC-01 Submit supplier & raw material carbon data

> **EcoTrace — HPDQ Carbon Management & Reporting System.** This document expands the team's existing activity diagram into the DBIZ2 activity-diagram template. The Mermaid flow is preserved from the supplied file. 

| Field | Value |
|---|---|
| AD ID | AD-UC-01 |
| Use case | UC-01 Submit supplier & raw material carbon data |
| Feature | F-01 — Carbon Traceability & Product Carbon Footprint Management |
| Primary actor | Procurement Department |
| Lanes | Procurement Department · EcoTrace |
| Inputs (L3–L5) | Context Diagram; Use Case Diagram; SD-UC-01; W0 Table 1 and Table 2; W1 FR-01-01; W4 Prioritization; Architecture; Screen List; Usage Flow; Screen Transition; Screen Spec S-F01-02 |
| Trigger | The Procurement Department submits supplier delivery information. |
| Preconditions | The Procurement user is authenticated and authorized to access Supplier & Raw Material Carbon Data Entry. The shared sign-in/access gate is UC-10 (S-F00-01) with authorization under FR-04-03; these are preconditions, not steps in UC-01. |
| Success end state | EcoTrace has successfully stored the submitted supplier delivery record (supplier identity, raw material type, quantity, and declared carbon data), and the Procurement Department receives a success confirmation. |
| Failure end state | If storage fails, the user receives an unsuccessful-submission outcome; the flow ends without a confirmed saved record. Incomplete/invalid input is a **correction loop**, not a terminal failure. |
| FRs covered | FR-01-01 (Must; W1 maps this single L2 requirement to UC-01, with FE and BE work items) |
| Business rules applied | FR-01-01 requires capture of supplier identity, raw material type, quantity and supplier-declared carbon data. W1 BE work item requires validation and storage. The validation/save outcome scenarios are documented in SD-UC-01; no separate BR IDs are supplied in the current repository. |
| Version / status | v0.1 |
| Author | Huyen Khanh |

## 1. Diagram

Notation follows the DBIZ2 activity profile (`mermaid-activity-patterns.md`). The diagram uses `swimlane-beta`, requiring Mermaid 11.16 or later.

```mermaid
swimlane-beta TB
  subgraph ACT["Procurement Department"]
    start@{ shape: sm-circ }
    a2("Submit supplier delivery information")
    a4("Correct missing or invalid delivery information")
    a3("Review successful submission outcome")
    a5("Review unsuccessful submission outcome")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end
  subgraph SYS["EcoTrace"]
    m1{" "}
    d1{"Required delivery information complete and valid?"}
    s2("Return input errors and request correction (FR-01-01)")
    s1("Capture and store supplier identity, raw material type, quantity and declared carbon data (FR-01-01)")
    d2{"Delivery record stored successfully?"}
    s3("Report that submission was not saved (FR-01-01)")
    s4("Confirm successful submission (FR-01-01)")
  end
  start --> m1 --> a2 --> d1
  d1 -->|"[no]"| s2 --> a4 --> m1
  d1 -->|"[yes]"| s1 --> d2
  d2 -->|"[no]"| s3 --> a5 --> stop2
  d2 -->|"[yes]"| s4 --> a3 --> stop1
```

Reading notes:

- `m1` is a **merge** for the first submission and the retry after the Procurement Department corrects invalid information. It does **not** mean those flows must occur concurrently.
- `d1` checks the required delivery information before the storage action. The `[no]` branch returns validation errors and loops back through user correction; the `[yes]` branch proceeds to record storage.
- `d2` checks whether the record was stored. The `[no]` branch terminates with an unsuccessful outcome, while the `[yes]` branch confirms success. The source diagram does not include a retry after a storage failure.
- The diagram has no fork/join or explicit object-state node. It makes no claim about the record's complete lifecycle or duplicate-submission handling.

## 2. Node trace

Every EcoTrace action cites `FR-01-01` in the source. The decision nodes are traced to the same FR at the **activity level**; the exact validation/persistence clauses still need to be checked against W1.

| Node | Kind | Text | Traces to |
|---|---|---|---|
| `m1` | Merge | Initial submission / corrected resubmission | Flow control; not a requirement action |
| `d1` | Decision | Required delivery information complete and valid? | FR-01-01 (inferred from linked actions; validate against W1) |
| `s2` | Action | Return input errors and request correction | FR-01-01 |
| `s1` | Action | Capture and store supplier identity, raw material type, quantity and declared carbon data | FR-01-01 |
| `d2` | Decision | Delivery record stored successfully? | FR-01-01 (inferred from linked actions; validate against W1) |
| `s3` | Action | Report that submission was not saved | FR-01-01 |
| `s4` | Action | Confirm successful submission | FR-01-01 |

## 3. Branches and acceptance tests

One row for each outgoing decision branch; the `[yes]` branch of `d1` is separated from the two possible outcomes at `d2`. These are **proposed acceptance scenarios** derived from the supplied flow, not confirmed implemented tests.

| Branch | Given | When | Then |
|---|---|---|---|
| `d1 [no]` | A supplier delivery submission is missing a required field or includes an invalid value | Procurement submits the delivery information | EcoTrace returns input errors, asks for correction, and the Procurement Department can correct and resubmit; the storage action is not reached on this attempt. |
| `d1 [yes]` | All required delivery information is complete and valid | Procurement submits the delivery information | EcoTrace attempts to capture and store supplier identity, material type, quantity, and declared carbon data; the flow proceeds to `d2`. |
| `d2 [no]` | Valid information was submitted, but the record was not saved successfully | EcoTrace evaluates the storage outcome | EcoTrace reports that the submission was not saved; Procurement reviews the unsuccessful outcome and the flow ends. |
| `d2 [yes]` | Valid information was submitted and the delivery record was stored successfully | EcoTrace evaluates the storage outcome | EcoTrace confirms submission success; Procurement reviews the confirmation and the flow ends. |

## 4. Object states used

The supplied diagram does **not** show an explicit object node such as `DeliveryRecord [Stored]`. “Stored successfully” and “not saved” are **branch outcomes**, not approved enumeration values for a record's state field.

| Object | States that appear in this diagram | Full state list (all diagrams) |
|---|---|---|
| Supplier delivery record | No formal lifecycle status is defined; successful/unsuccessful save outcomes are shown | No approved record-status enumeration is specified in W1 or the available screen spec. Do not infer additional states. |

## 5. Screens touched

All actor actions and submission outcomes in this activity remain on **S-F01-02 — Supplier & Raw Material Carbon Data Entry**. The Screen List and the existing Screen Spec S-F01-02 both map this screen to UC-01 / FR-01-01.

| Procurement Department action / outcome | Screen ID |
|---|---|
| Enter supplier identity, raw material type, quantity and supplier-declared carbon data; submit the supplier delivery | S-F01-02 — Supplier & Raw Material Carbon Data Entry |
| Review validation feedback, correct missing/invalid information and resubmit | S-F01-02 — Validation Error state |
| Review unsuccessful storage outcome | S-F01-02 — Save Failure state |
| Review successful submission confirmation and captured-data summary | S-F01-02 — Success state |
| Sign in before opening the submission screen (precondition only; not part of UC-01 activity) | S-F00-01 — Sign in (UC-10) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Which delivery fields are mandatory, and what are the validation rules for quantities, material types and declared carbon data?] | Yes — for precise acceptance tests | No formal validation rules assumed beyond the `d1` check. |
| 2 | Should correction feedback and save-failure feedback be formalized as separate EARS requirements, or remain team-confirmed outcome scenarios? | Clarification for formal requirement coverage | W1 FR-01-01 states the data-capture SHALL and its BE work item says validate/store. SD-UC-01 records validation and save outcomes as team-confirmed What-if scenarios, not separate SHALL statements. The diagram retains them as modeled scenarios, not as additional FR claims. |
| 3 | [NEEDS CLARIFICATION: Can Procurement retry a save failure, and how are duplicate submissions prevented?] | No — for documenting the **current** flow; yes if retry/idempotency is a required scenario | Current diagram ends on storage failure; no retry or deduplication rule is assumed. |
| 4 | Are there separately numbered Business Rules (BR IDs) maintained outside the current repository? | No — for current AD completion | W0/W1, Architecture, Screen List, Usage Flow and Screen Spec provide the feature, actor, access precondition and screen mapping. No separate BR catalogue or BR IDs are present in the inspected source set; the diagram cites FR-01-01 instead. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live (Mermaid 11.16+) | Done | — | — |
| Every decision has guards on all outgoing flows | Pass (static): `d1` and `d2` each have `[yes]` / `[no]` | Draft review | 2026-11-05 |
| Every fork has a matching join | N/A — no fork nodes | Draft review | 2026-11-05 |
| Every system action cites an FR | Pass (static): 4 of 4 EcoTrace actions | Draft review | 2026-11-05 |
| Every FR of UC-01 appears in the diagram | Pass — W1 maps UC-01 to FR-01-01 only; both FE and BE work items sit under this L2 FR | Assistant (source cross-check) | 2026-11-19 |
| Lane names match the Context Diagram and Use Case Diagram | Pass — Procurement is the actor in W0/Use Case Diagram and Context Diagram; the Screen Spec uses the expanded label Procurement & Raw Materials Department | Assistant (source cross-check) | 2026-11-19 |
| Source reconciliation | Pass — Screen ID, actor alias, direct FR scope and sign-in precondition checked against W0/W1/Screen List/Usage Flow/Screen Spec; validation-rule and retry uncertainties remain in Section 6 | Assistant | 2026-11-19 |
