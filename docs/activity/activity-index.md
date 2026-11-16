# Activity Diagram Index: EcoTrace

One row per use case in the Use Case inventory. Every use case appears, even when it needs no
diagram, so a reader can see that the decision was made on purpose.

## Inputs check (Block 1: what Lesson 3 and Lesson 4 gave us)

| # | Finding | Source to fix | Action | Owner | Status |
|---|---|---|---|---|---|
| 1 | UC-01 is "Submit production & energy data" in W0, but "Submit supplier & raw material carbon data" in the Use Case Diagram, W1 and AD-UC-01 | W0 / Use Case Diagram | Confirm the approved UC-01 name and actor, then align the source documents | Not assigned | Open |
| 2 | `AD-UC-05.md` describes LabDesk no-show seats, not EcoTrace UC-05 "Perform Scope 3 assessment" | Activity Diagram | Replace the teaching example with an EcoTrace UC-05 activity if a diagram is required | Not assigned | Open |
| 3 | SD-UC-05 is listed in the Use Case Diagram, but the corresponding file is absent | Sequence Diagrams | Complete or formally account for the missing sequence diagram before final cross-check | Not assigned | Open |
| 4 | AD-UC-04 refers to proposed `FR-01-09`, while W1 currently stops at `FR-01-08` for F-01 | W1 / AD-UC-04 | Decide whether to add the requirement to W1 or align the AD to existing approved FRs | Not assigned | Open |
| 5 | The Screen List marks S-F04-02 (Audit Log Viewer) as Must for reviewing system activity, but W1 FR-04-04 only specifies automatic recording; FR-04-03 is traced to UC-11 | W1 / Screen List | Confirm whether audit-log viewing needs an explicit FR or whether the viewer should be deferred/reclassified | Not assigned | Open |

## Use cases

Rule used in this course: draw an activity diagram for every **Must** use case that has at least
one decision, more than one lane, or a time trigger. A use case that is a single display step gets
"not needed" and a one-line reason. A use case has at most one diagram, and one diagram never
covers two use cases.

| Use case | Name | Feature | Priority | Activity diagram | Status | Reason if not needed |
|---|---|---|---|---|---|---|
| UC-01 | Submit supplier & raw material carbon data | F-01 | Must | `AD-UC-01.md` | Present; review not stated | |
| UC-02 | Manage GHG methodology & emission factors | F-01 | Must | `AD-UC-02.md` | Present; review not stated | |
| UC-03 | Access batch carbon intensity | F-01 | Must | `AD-UC-03.md` | Present; Must-only FR-01-03; FR-01-04 Should excluded; review pending | |
| UC-04 | Calculate shipment-level PCF | F-01 | Must | `AD-UC-04.md` | Present; FR-01-09 unresolved | |
| UC-05 | Perform Scope 3 assessment | F-01 | Should | Not provided | Open | |
| UC-06 | View carbon traceability record | F-01 | Must | `AD-UC-06.md` | Draft; FR-01-07 Must slice only | |
| UC-07 | Estimate & analyze CBAM exposure for shipment | F-02 | Must | `AD-UC-07.md` | Draft | |
| UC-08 | Manage CBAM regulatory rules & parameters | F-02 | Must | `AD-UC-08.md` | Draft; review pending | |
| UC-09 | Compare and recommend green steel options | F-03 | Must | `AD-UC-09.md` | Present; review not stated | |
| UC-10 | Authenticate and sign in to the platform | F-04 | Must | `AD-UC-10.md` | Draft; source reconciliation pending | |
| UC-11 | Configure Role-Based Access Control (RBAC) and system authorization | F-04 | Must | `AD-UC-11.md` | Present; review not stated | |
| UC-12 | Monitor, track, and export system audit logs | F-04 | Must | `AD-UC-12.md` | Present; Must-only FR-04-04; FR-04-05/-06 Should excluded; viewer scope clarification pending | |

Object state lists collected from all diagrams (copy into the data model in DBIZ3):

| Object | States |
|---|---|
| GHG Methodology & Emission Factor | Created, Versioned |
| Batch carbon intensity result | Calculated (persistence not specified; see AD-UC-03 open question) |
| Shipment PCF | Calculated |
| Supplier / production / shipment traceability records | Created; linked in the traceability model |
| Traceability graph | Existing or initial graph, Updated (logical states) |
| Shipment Batch | Selected, Loaded |
| CBAM Exposure Report | Generated |
| CBAM regulatory rule set | Draft; Active / future-effective (exact status enum not finalized) |
| Comparison & Recommendation Results | Calculated, Ranked |
| Audit log entry | Recorded (immutable is a property, not a status enum) |
