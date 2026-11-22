# Screen Spec: S-F04-02 Audit Log Viewer

> EcoTrace — HPDQ Carbon Management & Reporting System. Draft for team review.

| Field | Value |
|---|---|
| Screen ID | S-F04-02 |
| Screen name | Audit Log Viewer |
| Actor | System Admin; Auditor |
| Priority | Must |
| Feature | F-04 Platform, Access & Operations |
| Activity diagrams | AD-UC-12 (records the audit entry automatically; the viewer interaction is not modelled there, see open question 1) |
| Mockup images | `img/S-F04-02 — Default : Audit Log Viewer.png` (default), `img/S-F04-02 — Default : Audit Log Viewer - numbered.png`, one state image (section 2) |
| Frame width | 1440px (exported at 1×, 1440 pixels) |
| Figma frame | [01 — Done screens](https://www.figma.com/design/FTJibQtZrHIMHfwsJx7lT9/EcoTrace?node-id=8-73) |
| Status | Draft for team review |

## 1. Purpose

**Shown when:** a System Admin or Auditor opens **Platform / Audit Logs** after signing in and passing the authorization check, to review system activity that EcoTrace recorded as immutable audit events. A request from a user without permission must not expose any audit detail.

**The user leaves this screen when:** they return to the EcoTrace workspace, or (System Admin) go back to User & Access Management (S-F04-01).

## 2. Mockup

![S-F04-02 — Default](img/S-F04-02%20%E2%80%94%20Default%20%3A%20Audit%20Log%20Viewer.png)

**Numbered:** each number refers to the matching # in the element inventory in section 3. Both designed UI states are shown directly below. Elements 3–5 appear in the Default state, and elements 6–8 only in the Access Denied state.

| Default — numbered | Access Denied — numbered |
|---|---|
| ![S-F04-02 — Default numbered](img/S-F04-02%20%E2%80%94%20Default%20%3A%20Audit%20Log%20Viewer%20-%20numbered.png) | ![S-F04-02 — Access Denied numbered](img/S-F04-02%20%E2%80%94%20Access%20Denied%20-%20numbered.png) |

All colours, fonts and corners follow the shared design conventions in
[ui-conventions.md](../ui-conventions.md).

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | App shell (sidebar + top bar) | Global layout | EcoTrace navigation: Overview; F-04 Platform & Access with Audit Logs (active); Help & Support. Top bar shows the breadcrumb Platform / Audit Logs and the user's role (Auditor in the mockup). | Yes | Navigation is role-aware: the Auditor sees Audit Logs only; a System Admin also sees User & Access Management (UI Conventions — Navigation). |
| 2 | Page title and description | Header | "Audit Log Viewer — Review system-altering activity recorded as immutable audit events." | Yes | Describes what is shown; does not promise search or export. |
| 3 | Immutable audit entries notice | Information banner | "EcoTrace automatically records a user ID, action, timestamp and payload for system-altering actions." (FR-04-04) | Yes (Default state) | Static explanatory text. |
| 4 | Recorded system activity header with Read-only badge | Section header and status badge | "Recorded system activity"; "The table shows the fields defined by FR-04-04. Actual records come from the audit database." | Yes | The Read-only badge carries a text label, not colour alone. The screen offers no way to edit or delete an entry. |
| 5 | Audit event table | Data table | Columns ACTION, USER ID, TIMESTAMP (ICT) and PAYLOAD, with one row per audit entry from the audit log database (FR-04-04) | Yes, when entries exist | Show only returned values; the grey bars in the mockup are placeholders. Timestamps use dd/MM/yyyy HH:mm with ICT (UI Conventions §2). Never display passwords, OTPs, tokens or other secrets from a payload. |
| 6 | Access denied notice | Error banner | Result of the FR-04-03 check: "You do not have permission to view audit logs. Contact a System Admin." | Conditional | Says that access is unavailable and who can help (UI Conventions §4). |
| 7 | Audit records protected panel | Restricted view | "Audit records are protected" — no audit entry details are disclosed to unauthorized users | Conditional | Replaces elements 3–5. Renders no audit entry. Contains the Return to workspace button (element 8). |
| 8 | Return to workspace | Secondary button | Leaves the audit area | Conditional (Access Denied state) | Returns to the workspace without revealing audit data. |

The note "Design placeholders — no audit events, user IDs, dates or payload values have been invented" and the line "Do not expose passwords, OTPs, access tokens…" in the mockup are design annotations, not user-facing elements. Search, filter and export controls are not part of this design (they belong to the Should requirements FR-04-05 and FR-04-06).

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default — Audit Log Viewer | Page title, immutable-entries notice, and the Recorded system activity panel with a Read-only badge and the audit event table. | An authorized System Admin or Auditor opens Audit Logs. |
| Access Denied — Audit records protected | Access denied notice and the protected-records panel with Return to workspace. No audit entry is visible. | The FR-04-03 permission check fails. |

Loading, empty and system-error states are not designed yet (open question 6).

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Audit Logs navigation | Opens Audit Logs | Verifies the user's RBAC permission (FR-04-03) and loads the recorded audit entries from the audit log database (FR-04-04). | S-F04-02, or the Access Denied state |
| 2 | Audit event table | Reviews the entries | Shows the returned entries read-only. No write is performed and no entry can be changed. | Stays on S-F04-02 |
| 3 | User & Access Management navigation (System Admin only) | Opens User & Access Management | Leaves the audit area. | S-F04-01 |
| 4 | Return to workspace | Selects Return to workspace (Access Denied state) or uses global navigation | Leaves the audit area. | Workspace (outside the Screen List) |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | Verify RBAC permission before showing any audit data. | FR-04-03 |
| SR-002 | Audit entries are immutable. The screen has no control to edit or delete an entry. | FR-04-04 |
| SR-003 | Show the fields that FR-04-04 records: action, user ID, timestamp and payload. | FR-04-04 |
| SR-004 | Use only data returned by the audit log database. Do not invent events, user IDs, dates or payload values. | Mockup note; UI Conventions §4 |
| SR-005 | Do not display passwords, OTPs, access tokens or other authentication secrets in any audit detail. | UI Conventions §4 |
| SR-006 | Format timestamps as dd/MM/yyyy HH:mm and show the time zone (ICT, UTC+07:00). | UI Conventions §2 |
| SR-007 | The Access Denied state discloses no audit entry. | FR-04-03 |
| SR-008 | Filters, search and export are not part of this Must design. Add them only when FR-04-05 and FR-04-06 are approved. | W1 FR-04-05, FR-04-06 (Should); AD-UC-12 |
| SR-009 | Show only the F-04 navigation items the user's role allows. | UI Conventions — Navigation |
| SR-010 | Use English consistently; never rely on colour alone for status or errors. | UI Conventions §§1, 5 |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-04-04 | Displays the immutable audit entries that FR-04-04 records. W1 only requires the recording; viewing is not written as a separate FR (open question 1). |
| FR-04-03 | Checks permission before showing the entries; shows the Access Denied state when the check fails. |
| FR-04-05, FR-04-06 | Export and filter of audit logs (Should). Not designed on this screen yet. |
| FR-04-01 | Sign-in on S-F00-01 happens before this screen is reachable. |

## 8. Responsive and accessibility notes

- Smallest supported width: 360px; the sidebar collapses into a drawer below 1200px.
- On narrow screens the audit table scrolls or becomes a readable list, keeping the four fields together for each entry.
- Table headers are marked up as headers. The Read-only badge and error notice are text labels, not colour alone.
- Body text contrast at least 4.5:1.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | The Screen List marks this viewer as Must, but W1 FR-04-04 only requires automatic recording. Is viewing the stored entries an additional Must requirement, or should the viewer be reclassified? | Yes — Screen List and W1 traceability | Open (AD-UC-12 Q5) |
| 2 | Which actions count as "system-altering", and are failed attempts logged too? | Yes — event coverage | Open (AD-UC-12 Q1, Q2) |
| 3 | Which roles may view the logs (System Admin and Auditor), and what are their RBAC keys? | Yes — authorization contract | Open |
| 4 | Is paging, sorting, search or a date filter needed in v1? FR-04-06 describes a filter UI as Should. | No | Open |
| 5 | Can a payload contain sensitive data that must be masked, and how long are entries retained? | Yes — security design | Open (AD-UC-12 Q4) |
| 6 | Which states are required beyond Default and Access Denied (loading, empty, retrieval error)? | No | Open |
| 7 | Is viewing the audit log itself recorded as an event? | No | Open |

---

## Completion checklist

- [x] The mockup is a separate cropped image file, named with the Screen ID, at the frame width.
- [x] The numbered image exists, and its numbers correspond to the # column of the element inventory.
- [x] Every primary visible element in the mockup appears in the element inventory.
- [ ] Every colour in the mockup is confirmed as a design token from ui-conventions.md.
- [x] Every input element has an approved validation rule or an explicit "none" (this screen has no input element).
- [ ] All states/surfaces are filled in (only Default and Access Denied are designed).
- [x] Every navigation target is a Screen ID that exists in the Screen List or the stated workspace.
- [ ] Every data field has been confirmed against the final service data contract.
- [x] Every linked FR exists in the Function List; the viewer scope is logged in open question 1.
