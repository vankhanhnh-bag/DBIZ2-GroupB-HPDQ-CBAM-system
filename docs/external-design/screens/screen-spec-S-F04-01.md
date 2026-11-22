# Screen Spec: S-F04-01 User & Access Management

> EcoTrace — HPDQ Carbon Management & Reporting System. Draft for team review.

| Field | Value |
|---|---|
| Screen ID | S-F04-01 |
| Screen name | User & Access Management |
| Actor | System Admin |
| Priority | Must |
| Feature | F-04 Platform, Access & Operations |
| Activity diagrams | AD-UC-11 (actions "Request RBAC configuration access", "Define role and permission assignments", "Submit role configuration", "Review updated access policies") |
| Mockup images | `img/S-F04-01 — Default : Authorized.png` (default), `img/S-F04-01 — Default : Authorized - numbered.png`, two state images (section 2) |
| Frame width | 1440px (exported at 1×, 1440 pixels) |
| Figma frame | [01 — Done screens](https://www.figma.com/design/FTJibQtZrHIMHfwsJx7lT9/EcoTrace?node-id=8-73) |
| Status | Draft for team review |

## 1. Purpose

**Shown when:** a System Admin opens **Platform / Access** after signing in and passing the authorization check, to configure Role-Based Access Control (RBAC): the permissions of each role and the role assigned to each user. A request from a user without permission must not expose any protected configuration.

**The user leaves this screen when:** they open Audit Logs (S-F04-02), or use global navigation to return to the EcoTrace workspace. After a successful save the screen stays open and shows the updated result.

## 2. Mockup

![S-F04-01 — Default](img/S-F04-01%20%E2%80%94%20Default%20%3A%20Authorized.png)

**Numbered:** each number refers to the matching # in the element inventory in section 3. All three designed UI states are shown directly below. Element 3 appears only in the Default state, element 11 only in the Success state, and elements 12–13 only in the Access Denied state.

| Default — numbered | Success / Policy Updated — numbered |
|---|---|
| ![S-F04-01 — Default numbered](img/S-F04-01%20%E2%80%94%20Default%20%3A%20Authorized%20-%20numbered.png) | ![S-F04-01 — Success numbered](img/S-F04-01%20%E2%80%94%20Success%20%3A%20Policy%20Updated%20-%20numbered.png) |

| Access Denied — numbered | |
|---|---|
| ![S-F04-01 — Access Denied numbered](img/S-F04-01%20%E2%80%94%20Access%20Denied%20-%20numbered.png) | |

All colours, fonts and corners follow the shared design conventions in
[ui-conventions.md](../ui-conventions.md).

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | App shell (sidebar + top bar) | Global layout | EcoTrace navigation: Overview; F-04 Platform & Access with User & Access Management (active) and Audit Logs; Help & Support. Top bar shows the breadcrumb Platform / User & Access Management and the System Admin context. | Yes | F-04 administrative navigation is shown only to authorized administrative roles, not in the standard user sidebar (UI Conventions — Navigation). |
| 2 | Page title and description | Header | "User & Access Management — Configure role-based access control (RBAC) and user-role assignments." | Yes | Keep the full term on first use. |
| 3 | Protected administration notice | Information banner | "Only authorized System Admins can update role and permission configurations." | Yes (Default state) | Static. In the Success state it is replaced by element 11. |
| 4 | Role selector | Select | A configured role from the RBAC store (FR-04-02) | Yes | Offers existing configured roles only. Do not invent role names or IDs; role IDs follow the source RBAC records. |
| 5 | Permission assignments table | Data table with a checkbox column | Permission and Assignment columns, listing the approved permissions from the RBAC store (FR-04-02) | Yes, after a role is chosen | Only approved permissions can be assigned. Rows come from the RBAC store; the grey bars in the mockup are placeholders, not data. |
| 6 | Save role configuration | Primary button | Submits the role and permission configuration (FR-04-02; audit entry per FR-04-04) | Yes | Label is verb + object (UI Conventions §1). The authorization check (FR-04-03) is repeated on submit. |
| 7 | Return to workspace | Secondary button | Leaves the administration area | Yes | Returns to the workspace without changing or revealing anything. Also offered in the Access Denied state. |
| 8 | User selector | Select | A user record from the RBAC store | Yes | Do not fabricate user IDs; show only returned records. |
| 9 | Assigned role selector | Select | The configured role to assign to the selected user (FR-04-02) | Yes | Offers existing configured roles only. Whether a user can hold more than one role is not specified (open question 4). |
| 10 | Current assignment panel | Read-only panel | The selected user's current assignment, shown from the authorized user-role mapping | Conditional (after a user is chosen) | Display only what the mapping returns; no fabricated user or role IDs. |
| 11 | RBAC updated notice | Success banner | "RBAC configuration updated. An immutable access-change audit entry is recorded (FR-04-04)." | Conditional | Shown only after the update has been completed. Pair colour with icon and text. |
| 12 | Access denied notice | Error banner | Result of the FR-04-03 check: "You do not have permission to change access settings. Contact a System Admin." | Conditional | Says that access is unavailable and who can help (UI Conventions §4). Shows no protected data. |
| 13 | Protected configuration panel | Restricted view | "RBAC configuration is protected" — role definitions, permission assignments and user-role mapping are not shown without authorization | Conditional | Replaces elements 4–6 and 8–10. Renders no role, permission or user data. Contains the Return to workspace button (element 7). |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default — Authorized | Page title, protected-administration notice, Role configuration card (role selector, permission table, Save, Return) and User-role mapping card (user selector, assigned role selector, current assignment). | An authorized System Admin opens User & Access Management. |
| Success — Policy Updated | The same layout with the RBAC updated notice in place of the protected-administration notice. | The role configuration was saved and the access-control policy was updated. |
| Access Denied — Protected configuration unavailable | Access denied notice and the protected-configuration panel with Return to workspace. No role, permission or user data is visible. | The FR-04-03 permission check fails. |

Loading, validation-error, update-failure and empty states are not designed yet (open questions 2, 3 and 7).

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Platform / Access navigation | Opens User & Access Management | Verifies the user's RBAC permission (FR-04-03) and loads the roles, permissions and user records the user may see. | S-F04-01, or the Access Denied state |
| 2 | Role selector | Chooses a configured role | Shows that role's permission assignments from the RBAC store. | Stays on S-F04-01 |
| 3 | Permission table | Ticks or clears permissions for the role | Keeps the selection in the form; nothing is stored until Save. | Stays on S-F04-01 |
| 4 | Save role configuration | Selects Save | Checks permission again (FR-04-03), updates the access-control policy (FR-04-02), records an immutable audit entry (FR-04-04), and shows the RBAC updated notice. | Stays on S-F04-01 (Success state); Access Denied state if the check fails |
| 5 | User selector and Assigned role selector | Chooses a user and a role | Shows the current assignment from the authorized user-role mapping. | Stays on S-F04-01 |
| 6 | Audit Logs navigation | Opens Audit Logs | Opens the audit viewer, subject to its own permission check. | S-F04-02 |
| 7 | Return to workspace | Selects Return to workspace | Leaves the administration area. | Workspace (outside the Screen List) |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | Verify RBAC permission before showing any role, permission or user data. | FR-04-03; AD-UC-11 d1 |
| SR-002 | Only authorized System Admins can update roles, permissions and user-role assignments. | FR-04-03; AD-UC-11 |
| SR-003 | Opening or reading the screen changes nothing. The policy changes only when Save is submitted. | FR-04-02; AD-UC-11 s3 |
| SR-004 | Show the success notice only after the policy update has completed. An immutable audit entry is recorded for each successful change. | FR-04-02; FR-04-04; AD-UC-11 s3, s4 |
| SR-005 | Assign only roles and permissions that exist in the RBAC store. Do not invent roles, permission keys, user IDs or statuses. | UI Conventions §4; mockup note |
| SR-006 | The Access Denied state shows no protected role, permission or user data. | FR-04-03 |
| SR-007 | Show F-04 navigation only to authorized administrative roles. | UI Conventions — Navigation |
| SR-008 | Do not expose passwords, OTPs or tokens anywhere on this screen. | UI Conventions §4 |
| SR-009 | Use English consistently; button labels are verb + object. | UI Conventions §1 |
| SR-010 | Pair success and error colours with text or an icon. | UI Conventions §§4–5 |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-04-02 | Lets the System Admin define role and permission assignments and the user-role mapping, and submit them to update the access-control policy. |
| FR-04-03 | Checks permission before the screen and before Save; shows the Access Denied state when the check fails. |
| FR-04-04 | Each successful change produces an immutable audit entry; the Success notice refers to it. |
| FR-04-01 | Sign-in on S-F00-01 happens before this screen is reachable. |

## 8. Responsive and accessibility notes

- Smallest supported width: 360px; the sidebar collapses into a drawer below 1200px.
- On narrow screens the two cards stack in one column and the permission table scrolls or becomes a readable list.
- Every select, checkbox and button has a visible label, a visible keyboard focus and an accessible name.
- Success and error meaning is never conveyed by colour alone. Body text contrast at least 4.5:1.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | The mockup has one Save button, inside the Role configuration card. Does it also save the user-role mapping, or is a separate action needed? | Yes — interaction contract | Open |
| 2 | How are invalid or conflicting permission assignments and a failed policy update shown? | Yes — failure paths | Open (AD-UC-11 Q2) |
| 3 | If recording the audit entry fails after the policy update, is the change rolled back, retried or kept? | Yes — integrity | Open (AD-UC-11 Q3) |
| 4 | Can a user hold more than one role? | No — selector behaviour | Open |
| 5 | Should an unauthorized attempt create a separate security audit entry? | No | Open (AD-UC-11 Q4) |
| 6 | W1 lists the screen work item "Admin dashboard UI for assigning roles/permissions" under FR-04-01-FE, while FR-04-02 has no FE row. Should it move to FR-04-02? | No — W1 clean-up | Open |
| 7 | Do the user and role lists need search or paging, and what are the loading and empty states? | No | Open |

---

## Completion checklist

- [x] The mockup is a separate cropped image file, named with the Screen ID, at the frame width.
- [x] The numbered image exists, and its numbers correspond to the # column of the element inventory.
- [x] Every primary visible element in the mockup appears in the element inventory.
- [ ] Every colour in the mockup is confirmed as a design token from ui-conventions.md.
- [ ] Every input element has an approved validation rule or an explicit "none" (validation rules are pending).
- [ ] All states/surfaces are filled in (only Default, Success and Access Denied are designed).
- [x] Every navigation target is a Screen ID that exists in the Screen List or the stated workspace.
- [ ] Every data field has been confirmed against the final service data contract.
- [x] Every linked FR exists in the Function List and the relevant AD-UC-11 actions are covered by interaction rows.
