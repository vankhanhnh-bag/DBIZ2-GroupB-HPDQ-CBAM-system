# Screen Spec: S-F02-03 CBAM Regulatory Rule Set Editor

> EcoTrace — HPDQ Carbon Management & Reporting System. Draft for team review.

| Field | Value |
|---|---|
| Screen ID | S-F02-03 |
| Screen name | CBAM Regulatory Rule Set Editor |
| Actor | Compliance / Product Owner |
| Priority | Must |
| Feature | F-02 CBAM Compliance & Carbon Cost Estimation |
| Activity diagrams | AD-UC-08 (actions "Edit regulatory parameters", "Save draft", "Validate and activate") |
| Mockup images | `S-F02-03 — Default.png` (default), `S-F02-03 — Default — numbered.png`, four state images (section 2) |
| Frame width | 1440px (exported at 1×, 1440 pixels) |
| Figma frame | [01 — Done screens](https://www.figma.com/design/FTJibQtZrHIMHfwsJx7lT9/EcoTrace?node-id=8-73) |
| Status | Draft for team review |

## 1. Purpose

**Shown when:** a Compliance / Product Owner selects **Create New Rule Set** or **Edit** on an available draft in S-F02-02 and the relevant edit permission has been granted. Existing active/historical configurations must not silently become editable drafts.

**The user leaves this screen when:** they save a draft and retain it, cancel an activation confirmation, receive validation/authorization feedback, return to Regulatory Settings or complete activation/scheduling and return to S-F02-02. An unsuccessful activation must not be represented as success.

## 2. Mockup

![S-F02-03](img/S-F02-03%20%E2%80%94%20Default.png)

**Numbered:** each number is the # of the element inventory in section 3. Elements 9–16 (validation messages (9–10), confirmations (11–12) and modal controls (13–16) appear in their corresponding images.)

| Default, numbered | Error, numbered |
|---|---|
| ![S-F02-03 numbered](img/S-F02-03%20%E2%80%94%20Default%20%E2%80%94%20numbered.png) | ![S-F02-03 error numbered](img/S-F02-03%20%E2%80%94%20Validation%20Error%20%E2%80%94%20numbered.png) |

| State | Image |
|---|---|
| Default | `img/S-F02-03%20%E2%80%94%20Default.png` |
| Validation Error | `img/S-F02-03%20%E2%80%94%20Validation%20Error.png` |
| Draft Saved | `img/S-F02-03%20%E2%80%94%20Draft%20Saved.png` |
| Activation Success | `img/S-F02-03%20%E2%80%94%20Activation%20Success.png` |
| Confirmation modal (M-F02-03) | `img/M-F02-03%20%E2%80%94%20Confirm%20Rule%20Set%20Activation.png` |

The numbered modal image is `img/M-F02-03%20%E2%80%94%20Confirm%20Rule%20Set%20Activation%20%E2%80%94%20numbered.png`; M-F02-03 is an overlay on this screen, not a separate screen.

All colours, fonts and corners follow the shared design conventions in
[ui-conventions.md](../ui-conventions.md).

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | App shell (sidebar + top bar) | Global layout | EcoTrace navigation, actor context and breadcrumbs | Yes | Reuse shared F-01 shell and role-appropriate F-02 navigation. |
| 2 | Page title and mode | Header | Regulatory Rule Set Editor; New / Edit Draft as appropriate | Yes | Distinguish create vs edit without implying changes are already saved. |
| 3 | Rule-set information, draft context and version reference | Form section / read-only status | Rule set name, version reference, configuration status and draft-only warning; canonical identifiers supplied by service when available | Yes | Do not treat example Draft 02 labels or the warning as verified source data. Active configurations are not silently edited in place. |
| 4 | Regulatory parameter editor | Field group / data table / reference links | Schema-defined parameter groups, configured methods/values and attached source references (FR-02-15) | Yes | Backend validates mandatory/range rules before activation; exact schema, units and allowed ranges remain open. |
| 5 | Source references | Text input / reference rows | Regulatory/source citations associated with configured parameters (FR-02-15) | Yes | Persist references as part of draft; exact required format and URL validation are open. |
| 6 | Effective date | Date picker / input | Requested effective date for activation (FR-02-17) | Yes, for activation | Must not conflict with existing effective periods; do not invent timezone/scheduling rules. |
| 7 | Save Draft | Primary/secondary action | New or edited draft values and references | Yes | Save draft version (FR-02-15) after edit authorization; record change (FR-04-04). Draft save does not mean activation. |
| 8 | Validate & Activate request / validation checklist | Action button / checklist panel | Primary Validate & Activate, pre-activation checks and illustrated Review Activation button (FR-02-16–17) | Conditional | Activation requires authorization and validation. The precise ordering between Review Activation, modal confirmation and service checks needs team confirmation. |
| 9 | Validation error summary | Error banner / checklist feedback | Missing mandatory values, invalid ranges and conflicting effective periods returned by service (FR-02-16) | Conditional | Block activation; error banner/checklist show reasons without implying a successful change. |
| 10 | Field-level errors | Inline feedback | Parameter, source or effective-date validation reasons from service | Conditional | Attach error text to affected control; preserve user draft context where allowed. |
| 11 | Draft saved confirmation | Toast / banner | Confirmation of persisted draft/version (FR-02-15) | Conditional | Clearly say Saved as draft; do not show Active without authorization and activation. |
| 12 | Activation/scheduling confirmation | Toast / banner | Returned activation result with effective date/status (FR-02-17) | Conditional | Distinguish successfully applied now from future-effective/scheduled outcome if supported by service. |
| 13 | Confirm Activation overlay | Modal | M-F02-03 over S-F02-03 (FR-02-17) | Conditional | Warn of system-wide consequence for applicable future estimates and previous-version retention. |
| 14 | Modal rule-set summary | Read-only metadata | Current draft reference/version and requested effective date | Conditional | Display only returned/current input context; exact version label is unconfirmed. |
| 15 | Modal Cancel | Secondary button | Dismiss confirmation | Conditional | Close modal with no activation request; leave saved draft unaffected. |
| 16 | Modal Confirm Activation | Primary button | Confirm activation request | Conditional | Submit once; authorize activation, validate and then activate/version only if valid. |
| 17 | Action-specific access feedback | Banner | RBAC edit/activate check results (FR-04-03) | Conditional | Prevent unauthorized write/activation; protect draft and configuration data. |
| 18 | Version Impact and prior-version traceability | Read-only information panel | Applicable effective date, future estimate use and prior-version preservation (FR-02-17) | Conditional | The informational panel does not itself implement an unapproved version-history viewer. |
| 19 | Back to Regulatory Settings | Secondary button / breadcrumb | S-F02-02 navigation | Yes | Return without asserting an unsaved change was committed; exact unsaved-changes prompt not specified. |

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default — Create/edit draft | Editable schema-defined parameter values and source references, a requested effective date where relevant, Save Draft and an activation entry action; prior version context is read-only. | Authorized user opens S-F02-03 from S-F02-02. |
| Validation Error — Activation blocked | Error banner plus field-level explanations of missing mandatory values, invalid ranges or conflicting effective periods. No active/scheduled success is asserted. | Activation validation fails after an authorized request. |
| Draft Saved — Not activated | Confirmation that the draft version was saved and logged. It remains a draft, and the user can leave or request activation. | Authorized Save Draft succeeds and audit change is recorded. |
| Activation Success — Activated or future-effective | Result explains that a validated version was activated or scheduled for its effective date; does not claim a future version is active immediately. Earlier versions remain preserved. | Authorized, validated activation completes and is logged. |
| Confirm Activation — Modal M-F02-03 | Dialog over dimmed editor with current draft, effective date, consequences and Cancel / Confirm Activation. No underlying page navigation. | User requests activation; confirmation is requested before the consequential action. |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Create New Rule Set / Edit draft | Opens editor from settings | Checks edit permission; opens the draft/new form only if authorized. | S-F02-02 → S-F02-03 |
| 2 | Regulatory parameters and source references | Enters/updates supported fields | Keeps draft input context; final required/range checks are performed before activation. | Stays on S-F02-03 |
| 3 | Save Draft | Clicks | Checks permission, saves the new/updated draft version and records immutable audit entry. | S-F02-03 — Draft Saved |
| 4 | Activate / Validate & Activate | Requests activation | Opens M-F02-03 to confirm the consequential system-wide action. | M-F02-03 overlay on S-F02-03 |
| 5 | Cancel on M-F02-03 | Clicks | Dismisses modal without activation; retains already saved draft. | S-F02-03 |
| 6 | Confirm Activation on M-F02-03 | Confirms | Checks activation permission, validates mandatory values, ranges and effective-period conflicts; blocks invalid activation with reasons. | S-F02-03 — Validation Error if invalid |
| 7 | Valid activation result | Reviews confirmation | System versions rule set with effective date, preserves history, logs change and returns activated/scheduled outcome. | S-F02-03 — Activation Success; then S-F02-02 |
| 8 | Edit permission denied | Attempts a protected write | Denies write; no new persisted draft or audit success is claimed. | Safe in-place feedback; S-F02-02 or S-F02-03 depending on point of failure |
| 9 | Activation permission denied | Attempts activation | Denies activation; draft is not activated and no activation success is shown. | Stays on S-F02-03 |
| 10 | Back to Regulatory Settings | Navigates back | Returns to overview. Unsaved-draft warning behavior is not specified. | S-F02-02 |
| 11 | Return to workspace | Uses global navigation | Leaves UC-08 configuration workflow. | Workspace (outside Screen List) |

The modal is **not** a third screen and has no separate Screen Spec: it is fully documented as part of S-F02-03.

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | Only authorized users may create/edit regulatory drafts; perform a protected-action permission check rather than relying on a hidden button. | FR-04-03; SD-UC-08 |
| SR-002 | Saving a new or changed rule set persists the configured parameter values and source references as a draft version. | FR-02-15 |
| SR-003 | A saved draft is not automatically activated; its saved status and activation outcome are distinct. | FR-02-15–17; AD-UC-08 d3 |
| SR-004 | The system records an immutable audit event for a completed draft change. | FR-04-04; SD-UC-08 |
| SR-005 | When activation is requested, check the activation permission and validate the draft for mandatory values, range validity and conflicting effective periods. | FR-02-16; FR-04-03; SD-UC-08 |
| SR-006 | Block activation for invalid or conflicting drafts and show specific reasons; never represent blocked activation as applied. | FR-02-16 |
| SR-007 | Only a validated, authorized rule set is versioned for activation with an effective date; preserve preceding versions for traceability. | FR-02-17 |
| SR-008 | The applicable version is used for new CBAM estimates according to its effective date; do not recalculate completed estimates or pretend a future-effective version is active now. | FR-02-17; SD-UC-08 |
| SR-009 | Record an immutable audit event on a completed activation, not a fabricated success for denied/invalid attempts. | FR-04-04; SD-UC-08 |
| SR-010 | M-F02-03 is a consequence-confirmation overlay with Cancel and Confirm Activation; it must not navigate or be represented as another Screen ID. | Screen List; Screen Transition §5; UI Conventions Modal |
| SR-011 | Do not infer a distinct RBAC permission key or role hierarchy for view/edit/activate, even though SD-UC-08 checks the three actions. | FR-04-03; AD-UC-08 open questions |
| SR-012 | Do not invent parameter names, regulatory thresholds, allowed range limits, date conflict algorithms or legal citations. | FR-02-15–17; AD-UC-08 open questions |
| SR-013 | Use the shared EcoTrace shell and English interface. Keep date formatting dd/MM/yyyy and source identifiers unchanged. | UI Conventions §§1–3 |
| SR-014 | Pair status/errors with visible text and accessible names; identify which fields need correction. | UI Conventions §§4–5 |
| SR-015 | After an activated/scheduled result, show the correct effective-date status in the settings overview, not an unapproved new lifecycle enum. | FR-02-17; Screen Transition §5 |

## 7. Linked requirements

| FR ID | What this screen does for it |
| --- | --- |
| FR-02-15 | Create/edit form gathers configured regulatory parameter values and source references and saves them as a draft version. |
| FR-02-16 | Blocks activation for missing required values, invalid ranges or overlapping effective periods and displays reasons beside the draft. |
| FR-02-17 | Confirmation and authorization precede versioned activation/scheduling with effective date; prior versions are retained and applicable future estimates use the correct version. |
| FR-04-03 | RBAC checks protect edit and activate operations; denied actions cannot mutate protected settings. |
| FR-04-04 | Completed draft saves and activations create immutable audit records. |
| FR-02-14 | S-F02-02 is the read-only starting/return screen showing active and scheduled regulatory configurations. |

## 8. Responsive and accessibility notes

- Smallest supported width: 360px; sidebar collapses below 1200px.
- On narrow screens the parameter fields stack in one column.
- Validation errors show text; modal has a clear title and keyboard focus.
- Body text contrast at least 4.5:1.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | Which regulatory parameter fields, names, units and official source-reference formats are required in a CBAM rule-set draft? | Yes — form/data contract | Open |
| 2 | What mandatory-value rules, permissible ranges and effective-period overlap rules are enforced, and are any validations applied before Save Draft? | Yes — activation acceptance criteria | Open |
| 3 | Are view, edit and activate different RBAC permission keys? Which actors may save drafts versus activate them? | Yes — access policy | Open; SD-UC-08 documents separate checks but not keys |
| 4 | How is a future effective date represented (Scheduled vs Active); what clock/event makes the version applicable? | Yes — scheduling lifecycle | Open |
| 5 | Can a rejected activation be corrected/retried without leaving the editor, or does UC-08 need a new invocation? | No — recovery-flow decision | Open; AD-UC-08 currently ends the invalid branch |
| 6 | Does activation confirmation occur before or after service-side validation and authorization, and how should errors return after the modal? | Yes — UI/sequence handoff | Open; Screen Transition places modal before confirmation; SD-UC-08 models checks after the activation request |
| 7 | What happens on draft-save failure, activation service failure or audit-log write failure? Is operation atomic/rolled back? | Yes — failure handling | Open; SD-UC-08 does not define these error branches |
| 8 | Should prior versions be shown in the editor or only retained in the data service? How can users inspect historical sources? | No — history presentation | Open |
| 9 | Are rule-set identifiers/versions generated on draft save, and what is the approved display format? | No — display contract | Open |
| 10 | Should Review Activation be a distinct step from the primary Validate & Activate button, and what is the approved trigger order for M-F02-03 and the validation checklist? | Yes — navigation/interaction agreement | Open; the current mockup illustrates both controls. |
---

---

## Completion checklist

- [x] The mockup is a separate cropped image file, named with the Screen ID, at the frame width.
- [x] The numbered image exists, and its numbers correspond to the # column of the element inventory.
- [x] Every primary visible element in the mockup appears in the element inventory.
- [ ] Every colour in the mockup is confirmed as a design token from ui-conventions.md.
- [ ] Every input element has an approved validation rule or an explicit "none" (the CBAM field contract is pending).
- [x] All five states/surfaces are filled in.
- [x] Every navigation target is a Screen ID that exists in the Screen List or the stated workspace.
- [ ] Every data field has been confirmed against the final service data contract.
- [x] Every linked FR exists in the Function List and relevant AD-UC-08 actions are covered by interaction rows.
