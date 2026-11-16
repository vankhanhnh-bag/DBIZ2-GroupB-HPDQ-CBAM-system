# Screen Spec: S-F01-05 Scope 3 Assessment

> **Product:** EcoTrace — HPDQ Carbon Management & Reporting System  
> **Status:** Draft for team review  
> **Source of truth:** W0 Scope / UC-05 inventory, FR-01-06 in W1, Screen List and UI Conventions.  
> **Source limitation:** No SD-UC-05 or AD-UC-05 file is currently present in the repository. The workflow below is therefore limited to the W0/W1 requirement and the Screen List description; detailed business rules are not inferred.

| Field | Value |
|---|---|
| Screen ID | S-F01-05 |
| Screen name | Scope 3 Assessment |
| Actor | Procurement Department / Procurement & Raw Materials Department |
| Priority | Should |
| Feature | F-01 Carbon Traceability & Product Carbon Footprint Management |
| Use case | UC-05 Perform Scope 3 assessment |
| Sequence / activity diagrams | Not present in the current repository; request, processing and outcome details require confirmation |
| Linked requirement | FR-01-06 (Should) |
| Requirement statement | WHEN a Procurement Department user requests a Scope 3 assessment for a supplier or a raw material category, the system SHALL aggregate the associated supplier-declared and calculated emissions into a Scope 3 category summary. |
| Inputs | One assessment scope: a supplier OR a raw material category |
| Output | A Scope 3 category summary aggregating associated supplier-declared and calculated emissions |
| Important limitation | Scope 3 category taxonomy, aggregation formula, units, reporting boundary, date period, filters, data sufficiency rules and failure behaviour are not specified by the current FR. |

## 1. Purpose

**Shown when:** an authorized Procurement Department user opens Scope 3 Assessment to request a summary for a selected supplier or raw material category.

**The user does:** chooses an assessment basis (Supplier or Raw material category), selects one target within that basis, and requests the assessment.

**The system does:** aggregates the supplier-declared and calculated emissions associated with the selected scope and returns a Scope 3 category summary.

**The user leaves this screen when:** they review the returned summary or navigate to another permitted EcoTrace workspace feature. No dedicated post-assessment destination is specified in UC-05 / FR-01-06.

**Scope boundary:** the screen is a user-facing request and summary view. It does not define a new emissions-calculation formula, invent Scope 3 categories, add an approval workflow, or claim independent verification/certification.

## 2. Mockup

The Figma plugin creates five editable 1440 × 1024 frames on a dedicated page:

| Frame | State |
|---|---|
| `S-F01-05 — Default` | Choose Supplier or Raw material category and select the target |
| `S-F01-05 — Loading · Aggregating Emissions` | Request is being processed and associated emissions are being aggregated |
| `S-F01-05 — Success · Category Summary Available` | Scope 3 category summary is displayed |
| `S-F01-05 — No Associated Emissions` | Proposed empty state when the selected scope has no associated supplier-declared or calculated emissions available |
| `S-F01-05 — System Error` | Proposed technical-error fallback when the assessment cannot be retrieved or completed |

The visual language follows `ui-conventions.md`: Hòa Phát navy, EcoTrace blue/mint accents, white content surfaces, compact operational-user sidebar, top app bar, clear scope selector, restrained banners and readable summary tables.

The standard operational-user sidebar contains Overview, applicable F-01 / F-02 / F-03 destinations, and Help & Support. It **does not** show F-04 administration links.

No live supplier names, raw-material category names, emissions quantities, units, calculation period or Scope 3 category taxonomy are fabricated. Any placeholders in the Figma frames are clearly labelled as prototype placeholders.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation / behaviour |
|---|---|---|---|---|---|
| 1 | Operational application shell | Sidebar + top bar | EcoTrace navigation; signed-in Procurement role | Yes | Show Overview, applicable F-01/F-02/F-03 destinations and Help & Support only; hide F-04 administration navigation. Authorization remains enforced by the platform (FR-04-03). |
| 2 | Breadcrumb and page title | Header | Carbon Traceability & PCF / Scope 3 Assessment | Yes | Identifies the current screen. |
| 3 | Assessment basis | Segmented control / radio cards | Supplier OR Raw material category | Yes | User chooses one scope basis. The request must not combine both bases unless the business rule is later expanded. |
| 4 | Supplier selector | Searchable select | Supplier identity / records available to the user | Conditional | Used when Supplier is the chosen basis. Supplier master source and search fields are not defined in FR-01-06. |
| 5 | Raw material category selector | Select / searchable input | Raw material category available to the user | Conditional | Used when Raw material category is the chosen basis. Exact category list and taxonomy are not defined. |
| 6 | Generate Scope 3 Assessment | Primary button | Static action label | Yes | Requests an assessment for the selected supplier OR selected raw material category. Keep disabled until the user has selected a basis and target. |
| 7 | Assessment scope summary | Read-only summary | Selected basis and target | Yes, after selection | Preserves the exact selected supplier/category label in the result context. |
| 8 | Supplier-declared emissions contribution | Summary metric / table column | Supplier-declared emissions associated with the selected scope (FR-01-06) | Yes, when returned | Display returned values and units only. Do not infer how supplier declarations are normalized. |
| 9 | Calculated emissions contribution | Summary metric / table column | Calculated emissions associated with the selected scope (FR-01-06) | Yes, when returned | Display returned values and units only. Do not invent the source calculation or formula. |
| 10 | Scope 3 category summary | Data table / summary panel | Aggregated supplier-declared and calculated emissions grouped into a Scope 3 category summary (FR-01-06) | Yes, in success state | Use category labels, values and units returned by the assessment service. Exact category taxonomy and aggregation formula remain open questions. |
| 11 | Loading feedback | Inline progress / loading state | Assessment request and aggregation in progress | Conditional | Show a progress message and prevent duplicate activation while processing. Transient UI state only. |
| 12 | No-associated-emissions notice | Empty-state banner / panel | No relevant associated supplier-declared or calculated emissions returned for the selected scope | Conditional | Proposed empty-state fallback. Confirm expected system response and whether partial results are possible. |
| 13 | System-error notice | Error banner | Assessment request or aggregation cannot be completed/retrieved | Conditional | Generic presentation fallback based on UI Conventions. UC-05 failure handling is not specified in the repository. |

## 4. States

The five states below provide a consistent presentation set for S-F01-05. **Only the request scope and aggregation output are directly specified by FR-01-06.** Loading is transient presentation feedback. No-associated-emissions and system-error are proposed UI fallbacks because no UC-05 sequence/activity diagram or failure rules are currently present in the repository.

| State | What the user sees | Trigger | Source / notes |
|---|---|---|---|
| **1. Default — Select assessment scope** | Page title; an Assessment basis selector with **Supplier** and **Raw material category** options; the corresponding target selector; a **Generate Scope 3 Assessment** button; and an empty result panel stating that no assessment has been generated yet. | Procurement Department user opens S-F01-05. | Screen List entry for S-F01-05; FR-01-06 says the user requests an assessment for a supplier or raw material category. |
| **2. Loading — Aggregating emissions** | Selected basis and target remain visible. The primary action is disabled and shows **Generating assessment…**. The result panel uses loading placeholders with a message that EcoTrace is aggregating associated supplier-declared and calculated emissions into a Scope 3 category summary. | User requests the assessment for a selected supplier or raw material category. | Transient UI feedback inferred from the request/aggregation in FR-01-06; it is not a persisted business status. |
| **3. Success — Category summary available** | A success banner confirms that the Scope 3 assessment is available. The selected supplier/category is identified. A category-summary table shows the returned Scope 3 category labels and aggregated values. Supplier-declared and calculated emissions are shown as contributing inputs where the response provides them. Values and units come from the service response; the design preview does not invent numeric data. | EcoTrace completes aggregation and returns a Scope 3 category summary. | Directly represents the output required by FR-01-06. Exact columns, units, taxonomy and formula must be confirmed in the data contract. |
| **4. No Associated Emissions — Empty result** | The selected scope remains visible. The result panel states: **No associated emissions are available for this assessment scope.** Supporting text explains that no supplier-declared or calculated emissions were returned for the selected supplier/category. No numeric summary is displayed. The user can change the scope and request another assessment. | Proposed fallback when the assessment returns no associated emissions for the selected scope. | Not specified as a formal UC-05 branch. Treat as a design hypothesis until the team confirms the expected no-data response. |
| **5. System Error — Assessment unavailable** | Error banner: **Scope 3 assessment could not be completed.** Supporting text: **EcoTrace could not retrieve or aggregate the emissions associated with the selected scope. No category summary is displayed.** The selected scope remains visible. No automatic retry or rollback behaviour is assumed. | Proposed fallback when the assessment request or aggregation service fails. | No technical-failure branch exists in the current UC-05 source documents. This is a generic presentation fallback only; recovery behaviour requires confirmation. |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Assessment basis | Selects Supplier or Raw material category | Shows the matching target selector and clears a target from the previously selected basis, if any. | Stays on S-F01-05 |
| 2 | Supplier selector | Selects a supplier | Retains the selected supplier as the assessment scope. | Stays on S-F01-05 |
| 3 | Raw material category selector | Selects a material category | Retains the selected category as the assessment scope. | Stays on S-F01-05 |
| 4 | Generate Scope 3 Assessment | Clicks after selecting a basis and target | Sends the assessment request and shows loading feedback while associated emissions are aggregated. | Stays on S-F01-05 |
| 5 | Scope 3 category summary | Reviews returned summary | Displays the category labels and aggregated emissions values/units returned by EcoTrace. | Stays on S-F01-05 |
| 6 | No-associated-emissions notice | Changes the selected scope | Returns the result area to the initial empty presentation for a new request. | Stays on S-F01-05 |
| 7 | System-error notice | Changes the selected scope or leaves the screen | No automatic retry is assumed; a new request is user-initiated after changing/reselecting the scope. | Stays on S-F01-05 / another permitted screen |
| 8 | Global navigation | Selects another permitted feature | Opens the selected EcoTrace destination subject to authorization. | Target screen outside UC-05 |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | A Scope 3 assessment is requested for a supplier OR a raw material category. | FR-01-06 |
| SR-002 | The assessment aggregates associated supplier-declared emissions and calculated emissions. | FR-01-06 |
| SR-003 | The output is a Scope 3 category summary. | FR-01-06 |
| SR-004 | The user must choose one assessment basis and a target before the request can be sent. | Necessary UI input to satisfy the alternatives in FR-01-06; confirm detailed UX/business rule |
| SR-005 | Do not invent Scope 3 category names, category mapping rules, calculation formula, reporting period, units or emissions boundaries. | Not specified in FR-01-06 / W1 |
| SR-006 | Display supplier-declared and calculated emissions using the values and units returned by the assessment service. | FR-01-06; exact representation requires data-contract confirmation |
| SR-007 | The no-associated-emissions state is a proposed empty-state fallback, not a confirmed UC-05 business branch. | No UC-05 sequence/activity failure branch currently exists in the repository |
| SR-008 | The system-error state is a generic presentation fallback; retry, rollback and partial-result rules are not assumed. | UI Conventions §4; UC-05 technical failure handling not specified |
| SR-009 | The standard operational-user sidebar does not display F-04 administration links. Sign-in is not a sidebar item. | UI Conventions; shared authorization policy FR-04-03 |
| SR-010 | Status and error outcomes must be communicated with text/icon as well as colour. | UI Conventions §5 |

## 7. Linked requirements

| ID | Priority | What this screen does for it |
|---|---|---|
| FR-01-06 — Perform Scope 3 assessment | Should | Allows Procurement to request an assessment for a supplier or raw material category and displays the aggregated supplier-declared and calculated emissions as a Scope 3 category summary. |

**Priority:** W1 and Screen List both classify FR-01-06 / S-F01-05 as Should. This screen is included in the design set at Should priority; it must not be represented as a Must commitment without an approved prioritization change.

## 8. Responsive and accessibility notes

- Follow the shared EcoTrace application shell from `ui-conventions.md`: standard-user sidebar (no F-04 admin links), top bar, breadcrumbs and consistent page-title placement.
- On smaller widths, collapse the sidebar into a drawer and stack the assessment criteria, summary table and contributing-emissions panels in one column.
- Use an explicit label for Assessment basis and the supplier/category target selector; do not rely on placeholder text alone.
- Make it clear that Supplier and Raw material category are alternatives, not simultaneous required inputs.
- Keep the selected assessment scope visible in Loading, Success, No-associated-emissions and System-error states.
- Use semantic table headers for category summaries; right-align numeric emissions values and keep units adjacent to values.
- During loading, show a readable status message and visible progress feedback; do not rely on animation alone.
- Pair success, empty and error colours with text and/or icons. Ensure a logical keyboard order and visible focus state.
- Ensure body text contrast at least 4.5:1 and interactive targets at least 24 × 24 CSS px, preferably 40–44 px for common controls, per UI Conventions §5.

## 9. Open questions

| # | Question | Blocking? | Status / default |
|---|---|---|---|
| 1 | Can the team provide / add SD-UC-05 and AD-UC-05 so that the request, aggregation and outcome branches can be traced at the same level as other screens? | Yes — for detailed workflow sign-off | Not present in the current repository; this spec uses only FR-01-06 and Screen List. |
| 2 | What is the approved Scope 3 category taxonomy and how are supplier/material records mapped to those categories? | Yes — for success-table content | Open. Figma uses generic returned-category placeholders only. |
| 3 | What aggregation formula and boundary are used to combine supplier-declared and calculated emissions? Are values summed, reconciled, substituted or treated differently? | Yes — for displaying meaningful numeric totals | Open. No formula is assumed. |
| 4 | What units and denominator apply to supplier-declared emissions, calculated emissions and the resulting category summary? | Yes — for numeric formatting | Open. Use placeholders until the data contract is confirmed. |
| 5 | Does the user select an individual supplier/material category, or can the assessment include multiple suppliers/categories at once? | Yes — for selector cardinality | Open. Plugin uses one selected supplier OR one selected category, matching the singular wording in FR-01-06. |
| 6 | What date period, site, material filters or status filters (if any) apply to the assessment? | No for the stated FR path; yes if filters are expected | Open. No additional filters are added. |
| 7 | What should EcoTrace return when no associated emissions exist, or only one of the two emissions sources is available? | Yes — for formalizing the empty/partial-result state | Open. No-data frame is a proposed fallback; partial-result handling is not assumed. |
| 8 | What should the user see if the request, aggregation or source retrieval fails? Is manual retry supported? | No for the happy path; yes before approving recovery behaviour | Open. System-error frame is a generic UI fallback; no automatic retry is assumed. |

---

## Completion checklist

- [x] Screen ID, actor, feature and priority match the Screen List / W0-W1 baseline.
- [x] FR-01-06 is reflected without adding unapproved functions.
- [x] Five states are specified: Default, Loading, Success, No Associated Emissions and System Error.
- [x] Supplier and raw material category are treated as alternative assessment scopes.
- [x] Success output is a Scope 3 category summary aggregating supplier-declared and calculated emissions.
- [x] No category taxonomy, formula, unit, period or numeric example is invented.
- [x] Standard operational-user sidebar excludes F-04.
- [x] Missing UC-05 sequence/activity diagrams and the resulting traceability limitation are documented.
- [ ] Figma plugin executed and five frames visually reviewed by the team.
- [ ] Open questions 1–5 confirmed against the UC-05 workflow and data contract.
- [ ] Priority and Must/Should release scope confirmed during team review.
