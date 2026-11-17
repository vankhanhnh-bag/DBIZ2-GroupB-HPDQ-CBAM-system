# Screen Spec: S-F02-03 CBAM Regulatory Rule Set Editor

> **Product:** EcoTrace — HPDQ Carbon Management & Reporting System  
> **Status:** Draft for team review  
> **Source of truth:** UC-08, SD-UC-08, AD-UC-08, FR-02-15–FR-02-17 in W1, shared FR-04-03–FR-04-04, Screen List, Screen Transition Diagram, Usage Flow and UI Conventions.

| Field | Value |
|---|---|
| Screen ID | S-F02-03 |
| Screen name | CBAM Regulatory Rule Set Editor |
| Actor | Compliance / Product Owner |
| Priority | Must |
| Feature | F-02 CBAM Compliance & Carbon Cost Estimation |
| Use case | UC-08 Manage CBAM regulatory rules & parameters |
| Activity diagram | AD-UC-08 — create/edit a draft, save and log changes, request activation, validate, version and log activation |
| Sequence diagram | SD-UC-08 — authorize edit/activate requests, persist draft, validate effective periods, activate or schedule a version and append immutable audit records |
| Linked requirements | FR-02-15, FR-02-16, FR-02-17 (all Must); shared FR-04-03 and FR-04-04 (Must) |
| Mockup / Figma | [EcoTrace — 01 Done screens](https://www.figma.com/design/FTJibQtZrHIMHfwsJx7lT9/EcoTrace?node-id=8-73) — S-F02-03 state frames and M-F02-03 overlay; exact frame node links pending verification |
| User inputs | Configured regulatory parameter values, source references; effective date for an activation request |
| Important limitation | Required parameter names, allowed values/ranges, official regulatory sources, conflict-resolution policy, edit/activation RBAC keys and the precise schedule trigger are unspecified. Keep them as open questions, not fabricated requirements. |

## 1. Purpose

**Shown when:** a Compliance / Product Owner selects **Create New Rule Set** or **Edit** on an available draft in S-F02-02 and the relevant edit permission has been granted. Existing active/historical configurations must not silently become editable drafts.

**The user provides:** regulatory parameter values and source references for a new or existing draft (FR-02-15), then decides whether to keep that draft or request activation with an effective date (FR-02-17). The full list of specific regulatory parameters is defined by the future data contract, not by this Screen Spec.

**The system does:** enforces edit and activation permissions (FR-04-03), saves a distinct draft version and records the change in an immutable audit event (FR-02-15; FR-04-04). If activation is requested, it checks mandatory values, permissible ranges and conflicting effective periods (FR-02-16); invalid drafts stay unactivated and reasons are shown. A valid, authorized activation versions the rule set with an effective date, preserves earlier versions for traceability, makes the applicable version available to later UC-07 estimates and records activation (FR-02-17; FR-04-04).

**The user leaves this screen when:** they save a draft and retain it, cancel an activation confirmation, receive validation/authorization feedback, return to Regulatory Settings or complete activation/scheduling and return to S-F02-02. An unsuccessful activation must not be represented as success.

## 2. Mockup

Match the **same editable EcoTrace shell and F-01 styling**: Hòa Phát navy sidebar, top application bar, breadcrumbs, white form cards, 8 px spacing rhythm and **1440 × 1024** desktop reference. Keep the existing branding and do not add author/designer names. Do not flatten the UI into one image.

| Frame | State / surface |
|---|---|
| S-F02-03 — Default | New/edit draft form with parameter rows, source references, Save Draft and an activation entry action |
| S-F02-03 — Validation Error | Activation blocked; missing required values, invalid ranges or overlapping effective periods explained beside affected fields/section |
| S-F02-03 — Draft Saved | Draft was persisted and audit event recorded; it is not yet active |
| S-F02-03 — Activation Success | Successful activation or effective-date scheduling feedback; prior versions preserved |
| M-F02-03 — Confirm Rule Set Activation | Actual centered modal **over** S-F02-03, with dimmed editor background and Cancel / Confirm Activation; not a separate Screen ID |

The default screen should distinguish editable draft parameters from read-only version/history metadata. Any example dates, version IDs, units or parameter values must be marked as placeholders; do not portray invented regulatory thresholds as real rules. M-F02-03 is a presentation overlay. Its confirmation wording should mention impact on **future applicable** CBAM estimates, not retroactive updates to completed estimates.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation / behaviour |
|---|---|---|---|---|---|
| 1 | App shell (sidebar + top bar) | Global layout | EcoTrace navigation, actor context and breadcrumbs | Yes | Reuse shared F-01 shell and role-appropriate F-02 navigation. |
| 2 | Page title and mode | Header | Regulatory Rule Set Editor; New / Edit Draft as appropriate | Yes | Distinguish create vs edit without implying changes are already saved. |
| 3 | Rule-set/version reference | Read-only label | Service-generated identifier and version context, where available | Conditional | Preserve canonical IDs and historical references; no invented version numbers. |
| 4 | Regulatory parameter editor | Text input / Select / field rows | Parameter values and units provided by the approved regulatory schema (FR-02-15) | Yes | Show only schema-defined fields. Mandatory rules/ranges are validated by service; exact list is unconfirmed. |
| 5 | Source references | Text input / reference rows | Regulatory/source citations associated with configured parameters (FR-02-15) | Yes | Persist references as part of draft; exact required format and URL validation are open. |
| 6 | Effective date | Date picker / input | Requested effective date for activation (FR-02-17) | Yes, for activation | Must not conflict with existing effective periods; do not invent timezone/scheduling rules. |
| 7 | Save Draft | Primary/secondary action | New or edited draft values and references | Yes | Save draft version (FR-02-15) after edit authorization; record change (FR-04-04). Draft save does not mean activation. |
| 8 | Validate / Activate request | Action button | User request to activate a draft | Conditional | Enter activation flow; activation permission and validation are required (FR-02-16–17). |
| 9 | Validation error summary | Banner | Missing mandatory values, invalid ranges, conflicting effective periods returned by service | Conditional | Block activation and provide clear correction instructions; no generic success. |
| 10 | Field-level errors | Inline feedback | Parameter, source or effective-date validation reasons from service | Conditional | Attach error text to affected control; preserve user draft context where allowed. |
| 11 | Draft saved confirmation | Toast / banner | Confirmation of persisted draft/version (FR-02-15) | Conditional | Clearly say Saved as draft; do not show Active without authorization and activation. |
| 12 | Activation/scheduling confirmation | Toast / banner | Returned activation result with effective date/status (FR-02-17) | Conditional | Distinguish successfully applied now from future-effective/scheduled outcome if supported by service. |
| 13 | Confirm Activation overlay | Modal | M-F02-03 over S-F02-03 (FR-02-17) | Conditional | Warn of system-wide consequence for applicable future estimates and previous-version retention. |
| 14 | Modal rule-set summary | Read-only metadata | Current draft reference/version and requested effective date | Conditional | Display only returned/current input context; exact version label is unconfirmed. |
| 15 | Modal Cancel | Secondary button | Dismiss confirmation | Conditional | Close modal with no activation request; leave saved draft unaffected. |
| 16 | Modal Confirm Activation | Primary button | Confirm activation request | Conditional | Submit once; authorize activation, validate and then activate/version only if valid. |
| 17 | Action-specific access feedback | Banner | RBAC edit/activate check results (FR-04-03) | Conditional | Prevent unauthorized write/activation; protect draft and configuration data. |
| 18 | Previous-version traceability | Read-only note/link | Retained earlier rule-set versions (FR-02-17) | Conditional | Preserve prior versions; no invented history viewer or separate screen. |
| 19 | Back to Regulatory Settings | Secondary button / breadcrumb | S-F02-02 navigation | Yes | Return without asserting an unsaved change was committed; exact unsaved-changes prompt not specified. |

## 4. States

| State | What the user sees | Trigger | Source / notes |
|---|---|---|---|
| **1. Default — Create/edit draft** | Editable schema-defined parameter values and source references, a requested effective date where relevant, Save Draft and an activation entry action; prior version context is read-only. | Authorized user opens S-F02-03 from S-F02-02. | Screen List; FR-02-15; SD-UC-08 edit branch. |
| **2. Validation Error — Activation blocked** | Error banner plus field-level explanations of missing mandatory values, invalid ranges or conflicting effective periods. No active/scheduled success is asserted. | Activation validation fails after an authorized request. | FR-02-16; SD-UC-08 invalid branch; AD-UC-08 d4. |
| **3. Draft Saved — Not activated** | Confirmation that the draft version was saved and logged. It remains a draft, and the user can leave or request activation. | Authorized Save Draft succeeds and audit change is recorded. | FR-02-15; FR-04-04; AD-UC-08 endDraft / SD-UC-08 draft saved response. |
| **4. Activation Success — Activated or future-effective** | Result explains that a validated version was activated or scheduled for its effective date; does not claim a future version is active immediately. Earlier versions remain preserved. | Authorized, validated activation completes and is logged. | FR-02-17; FR-04-04; SD-UC-08 activation/scheduling result. |
| **5. Confirm Activation — Modal M-F02-03** | Dialog over dimmed editor with current draft, effective date, consequences and Cancel / Confirm Activation. No underlying page navigation. | User requests activation; confirmation is requested before the consequential action. | Screen List Modal; Screen Transition §5; UI Conventions Modal guidance. |
| **6. Edit permission denied — In-place feedback** | No protected update is saved and a clear access explanation appears in the current authorized context. | Edit authorization fails. | SD-UC-08 edit-not-authorized branch; FR-04-03. |
| **7. Activation permission denied — In-place feedback** | The draft is not activated; permission feedback is shown without implying validation success. | Activation authorization fails. | SD-UC-08 activation-not-authorized branch; FR-04-03. |

The final two are **conditional feedback variations**, not additional required top-level Figma frames or persisted rule-set statuses. Loading, save failure and retry are not specified as separate required frames here; if the design adds them, label as proposed and confirm service behavior first. The draft-versus-activated distinction is a business outcome; "Validation Error" and "Draft Saved" are UI presentations.

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

| FR ID | Priority | What this screen does for it |
|---|---|---|
| FR-02-15 | Must | Create/edit form gathers configured regulatory parameter values and source references and saves them as a draft version. |
| FR-02-16 | Must | Blocks activation for missing required values, invalid ranges or overlapping effective periods and displays reasons beside the draft. |
| FR-02-17 | Must | Confirmation and authorization precede versioned activation/scheduling with effective date; prior versions are retained and applicable future estimates use the correct version. |
| FR-04-03 | Must (shared) | RBAC checks protect edit and activate operations; denied actions cannot mutate protected settings. |
| FR-04-04 | Must (shared) | Completed draft saves and activations create immutable audit records. |
| FR-02-14 | Must (upstream) | S-F02-02 is the read-only starting/return screen showing active and scheduled regulatory configurations. |

## 8. Responsive and accessibility notes

- **Application shell:** Match F-01 in desktop design (1440 × 1024 reference), preserving sidebar, top bar, breadcrumbs and page spacing. Below 1200 px collapse sidebar; minimum mobile width 360 px.
- **Form responsiveness:** Stack parameter fields and source references into one column on narrow screens. Preserve units, labels and error hints; do not use placeholder text as the only field label.
- **Modal accessibility:** Set accessible dialog title/description, take focus on open, keep keyboard focus inside while open, return focus to activation action on cancel/close, and make consequences explicit.
- **Field validation:** Link error feedback to the offending input, show both summary and inline reasons, do not rely on color alone. Keep entered values visible when correcting invalid activation.
- **Keyboard, targets and contrast:** Logical tab sequence; visible focus; controls ≥24 × 24 CSS px, preferably 40–44 px; body text contrast at least 4.5:1.
- **Content conventions:** English interface; dd/MM/yyyy effective dates, service-defined numeric units and precision, full source provenance. Never use fabricated official CBAM values in mockups.

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

---

## Completion checklist

- [ ] A separate cropped S-F02-03 mockup image is linked or committed under screens/img.
- [x] Screen ID, actor, priority, UC-08 and three Must FRs match W1 and Screen List.
- [x] Save Draft and activation are distinct, with source references and effective date included.
- [x] Required input groups identify their source and unknown validation rules are recorded as open questions.
- [x] Validation Error, Draft Saved, Activation Success and the M-F02-03 overlay are documented.
- [x] M-F02-03 is a real confirmation dialog over the Editor, not a separate screen or a renamed full-page Editor.
- [x] Protected edit/activation and completed-change audit behavior are traced to shared FR-04-03 and FR-04-04.
- [x] Navigation and screen names follow Screen Transition §5 and the shared UI Conventions.
- [ ] Exact Figma frame/modal node links and visual review against F-01 are verified by the team.
- [ ] Open questions 1–4 and 6–7 are clarified with the system owner/team.
- [ ] Peer review is completed and the draft is marked reviewed.
