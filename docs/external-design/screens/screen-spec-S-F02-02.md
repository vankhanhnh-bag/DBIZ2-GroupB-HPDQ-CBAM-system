# Screen Spec: S-F02-02 CBAM Regulatory Settings

> **Product:** EcoTrace — HPDQ Carbon Management & Reporting System  
> **Status:** Draft for team review  
> **Source of truth:** UC-08, SD-UC-08, AD-UC-08, FR-02-14 in W1, shared FR-04-03, Screen List, Screen Transition Diagram, Usage Flow and UI Conventions.

| Field | Value |
|---|---|
| Screen ID | S-F02-02 |
| Screen name | CBAM Regulatory Settings |
| Actor | Compliance / Product Owner |
| Priority | Must |
| Feature | F-02 CBAM Compliance & Carbon Cost Estimation |
| Use case | UC-08 Manage CBAM regulatory rules & parameters |
| Activity diagram | AD-UC-08 — check protected access, review current configuration, optionally create/edit a rule-set draft and request activation |
| Sequence diagram | SD-UC-08 — check view permission, retrieve active and scheduled configurations, and enforce edit/activation authorization on subsequent actions |
| Linked requirements | FR-02-14 (Must); shared FR-04-03 (Must). FR-02-15–17 are handled by S-F02-03 after navigation. |
| Figma page | [EcoTrace — 01 Done screens](https://www.figma.com/design/FTJibQtZrHIMHfwsJx7lT9/EcoTrace?node-id=8-73) (individual frame deep links not yet verified) |
| Mockup images | `img/S-F02-02%20%E2%80%94%20Default.png`, `img/S-F02-02%20%E2%80%94%20Default%20%E2%80%94%20numbered.png`; state image table in section 2 |
| Frame size | 1440 × 1024 px (PNG at 1×) |
| Status | Draft for team review; numbered images added; Figma verification pending |
| Data shown | Active and scheduled CBAM regulatory rule sets, parameter values, effective dates and source references, as returned by the CBAM service |
| Important limitation | The canonical rule-set identifiers, parameter schema, status enum, sorting, search/filter controls and granular RBAC permission keys are not defined in W1. Do not invent them as confirmed requirements. |

## 1. Purpose

**Shown when:** an authenticated Compliance / Product Owner opens **CBAM & Carbon Cost > Regulatory Settings** and passes the view-authorization check. A denied request must not expose protected configuration data.

**The user does:** reviews which CBAM regulatory configuration is active or scheduled, including the associated parameter values, effective dates and source references. An authorized user may choose **Create New Rule Set** or **Edit** for an existing draft to proceed to S-F02-03. Viewing does not itself change the configuration.

**The system does:** checks the viewer's RBAC access (FR-04-03), retrieves applicable active/scheduled configurations from EcoTrace's CBAM service, and displays source and effective-date metadata (FR-02-14). When a draft-maintenance action is requested, the workflow moves to S-F02-03 only if the relevant edit operation is authorized. The draft-edit entry described in the Screen List is an interaction, not an additional requirement to expose every historical/draft record in the primary FR-02-14 results table.

**The user leaves this screen when:** they open S-F02-03 to create/edit a draft or use global navigation to return to the EcoTrace workspace. After successful activation/scheduling in S-F02-03, this screen can show the returned configuration and its updated effective-date context.

## 2. Mockup

![S-F02-02 CBAM Regulatory Settings — Default](img/S-F02-02%20%E2%80%94%20Default.png)

**Numbered:** each mint circle refers to the matching **#** in the element inventory in section 3, as in the teacher's worked example. Shared application-shell elements may carry the same number in the sidebar and top bar. Feedback elements **12–15** appear only in their corresponding states, not all on Default.

| Default, numbered | Access Denied, numbered |
|---|---|
| ![S-F02-02 Default numbered](img/S-F02-02%20%E2%80%94%20Default%20%E2%80%94%20numbered.png) | ![S-F02-02 Access Denied numbered](img/S-F02-02%20%E2%80%94%20Access%20Denied%20%E2%80%94%20numbered.png) |

| State | Mockup image | Numbered image |
|---|---|---|
| Default | [Default PNG](img/S-F02-02%20%E2%80%94%20Default.png) | [Numbered PNG](img/S-F02-02%20%E2%80%94%20Default%20%E2%80%94%20numbered.png) |
| Loading | [Loading PNG](img/S-F02-02%20%E2%80%94%20Loading.png) | [Numbered PNG](img/S-F02-02%20%E2%80%94%20Loading%20%E2%80%94%20numbered.png) |
| Empty | [Empty PNG](img/S-F02-02%20%E2%80%94%20Empty.png) | [Numbered PNG](img/S-F02-02%20%E2%80%94%20Empty%20%E2%80%94%20numbered.png) |
| Access Denied | [Access Denied PNG](img/S-F02-02%20%E2%80%94%20Access%20Denied.png) | [Numbered PNG](img/S-F02-02%20%E2%80%94%20Access%20Denied%20%E2%80%94%20numbered.png) |
| System Error | [System Error PNG](img/S-F02-02%20%E2%80%94%20System%20Error.png) | [Numbered PNG](img/S-F02-02%20%E2%80%94%20System%20Error%20%E2%80%94%20numbered.png) |

The five PNG states use a **1440 × 1024 px** desktop mockup and the shared EcoTrace UI conventions. Numbered copies are annotated versions of the existing UI Kit PNG renders; they are **not yet verified exports of the current editable Figma frames**. Values, version labels, counts and source examples are illustrative, not live CBAM configuration. Loading, Empty, Access Denied and System Error are presentation states, not new persisted rule-set statuses.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation / behaviour |
|---|---|---|---|---|---|
| 1 | App shell (sidebar + top bar) | Global layout | EcoTrace navigation and authenticated Compliance / Product Owner context | Yes | Reuse F-01 shell and role-appropriate navigation; F-04 administrative tools are not automatically shown to this actor. |
| 2 | Breadcrumb and page title | Header | CBAM & Carbon Cost / Regulatory Settings | Yes | Show where the user is in the feature. |
| 3 | Configuration status summary cards | KPI cards / section heading | Source-defined active and scheduled configuration counts; the mockup also illustrates a Draft count | Yes, when authorized | Counts and Draft card in the mockup are illustrative UI additions; confirm they are in scope and loaded from the data service, not hardcoded. |
| 4 | Regulatory Rule Sets section, filter tabs and data table | Section header / filter tabs / data table | Active/scheduled rule-set rows from CBAM service (FR-02-14); All / Active / Scheduled / Drafts tabs shown in the mockup | Yes, when records exist | Maintain readable headers and source data. Filter options/draft scope are a design proposal until approved; do not infer unsupported sorting or queries. |
| 5 | Rule-set reference / version | Read-only data cell | Identifier/version supplied by the service, if available | Conditional | Preserve the source identifier; exact identifier fields are not specified in W1. |
| 6 | Current configuration / governance overview | Read-only panels / rows | Regulatory parameter groups and configured methods/values, units, references, plus traceability/governance explanations (FR-02-14; FR-02-17 / FR-04-04 context) | Yes, when available | Display only returned parameter data; explanatory governance content is not a substitute for actual audit records or an approved history viewer. |
| 7 | Effective date | Read-only date cell | Effective date associated with the returned rule set (FR-02-14) | Yes, when returned | Format dd/MM/yyyy in the UI; do not calculate an unapproved activation schedule. |
| 8 | Source reference | Read-only link/text | Official/regulatory source reference stored for the rule set (FR-02-14) | Yes, when returned | Keep source provenance; only make clickable when a valid URL is present. |
| 9 | Status badge | Status label | Source-defined active or scheduled status | Yes, when returned | Show textual label plus color/icon; do not invent a full business-status taxonomy. |
| 10 | Rule-set row actions (View details / Edit draft) | Contextual links/buttons | Read-only View details for existing configurations; Edit draft for available editable draft (Screen List / FR-02-15 entry) | Conditional | View does not mutate. Edit navigates to S-F02-03 only after appropriate authorization; exact details presentation is unconfirmed. |
| 11 | Create New Rule Set | Primary button | User intent to create a draft (FR-02-15, on next screen) | Conditional | Navigate to S-F02-03; edit access must be enforced. |
| 12 | Permission-denied feedback | Banner / restricted view | FR-04-03 result | Conditional | Explain lack of access without rendering sensitive parameter values. |
| 13 | Loading indicator | Spinner / skeleton | In-progress regulatory-config query | Conditional | Keep labels understandable; avoid claiming the results are current before response. |
| 14 | Empty-state message | Informational panel | Empty result of authorized configuration query | Conditional | State that no records were returned; next action depends on permitted maintenance workflow. |
| 15 | Retrieval-error message | Error banner | Error returned by the regulatory service | Conditional | Do not claim configuration is empty or unchanged; retry behavior is to be confirmed. |
| 16 | Return to workspace | Sidebar navigation / button | Home page and the Access Denied state's Return to Workspace action | Yes | Return to the appropriate workspace without revealing protected settings. |

## 4. States

| State | What the user sees | Trigger | Source / notes |
|---|---|---|---|
| **1. Default — Configuration overview** | Page title and a read-only view of active/scheduled rule sets with returned parameter values, effective dates and references. Where a permitted draft-maintenance route exists, show Create New Rule Set / Edit. | Authorized user opens Regulatory Settings and configurations load successfully. | FR-02-14; AD-UC-08 system action s2; SD-UC-08 authorized branch. |
| **2. Loading — Retrieving configurations** | Existing layout with a loading cue while the system retrieves settings; do not mark placeholder values as authoritative. | View authorization passed and the retrieval request is outstanding. | Transient presentation inferred from SD-UC-08; not a new business state. |
| **3. Empty — No configurations returned** | Clear empty-result guidance; do not show fabricated active/scheduled records. | Authorized query returns no matching configurations. | UI-level contingency; no empty-state business rule specified in W1. |
| **4. Access Denied — Protected settings unavailable** | Text-based authorization error and safe navigation; no protected parameters, effective dates or references are visible. | FR-04-03 view permission check fails. | SD-UC-08 view-not-authorized branch; AD-UC-08 endDenied. |
| **5. System Error — Retrieval unavailable** | Error identifies that regulatory settings could not be retrieved; no success message or populated table is shown as current. | Network/service/data retrieval fails. | UI-level contingency; exact retry and cached-data policy are open. |

**State boundaries:** Active and Scheduled are *source-level regulatory configuration labels used by FR-02-14*; Default, Loading, Empty, Access Denied and System Error are *screen presentations*. Do not treat them as one lifecycle enumeration.

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Regulatory Settings navigation | Opens the destination | Checks view permission, loads authorized active/scheduled configurations and displays source/effective-date details. | S-F02-02 |
| 2 | Regulatory configurations table | Reviews displayed records and their parameter/source information | Returns the stored, authorized regulatory configuration; no write is performed. | Stays on S-F02-02 |
| 3 | Create New Rule Set | Selects creation | Checks edit permission as required for protected configuration maintenance; opens draft editor only when allowed. | S-F02-03 if authorized; otherwise S-F02-02 with access feedback |
| 4 | Edit on a draft | Selects available draft | Checks edit permission; loads editable draft context for S-F02-03. | S-F02-03 if authorized; otherwise S-F02-02 with access feedback |
| 5 | View-only choice | Does not request any write | Ends UC-08 viewing flow without changing regulatory rules. | Stays on S-F02-02 / workspace |
| 6 | Return from Editor | Uses Back / returns after activation outcome | Reloads applicable regulatory configuration as appropriate for the operation; previous versions remain traceable. | S-F02-02 |
| 7 | Return to workspace | Uses global navigation | Leaves regulatory settings. | Workspace (outside the Screen List) |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | Enforce RBAC view permission before disclosing any protected regulatory configuration. | FR-04-03; SD-UC-08 |
| SR-002 | An authorized viewer can see active and scheduled rule sets, parameter values, effective dates and source references. | FR-02-14 |
| SR-003 | Do not modify a rule set merely by opening or reviewing S-F02-02. | AD-UC-08 d2 view-only branch |
| SR-004 | Create/edit entry leads to S-F02-03; protected editing requires permission, not simply a visible control. | Screen List; Screen Transition §5; SD-UC-08 |
| SR-005 | When edit permission is denied, keep the user in a safe read-only context and do not reveal editable protected content. | Screen Transition §5; SD-UC-08 |
| SR-006 | Read-only configuration display does not itself create an audit event; audit logging applies to completed system-altering changes. | FR-04-04; SD-UC-08 |
| SR-007 | Use only server-supplied regulatory values, identifiers, sources, effective dates and status labels. | FR-02-14; UI Conventions §4 |
| SR-008 | The saved/activated or future-effective result from S-F02-03 is reflected only according to the effective-date rule; do not assume a future version has already become active. | FR-02-17; SD-UC-08; Screen Transition §5 |
| SR-009 | Use the shared EcoTrace application shell and role-appropriate navigation, matching F-01 visual conventions. | UI Conventions — Visual direction / Navigation |
| SR-010 | Use English consistently, and format dates as dd/MM/yyyy, preserving source identifiers and units. | UI Conventions §§1–2 |
| SR-011 | Pair error, loading and status text with more than color alone; never show protected details in the denied state. | UI Conventions §§4–5; FR-04-03 |
| SR-012 | Do not implement unapproved rule-set filtering, sorting, version-status enums or official CBAM field/threshold lists as fixed requirements. | W1 FR-02-14; AD-UC-08 open questions |

## 7. Linked requirements

| FR ID | Priority | What this screen does for it |
|---|---|---|
| FR-02-14 | Must | Lists active and scheduled CBAM regulatory rule sets with parameter values, effective dates and source references for authorized Compliance / Product Owner users. |
| FR-04-03 | Must (shared) | Checks access to protected configuration and edit navigation; prevents disclosure/actions without permission. |
| FR-02-15 | Must (navigation only) | Provides an entry to the create/edit draft form on S-F02-03, which implements draft saving. |
| FR-02-16 | Must (downstream) | Validation is handled on S-F02-03, not through a separate settings screen. |
| FR-02-17 | Must (downstream) | After versioning and activation/scheduling on S-F02-03, the settings overview reflects the returned applicable configuration. |

FR-04-04 (audit logging) applies to **writes performed from S-F02-03**, not a read-only view of S-F02-02.

## 8. Responsive and accessibility notes

- **Application shell:** Match the F-01 desktop shell (1440 × 1024 reference), compact left sidebar, top app bar, breadcrumbs and white main content. Below 1200 px, collapse sidebar to a drawer; support a minimum width of 360 px.
- **Regulatory table:** At narrow widths, allow horizontal scrolling or use a readable stacked record view. Retain parameter names, returned values and units, effective dates, source references and accessible headers.
- **Status and feedback:** Do not rely on badge color alone to distinguish active/scheduled, access-denied, loading or errors; provide visible text.
- **Keyboard and touch:** Logical tab order through navigation and permitted actions; visible focus state; targets at least 24 × 24 CSS px, preferably 40–44 px.
- **Contrast and language:** Body-text contrast at least 4.5:1; English interface copy, semantic table headers, screen-reader-friendly error announcements.
- **Security:** Never expose protected configuration data in the Access Denied state or in assistive-only hidden content.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | What are the canonical rule-set identifier/version fields and source-reference formats returned by the CBAM service? | Yes — data-table contract | Open |
| 2 | What exact parameter schema, units and source metadata are available for each active/scheduled version? | Yes — complete information architecture | Open |
| 3 | Are view, edit and activate permissions independently configured, and what are their actual RBAC keys? | Yes — action-specific authorization contract | Open; SD-UC-08 checks the actions without defining keys |
| 4 | Does a future effective date create a source status named Scheduled, and how is applicability determined for an estimate? | Yes — version lifecycle semantics | Open |
| 5 | Should drafts, prior versions or full change history be visible in this table or through a separate affordance? | No — screen-list draft-edit route must be reconciled | Open |
| 6 | Should users search, filter or sort configurations, and by which fields? | No — not specified in W1 | Open |
| 7 | What are the approved empty, retrieval-failure and retry behaviors? May cached configuration be displayed, and under which disclaimer? | No — UI fallback policy | Open |

| 8 | Are the mockup's KPI counts, Draft status card, filter tabs and View details affordance approved for v1, and what data fields back them? | No — UI details | Open; not explicitly required by FR-02-14. |
---

## Completion checklist

- [x] A separate cropped 1440 × 1024 PNG exists for S-F02-02 Default and is embedded above.
- [x] A numbered Default PNG exists, and mint numbers correspond to the # column of the element inventory.
- [x] All five UI state PNGs (Default, Loading, Empty, Access Denied, System Error) and their numbered counterparts are linked in section 2.
- [x] The inventory covers the primary visible page groups, rule-set filters, status summary, row actions, configuration detail and conditional feedback.
- [x] The protected data source and relevant validations/permission checks are documented without inventing regulator-approved values or RBAC keys.
- [x] Navigation to S-F02-03 and to workspace uses Screen IDs consistent with the Screen List and Screen Transition.
- [x] Linked FRs exist in W1 and relevant AD-UC-08 view/maintain actions are described.
- [ ] Numbered PNGs and styling have been peer-compared with the actual latest Figma native frames (these PNGs originate from the earlier UI Kit).
- [ ] The illustrated KPI/filter/View details behavior and open questions 1–4, 8 have been confirmed by the team.
- [ ] Peer review completed; mark Status as Reviewed only after sign-off.
