# Screen Spec: S-F01-02 Supplier & Raw Material Carbon Data Entry

> **Product:** EcoTrace — HPDQ Carbon Management & Reporting System  
> **Status:** Draft for team review  
> **Source of truth:** UC-01, SD-UC-01, AD-UC-01, FR-01-01 in W1, Screen List and UI Conventions.

| Field | Value |
|---|---|
| Screen ID | S-F01-02 |
| Screen name | Supplier & Raw Material Carbon Data Entry |
| Actor | Procurement & Raw Materials Department (called Procurement Department in UC-01 / SD-UC-01) |
| Priority | Must |
| Feature | F-01 Carbon Traceability & Product Carbon Footprint Management |
| Use case | UC-01 Submit supplier & raw material carbon data |
| Activity diagram | AD-UC-01 — submit delivery, validate required information, correct/resubmit when invalid, store the record, review success/failure outcome |
| Sequence diagram | SD-UC-01 — submit supplier delivery; validate; store; return success or failure outcome |
| Linked requirement | FR-01-01 (Must) |
| Mockup / Figma plugin | EcoTrace — Build S-F01-02 States; creates five editable state frames on page 02 — F-01 Supplier Data Entry |
| Inputs | Supplier identity; raw material type; quantity; supplier-declared carbon data |
| Important limitation | Exact validation rules, quantity/carbon units, supplier lookup method, and post-save-failure input retention are not defined in the current source documents. |

## 1. Purpose

**Shown when:** an authorized Procurement & Raw Materials Department user opens Supplier & Raw Material Data Entry to submit a supplier delivery together with the supplier's raw-material carbon information.

**The user provides:** supplier identity, raw material type, quantity, and supplier-declared carbon data for the delivery.

**The system does:** validate the submitted delivery information; if it is complete and valid, store the supplier/raw-material carbon record and return a submission outcome. If information is missing or invalid, the system returns validation feedback so Procurement can correct and resubmit.

**The user leaves this screen when:** they review a successful submission outcome, review an unsuccessful save outcome, or navigate to another permitted workspace feature. The current UC-01 flow does not specify a dedicated post-submit destination.

## 2. Mockup

The Figma plugin creates five editable 1440 × 1024 frames on a dedicated page:

| Frame | State |
|---|---|
| S-F01-02 — Default | Empty data-entry form |
| S-F01-02 — Loading · Submitting | Submission request in progress |
| S-F01-02 — Validation Error | Correction required; user can resubmit |
| S-F01-02 — Save Failure | Record was not stored; no retry action is shown because AD-UC-01 does not define a save-failure retry |
| S-F01-02 — Success | Record stored and submission confirmation shown |

The visual language follows ui-conventions.md: Hòa Phát navy, EcoTrace mint/blue accents, white content surfaces, compact sidebar, top app bar, clear form labels, restrained borders and readable feedback banners.

The screen must not use invented live supplier names, material values, quantities, carbon figures or units. Any sample content in the Figma frames is explicitly placeholder content.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation / behaviour |
|---|---|---|---|---|---|
| 1 | Application shell | Sidebar + top bar | EcoTrace navigation; signed-in role / department | Yes | Use the standard operational-user sidebar: Overview, F-01 / F-02 / F-03 destinations and Help & Support. Do not show F-04 admin links on this screen. Sign-in is not a sidebar item; F-04 navigation is reserved for authorized System Admins / Auditors (FR-04-03). |
| 2 | Breadcrumb and page title | Header | Carbon Traceability & PCF / Supplier & Raw Material Data | Yes | Identifies the current screen. |
| 3 | Supplier identity | Text input / searchable select (control type TBD) | Supplier identity supplied by Procurement for the delivery (FR-01-01) | Yes | Must capture the supplier identity. Whether users select an existing supplier or enter free text is not defined. |
| 4 | Raw material type | Select / text input (control type TBD) | Raw material type supplied by Procurement for the delivery (FR-01-01) | Yes | Must capture the material type. The controlled vocabulary / allowed values are not defined. |
| 5 | Quantity | Numeric input | Delivery quantity supplied by Procurement (FR-01-01) | Yes | Must capture quantity. Permitted units, decimal precision and range checks are not defined. |
| 6 | Supplier-declared carbon data | Input / structured data field (control type TBD) | Carbon data declared by the supplier for this delivery (FR-01-01) | Yes | Must capture the supplier-declared carbon data. Its precise schema, unit, basis and validation range are not defined. |
| 7 | Submit supplier delivery | Primary button | Static action label | Yes | Sends the four input groups for validation and storage. Do not add additional required fields unless approved in the data contract. |
| 8 | Validation feedback | Error banner / field feedback | Validation errors returned by the system (SD-UC-01, AD-UC-01) | Conditional | Explain that information is missing or invalid and direct the user to correct affected fields. Exact field-level messages depend on validation rules. |
| 9 | Submission progress | Loading indicator / disabled button | Submission request in progress | Conditional | Prevent duplicate submission while the request is pending. This is a transient UI state, not a persisted record status. |
| 10 | Save-failure feedback | Error banner | Save failure outcome returned by the service / store (SD-UC-01, AD-UC-01) | Conditional | Clearly state that the delivery record was not saved. Do not imply successful persistence. |
| 11 | Success confirmation | Success banner / confirmation panel | Save confirmation returned by the data store through the service (SD-UC-01) | Conditional | Confirm that the supplier/raw-material carbon record was stored. Do not invent a generated record ID or timestamp. |
| 12 | Captured data summary | Read-only summary | The four submitted values | Success state | Show values returned / confirmed by the saved record. If actual values are not available in the response contract, do not fabricate them. |

## 4. States

The five states below map to the initial form and the outcomes described by SD-UC-01 / AD-UC-01. Loading is a transient presentation state between the user submission and the system outcome.

| State | What the user sees | Trigger | Source / notes |
|---|---|---|---|
| **1. Default — Ready to submit** | Empty form with four labelled inputs: Supplier identity, Raw material type, Quantity, and Supplier-declared carbon data. Primary action: **Submit supplier delivery**. | Procurement opens S-F01-02. | FR-01-01 defines the four data groups; AD-UC-01 starts with submission of supplier delivery information. |
| **2. Loading — Submitting** | The entered form remains visible; inputs are temporarily disabled; the primary button shows **Submitting…** with a progress indicator. A neutral message explains that EcoTrace is validating and submitting the delivery. | User selects **Submit supplier delivery** and the request is being processed. | Transient feedback inferred from the request/response sequence. No new persisted business status is introduced. |
| **3. Validation Error — Correction required** | Error banner: **Some delivery information is missing or invalid.** Supporting text: **Review the validation feedback, correct the affected fields, and submit again.** Form remains editable; primary action returns to **Submit supplier delivery**. Field-level feedback is shown only for fields identified by the validator. | The system determines that required delivery information is missing or invalid. | SD-UC-01 validation alternative; AD-UC-01 decision d1 [no] and correction loop. Exact mandatory-field logic and error messages remain TBD. |
| **4. Save Failure — Submission not saved** | Error banner: **Submission not saved.** Supporting text: **EcoTrace could not store this supplier delivery record. No record was saved.** The screen does not show a successful confirmation. No automatic retry or **Try again** action is displayed in this version. | Information passed validation, but storage returned a failure outcome. | SD-UC-01 save-failure alternative; AD-UC-01 decision d2 [no]. AD-UC-01 ends after the unsuccessful outcome and does not specify a save-failure retry. Whether the entered values remain on screen is an open question. |
| **5. Success — Submission saved** | Success banner: **Supplier delivery submitted successfully.** A read-only confirmation area summarises Supplier identity, Raw material type, Quantity and Supplier-declared carbon data using the values from the successful submission. | The data store confirms that the record was stored successfully. | SD-UC-01 save-success alternative; AD-UC-01 decision d2 [yes], followed by confirmation and Procurement review. The confirmation wording is a team-confirmed outcome, not a separate SHALL sentence in W1. |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Supplier identity | Enters or selects supplier identity | Retains the supplied identity as part of the delivery input. | Stays on S-F01-02 |
| 2 | Raw material type | Enters or selects material type | Retains the supplied material type as part of the delivery input. | Stays on S-F01-02 |
| 3 | Quantity | Enters delivery quantity | Retains the supplied quantity. | Stays on S-F01-02 |
| 4 | Supplier-declared carbon data | Enters supplier-declared carbon data | Retains the declared data for submission. | Stays on S-F01-02 |
| 5 | Submit supplier delivery | Clicks | Sends the four data groups for system validation; loading feedback is shown while processing. | Stays on S-F01-02 |
| 6 | Validation feedback | Reviews errors, corrects affected fields and submits again | System validates the corrected information. The validation loop can repeat; the number of retries is not specified. | Stays on S-F01-02 |
| 7 | Save-failure outcome | Reviews unsuccessful submission outcome | System indicates that the record was not saved. No retry action is defined in AD-UC-01. | Stays on S-F01-02 / user navigates away |
| 8 | Success confirmation | Reviews successful submission outcome | System confirms that the record was stored. | Stays on S-F01-02 / user navigates away |
| 9 | Global navigation | Selects another permitted feature | Opens the selected EcoTrace destination subject to authorization. | Target screen outside UC-01 |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | The submission captures supplier identity, raw material type, quantity and supplier-declared carbon data for one supplier delivery. | FR-01-01 |
| SR-002 | The screen must not introduce additional business fields as required inputs unless approved in the UC-01 data contract. | FR-01-01; current source scope |
| SR-003 | The system validates submitted delivery information before attempting to store the supplier/raw-material carbon record. | SD-UC-01; AD-UC-01 d1 |
| SR-004 | If information is missing or invalid, the system returns validation feedback and Procurement can correct and resubmit. | SD-UC-01 validation branch; AD-UC-01 d1 [no] |
| SR-005 | The system stores the record only after the validation outcome is valid. | SD-UC-01; AD-UC-01 d1 [yes] |
| SR-006 | A storage failure must not be displayed as a successful submission; show the unsuccessful outcome. | SD-UC-01 save-failure branch; AD-UC-01 d2 [no] |
| SR-007 | A success confirmation is displayed only after the data store confirms that the record was stored. | SD-UC-01 save-success branch; AD-UC-01 d2 [yes] |
| SR-008 | Do not add a retry after storage failure or duplicate-submission/idempotency behaviour until the team defines it. | AD-UC-01 reading notes and open question 3 |
| SR-009 | Do not infer quantity units, carbon units, permitted ranges, decimal precision or raw-material code lists from the screen design. | Not specified in FR-01-01 / SD-UC-01 / AD-UC-01 |
| SR-010 | Status and validation outcomes must be conveyed with text/icon as well as colour. | UI Conventions §5 |

## 7. Linked requirements

| ID | What this screen does for it |
|---|---|
| FR-01-01 (Must) | Captures supplier identity, raw material type, quantity and supplier-declared carbon data for a supplier delivery; supports submission to the system for validation and storage. |

No other FR is assigned to this screen in the current Screen List / W1 mapping.

## 8. Responsive and accessibility notes

- Follow the shared EcoTrace application shell from ui-conventions.md: persistent desktop sidebar, top bar, breadcrumbs and consistent page-title placement.
- On smaller widths, collapse the sidebar into a drawer and stack the form and information panel into one column.
- Use explicit labels above all four fields; do not rely on placeholder text as the only label.
- Mark required fields consistently. Exact validation constraints and field-specific messages must come from the approved data contract.
- Keep keyboard order aligned with the visual form order: Supplier identity → Raw material type → Quantity → Supplier-declared carbon data → Submit supplier delivery.
- During submission, provide a visible progress indicator and a readable label; do not rely on animation alone.
- Validation errors must be associated with the affected controls and explained in text.
- Success and failure messages must remain understandable without colour; use iconography and text.
- Ensure body text contrast at least 4.5:1 and interactive targets at least 24 × 24 CSS px, preferably 40–44 px for common controls, per UI Conventions §5.

## 9. Open questions

| # | Question | Blocking? | Status / default |
|---|---|---|---|
| 1 | Which exact fields are mandatory and what validation rules apply to supplier identity, material type, quantity and supplier-declared carbon data? | Yes — for final validation and acceptance criteria | Open. The screen captures the four FR-01-01 data groups; it does not invent field-level rules. |
| 2 | Is supplier identity selected from a supplier master list, searched, or entered as free text? Is raw material type a controlled list? | Yes — for final control type | Open. Figma shows neutral text/select-style controls pending confirmation. |
| 3 | Which quantity units and carbon-data units/basis are supported, and must the unit be stored with the value? | Yes — for field validation and display | Open. The screen does not pre-populate a unit or allowed-value list. |
| 4 | After storage failure, should the previously entered values remain available on the screen, and is manual resubmission permitted? | No for the current AD flow; yes before adding retry behaviour | Open. Current AD-UC-01 ends after the unsuccessful outcome and contains no storage-failure retry. |
| 5 | Does the successful save response return a delivery-record ID and timestamp that should be displayed? | No | Open. No generated identifier or timestamp is assumed. |

---

## Completion checklist

- [x] Screen ID, actor, feature, priority and UC-01 match the Screen List / W0-W1 baseline.
- [x] All four FR-01-01 data groups appear in the element inventory.
- [x] Five states are specified: Default, Loading, Validation Error, Save Failure and Success.
- [x] Validation correction/resubmission loop matches AD-UC-01.
- [x] Storage failure and success are separate outcomes.
- [x] No save-failure retry action is invented.
- [x] Units, ranges, master-data controls and generated identifiers are marked as open questions.
- [ ] Figma plugin executed and five frames visually reviewed by the team.
- [ ] Open questions 1–3 confirmed against the supplier/raw-material data contract.
