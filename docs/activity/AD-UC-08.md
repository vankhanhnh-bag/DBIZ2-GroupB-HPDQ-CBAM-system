# Activity Diagram: AD-UC-08 Manage CBAM regulatory rules & parameters

| Field | Value |
|---|---|
| AD ID | `AD-UC-08` |
| Use case | UC-08 Manage CBAM regulatory rules & parameters |
| Feature | F-02 CBAM Compliance & Carbon Cost Estimation |
| Primary actor | Compliance / Product Owner |
| Lanes | Compliance / Product Owner · HPDQ Carbon Management & Reporting system |
| Inputs (L3–L5) | EcoTrace Context Diagram; Use Case Diagram; W0 UC-08; W1 FR-02-14 through FR-02-17; shared Must FR-04-03 and FR-04-04; `docs/use-case/SD-UC-08.md` (available; cross-diagram alignment still requires review). |
| Trigger | Authorized Compliance / Product Owner requests CBAM regulatory configuration. |
| Preconditions | User has an authenticated session. View permission is checked before disclosing protected settings; edit and activation permissions are checked on the respective protected actions. Existing rules can be retrieved. |
| Success end state(s) | Existing configuration is viewed without change; or draft is saved; or validated version is activated with an effective date and previous versions preserved. |
| Failure end state(s) | View, edit or activation access denied without making the unauthorized change; or activation blocked with validation reasons, leaving the draft unactivated. |
| FRs covered | UC-08 Must: **FR-02-14, FR-02-15, FR-02-16, FR-02-17 (4/4)**; shared Must: **FR-04-03, FR-04-04**. |
| Business rules applied | Mandatory values, valid ranges, nonconflicting effective periods (FR-02-16); effective-date versioning, prior-version traceability (FR-02-17). No separately numbered BRs provided. |
| Version / status | v0.3 — SD-UC-08 authorization alignment; peer review pending |
| Author | Huyền My |

## 1. Diagram

Below is the **short-label, separate-final** version. It preserves all Must logic. `swimlane-beta LR` places the partitions side by side; Mermaid may still place actions vertically inside each partition.

```mermaid
swimlane-beta TB
  subgraph ACT["Compliance / Product Owner"]
    start@{ shape: sm-circ }
    a1("Request CBAM rule settings")
    a2("Review rule configurations")
    d2{"Edit a rule set? (FR-02-15)"}
    a3("Edit parameters and sources")
    d3{"Request activation? (FR-02-17)"}
    a4("Review validation reasons")
    a5("Review activation result")
    endView@{ shape: fr-circ }
    endDraft@{ shape: fr-circ }
    endInvalid@{ shape: fr-circ }
    endSuccess@{ shape: fr-circ }
  end

  subgraph SYS["HPDQ Carbon Management & Reporting system"]
    s1("Check view permission (FR-04-03)")
    d1{"Authorized to view? (FR-04-03)"}
    endDenied@{ shape: fr-circ }
    s2("Show active and scheduled rules (FR-02-14)")
    se("Check edit permission (FR-04-03)")
    de{"Authorized to edit? (FR-04-03)"}
    sed("Show edit access denied")
    endEditDenied@{ shape: fr-circ }
    s3("Save rule-set draft (FR-02-15)")
    s4("Log draft change (FR-04-04)")
    sa("Check activation permission (FR-04-03)")
    da{"Authorized to activate? (FR-04-03)"}
    sad("Show activation access denied")
    endActivationDenied@{ shape: fr-circ }
    s5("Validate draft for activation (FR-02-16)")
    d4{"Draft valid? (FR-02-16)"}
    s6("Block activation; explain errors (FR-02-16)")
    s7("Activate versioned rules (FR-02-17)")
    s8("Use applicable version for new estimates (FR-02-17)")
    s9("Log activation (FR-04-04)")
  end

  start --> a1 --> s1 --> d1
  d1 -->|"[unauthorized]"| endDenied
  d1 -->|"[authorized]"| s2 --> a2 --> d2
  d2 -->|"[view only]"| endView
  d2 -->|"[create or edit]"| a3 --> se --> de
  de -->|"[unauthorized]"| sed --> endEditDenied
  de -->|"[authorized]"| s3 --> s4 --> d3
  d3 -->|"[keep draft]"| endDraft
  d3 -->|"[activate]"| sa --> da
  da -->|"[unauthorized]"| sad --> endActivationDenied
  da -->|"[authorized]"| s5 --> d4
  d4 -->|"[invalid]"| s6 --> a4 --> endInvalid
  d4 -->|"[valid]"| s7 --> s8 --> s9 --> a5 --> endSuccess
```

**Reading notes**

- Seven **separate activity finals** cover view-only, saved draft, validation blocked, activation successful, view denied, edit denied, and activation denied. These are alternative endpoints, not sequential actions.
- Six decisions have two mutually exclusive guards each: protected viewing (`d1`), view-or-maintain choice (`d2`), permission to edit (`de`), keep draft or request activation (`d3`), permission to activate (`da`), and activation validation (`d4`).
- The additional edit and activation checks align the activity with the existing action-level authorization branches in `SD-UC-08.md` (FR-04-03). **No separate RBAC permission-key names, roles or scopes are invented.**
- The user enters draft parameters before requesting Save Draft; the system checks edit permission before persisting. It checks activation permission before validating/activating. Versioned activation affects only estimates to which that version applies by its effective date.
- The specific point at which the Figma confirmation modal appears is an external-design decision; M-F02-03 remains an overlay on S-F02-03, not a new use case or a system service lane.

## 2. Node trace

| Node | Kind | Short label | Trace / full meaning |
|---|---|---|---|
| s1 | System action | Check view permission | FR-04-03: check access before displaying protected settings |
| d1 | Decision | Authorized to view? | FR-04-03: `[authorized]` / `[unauthorized]` |
| s2 | System action | Show active and scheduled rules | FR-02-14: display active/scheduled rule sets, parameters, effective dates and references |
| d2 | Actor choice | Edit a rule set? | FR-02-14 and FR-02-15: view-only or maintain a draft |
| se | System action | Check edit permission | FR-04-03: check permission before persisting a protected draft change; matches SD-UC-08 |
| de | Decision | Authorized to edit? | FR-04-03: `[authorized]` / `[unauthorized]` |
| sed | System action | Show edit access denied | FR-04-03: explain denied write without persisting a draft |
| s3 | System action | Save rule-set draft | FR-02-15: save changed parameters and references as draft |
| s4 | System action | Log draft change | FR-04-04: append immutable record of completed draft change |
| d3 | Actor choice | Request activation? | FR-02-15 and FR-02-17: keep draft or request activation |
| sa | System action | Check activation permission | FR-04-03: check permission before consequential activation; matches SD-UC-08 |
| da | Decision | Authorized to activate? | FR-04-03: `[authorized]` / `[unauthorized]` |
| sad | System action | Show activation access denied | FR-04-03: do not activate draft or claim success |
| s5 | System action | Validate draft for activation | FR-02-16: required values, allowed ranges, conflicting effective periods |
| d4 | Decision | Draft valid? | FR-02-16: `[valid]` / `[invalid]` |
| s6 | System action | Block activation; explain errors | FR-02-16: reject invalid activation and show reasons |
| s7 | System action | Activate versioned rules | FR-02-17: version with effective date and preserve history |
| s8 | System action | Use applicable version for new estimates | FR-02-17: select version applicable to later UC-07 estimates by effective date |
| s9 | System action | Log activation | FR-04-04: immutable record of completed activation |

## 3. Branches and acceptance tests

All values below are **test fixtures**, not actual regulatory thresholds or policy decisions.

| Branch | Given | When | Then |
|---|---|---|---|
| d1 `[unauthorized]` | U1 lacks view access to protected CBAM configuration | U1 opens Regulatory Settings | No protected configuration is disclosed; flow ends at `endDenied` |
| d1 `[authorized]` | U2 has view access | U2 opens Regulatory Settings | Active/scheduled configurations, values, dates and references are displayed |
| d2 `[view only]` | U2 has read access | U2 reviews the rules without editing | Configuration remains unchanged; flow ends at `endView` |
| d2 `[create or edit]` | U2 requests draft maintenance | U2 submits draft parameters/references | System checks edit permission before writing |
| de `[unauthorized]` | U2 can view but not edit | U2 requests Save Draft | Save blocked with access feedback; no draft change; flow ends at `endEditDenied` |
| de `[authorized]` | U3 can edit drafts | U3 submits a draft | Draft persisted and completed change audited; actor chooses whether to activate |
| d3 `[keep draft]` | Draft D-003 was saved (test fixture only) | U3 does not request activation | Draft stays unactivated; flow ends at `endDraft` |
| d3 `[activate]` | Draft D-003 was saved | U3 requests activation | System checks activation permission before validation |
| da `[unauthorized]` | U3 may edit but not activate | U3 requests activation | Activation denied with explanation; draft unchanged and unactivated; ends at `endActivationDenied` |
| da `[authorized]` | U4 has activation permission | U4 requests activation | System validates required values, ranges and effective-period conflicts |
| d4 `[invalid]` | D-003 has a missing value, invalid range or overlapping period | U4 requests activation | System blocks activation, shows reasons, preserves unactivated draft; ends at `endInvalid` |
| d4 `[valid]` | D-003 has valid values and nonconflicting sample effective date (e.g. 01/01/2027) | U4 requests activation | System versions rules, retains history, applies version by effective date and logs activation; ends at `endSuccess` |

*The illustrative U1–U4 and D-003 identifiers are test fixtures, not approved role/permission keys or actual regulatory records.*

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| CBAM regulatory rule set | Draft, Active / future-effective (business meaning) | **Not finalized** as an enum. W1 refers to `active` and `scheduled` rule sets; confirm the exact statuses and transitions before defining data-model values. |
| CBAM rule-set change audit | Immutable event recorded | Event creation, not a separately defined state; no state enum specified in W1. |

## 5. Screens touched

*Screen IDs taken from the Lesson 8 Screen List; detailed Screen Specifications are not part of this update.*

| Actor action | Screen ID |
|---|---|
| Request and review CBAM rule settings | `S-F02-02` (CBAM Regulatory Settings) |
| Edit parameters and source references | `S-F02-03` (CBAM Regulatory Rule Set Editor) |
| Request activation and review validation/activation outcome | `S-F02-03` (validation and `M-F02-03` confirmation modal); `S-F02-02` (result/status) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: Are view, edit and activation permissions distinct RBAC keys, and who receives each permission?] | Yes | Separate **checks** are shown as in SD-UC-08; no permission-key or role names are invented. |
| 2 | [NEEDS CLARIFICATION: Does future-date activation create a Scheduled status first, and what date/time determines applicability?] | Yes | Only FR-02-17 effective-date applicability is asserted. |
| 3 | [NEEDS CLARIFICATION: What exact CBAM parameter schema, mandatory fields, allowed ranges and effective-period conflict rules are enforced?] | Yes | Diagram checks only the failure categories named in FR-02-16. |
| 4 | [NEEDS CLARIFICATION: Should an invalid draft be corrected and activation retried in the same use-case invocation?] | No | Invalid activation ends; a new invocation may start. |
| 5 | [NEEDS CLARIFICATION: Should denied/failed authorization and activation attempts also generate audit logs?] | No | Only completed system-altering actions are logged (FR-04-04). |
| 6 | [NEEDS CLARIFICATION: Should Compliance / Product Owner be reconciled with the Internal Departments entity in Context Diagram?] | No | Existing UC-08 actor label is preserved. |
| 7 | [NEEDS CLARIFICATION: Is M-F02-03 displayed before or after permission/validation checks, and how are validation errors returned after confirmation?] | Yes for final screen interaction | Screen Transition §5 shows a confirmation overlay on S-F02-03; the activity remains service/order-neutral about modal mechanics, while action-level checks follow SD-UC-08. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Mermaid Live render (11.16+) | **Pending manual render** (syntax checked statically) | Team | — |
| All decision guards exhaustive and disjoint | Static pass: 6 decisions, each 2 guarded outcomes | Assistant | 2026-10-26 |
| No actions with multiple incoming control flows | Static pass: separate final nodes eliminate long back-routing | Assistant | 2026-10-26 |
| Every fork has a matching join | N/A (no fork) | Assistant | 2026-10-26 |
| All system actions cite an existing Must FR | Static pass: 13 of 13 system actions | Assistant | 2026-10-26 |
| Every UC-08 Must FR appears | **4 of 4** (FR-02-14 through FR-02-17), plus shared FR-04-03 and FR-04-04 | Assistant | 2026-10-26 |
| Lane names match known Context/use-case actors | Static pass; role-label ambiguity recorded in Q6 | Assistant | 2026-10-26 |
| Sequence cross-check | Aligned: view, edit and activation checks now appear in both AD and SD; exact permission keys remain open | Static alignment; peer review pending | 2026-11-17 |
| Static critic | Rechecked 6 decisions / 12 guards and independent denial outcomes; peer review pending | Assistant | 2026-11-17 |
| Peer review | Pending | Assigned peer team | — |

---

Source: DBIZ2 Lesson 7 template and course activity profile. **Revised UC-08 only; aligned authorization decisions with SD-UC-08 without adding non-Must requirements or new permission keys.**
