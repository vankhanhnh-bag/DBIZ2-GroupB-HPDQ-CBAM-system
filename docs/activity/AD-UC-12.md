# Activity Diagram: AD-UC-12 Monitor, track, and export system audit logs

| Field | Value |
|---|---|
| AD ID | AD-UC-12 |
| Use case | UC-12 Monitor, track, and export system audit logs |
| Feature | F-04 Platform, Access & Operations |
| Primary actor (W0) | System Admin / Auditor |
| Event initiator in the Must flow | Any user performing a system-altering action |
| Lanes | User performing a system-altering action · EcoTrace system |
| Inputs (L3–L5) | Context Diagram; Use Case Diagram; W0 Table 1 and Table 4; W1 FR-04-04, FR-04-05 and FR-04-06; W4 Prioritization; Architecture v1.1; SD-UC-12; SD-UC-11 |
| Trigger | A user performs a system-altering action in an originating EcoTrace feature. |
| Preconditions | An originating EcoTrace feature performs a system-altering action attributable to a user. |
| Success end state | EcoTrace automatically records an immutable audit log entry containing the action, user ID, timestamp and payload. |
| Failure end state | Not specified by FR-04-04. Audit-storage failure and recovery behavior remain open. |
| FRs covered | FR-04-04 (Must only) |
| Excluded related FRs | FR-04-05 and FR-04-06 (Should) — retrieve and export formatted audit logs as CSV/PDF |
| Business rules applied | System-altering actions are automatically recorded as immutable audit log entries. W1's BE work item identifies action, user ID, timestamp and payload as captured data. |
| Version / status | v1.0 · Draft for review |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph ACT["User performing a system-altering action"]
    start@{ shape: sm-circ }
    a1("Perform a system-altering action in an originating EcoTrace feature")
  end
  subgraph SYS["EcoTrace system"]
    s1("Receive the system-altering action event (FR-04-04)")
    s2("Capture action, user ID, timestamp and payload (FR-04-04)")
    s3("Write an immutable audit log entry to the audit log database (FR-04-04)")
    o1["Audit log entry [Recorded; immutable]"]
    stop@{ shape: fr-circ }
  end
  start --> a1 --> s1 --> s2 --> s3 --> o1 --> stop
```

Reading notes:

- This is the **Must-only** activity for UC-12. FR-04-04 is Must; FR-04-05 and FR-04-06 are Should and are not included.
- W0 names System Admin / Auditor as the primary UC-12 actor. The Must trigger in FR-04-04 is broader: any user performing a system-altering action. The actor lane therefore shows the event initiator, not a user manually creating an audit record.
- The EcoTrace system automatically receives the event, captures the data specified by the FR-04-04 BE work item, and writes the immutable entry.
- The Audit Log Viewer (S-F04-02) is listed in the Screen List, but the current W1 Must statement does not explicitly define a read/search/display action. The viewer interaction is therefore not added as an activity without a clarified requirement.
- No decision, retry, rollback, or failed-event branch is modeled because FR-04-04 does not specify those behaviors.
- The diagram treats EcoTrace as a black box. The Event Listener and audit database shown as lifelines in SD-UC-12 are not separate activity swimlanes.

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| a1 | Actor action / trigger | Perform a system-altering action in an originating EcoTrace feature | FR-04-04 WHEN condition; originating use case |
| s1 | System action | Receive the system-altering action event | FR-04-04; Event Listener work item |
| s2 | System action | Capture action, user ID, timestamp and payload | FR-04-04 BE L3: Event Listener captures these fields |
| s3 | System action | Write an immutable audit log entry to the audit log database | FR-04-04 (Must); W0 Table 4: Compliance Reports & Audit Logs (ILF) |
| o1 | Object node | Audit log entry [Recorded; immutable] | Output of FR-04-04; "immutable" is a property, not a separate status enum |

## 3. Branches and acceptance tests

No decision nodes are included. FR-04-04 specifies automatic logging when a user performs a system-altering action, but it does not define alternate outcomes for failed storage or unsuccessful originating operations.

| Scenario | Given | When | Then |
|---|---|---|---|
| System-altering action recorded | A user performs an action that the originating EcoTrace feature classifies as system-altering | The originating feature emits the action event to the Access & audit service | EcoTrace captures the action, user ID, timestamp and payload, then records an immutable audit log entry (FR-04-04). |

## 4. Object states used

| Object | State shown | Notes |
|---|---|---|
| Audit log entry | Recorded | "Immutable" is an entry property, not a state transition. W1 does not define a full audit-entry status lifecycle. |

## 5. Screens touched

The Must activity automatically records a system-altering event; it does not require the System Admin / Auditor to manually create or edit an audit-log entry.

| Actor action / outcome | Screen ID |
|---|---|
| Sign in before opening the audit area (precondition only; UC-10, not part of the UC-12 recording activity) | `S-F00-01` — Sign In |
| Perform a system-altering action in the originating EcoTrace feature (trigger for the audit event) | Varies by originating use case; outside UC-12, so no single Screen ID applies |
| EcoTrace automatically records the immutable audit-log entry (FR-04-04, Must) | N/A — background system processing; no dedicated user screen |
| System Admin / Auditor opens the audit area to review recorded system activity (related viewer interaction; not modeled in the FR-04-04 Must recording flow) | `S-F04-02` — Audit Log Viewer |
| Export audit logs as CSV/PDF (FR-04-05 and FR-04-06, Should; excluded from this Must activity) | Not explicitly mapped to an export screen in the current Screen List. `S-F04-02` is the related audit area, but its mapping to the export FRs needs confirmation. |

Screen IDs above are taken from the Screen List and Screen Transition Diagram. The detailed `screen-spec-S-F04-02.md` is not present in the repository, so no additional viewer controls or export interactions are assumed.

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | Which operations are formally classified as "system-altering" (e.g. create, update, delete, submit, activate, role changes)? | Yes — for complete event coverage | Use the generic FR-04-04 event trigger; do not invent an approved event taxonomy. |
| 2 | Are only successfully completed changes logged, or must rejected/failed attempts also create audit events? | Yes — for complete audit semantics | Model only the generic system-altering action event; failed-attempt logging is not assumed. |
| 3 | What should happen if the Event Listener or audit database is unavailable: block the originating change, queue/retry the event, or allow the change with an alert? | Yes — for failure-path and integrity design | No recovery branch is assumed. |
| 4 | What are the audit-log retention period, immutability enforcement mechanism and privileged-access rules? | Yes — for detailed security/data design | Only the immutability property stated in FR-04-04 is shown. |
| 5 | The Screen List marks S-F04-02 (Audit Log Viewer) as Must and says System Admin/Auditor reviews system activity, but W1 FR-04-04 only requires automatic recording; FR-04-03 is traced to UC-11. Is read/search/view of stored logs an additional Must requirement, or should the viewer be deferred/reclassified? | Yes — for Screen List / W1 traceability sign-off | Do not add a viewer action until a matching FR and access rule are confirmed. |
| 6 | FR-04-05 and FR-04-06 both describe audit-log export and are Should. Are they intentionally separate L2 requirements (BE and FE), or should they be one L2 with two L3 work items? | No — for this Must diagram; yes for W1 cleanup | Keep both Should FRs outside this diagram pending team decision. |
| 7 | Does the originating feature emit an audit event only after the state change is committed, and is audit persistence synchronous or asynchronous? | Yes — for transaction and ordering design | SD-UC-11 shows the RBAC change before the audit event; do not generalize that transaction policy to all features without confirmation. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Mermaid syntax / render | Static syntax reviewed; not independently rendered on mermaid.live | Draft | 2026-11-16 |
| Every decision has guarded outgoing flows | N/A — no decision nodes | Draft | 2026-11-16 |
| No action has two incoming control flows | Pass — linear flow | Draft | 2026-11-16 |
| Every fork has a matching join | N/A — no fork/join | Draft | 2026-11-16 |
| Every system action cites an FR | Pass — s1, s2 and s3 trace to FR-04-04 | Draft | 2026-11-16 |
| Every Must FR of UC-12 appears in the diagram | Pass — FR-04-04 included; FR-04-05 and FR-04-06 are Should and excluded | Draft | 2026-11-16 |
| Lane names follow the Context Diagram / black-box convention | Pass — generic event initiator and one EcoTrace system lane; internal components are not activity lanes | Draft | 2026-11-16 |
| Related sequence diagram uses the same trigger, event fields and outcome | Pass against SD-UC-12 | Draft | 2026-11-16 |
| Screen List / W1 mismatch is recorded | Pass — S-F04-02 viewer scope is in Section 6 | Draft | 2026-11-16 |
| Peer review / AI critic | Not yet performed | — | — |

---

*Template and notation follow the team's AD-UC-10 / AD-UC-11 structure and Mermaid `swimlane-beta` convention. This document models only the FR-04-04 Must recording behavior.*
