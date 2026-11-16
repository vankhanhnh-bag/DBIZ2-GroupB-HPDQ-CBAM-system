# Screen Spec: S-F01-04 Batch Carbon Intensity

> **Product:** EcoTrace — HPDQ Carbon Management & Reporting System  
> **Status:** Draft for team review  
> **Source of truth:** UC-03, SD-UC-03, AD-UC-03, FR-01-03 / FR-01-04 in W1, Screen List and UI Conventions.

| Field | Value |
|---|---|
| Screen ID | S-F01-04 |
| Screen name | Batch Carbon Intensity |
| Actor | Production Department |
| Feature | F-01 Carbon Traceability & Product Carbon Footprint Management |
| Use case | UC-03 Access batch carbon intensity |
| Activity diagram | AD-UC-03 — Must-only background calculation path triggered when production, energy and applicable emission-factor inputs are available |
| Sequence diagram | SD-UC-03 — Must-only calculation path: production and energy inputs are supplied for the same batch; applicable methodology / emission factors are retrieved; batch carbon intensity is calculated |
| Linked requirements | FR-01-03 (Must — calculation); FR-01-04 (Should — user selection and display of calculated intensity and contributing data sources) |
| Screen List priority | Must (as currently recorded in Screen List) |
| Priority discrepancy | W1 classifies FR-01-03 as Must and FR-01-04 as Should. SD-UC-03 / AD-UC-03 explicitly exclude the FR-01-04 user selection/display interaction from the Must-only calculation path. This screen's user-facing display behavior therefore maps to FR-01-04 (Should), while the calculation is performed in the background under FR-01-03 (Must). Confirm whether the Screen List priority should remain Must or be changed to Should. |
| Mockup / Figma plugin | EcoTrace — Build S-F01-04 States; creates five editable state frames on page 04 — F-01 Batch Carbon Intensity |
| Source data categories | Production data; energy and operational data; applicable GHG methodology and emission factors |
| Important limitation | Exact calculation formula, unit, process boundary, factor-selection rule, energy-source mapping, persistence/retrieval design and failure handling are not fully specified in the current source documents. |

## 1. Purpose

**Shown when:** an authorized Production Department user opens Batch Carbon Intensity to select a production batch and view the calculated carbon intensity and its contributing data sources.

**The user does:** selects a production batch for review.

**The system does:** provides the calculated batch carbon intensity and its contributing data sources. The underlying calculation is triggered when production data, energy data and applicable emission-factor data are available for the same batch. EcoTrace uses the applicable GHG methodology and emission factors maintained through UC-02.

**Calculation versus display:** FR-01-03 (Must) defines the calculation. FR-01-04 (Should) defines the user-facing selection and display. The current SD-UC-03 and AD-UC-03 cover only the Must calculation path and explicitly exclude the selection/display interaction.

**The user leaves this screen when:** they return to the workspace or navigate to another permitted feature. No separate next screen is specified by UC-03.

## 2. Mockup

The Figma plugin creates five editable 1440 × 1024 frames on a dedicated page:

| Frame | State |
|---|---|
| `S-F01-04 — Default` | No batch selected; user can select a production batch |
| `S-F01-04 — Loading · Retrieving Intensity` | Selected batch and its calculated result/source data are being retrieved |
| `S-F01-04 — Success · Intensity Available` | Calculated batch carbon intensity and contributing data sources are displayed |
| `S-F01-04 — No Calculated Result` | A selected batch has no calculated intensity currently available |
| `S-F01-04 — System Error` | The screen cannot retrieve the batch intensity data |

The visual language follows `ui-conventions.md`: Hòa Phát navy, EcoTrace blue/mint accents, white content surfaces, compact operational-user sidebar, top app bar, clear batch selector, readable source panels and restrained banners.

The standard operational-user sidebar contains Overview, applicable F-01 / F-02 / F-03 destinations and Help & Support. It does not show F-04 administration links.

No real batch identifiers, numeric intensity values, units, methodology names or emission-factor values are fabricated. Placeholder values in the Figma frames are labelled as prototype placeholders.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation / behaviour |
|---|---|---|---|---|---|
| 1 | Operational application shell | Sidebar + top bar | EcoTrace navigation; signed-in Production Department role | Yes | Show Overview, applicable F-01/F-02/F-03 destinations and Help & Support only; hide F-04 administration navigation. Authorization remains enforced by the platform (FR-04-03). |
| 2 | Breadcrumb and page title | Header | Carbon Traceability & PCF / Batch Carbon Intensity | Yes | Identifies the current screen. |
| 3 | Production batch selector | Select / searchable input | Batch identifier supplied by Production / available batch records | Yes | User selects the production batch whose calculated intensity is to be viewed. The authoritative batch lookup source and selector behaviour are not specified. |
| 4 | View intensity | Primary button | Static action label | Yes | Requests the user-facing retrieval/display described by FR-01-04 (Should). This is not the FR-01-03 calculation trigger. |
| 5 | Batch context | Summary fields | Selected batch identifier and any returned batch context | Yes, after selection | Preserve the identifier supplied by the source. Do not invent product, line, plant or date fields unless present in the response contract. |
| 6 | Batch carbon intensity | Primary result card | Calculated output of FR-01-03, retrieved for display under FR-01-04 | Yes, in success state | Show the returned value and its confirmed unit/denominator. The unit is not specified in W1 and must not be invented. |
| 7 | Production data source | Read-only source item | Production inputs used for the same batch | Yes, in success state | Identify the contributing production-data source where returned. Exact source system is not resolved in the current Context Diagram / W0 / Architecture. |
| 8 | Energy and operational data source | Read-only source item | Energy data used for the same batch | Yes, in success state | Identify the contributing energy-data source where returned. Do not assume an unconfirmed SAP / SCADA / meter endpoint. |
| 9 | Methodology and emission-factor source | Read-only source item | Applicable methodology version, factor values and units from the GHG methodology / emission-factor library (UC-02 / FR-01-02) | Yes, in success state | Display only source metadata returned by the system. Do not infer factor values or effective-date selection rules. |
| 10 | Loading feedback | Inline progress / loading state | Retrieval of selected batch carbon intensity and source data | Conditional | Show a progress state while the screen request is active; disable repeated activation during the request. |
| 11 | No-result notice | Empty / information banner | No calculated intensity currently available for the selected batch | Conditional | This is a proposed UI empty state. The exact system response when inputs are missing or calculation is not yet available is not specified in FR-01-03 / AD-UC-03. |
| 12 | System-error notice | Error banner | Retrieval failure / unavailable service | Conditional | This is a generic presentation fallback based on UI Conventions. The UC-03 source diagrams do not specify a system/network failure branch. |
| 13 | Data-source summary | Read-only list / panel | Contributing production, energy and applicable factor/methodology categories | Recommended | Supports FR-01-04's requirement to show contributing data sources. Do not imply a more detailed calculation breakdown unless the data contract provides it. |

## 4. States

The five states below provide a complete presentation set for the screen. Only the success/calculated content is directly required by the source requirements. Loading is transient presentation feedback. The no-result and system-error states are proposed UI fallbacks because UC-03 does not define their exact response behaviour; they must not be interpreted as approved business rules.

| State | What the user sees | Trigger | Source / notes |
|---|---|---|---|
| **1. Default — Select a batch** | Page title, a labelled Production Batch selector, a **View intensity** button, and neutral empty placeholders for Batch Carbon Intensity and the three contributing source categories. Helper text explains that the screen displays a calculated intensity for the selected batch. | Production Department user opens S-F01-04. | Screen List describes a user selecting a production batch; FR-01-04 describes selection and display (Should). This user interaction is excluded from the Must-only SD-UC-03 / AD-UC-03 path. |
| **2. Loading — Retrieving intensity** | Selected batch identifier remains visible. The result card and source panel show loading placeholders. A progress message reads **Retrieving batch carbon intensity and contributing data sources…** The View intensity button is temporarily disabled. | User requests the display for a selected batch. | Transient presentation state inferred from the FR-01-04 retrieval/display operation; not a persisted batch status. |
| **3. Success — Intensity available** | Batch identifier; a prominent Batch Carbon Intensity value with unit returned by the system; and contributing source information grouped as Production data, Energy data, and Applicable methodology & emission factors. The result is labelled **Calculated**. | EcoTrace returns a calculated intensity and contributing data sources for the selected batch. | FR-01-03 output is Batch carbon intensity [Calculated]; FR-01-04 (Should) requires the user-facing value and contributing sources. The exact formula and unit are not specified. |
| **4. No Calculated Result — Result unavailable** | The selected batch remains visible. The result area says **No calculated carbon intensity is currently available for this batch.** The source panel identifies the categories that are not available only if the response provides that detail. The user can return to the batch selector and choose another batch. | The selected batch has no calculated result available for display. | Proposed empty state only. AD-UC-03 says missing-input and calculation-failure behaviour is not specified; do not assert a cause or prescribe a remediation workflow until the team defines it. |
| **5. System Error — Unable to retrieve data** | Error banner: **Batch carbon intensity could not be retrieved.** Supporting text: **EcoTrace could not load the selected batch result or its contributing sources. Return to batch selection and try another request later.** No numeric result is shown. | A retrieval request cannot be completed because the data/service is unavailable. | Generic presentation fallback based on UI Conventions §4. No system/network failure branch or retry policy is defined in UC-03; confirm before treating this as a formal requirement. |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Production batch selector | Selects a production batch | Retains the selected batch identifier as the target for result retrieval. | Stays on S-F01-04 |
| 2 | View intensity | Clicks | Requests the calculated batch intensity and contributing data sources; displays loading feedback while retrieving. | Stays on S-F01-04 |
| 3 | Batch carbon intensity result | Reviews returned value | Displays the calculated value and its confirmed unit/denominator. | Stays on S-F01-04 |
| 4 | Contributing source panel | Reviews source details | Displays production, energy and applicable methodology/emission-factor source information returned by EcoTrace. | Stays on S-F01-04 |
| 5 | No-result notice | Returns to batch selection | Allows the user to select a different batch. No recalculation trigger is added to this screen. | Stays on S-F01-04 |
| 6 | System-error notice | Returns to batch selection | Clears the current result view and lets the user select a batch again. No automatic retry is assumed. | Stays on S-F01-04 |
| 7 | Global navigation | Selects another permitted feature | Opens the selected EcoTrace destination subject to authorization. | Target screen outside UC-03 |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | Batch carbon intensity is calculated when production, energy and applicable emission-factor data are available for the same batch. | FR-01-03 (Must) |
| SR-002 | The calculation uses production data, energy data and applicable emission factors. | FR-01-03; SD-UC-03 |
| SR-003 | The applicable GHG methodology and emission factors are retrieved from the library maintained through UC-02 / FR-01-02. | SD-UC-03; supporting FR-01-02 |
| SR-004 | The user-facing screen displays the calculated carbon intensity and its contributing data sources when a Production Department user selects a batch. | FR-01-04 (Should) |
| SR-005 | Do not display a numeric batch carbon-intensity value without its confirmed unit and denominator. | UI Conventions §2; unit is an open question in AD-UC-03 |
| SR-006 | Do not invent a calculation formula, process boundary, factor effective-date rule or source-system endpoint. | AD-UC-03 open questions 1–3 |
| SR-007 | Do not imply that the calculated intensity is persisted unless the data model / response contract confirms storage. FR-01-04 expects retrieval, but FR-01-03 and AD-UC-03 do not specify a persistence action. | FR-01-03 / FR-01-04; AD-UC-03 open question 2 |
| SR-008 | The no-result and system-error states are proposed presentation fallbacks; do not treat them as approved system outcomes until UC-03 failure behaviour is defined. | AD-UC-03 failure end state / open question 4 |
| SR-009 | The standard operational-user sidebar does not display F-04 administration links. Sign-in is not a sidebar item. | UI Conventions; shared authorization policy FR-04-03 |
| SR-010 | Status and error outcomes must be communicated with text/icon as well as colour. | UI Conventions §5 |

## 7. Linked requirements

| ID | Priority | What this screen does for it |
|---|---|---|
| FR-01-03 | Must | The underlying service calculates batch carbon intensity from production, energy and applicable emission-factor data when those inputs are available for the same batch. The calculation occurs in the background; it is not a user-triggered calculation action on this screen. |
| FR-01-04 | Should | Provides the user-facing batch selection and display of the calculated carbon intensity and its contributing data sources. |

**Priority note:** W1 classifies FR-01-03 as Must and FR-01-04 as Should. The current Screen List marks S-F01-04 as Must while linking both FRs. Confirm the intended screen priority and whether the Should display capability is included in the Must delivery baseline.

## 8. Responsive and accessibility notes

- Follow the shared EcoTrace application shell from `ui-conventions.md`: standard-user sidebar (no F-04 admin links), top bar, breadcrumbs and consistent page-title placement.
- On smaller widths, collapse the sidebar into a drawer and stack the batch selector, intensity result and source panels in one column.
- Keep the selected batch identifier visible in loading, success, no-result and error states.
- Use an explicit label for the batch selector; do not rely on placeholder text alone.
- Display the carbon-intensity unit alongside the value, never in a distant footnote.
- Use semantic headings for the result and source groups. Make each contributing source identifiable by its text label, not by colour alone.
- During loading, show a readable status message and a visible progress indicator; do not rely on animation alone.
- Pair success, no-result and error colours with text and/or icons. Ensure keyboard focus and reading order follow the visual order.
- Ensure body text contrast at least 4.5:1 and interactive targets at least 24 × 24 CSS px, preferably 40–44 px for common controls, per UI Conventions §5.

## 9. Open questions

| # | Question | Blocking? | Status / default |
|---|---|---|---|
| 1 | Should S-F01-04 remain Must, even though W1 classifies the user-facing FR-01-04 display function as Should and the Must-only UC-03 sequence/activity diagrams explicitly exclude the display interaction? | Yes — for final priority / scope sign-off | Open. This spec preserves the Screen List's Must label but flags the inconsistency. |
| 2 | What is the authoritative unit and denominator for batch carbon intensity? | Yes — for final value formatting | Open. Figma uses a neutral value and unit placeholder. |
| 3 | Which exact production, energy and operational data systems are authoritative, and how are they mapped to the Integration Adapter? | Yes — for source labels and integration detail | Open. The screen uses generic source categories rather than assuming SAP, SCADA or meter endpoints. |
| 4 | What calculation formula, process boundary and emission-factor effective-date rule apply? | Yes — for detailed calculation breakdown | Open. No formula or selection rule is invented. |
| 5 | Must the calculated batch intensity be persisted, and which data entity/store supports FR-01-04 retrieval? | Yes — before claiming persistence | Open. The screen displays the calculated result but does not claim a persistence operation. |
| 6 | What should the UI show if required production, energy or factor data are missing or inconsistent? | Yes — for formalizing the no-result state | Open. The no-result frame is a proposed empty-state design only. |
| 7 | What should happen when the result/source retrieval service is unavailable? Is a manual retry supported? | No for the happy path; yes before approving error recovery | Open. The system-error frame is a generic UI fallback; no automatic retry is assumed. |

---

## Completion checklist

- [x] Screen ID, actor and feature match the Screen List / W0 baseline.
- [x] FR-01-03 calculation path is separated from FR-01-04 user-facing display.
- [x] Five presentation states are specified: Default, Loading, Success, No Calculated Result and System Error.
- [x] The success state shows the calculated intensity and the three source categories used by the calculation.
- [x] No formula, unit, source endpoint, persistence action or unconfirmed recovery rule is invented.
- [x] Standard operational-user sidebar excludes F-04.
- [x] Priority discrepancy between Screen List and W1/UC-03 diagrams is recorded.
- [ ] Figma plugin executed and five frames visually reviewed by the team.
- [ ] Open questions 1–5 confirmed against the UC-03 / data contract baseline.
- [ ] Screen List priority reconciled with W1 and W4.
