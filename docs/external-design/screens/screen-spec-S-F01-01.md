# Screen Spec: S-F01-01 Shipment-level PCF Calculation

> **Product:** EcoTrace — HPDQ Carbon Management & Reporting System  
> **Status:** Draft — initial Figma frame exists; result and missing-data state frames are specified below but not yet placed on the Figma canvas.  
> **Source of truth:** UC-04 sequence diagram, AD-UC-04, FR-01-05 in W1, Screen List and UI Conventions.

| Field | Value |
|---|---|
| Screen ID | S-F01-01 |
| Screen name | Shipment-level PCF Calculation |
| Actor | Export Sales & Marketing |
| Priority | Must |
| Feature | F-01 Carbon Traceability & Product Carbon Footprint Management |
| Use case | UC-04 Calculate shipment-level PCF |
| Activity diagram | AD-UC-04 — select shipment, check required shipment/batch references, request logistics emissions, calculate, store and review shipment PCF |
| Linked sequence | SD-UC-04 — shipment context from SAP S/4HANA; constituent-batch linkage and carbon intensity; logistics emissions from Hoa Phat Logistics; calculation, storage and display |
| Mockup | [Figma — S-F01-01 initial frame](https://www.figma.com/design/FTJibQtZrHIMHfwsJx7lT9/EcoTrace?node-id=11-2) |
| Status values | `Calculated` is the only shipment-PCF state explicitly named in AD-UC-04. Do not invent additional persisted business statuses. |

## 1. Purpose

**Shown when:** an authorized Export Sales & Marketing user opens Shipment PCF from the EcoTrace workspace or follows the UC-04 step when a shipment PCF is required.

**The user does:** selects a shipment and requests calculation of its shipment-level Product Carbon Footprint (PCF).

**The system does:** retrieves shipment/order context and product/batch identifiers from SAP S/4HANA; retrieves constituent-batch linkage and batch carbon intensities from the EcoTrace carbon-data/traceability records; retrieves applicable logistics emissions from Hoa Phat Logistics; calculates the shipment-level PCF; stores the PCF record; and displays the result.

**The user leaves this screen when:** they navigate back to the EcoTrace workspace or continue to another feature using the shipment PCF. UC-04 itself does not specify a dedicated post-calculation destination.

## 2. Mockup

The initial-state frame is already present in the Figma file. It uses the agreed EcoTrace visual direction: navy sidebar, white work area, blue primary action, mint accent, breadcrumb and dashboard-style panels.

- [Open S-F01-01 initial state in Figma](https://www.figma.com/design/FTJibQtZrHIMHfwsJx7lT9/EcoTrace?node-id=11-2)
- Two additional states are specified in section 4. They are not yet represented as separate Figma frames because the Figma Starter-plan MCP edit limit was reached during this design session.

The supplied concept mockup is a visual reference only. It is not the source for shipment values, calculation results, units, or business rules.

## 3. Element inventory

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation / behaviour |
|---|---|---|---|---|---|
| 1 | App shell | Navigation shell | EcoTrace feature navigation; current signed-in role | Yes | For standard operational users, show Overview, applicable F-01 / F-02 / F-03 destinations and Help & Support only; hide F-04 administration links. Sign-in is a shared entry screen, not a sidebar item. F-04 navigation is reserved for authorized System Admins / Auditors. Authorization remains enforced by the platform (FR-04-03). |
| 2 | Breadcrumb and page title | Header | Carbon Traceability & PCF / Shipment PCF | Yes | Current page is clearly identified. |
| 3 | Shipment selector | Select / searchable input | Shipment identifier and related order/product context retrieved from SAP S/4HANA (SD-UC-04) | Yes | A shipment must be selected before calculation is requested. Preserve the source-system identifier. |
| 4 | Calculate PCF | Primary button | Static action label | Yes | Enabled only when a shipment is selected; disable while the request is being processed. |
| 5 | Data lineage and source systems | Read-only lineage / source-system panel | Four ordered steps: shipment and order context — SAP S/4HANA; batch linkage and carbon intensity — EcoTrace carbon data / traceability; applicable logistics emissions — Hoa Phat Logistics; shipment-level PCF aggregation — EcoTrace PCF service | Yes, after a shipment is selected | Show the source or processing step and its returned state. Use text labels such as `Available`, `Retrieved` or `Unavailable` as supported by the response. In a system/network error state, distinguish a source request that failed from a downstream step that was not completed whenever that information is available. Do not imply that every source system failed when the failure is only known to affect one request. |
| 6 | Calculation basis | Read-only calculation-basis panel | The two input categories used by FR-01-05: constituent-batch carbon intensity and applicable logistics emissions | Yes, after a shipment is selected | Display the two input categories as separate, non-editable items. Show an input as `Unavailable` when its value was not returned. Do not invent a mathematical formula, input value, unit or aggregation rule beyond FR-01-05. |
| 7 | Results | Summary cards and shipment-level PCF result card | Summary metrics for shipment-level PCF, constituent batches, batch carbon intensity and logistics emissions; final shipment-level PCF result | Yes, after the calculation request; content depends on its outcome | When calculation succeeds, show the returned values and their source units; show the PCF only with its confirmed unit and denominator. When a required source/network request fails, show `Unavailable` for values that were not returned and `No PCF result available` for the final result. Do not substitute zero, a stale result or an invented unit. |
| 8 | Conditional error banner — PCF request could not be completed | Error banner / alert | Heading: **We couldn't complete the PCF request**. Body: **One or more required source or network requests failed. No shipment PCF result is available.** | Conditional — shown when a required source-system or network request fails | Place below the shipment-selection panel as shown in the System / Network Error mockup. Pair the error icon and message; do not rely on red colour alone. Keep the selected shipment visible. Name a specific failed source only when the response identifies it. This banner communicates the failed request; it does not define retry, rollback or data-retention behaviour. |
## 4. States

| State | What the user sees | Trigger | Notes / source |
|---|---|---|---|
| Initial / no shipment selected | Page title, shipment selector, disabled Calculate PCF button, and neutral empty placeholders for shipment context, batch inputs, logistics emissions and PCF result. | User opens S-F01-01. | This is the initial Figma frame. No shipment-specific result is shown. |
| Loading / calculating | Selected shipment remains visible; Calculate PCF is disabled; a progress indicator and concise message explain that EcoTrace is retrieving shipment, batch and logistics inputs and calculating PCF. | User selects a shipment and requests PCF; the sequence is in progress. | Transient presentation state inferred from the multi-step SD-UC-04 request. It is not a new FR or persisted PCF status. |
| Missing required shipment/batch data | PCF result is not shown. A prominent notice identifies the missing shipment reference, batch carbon intensity or shipment-to-batch linkage; the user can correct the source data or select another shipment and retry. | AD-UC-04 decision: shipment, batch intensities and linkage are not all available. | AD-UC-04 describes the notice and uses SH-002 / B-02 as an acceptance-test example. The failure behaviour is currently linked to proposed FR-01-09, which is not yet present in W1; see section 9. |
| Success / calculated | A success indicator labelled **Calculated**; selected shipment context; constituent batches and retrieved batch carbon intensities; applicable logistics emissions; and the shipment-level PCF result returned by the calculation. Indicate that the PCF record has been stored. | Calculation completes and the result is stored in PCF Records. | Success end state from AD-UC-04 and steps 9–12 of SD-UC-04. `Calculated` is the state named in AD-UC-04. |
| System / network error | **Not specified for UC-04.** Do not add a detailed retry, rollback or data-retention rule until the team defines the expected behaviour. | External source or service is unavailable. | Open question; general UI message patterns in ui-conventions.md do not define this use-case-specific recovery behaviour. |

### State content examples

Use the identifiers in the AD-UC-04 acceptance tests only as clearly labelled test data:

- **Success example:** shipment `SH-001`, constituent batches `B-01` and `B-02`; linkage and batch intensities are available.
- **Missing-data example:** shipment `SH-002`; batch `B-02` has no recorded carbon intensity.

These are acceptance-test examples, not confirmed live HPDQ records. Do not populate production-looking numeric PCF or emissions values from the original concept mockup.

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Shipment selector | Selects a shipment | Shipment selection is retained as the input for UC-04. Shipment/order context is retrieved from SAP S/4HANA when the request runs. | Stays on S-F01-01 |
| 2 | Calculate PCF | Clicks | Starts the UC-04 request; shows loading feedback and prevents duplicate submission. | Stays on S-F01-01 |
| 3 | Missing-data notice | Reviews the named missing shipment/batch item | No PCF is calculated or displayed. User corrects the source data outside this screen or chooses another shipment, then retries. | Stays on S-F01-01 |
| 4 | Calculate PCF (retry) | Clicks after required data is available | Repeats the UC-04 calculation request. | Stays on S-F01-01 |
| 5 | PCF result | Reviews the returned value and contributing inputs | Displays the shipment-level PCF and indicates that the record has been stored. | Stays on S-F01-01 |
| 6 | Breadcrumb / feature navigation | Opens another permitted feature or returns to the workspace | Navigates using the global EcoTrace application shell. | Target screen depends on the selected feature; outside UC-04 |

No additional actions such as editing the calculation, manually overriding emissions, exporting a PCF report, or verifying/certifying the result are defined by UC-04 / FR-01-05. Do not add them as functional controls without a linked requirement.

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | A shipment must be selected before the user can request a shipment-level PCF calculation. | UC-04 trigger and SD-UC-04 step 1 |
| SR-002 | The PCF calculation uses the carbon intensity of constituent batches and applicable shipment logistics emissions. | FR-01-05 |
| SR-003 | The system retrieves shipment/order context and product/batch identifiers from SAP S/4HANA. | SD-UC-04 steps 3–4 |
| SR-004 | The system obtains constituent-batch linkage and batch carbon intensities from the carbon-data / traceability records. | SD-UC-04 steps 5–6; supporting FR-01-03 and FR-01-07 |
| SR-005 | The system retrieves applicable logistics emissions from Hoa Phat Logistics before calculating the shipment PCF. | SD-UC-04 steps 7–8 |
| SR-006 | The calculated shipment-level PCF is stored in PCF Records and then displayed to the requester. | SD-UC-04 steps 10–12; verify the persistence criterion against W1 (see section 9). |
| SR-007 | If shipment/batch references, batch carbon intensity or their linkage are missing, the PCF is not calculated and the user is shown a missing-data notice. | AD-UC-04 decision d1 and failure end state; currently associated with proposed FR-01-09, pending W1 confirmation. |
| SR-008 | Do not display a numeric PCF value without its confirmed unit and denominator. | UI Conventions §2; confirm the unit/denominator in the PCF data contract. |
| SR-009 | Status must be communicated with text (for example, **Calculated** or **Missing data**) and not colour alone. | UI Conventions §5 |

## 7. Linked requirements

| ID | What this screen does for it |
|---|---|
| FR-01-05 (Must) | Provides the shipment selection and result-display interface for calculating shipment-level PCF from constituent-batch carbon intensity and applicable logistics emissions. |
| FR-01-03 (Must, supporting) | Displays the batch carbon intensity records used as inputs; the batch calculation itself is performed in the background and is not a user action on this screen. |
| FR-01-07 (Must, supporting) | Uses the constituent-batch / shipment traceability linkage needed to identify the batches contributing to the selected shipment. Linkage maintenance is not performed on this screen. |
| FR-01-09 (proposed; not in W1) | Missing-data notice and calculation-blocking branch in AD-UC-04. This must not be treated as a confirmed FR until added to W1 and approved. |

## 8. Responsive and accessibility notes

- Follow the shared EcoTrace application shell from ui-conventions.md: desktop sidebar and top bar; collapse the sidebar into a drawer below the agreed desktop breakpoint.
- On narrow screens, stack the shipment selector, summary cards and calculation/source panels in one column. Do not hide the missing-data notice or PCF result.
- Keep the shipment identifier visible in both the loading and result states so the user can identify which shipment is being calculated.
- Use a visible label for the shipment selector; do not rely on placeholder text alone.
- Keep the Calculate PCF button keyboard accessible and show a visible focus state. Prevent duplicate activation while loading.
- Use semantic heading order and readable table/list labels for batch and source records.
- Pair status colours with text and/or an icon. Ensure body text meets the contrast requirement in ui-conventions.md.
- Show numeric values with units from the returned data contract; do not infer units or round away precision required by the source.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | What is the authoritative unit and denominator for the shipment-level PCF value returned by FR-01-05? | Yes, for final UI value formatting | Open. Use a neutral value placeholder until confirmed in the PCF data contract. |
| 2 | AD-UC-04 introduces FR-01-09 for missing shipment/batch data, but FR-01-09 is not present in W1. Will the team add and approve this requirement? | Yes, for final FR traceability of the missing-data state | Open. The state is included in this design specification because it is explicitly shown in AD-UC-04, but the FR mapping remains provisional. |
| 3 | What should EcoTrace do if applicable logistics emissions are missing or invalid after the logistics request? | Yes, for this failure branch | Open. SD-UC-04 includes the logistics request but does not define a failure response; AD-UC-04's current decision occurs before that request. |
| 4 | Does FR-01-05's approved acceptance criterion explicitly require persistent storage of the calculated shipment PCF, or should storage be specified separately? | No, but required before final requirement sign-off | Open for W1 traceability. SD-UC-04 includes storing the PCF in PCF Records; the current W1 L2 wording focuses on calculation and does not explicitly state persistence. |
| 5 | Should a network/source-system failure keep the shipment selected and allow a retry, or return the screen to its initial state? | No, unless retry behaviour is included in v1 | Open. Do not assume a recovery rule in this version. |

---

## Completion checklist

- [x] Screen ID, actor, feature, priority and UC-04 are aligned with the Screen List.
- [x] Initial state is represented by the current Figma frame.
- [x] Success state is specified from SD-UC-04 and AD-UC-04.
- [x] Missing shipment/batch data state is specified from AD-UC-04 and marked with its unconfirmed FR mapping.
- [x] Loading feedback is identified as a transient UI state, not a new persisted business status.
- [x] Displayed data fields are traced to the named source systems or FRs.
- [x] Unspecified numeric values and PCF units are not invented.
- [ ] Result state frame added to Figma.
- [ ] Missing-data state frame added to Figma.
- [ ] PCF unit/denominator confirmed by the team.
- [ ] FR-01-09 confirmed or removed/rewritten in AD-UC-04 and W1.
- [ ] Storage behaviour and missing-logistics-data behaviour reconciled with the approved requirements.
