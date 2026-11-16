# Sequence Diagram: SD-UC-12 Monitor, track, and export system audit logs

| Field | Value |
|---|---|
| Use case | **UC-12 Monitor, track, and export system audit logs** |
| Primary actor (W0) | System Admin / Auditor |
| Event initiator in the Must flow | Any user performing a system-altering action |
| Version | v1.0 |
| FRs covered | **FR-04-04 (Must)** — Record system audit logs |
| Related but excluded | FR-04-05 and FR-04-06 (Should) — Export audit logs |
| Trigger | A user performs a system-altering action in an originating EcoTrace feature. |

## Diagram

```mermaid
sequenceDiagram
    autonumber
    actor U as User performing a system-altering action
    participant F as Originating EcoTrace feature
    participant ACC as Access & audit service / Event listener
    database DB as EcoTrace audit log database

    U->>F: Perform a system-altering action
    F->>ACC: Emit system-altering action event (action, user ID, payload)
    ACC->>ACC: Capture action, user ID, timestamp and payload (FR-04-04)
    ACC->>DB: Store immutable audit log entry (FR-04-04)
    DB-->>ACC: Audit entry persisted
    Note over ACC,DB: Audit recording is automatic; no manual edit or deletion is part of this Must flow.
```

## Scope and reading notes

- This is the **Must-only** slice of UC-12. W4 confirms FR-04-04 as Must; FR-04-05 and FR-04-06 are Should and are intentionally excluded.
- W0 names **System Admin / Auditor** as UC-12's primary actor. The FR-04-04 trigger, however, is any user performing a system-altering action. That user initiates the source action; the Admin/Auditor is the intended audit-log stakeholder, not the person who manually creates each log entry.
- The originating EcoTrace feature emits the action event to the Access & audit service. The service's Event Listener captures the action, user ID, timestamp and payload in the audit log database, matching the FR-04-04 BE work item.
- UC-11 / FR-04-02 and FR-04-03 remain responsible for RBAC configuration and authorization. They are not redrawn as audit-log configuration steps here.
- This sequence models **recording** audit events only. It does not model opening the Audit Log Viewer, searching/filtering records, or exporting CSV/PDF files.
- W1 does not specify the complete event taxonomy, failed-operation logging policy, retention period, or recovery behavior if audit storage is unavailable. These are recorded as open questions rather than invented branches.

## Traceability

| Sequence step | Interaction | Traceability |
|---|---|---|
| 1 | A user performs a system-altering action in an originating feature | FR-04-04 trigger: "WHEN a user performs a system-altering action" |
| 2 | The originating feature emits an action event containing the action, user ID and payload | FR-04-04 BE L3: Event Listener captures action, user ID, timestamp and payload |
| 3 | The Access & audit service captures the event and timestamp | FR-04-04 (Must) and FR-04-04 BE L3 |
| 4 | The service stores an immutable audit log entry | FR-04-04 (Must); W0 Table 4: Compliance Reports & Audit Logs (ILF) |
| 5 | The database acknowledges persistence | Storage interaction supporting the FR-04-04 BE work item; no user-facing result is specified |

## Open questions

| # | Question | Impact |
|---|---|---|
| 1 | Which operations are formally classified as "system-altering" (create, update, delete, submit, activate, role changes, etc.)? | Define the event taxonomy and acceptance tests; W1 does not enumerate event types. |
| 2 | Are only successfully completed changes logged, or must rejected/failed attempts also create audit events? SD-UC-11 shows a successful RBAC change being logged; FR-04-04 does not state the failure policy. | Do not assume failed-attempt logging until confirmed. |
| 3 | What should happen if the Event Listener or audit database is unavailable: block the originating change, queue/retry the event, or allow the change with an alert? | Transaction, durability and recovery behavior are unspecified. |
| 4 | What are the audit-log retention, immutability enforcement, and privileged-access rules? | Required for detailed data/security design; not specified by FR-04-04. |
| 5 | The Screen List marks S-F04-02 (Audit Log Viewer) as Must and says System Admin/Auditor reviews system activity, while W1 FR-04-04 only requires automatic recording and FR-04-03 is traced to UC-11. Is read/search/view of stored logs an additional Must requirement, or should the viewer be deferred/reclassified? | Resolve the screen/W1 traceability gap before adding a viewer interaction to UC-12. |
| 6 | FR-04-05 and FR-04-06 both describe audit-log export and are Should. Are these intentionally separate L2 requirements (BE and FE), or should they be one L2 requirement with two L3 work items? | Function-list consistency; both export behaviors remain outside this Must-only sequence. |

*Source baseline: W0 Table 1 and Table 4; W1 FR-04-04 to FR-04-06; W4 Prioritization; Use Case Diagram; Architecture v1.1; SD-UC-11.*
