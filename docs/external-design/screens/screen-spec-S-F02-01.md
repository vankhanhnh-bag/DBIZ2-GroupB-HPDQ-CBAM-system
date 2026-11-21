# Screen Spec: S-F02-01 CBAM Estimation & Analysis

> **Product:** EcoTrace — HPDQ Carbon Management & Reporting System  
> **Status:** Draft for team review  
> **Source of truth:** UC-07, SD-UC-07, AD-UC-07, FR-02-01 to FR-02-13 in W1, Screen List, Screen Transition Diagram, Usage Flow and UI Conventions.

| Field | Value |
|---|---|
| Screen ID | S-F02-01 |
| Screen name | CBAM Estimation & Analysis |
| Actor | Export Sales & Marketing Department; Procurement & Raw Materials Department |
| Priority | Must |
| Feature | F-02 CBAM Compliance & Carbon Cost Estimation |
| Use case | UC-07 Estimate & analyze CBAM exposure for shipment |
| Activity diagram | AD-UC-07 - select shipment batch, load shipment and PCF data, validate, calculate CBAM exposure and review the result |
| Sequence diagram | SD-UC-07 - retrieve shipment/product data from SAP S/4HANA, validate, retrieve EU ETS price, calculate emissions and cost, store estimate and return breakdown |
| Linked requirements | FR-02-01 to FR-02-13 (Must: FR-02-01–09; Should: FR-02-10–12; Could: FR-02-13) |
| Mockup / Figma plugin | EcoTrace - S-F02-01 CBAM Estimation & Analysis; editable screen and modal/state frames |
| Inputs | Selected order or shipment; associated product, net weight, destination market, planned departure date and verified carbon-intensity data loaded by the system |
| Important limitation | Exact input-control details, full calculation formula presentation, scenario bounds, historical chart period, report format and share options are not fully specified. Do not invent business rules or sample business values. |

## 1. Purpose

**Shown when:** an authenticated and authorized Export Sales & Marketing or Procurement & Raw Materials user opens **CBAM & Carbon Cost > CBAM Estimation**, or follows the Home dashboard shortcut. The screen is also reached after S-F01-01 when shipment-level PCF has to be calculated first.

**The user provides:** a valid order or shipment selection and requests a CBAM estimate. The system loads the associated product, net weight, destination market, planned departure date and verified carbon-intensity data.

**The system does:** validate shipment, product, carbon and destination inputs; block calculation and show reasons when required data is missing or invalid; otherwise calculate verified embedded emissions, exempt emissions, emissions subject to CBAM and estimated CBAM cost. It retrieves the applicable certificate price and recognizes eligible carbon price paid at origin where evidence is available. The completed estimate includes a calculation breakdown, sources, regulation version and assumptions, and is stored as a compliance record.

**The user leaves this screen when:** they return to the workspace, open S-F01-01 to calculate missing shipment PCF, or interact with the Share / View Assumptions modals. Downloading a report downloads a file and does not navigate to another screen.

## 2. Mockup

![S-F02-01 — Default](img/S-F02-01%20%E2%80%94%20Default)

**Numbered:** each number corresponds to the # of the element inventory in section 3.

### State images

| State | Image |
|---|---|
| Default | [S-F02-01 — Default](img/S-F02-01%20%E2%80%94%20Default) |
| Empty — Missing PCF | [S-F02-01 — Empty](img/S-F02-01%20%E2%80%94%20Empty) |
| Loading | [S-F02-01 — Loading](img/S-F02-01%20%E2%80%94%20Loading) |
| Validation Error | [S-F02-01 — Validation Error](img/S-F02-01%20%E2%80%94%20Validation%20Error) |
| Success | [S-F02-01 — Success](img/S-F02-01%20%E2%80%94%20Success) |
| M-F02-01 — CBAM Result & Share | [M-F02-01 — CBAM Result & Share](img/M-F02-01%20%E2%80%94%20CBAM%20Result%20%26%20Share) |
| M-F02-02 — View Assumptions | [M-F02-02 — View Assumptions](img/M-F02-02%20%E2%80%94%20View%20Assumptions) |

All images above are the existing mockup assets in `docs/external-design/screens/img/`. The five S-F02-01 images document the main screen states; the two M-F02-01 / M-F02-02 images document the result and assumptions overlays.

## 3. Element inventory

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation / behaviour |
|---|---|---|---|---|---|
| 1 | Application shell | Sidebar + top bar | EcoTrace navigation and signed-in user/department | Yes | Standard operational-user navigation. Show Overview, applicable F-01/F-02/F-03 destinations and Help & Support. Do not show F-04 admin navigation to standard users. |
| 2 | Breadcrumb and page title | Header | CBAM & Carbon Cost / CBAM Estimation & Analysis | Yes | Identifies the current location and screen. |
| 3 | Download Report | Secondary button (footer) | Report generated from shipment inputs, calculation results, assumptions and sources (FR-02-12) | Should | Positioned in the page header. Disabled until a completed estimate exists. Trigger file download and provide concise success/failure feedback. File format is not specified. |
| 4 | Share | Secondary button (footer) | Share action for a completed CBAM estimate (FR-02-13) | Could | Positioned in the page header. Disabled until a completed estimate exists. Opens M-F02-01. |
| 5 | Order / Shipment selector | Select / searchable selector | Valid order or shipment reference from shipment data (FR-02-01) | Yes | User selects a valid order or shipment. Exact selector type, search fields and list columns are not specified. |
| 6 | Shipment input summary | Read-only data panel | Product, net weight, destination market and planned departure date associated with the selected order/shipment (FR-02-01) | Yes, after selection | Auto-load from SAP / connected shipment data. Preserve source values and units; do not allow edits unless separately specified. |
| 7 | Shipment / PCF data status | Status badge / inline message | Availability and validity of required shipment and PCF inputs | Conditional | Explain missing or invalid required data using text and icon, not colour alone. Do not invent additional lifecycle statuses. |
| 8 | Calculate CBAM Estimate | Primary button | User action to request UC-07 estimate | Yes | Calculation is blocked until required shipment, product, carbon and destination data pass validation (FR-02-02). |
| 9 | Status detail / guidance panel | Error banner / field or data-row feedback | Validation reasons returned by the system (FR-02-02; SD-UC-07; AD-UC-07) | Conditional | Identify missing/invalid data and guide the user to select a valid batch or correct the source data. Exact validation messages depend on returned reasons. |
| 10 | Product Carbon Intensity (PCI) | KPI card | Verified carbon-intensity data for the selected shipment's product/batch (FR-02-01) | After successful calculation | Display value and methodology label. Exact display timing relative to Calculate is open — see Open Questions. |
| 11 | Verified Embedded Emissions | KPI card | Verified embedded emissions, calculated from product carbon intensity and shipment net weight (FR-02-03) | After successful calculation | Show value and unit. |
| 12 | Emissions Exempted (CBAM) | KPI card | Emissions exempt under configured CBAM exemption rules (FR-02-04) | After successful calculation | Show value and unit. Do not invent exemption values. |
| 13 | Emissions Subject to CBAM | KPI card | Emissions subject to CBAM, calculated as verified embedded emissions less exempt emissions (FR-02-05) | After successful calculation | Show value and unit. Chargeable emissions = verified embedded emissions minus exempt emissions. |
| 14 | Certificate price | KPI card | Applicable CBAM certificate price retrieved for the selected reference date from EU ETS market data (FR-02-06) | After successful calculation | Display the reference date and unit (€/tCO₂e or the source-defined unit). Do not hardcode a price. |
| 15 | Origin carbon-price recognition | KPI card | Eligible origin carbon price amount determined when evidence is available (FR-02-07) | Conditional | Show only the eligible amount/result supplied by the calculation engine. Do not assume a deduction when evidence is absent. |
| 16 | Estimated CBAM Cost | KPI card (emphasized) | Estimated CBAM cost in EUR (FR-02-08) | After successful calculation | Display only the calculated result returned by the system. Always show EUR. |
| 17 | Calculation breakdown and sources | Data table / detail section | Each calculation component, source, regulation version and assumption used (FR-02-09) | After successful calculation | Show component labels, values, units and the formula structure. Keep source and version traceability visible. |
| 18 | Shipment Information panel | Data panel | Customer, Port of Loading, Port of Destination, Shipping Method, No. of Containers, Incoterms (order/shipment data, SAP) | After successful calculation | Read-only summary of the shipment's logistics details. Preserve source values. |
| 19 | Regulation & Assumptions summary | Data panel | Regulation version, price reference date, product category, calculation method and exchange rate used for the estimate (FR-02-09) | After successful calculation | Shows the key assumptions already, before View Assumptions is clicked. Do not treat this panel and View Assumptions as the same element. |
| 20 | View Assumptions | Secondary button | Access to the full assumptions beyond the summary panel (FR-02-09; M-F02-02) | Yes, for completed estimate | Opens M-F02-02 as an overlay; it is not a separate screen. Exact additional content beyond element 20 is not specified. |
| 21 | Share result reference | Modal content | Shareable result reference and available sharing options (FR-02-13; M-F02-01) | Conditional | Present only after user selects Share for a completed estimate. Exact access options and expiry rules are not defined. |
| 22 | Copy / share action | Modal action | User action on the generated share reference | Conditional | Implement only the sharing options supported by the service. Do not invent external channels or permissions. |
| 23 | Return to workspace | Secondary navigation | Global navigation / workspace destination | Yes | Return to the relevant workspace; exact destination URL is outside UC-07. |
| 24 | View CBAM result | Primary button | Completed CBAM estimation result for the selected shipment (FR-02-03 to FR-02-09; M-F02-01) | Yes, after successful calculation | Display after shipment data has been validated and the CBAM estimate has been successfully calculated. Opens M-F02-01 — CBAM Result & Share as an overlay to let the user review the calculated result. Disabled or hidden before a completed estimate is available. |

## 4. States

| State | What the user sees | Trigger | Source / notes |
|---|---|---|---|
| **1. Default — Select shipment** | Page title and shipment/order selector. Before a selection is made, no estimate or calculated values are shown. After selection, the shipment input summary is populated with associated product, net weight, destination market, planned departure date and verified carbon-intensity data. Primary action: **Calculate CBAM Estimate**. | User opens S-F02-01 and has not yet requested an estimate, or selects a shipment/order. | Screen List; FR-02-01. |
| **2. Loading — Retrieving / calculating** | Keep the selected shipment context visible. Show a progress indicator and disable duplicate calculation requests while the system retrieves inputs or calculates the estimate. | User selects a shipment / requests an estimate and the system is processing. | Transient presentation inferred from the request/response sequence in SD-UC-07; no persisted business status is introduced. |
| **3. Validation Error — Calculation blocked** | Show an error banner explaining that required shipment, product, carbon or destination data is missing or invalid. Identify the returned missing/invalid reason and prompt the user to select a valid batch or resolve source data. Do not show a successful estimate. | The system finds missing or invalid required inputs. | FR-02-02; SD-UC-07 alt Missing / Invalid data; AD-UC-07 invalid branch. |
| **4. Calculated Result — Estimate available** | Show estimated CBAM cost in EUR; verified embedded emissions; exempt emissions; emissions subject to CBAM; applicable certificate price; eligible origin carbon-price recognition where evidence exists; calculation components, sources, regulation version and assumptions. Also show Should-level price scenarios and historical price trend when available. Provide **View Assumptions**, **Download Report** and **Share** actions according to their priorities. | Required inputs pass validation and the system completes and stores the estimate. | FR-02-03 to FR-02-11; SD-UC-07 valid branch. Download/share are FR-02-12 / FR-02-13. |
| **5. Report Download Feedback** | Provide a concise non-blocking confirmation when the report is generated/downloaded, or a clear error with a safe next step if generation fails. Do not imply a file was downloaded if the operation failed. | User selects **Download Report** on a completed estimate. | FR-02-12. Exact file format and failure handling are not specified. |
| **6. View Assumptions — Modal** | Overlay listing the full assumptions, regulation version, price reference date and calculation method used for the current estimate. | User selects **View Assumptions**. | M-F02-02; FR-02-09. |
| **7. Share Result — Modal** | Overlay presents the generated shareable result reference and available sharing options. | User selects **Share** on a completed estimate. | M-F02-01; FR-02-13. Exact options are not specified. |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Order / Shipment selector | Selects a valid order or shipment | Loads the associated product, net weight, destination market, planned departure date and verified carbon-intensity data. | Stays on S-F02-01 |
| 2 | Calculate CBAM Estimate | Clicks | Requests a CBAM estimate and validates the required shipment, product, carbon and destination inputs. | Stays on S-F02-01 |
| 3 | Validation feedback | Reviews the reason and selects/resolves valid shipment data | System re-loads or re-validates the selected inputs; user can request calculation again. | Stays on S-F02-01 |
| 4 | Calculated result | Reviews the estimate and breakdown | System displays the estimated cost, calculation components, sources, regulation version and assumptions. | Stays on S-F02-01 |
| 5 | View Assumptions | Clicks | Opens the full assumptions, regulation version, price reference date and calculation method for the current estimate. | M-F02-02 overlay on S-F02-01 |
| 6 | Close View Assumptions | Closes modal | Returns to the same completed estimate without losing its context. | S-F02-01 |
| 7 | Download Report | Clicks | Generates and downloads a report containing shipment inputs, calculation results, assumptions and sources. | Stays on S-F02-01; file download |
| 8 | Share | Clicks | Creates a shareable result reference and presents available sharing options. | M-F02-01 overlay on S-F02-01 |
| 9 | Share result option | Selects an available option | Performs the corresponding supported share/copy action. | Stays on M-F02-01 / S-F02-01 |
| 10 | Close Share modal | Closes modal | Returns to the same completed estimate. | S-F02-01 |
| 11 | Shipment PCF missing | Export Sales & Marketing selects the PCF preparation path | Opens shipment-level PCF calculation before returning to CBAM estimation. | S-F01-01, then S-F02-01 after PCF is stored |
| 12 | Return to workspace | Uses global navigation | Returns to the EcoTrace workspace. | Workspace (outside this screen list) |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | The user selects a valid order or shipment before the system loads the associated CBAM estimation inputs. | FR-02-01 |
| SR-002 | Load product, net weight, destination market, planned departure date and verified carbon-intensity data for the selected order/shipment. | FR-02-01 |
| SR-003 | Block calculation when required shipment, product, carbon or destination data is missing or invalid; display the validation reason. | FR-02-02 |
| SR-004 | Calculate verified embedded emissions from applicable product carbon intensity and shipment net weight. | FR-02-03 |
| SR-005 | Apply configured CBAM exemption rules to determine exempt emissions. | FR-02-04 |
| SR-006 | Calculate emissions subject to CBAM as verified embedded emissions less exempt emissions. | FR-02-05 |
| SR-007 | Retrieve and use the CBAM certificate price applicable to the selected reference date. | FR-02-06 |
| SR-008 | Recognize an origin carbon-price amount only when evidence is available and the amount is eligible under configured CBAM rules. | FR-02-07 |
| SR-009 | Calculate and display estimated CBAM cost in EUR using chargeable emissions, certificate price and recognized origin carbon-price amount. | FR-02-08 |
| SR-010 | Display each calculation component, source, regulation version and assumption used to produce the estimate. | FR-02-09 |
| SR-011 | Show low, base and high certificate-price scenarios and the resulting CBAM cost for each scenario. | FR-02-10 (Should) |
| SR-012 | Display the available historical EU ETS certificate-price trend for the configured period. | FR-02-11 (Should) |
| SR-013 | Generate a downloadable report containing shipment inputs, calculation results, assumptions and sources when requested. | FR-02-12 (Should) |
| SR-014 | When Share is selected for a completed estimate, create a shareable result reference and present available sharing options. | FR-02-13 (Could) |
| SR-015 | The optional shipment-level PCF calculation is a preparation path only when valid stored shipment PCF is unavailable; otherwise the user proceeds directly to CBAM estimation. | Screen Transition Diagram §4; Usage Flow — Export Sales & Marketing |
| SR-016 | Use the standard EcoTrace application shell and role-appropriate operational navigation. F-04 administration links must not appear in the standard-user sidebar. | UI Conventions — Visual direction / Navigation |
| SR-017 | Use English by default for the current design baseline; do not mix English and Vietnamese within one screen. | UI Conventions §1 |
| SR-018 | Format dates as `dd/MM/yyyy`; date/time as `dd/MM/yyyy HH:mm` and show ICT (UTC+07:00) where audit timestamps need a timezone. | UI Conventions §2 |
| SR-019 | Format numbers with decimal point and comma thousands separator; show percentage symbol and currency ISO code where applicable. Preserve source-system identifiers without reformatting. | UI Conventions §2 |
| SR-020 | Every chart identifies period, units, axis labels and legend; provide a text/table alternative. | UI Conventions — Data visualization; Accessibility minimums |
| SR-021 | Never communicate data quality, lifecycle stage, status or errors by colour alone. Pair semantic colour with text, icon, pattern or label. | UI Conventions — Visual direction; §5 |
| SR-022 | Do not invent business statuses, calculation values, certificate prices, scenario bounds, report formats or sharing permissions that are not defined by the relevant requirements/data contract. | UI Conventions §4; FR-02-01 to FR-02-13 |

## 7. Linked requirements

| FR ID | Priority | What this screen does for it |
|---|---|---|
| FR-02-01 | Must | Selects an order/shipment and displays its associated product, net weight, destination market, planned departure date and verified carbon-intensity data. |
| FR-02-02 | Must | Blocks calculation for missing/invalid shipment, product, carbon or destination data and displays validation reasons. |
| FR-02-03 | Must | Presents verified embedded emissions calculated from product carbon intensity and shipment net weight. |
| FR-02-04 | Must | Presents emissions exempt under configured CBAM exemption rules. |
| FR-02-05 | Must | Presents emissions subject to CBAM, calculated as verified embedded emissions less exempt emissions. |
| FR-02-06 | Must | Uses the certificate price applicable to the selected reference date. |
| FR-02-07 | Must | Displays eligible recognition of carbon price paid at origin when evidence is available. |
| FR-02-08 | Must | Displays estimated CBAM cost in EUR. |
| FR-02-09 | Must | Displays the calculation breakdown, sources, regulation version and assumptions; provides M-F02-02. |
| FR-02-10 | Should | Displays low, base and high certificate-price scenarios and the resulting CBAM costs. |
| FR-02-11 | Should | Displays the available historical EU ETS certificate-price trend for the configured period. |
| FR-02-12 | Should | Provides a report download containing shipment inputs, results, assumptions and sources. |
| FR-02-13 | Could | Provides Share and a shareable result reference with available sharing options through M-F02-01. |

## 8. Responsive and accessibility notes

- **Application shell:** Desktop uses a persistent left sidebar (about 240 px), top app bar (about 64 px), breadcrumbs, page title and main content with 24 px page padding. Below 1200 px, collapse the sidebar into a drawer.
- **Responsive content:** At mobile widths, use a single-column layout; stack KPI cards and allow wide breakdown/scenario tables to scroll or switch to a readable card/list presentation. Minimum supported width: 360 px.
- **Information never carried by colour alone:** Status badges, validation feedback, chart series and data-quality indicators must include text, icon, pattern or label.
- **Text and contrast:** Body text contrast at least 4.5:1. Use readable dense-data typography and preserve clear table headings and units.
- **Interactive targets:** At least 24 × 24 CSS px; prefer approximately 40–44 px for frequently used touch controls. All controls need visible keyboard focus, accessible names and logical tab order.
- **Charts:** Identify chart period, units, axes and legend; provide a text summary or tabular alternative.
- **Zoom and language:** Keep browser zoom enabled. Page language is English (`lang="en"`) for the current design baseline.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | Which exact order/shipment fields are used to search and identify a valid shipment in the selector (e.g. Order No. or Shipment No.)? | Yes — needed to finalize selector content and acceptance criteria. | Open |
| 2 | Are any loaded shipment inputs editable on this screen, or must all shipment/PCF fields remain read-only and be corrected in their source systems? | Yes — affects interaction and validation design. | Open |
| 3 | What exact validation conditions, ranges and destination/product eligibility rules apply beyond the examples in AD-UC-07? | Yes — required for complete field-level validation behaviour. | Open |
| 4 | What is the exact calculation presentation and unit/basis for verified embedded emissions, exempt emissions and origin carbon-price deduction? | Yes — needed to finalize breakdown labels, precision and units. | Open |
| 5 | What are the low/base/high certificate-price scenario bounds and how are they configured? | No — needed to finalize Should-level scenario details. | Open |
| 6 | What historical period, sampling frequency and source metadata should the EU ETS certificate-price trend use? | No — needed to finalize Should-level chart. | Open |
| 7 | What report format(s) are required (PDF, XLSX or another format), and what filename convention should be used? | No — needed to finalize download behaviour. | Open |
| 8 | Which sharing options are available, and what access control, expiry and revocation rules apply to the shareable result reference? | No — needed to finalize Could-level sharing modal. | Open |
| 9 | If the external EU ETS certificate-price API is unavailable, what fallback is approved? AD-UC-07 records a default of cached pricing from the previous business day. | No — confirm that this activity-diagram default is accepted as the product rule. | Open |

---

## Completion checklist

- [ ] The mockup is a separate cropped image file, named with the Screen ID.
- [x] Every visible element specified for this screen appears in the element inventory.
- [x] Every input element has a validation rule or an explicit “not applicable”. The only required user input is order/shipment selection; the other shipment fields are system-loaded.
- [x] All relevant states are filled in, including validation error and completed estimate; modal states are documented separately.
- [x] Navigation targets use Screen IDs present in the Screen List; workspace and file-download outcomes are explicitly identified as outside-screen outcomes.
- [x] Every element that displays data names its source requirement/data group or source system where defined.
- [x] Linked FRs exist in W1; priorities are preserved (FR-02-01–09 Must, FR-02-10–12 Should, FR-02-13 Could).
- [x] No unsupported business values, scenario bounds, report formats or sharing permissions are presented as confirmed requirements.
