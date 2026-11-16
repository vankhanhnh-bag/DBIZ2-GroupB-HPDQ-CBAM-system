# Screen Spec: S-F01-06 Carbon Traceability Record

> **Product:** EcoTrace — HPDQ Carbon Management & Reporting System  
> **Status:** Draft for team review  
> **Source of truth:** UC-06, SD-UC-06, AD-UC-06, FR-01-07 / FR-01-08 in W1/W4, Screen List and UI Conventions.

| Field | Value |
|---|---|
| Screen ID | S-F01-06 |
| Screen name | Carbon Traceability Record |
| Actor | Sustainability & ESG Department |
| Feature | F-01 Carbon Traceability & Product Carbon Footprint Management |
| Use case | UC-06 View carbon traceability record |
| Activity diagram | AD-UC-06 — Must-only background linkage process triggered when a supplier, production or shipment record is created |
| Sequence diagram | SD-UC-06 — created-record event is processed; applicable supplier/raw-material, production-batch and shipment links are resolved and updated |
| Linked requirements | FR-01-07 (Must — maintain carbon traceability linkage); FR-01-08 (Should — select a shipped product batch and display its full traceability record) |
| Screen List priority | Must (as currently recorded in Screen List) |
| Priority discrepancy | W1/W4 classify FR-01-07 as Must and FR-01-08 as Should. AD-UC-06 explicitly excludes the user-facing selection/display interaction (FR-01-08) from its Must-only background process. Therefore this viewer screen is directly supported by FR-01-08 (Should); FR-01-07 maintains the links in the background. Confirm whether the Screen List priority should remain Must or be changed to Should. |
| Inputs | A shipped product batch identifier selected by the Sustainability & ESG user |
| Output | The full carbon traceability record from raw material through production to shipment, using the applicable linked records and relationships returned by EcoTrace |
| Important limitation | Canonical identifiers, displayable record attributes, missing-reference behavior, reconciliation/backfill, and retrieval failure handling are not specified in FR-01-07 / FR-01-08. |

## 1. Purpose

**Shown when:** an authorized Sustainability & ESG user opens Carbon Traceability Record to follow a shipped product batch back through production to its raw-material sources.

**The user does:** selects a shipped product batch and requests its traceability record.

**The system does:** retrieves the applicable linked records and relationships, then displays the traceability chain from raw-material sources through production to the shipment / shipped batch.

**Background linkage versus screen display:** FR-01-07 (Must) maintains relationships automatically whenever supplier, production or shipment records are created. FR-01-08 (Should) defines the user-facing selection and display of the full traceability record. The current SD-UC-06 / AD-UC-06 model the background linkage path, not this screen interaction.

**The user leaves this screen when:** they return to the workspace or navigate to another permitted EcoTrace feature. No separate next destination is specified in UC-06.

## 2. Mockup

The Figma plugin creates five editable 1440 × 1024 frames on a dedicated page:

| Frame | State |
|---|---|
| `S-F01-06 — Default` | No shipped product batch selected |
| `S-F01-06 — Loading · Resolving Traceability` | EcoTrace is retrieving the selected batch and its linked lineage records |
| `S-F01-06 — Success · Traceability Chain Available` | Full raw-material → production → shipment chain is displayed |
| `S-F01-06 — Incomplete Traceability Data` | One or more expected lineage references are unavailable or unresolved |
| `S-F01-06 — System Error` | EcoTrace cannot retrieve the traceability record |

The visual language follows `ui-conventions.md`: Hòa Phát navy, EcoTrace blue/mint accents, white content surfaces, compact operational-user sidebar, top app bar, clear batch selector, readable lineage stages and restrained banners.

The standard operational-user sidebar contains Overview, applicable F-01 / F-02 / F-03 destinations and Help & Support. It does **not** show F-04 administration links.

No actual supplier names, batch IDs, shipment IDs, dates, quantities, carbon values or source-system identifiers are fabricated. Any displayed identifiers are explicitly labelled as values returned by EcoTrace / prototype placeholders.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation / behaviour |
|---|---|---|---|---|---|
| 1 | Operational application shell | Sidebar + top bar | EcoTrace navigation; signed-in Sustainability & ESG role | Yes | Show Overview, applicable F-01/F-02/F-03 destinations and Help & Support only; hide F-04 administration navigation. Authorization remains enforced by the platform (FR-04-03). |
| 2 | Breadcrumb and page title | Header | Carbon Traceability & PCF / Carbon Traceability Record | Yes | Identifies the current screen. |
| 3 | Shipped product batch selector | Searchable select | Shipped product batch identifiers available in EcoTrace | Yes | User selects the shipped batch whose upstream record chain is to be viewed. The authoritative lookup source and search fields are not specified. |
| 4 | View traceability record | Primary button | Static action label | Yes | Requests retrieval and display of the full traceability record for the selected shipped batch (FR-01-08, Should). |
| 5 | Selected batch context | Summary fields | Selected shipped-batch identifier and any returned shipment context | Yes, after selection | Preserve source identifiers; do not invent product, customer, shipment-date or quantity fields if they are not returned. |
| 6 | Supplier / raw-material source stage | Lineage node / record card | Applicable upstream supplier and raw-material source references linked by FR-01-07 | Yes, in success state where available | Display source-record references returned by EcoTrace. Do not invent supplier/material attributes or claim certification. |
| 7 | Raw-material lot stage | Lineage node / record card | Raw-material lot references and their relationship to the production batch | Yes, in success state where available | Display returned record references and relationship direction. |
| 8 | Production batch stage | Lineage node / record card | Production batch identifier and applicable consumed raw-material references | Yes, in success state | Display the production-batch reference returned by EcoTrace. Do not add operational metrics not specified by FR-01-08. |
| 9 | Shipment / shipped-batch stage | Lineage node / record card | Selected shipment and its shipped production-batch references | Yes, in success state | Show the link to the selected shipped batch and relevant source reference returned by EcoTrace. |
| 10 | Relationship connectors | Directional arrows / connector labels | Supplier → raw material → production batch → shipment relationships in the traceability model | Yes, in success state | Direction must follow the upstream-to-downstream chain. Do not imply a record relationship where no link is returned. |
| 11 | Traceability loading feedback | Inline progress / loading state | Retrieval of selected batch and linked lineage records | Conditional | Show progress while the record is retrieved; disable repeated activation during the request. |
| 12 | Incomplete-lineage notice | Warning / empty-state banner | One or more applicable lineage references are unavailable or unresolved | Conditional | Proposed presentation fallback only. AD-UC-06 says missing/invalid references and reconciliation behavior remain open; display only missing links identified by the response. |
| 13 | System-error notice | Error banner | Traceability-record retrieval failure | Conditional | Generic presentation fallback based on UI Conventions. UC-06 technical failure and retry behavior are not specified. |

## 4. States

The five states below provide a complete presentation set for S-F01-06. FR-01-08 directly supports selecting a shipped product batch and displaying the full traceability record. FR-01-07 and the current SD-UC-06 / AD-UC-06 describe background linkage maintenance. **Incomplete-lineage and system-error are proposed UI fallbacks** because the current source documents do not define those failure branches.

| State | What the user sees | Trigger | Source / notes |
|---|---|---|---|
| **1. Default — Select a shipped batch** | Page title, a labelled **Shipped product batch** selector, a **View traceability record** button, and an empty lineage panel showing the expected upstream-to-downstream structure: Raw-material sources → Production batch → Shipment. Helper text asks the user to select a shipped batch to view its lineage. | Sustainability & ESG user opens S-F01-06. | Screen List purpose and FR-01-08 (Should): user selects a shipped product batch. |
| **2. Loading — Resolving traceability** | Selected shipped-batch identifier remains visible. The lineage panel shows loading placeholders for supplier/raw-material sources, production batch and shipment. A progress message reads **Retrieving linked records and resolving the traceability chain…** The View traceability record button is temporarily disabled. | User requests the traceability record for a selected shipped batch. | Transient presentation state inferred from FR-01-08 retrieval/display and the linked-record resolution process described in SD-UC-06. |
| **3. Success — Traceability chain available** | A success banner confirms that the traceability record is available. A directional chain shows the returned supplier/raw-material source records, raw-material lot(s), production batch(es) and shipment / shipped batch. Record identifiers and relationship details are displayed only when returned by EcoTrace. | EcoTrace returns a complete set of applicable linked records and relationships for the selected batch. | FR-01-08 requires the full carbon traceability record from raw material through production to shipment. FR-01-07 maintains the relationships in the background. |
| **4. Incomplete Traceability Data — Some links unavailable** | The selected shipped batch remains visible. A warning banner reads **The full traceability chain is not available.** The chain shows available records and identifies missing/unresolved links only where the response provides that detail. No missing record is fabricated and no claim of a complete chain is shown. | Proposed fallback when one or more expected upstream/downstream references cannot be resolved. | AD-UC-06 explicitly leaves missing/invalid lineage references and later reconciliation behavior open. This is a design hypothesis, not an approved business outcome. |
| **5. System Error — Unable to retrieve record** | Error banner: **Traceability record could not be retrieved.** Supporting text: **EcoTrace could not load the linked records for the selected shipped batch. No complete traceability chain is displayed.** The selected batch remains visible. No automatic retry or reconciliation behavior is assumed. | Proposed fallback when the traceability-record retrieval service cannot complete the request. | No technical retrieval-failure branch is specified in FR-01-07 / FR-01-08 or the current SD-UC-06 / AD-UC-06. This is a generic UI fallback only. |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Shipped product batch selector | Selects a shipped product batch | Retains the selected batch identifier as the target record for traceability retrieval. | Stays on S-F01-06 |
| 2 | View traceability record | Clicks | Requests the full linked record and shows loading feedback while the data is retrieved. | Stays on S-F01-06 |
| 3 | Traceability chain | Reviews the returned stages and relationships | Displays the available supplier/raw-material, production-batch and shipment links in upstream-to-downstream order. | Stays on S-F01-06 |
| 4 | Incomplete-lineage notice | Reviews available and unresolved links; may select another shipped batch | Displays only the available chain data and the unresolved references returned by EcoTrace. No automatic repair/reconciliation is assumed. | Stays on S-F01-06 |
| 5 | System-error notice | Leaves the screen or selects another batch for a new request | Does not display a complete chain from an unsuccessful retrieval. No automatic retry is assumed. | Stays on S-F01-06 / another permitted screen |
| 6 | Global navigation | Selects another permitted feature | Opens the selected EcoTrace destination subject to authorization. | Target screen outside UC-06 |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | The user-facing view is requested by selecting a shipped product batch. | FR-01-08 (Should) |
| SR-002 | The displayed record covers the carbon traceability chain from raw material through production to shipment. | FR-01-08 (Should) |
| SR-003 | Supplier, production and shipment records are linked to applicable upstream raw-material sources and downstream shipped batches when those records are created. | FR-01-07 (Must) |
| SR-004 | The lineage direction is supplier/raw-material source → production batch → shipment / shipped batch. | FR-01-07; SD-UC-06; AD-UC-06 |
| SR-005 | Display only record identifiers, attributes and links returned by EcoTrace; do not fabricate unresolved references. | Data-contract constraint; AD-UC-06 open questions 2 and 4 |
| SR-006 | Do not claim that the calculated carbon intensity, PCF, certification or emissions verification is part of this screen unless a separate linked requirement/data contract provides it. | FR-01-07 / FR-01-08 scope |
| SR-007 | The incomplete-lineage state is a proposed fallback only; the system's missing/invalid-reference behavior and later reconciliation process are not specified. | AD-UC-06 failure end state / open questions 3–4 |
| SR-008 | The system-error state is a generic presentation fallback; retry, rollback and recovery behavior are not assumed. | No UC-06 failure branch in current source diagrams |
| SR-009 | The standard operational-user sidebar does not display F-04 administration links. Sign-in is not a sidebar item. | UI Conventions; shared authorization policy FR-04-03 |
| SR-010 | Status and error outcomes must be communicated with text/icon as well as colour. | UI Conventions §5 |

## 7. Linked requirements

| ID | Priority | What this screen does for it |
|---|---|---|
| FR-01-07 — Maintain carbon traceability linkage | Must | Supporting background capability: EcoTrace links supplier, production and shipment records to their applicable upstream raw-material sources and downstream shipped batches whenever records are created. This automatic process does not require user interaction on S-F01-06. |
| FR-01-08 — View carbon traceability record | Should | Direct user-facing capability of this screen: Sustainability & ESG selects a shipped product batch and views the full carbon traceability record from raw material through production to shipment. |

**Priority note:** W1/W4 classify FR-01-07 as Must and FR-01-08 as Should. The Screen List currently labels S-F01-06 as Must while linking both. AD-UC-06 says the Must-only linkage process does not include the viewer interaction. Confirm whether the screen priority should be changed to Should or whether a separate Must-level viewing requirement is missing.

**Sequence metadata note:** SD-UC-06's header currently lists FR-01-08, while its introduction and sequence flow describe the background linkage behavior of FR-01-07. AD-UC-06 records this mismatch as an open question. This Screen Spec preserves both source statements rather than silently correcting the Sequence Diagram.

## 8. Responsive and accessibility notes

- Follow the shared EcoTrace application shell from `ui-conventions.md`: standard-user sidebar (no F-04 admin links), top bar, breadcrumbs and consistent page-title placement.
- On smaller widths, collapse the sidebar into a drawer and stack the shipped-batch selector, lineage chain and record details in one column.
- Keep the selected shipped-batch identifier visible in loading, success, incomplete and error states.
- Use an explicit label for the shipped-batch selector; do not rely on placeholder text alone.
- The traceability chain must remain understandable without visual connectors alone. Use stage headings and textual relationship labels so screen readers and users of high-contrast modes can follow the sequence.
- Ensure that each record card has a clear type label (Supplier / Raw Material / Production Batch / Shipment) and accessible name.
- During loading, show a readable status message and visible progress feedback; do not rely on animation alone.
- Pair complete, incomplete and error states with text/icon as well as colour.
- Ensure body text contrast at least 4.5:1 and interactive targets at least 24 × 24 CSS px, preferably 40–44 px for common controls, per UI Conventions §5.

## 9. Open questions

| # | Question | Blocking? | Status / default |
|---|---|---|---|
| 1 | Should S-F01-06 remain Must even though W1/W4 classify the direct viewer requirement FR-01-08 as Should, while FR-01-07 is background linkage? | Yes — for final priority / scope sign-off | Open. This spec preserves the Screen List's Must label and records the discrepancy. |
| 2 | What are the canonical identifiers and reference fields for supplier records, raw-material lots, production batches and shipment records? | Yes — for final record-card design | Open. The screen uses generic identifiers returned by EcoTrace. |
| 3 | Which record attributes beyond identifiers should be displayed for each stage (e.g. supplier name, material type, batch number, shipment reference)? | Yes — for final element inventory / data contract | Open. No unsupported attributes are assumed. |
| 4 | What should EcoTrace do when an upstream/downstream reference is missing, invalid or points to a record that has not yet been created? Is incremental linking, reconciliation or backfill required? | Yes — for formalizing the incomplete-lineage state and lifecycle | Open, as recorded in AD-UC-06. |
| 5 | Is the traceability graph a separate store or a logical data model in the existing EcoTrace database? | No for the screen; yes for physical architecture | Open. The screen treats it as a logical traceability model. |
| 6 | What should the UI show if retrieving the selected batch's chain fails? Is manual retry supported? | No for the nominal display; yes before approving error recovery | Open. System-error state is a generic presentation fallback; no retry is assumed. |
| 7 | Should SD-UC-06 metadata be corrected to FR-01-07, or should its sequence flow be expanded to include FR-01-08 viewer interactions? | Yes — for sequence/activity traceability consistency | Open. AD-UC-06 identifies the same mismatch; this specification does not alter SD-UC-06. |

---

## Completion checklist

- [x] Screen ID, actor and feature match the Screen List / W0 baseline.
- [x] FR-01-07 background linkage is separated from FR-01-08 user-facing viewing.
- [x] Five states are specified: Default, Loading, Success, Incomplete Traceability Data and System Error.
- [x] Success state shows the upstream-to-downstream supplier/raw-material → production → shipment chain.
- [x] No record IDs, source attributes, carbon values or technical protocols are fabricated.
- [x] Standard operational-user sidebar excludes F-04.
- [x] Screen List priority discrepancy and SD-UC-06 FR metadata mismatch are documented.
- [ ] Figma plugin executed and five frames visually reviewed by the team.
- [ ] Open questions 1–4 confirmed against the UC-06 workflow and traceability data contract.
- [ ] Screen List / W1 priority and SD-UC-06 metadata reconciled during the final F-01 review.
