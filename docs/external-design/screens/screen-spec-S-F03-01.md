# Screen Spec: S-F03-01 Green Steel Filter & Comparison Criteria

> **Product:** EcoTrace — HPDQ Carbon Management & Reporting System  
> **Status:** Draft — all 7 UI state frames (Default, Dropdown Open, Criteria Selected, Validation Error, Loading · Comparing, No Matching Products, System/Data Error) are defined in the supplied design exports.  
> **Source of truth:** UC-09 sequence diagram (`SD-UC-09`), `AD-UC-09`, `FR-03-01` & `FR-03-02`, Screen List, and UI Conventions.

| Field | Value |
|---|---|
| Screen ID | `S-F03-01` |
| Screen name | Green Steel Filter & Comparison Criteria |
| Actor | Customers / Procurement |
| Priority | Must |
| Feature | `F-03` Green Steel Comparison & Recommendation |
| Use case | `UC-09` Compare and recommend green steel options |
| Activity diagram | `AD-UC-09` — input and submit comparison criteria (`a1`), validate parameters (`s1`, `d1`), identify validation errors (`s2`), query product profiles & CBAM costs (`s3`, `d2`), and check partial matches / suggest filter adjustments (`s4`) |
| Linked sequence | `SD-UC-09` — steps 1–7: Customer inputs criteria on EcoTrace web UI (`1`), submits filters to EcoTrace BE (`2`), validates parameters (`3`), queries Product & Carbon DB (`4–5`), and stores retrieved data in EcoTrace Database (`6–7`) |
| Mockup image | 7 numbered state images embedded in Section 2 from the repository's `img/` directory (covers 7 exported state frames: Default, Dropdown Open, Criteria Selected, Validation Error, Loading · Comparing, No Matching Products, System/Data Error) |
| Status values | No persisted business status is written on this screen; `Calculated & Ranked` is only produced downstream on `S-F03-02` (`AD-UC-09` object node `o1`). |

## 1. Purpose

**Shown when:** an authorized Customers / Procurement user navigates to **Green Steel / Product Comparison** (`F-03`) from the Main Menu / Home Page workspace, or returns from `S-F03-02` via **Edit criteria**.

**The user does:** selects the three mandatory comparison criteria (**Product category**, **Steel grade**, and **Destination market**) and submits them to filter and compare low-carbon steel options.

**The system does:** validates that all three required criteria are present; queries product profiles, PCF coverage, and destination CBAM cost rules from the Product & Carbon DB; caches retrieved data in the EcoTrace Database; and transitions to `S-F03-02` once matching options are retrieved (or triggers a correction loop on validation/empty/system errors).

**The user leaves this screen when:** valid comparison criteria match at least one product profile and data retrieval completes, automatically navigating to `S-F03-02` (**Green Steel Comparison & Recommendation Board**), or when the user navigates away via the global EcoTrace sidebar.

## 2. Mockup


Default — numbered
<img width="1453" height="1037" alt="S-F03-01 — Default — numbered" src="img/S-F03-01%20%E2%80%94%20Default%20%E2%80%94%20numbered.png" />
Validation Error — numbered
<img width="1453" height="1037" alt="S-F03-01 — Validation Error — numbered" src="img/S-F03-01%20%E2%80%94%20Validation%20Error%20%E2%80%94%20numbered%20.png" />
No Matching Products — numbered
<img width="1453" height="1037" alt="S-F03-01 — No Matching Products — numbered" src="img/S-F03-01%20%E2%80%94%20No%20Matching%20Products%20%E2%80%94%20numbered%20.png" />
Loading · Comparing — numbered
<img width="1453" height="1037" alt="S-F03-01 — Loading · Comparing — numbered" src="img/S-F03-01%20%E2%80%94%20Loading%20%C2%B7%20Comparing%20%E2%80%94%20numbered%20.png" />
Dropdown Open — numbered
<img width="1453" height="1037" alt="S-F03-01 — Dropdown Open — numbered" src="img/S-F03-01%20%E2%80%94%20Dropdown%20Open%20%E2%80%94%20numbered%20.png" />
Data Error — numbered
<img width="1453" height="1037" alt="S-F03-01 — Data Error — numbered" src="img/S-F03-01%20%E2%80%94%20Data%20Error%20%E2%80%94%20numbered%20.png" />
Criteria Selected — numbered
<img width="1453" height="1037" alt="S-F03-01 — Criteria Selected — numbered" src="img/S-F03-01%20%E2%80%94%20Criteria%20Selected%20%E2%80%94%20numbered%20.png" />


## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation / behaviour |
|---|---|---|---|---|---|
| 1 | App shell | Navigation shell | EcoTrace workspace navigation (`Overview`, `F-01 Carbon Traceability & PCF`, `F-02 CBAM Compliance`, `F-03 Green Steel → Product Comparison` active, `Help & Support`); role badge `Customers / Procurement` | Yes | Hide `F-04` administration links for Customers / Procurement users. Enforce role-based access control. |
| 2 | Breadcrumb & page header | Header | Breadcrumb: `Green Steel / Product Comparison`; Title: `Green Steel Filter & Comparison Criteria`; Subtitle: *"Select the product category, steel grade and destination market to compare low-carbon steel options."* | Yes | Static page identification. |
| 3 | Product category selector | Dropdown select | Product families from Product & Carbon DB (illustrative options: `Hot-rolled coil (HRC)`, `Cold-rolled coil (CRC)`, `Rebar`, `Wire rod`, `Plate`, `Structural sections`) | Yes (`*`) | Required (`FR-03-01`). Placeholder: `Select product category`. Shows checkmark (`✓`) on active selection when open. |
| 4 | Steel grade selector | Dropdown select | Steel grades & production routes from Product & Carbon DB (e.g., `S355 · Low-carbon EAF`, `SAE 1008 · Hydrogen DRI`) | Yes (`*`) | Required (`FR-03-01`). Placeholder: `Select steel grade`. Highlighted in red with inline error *"Please select a steel grade."* if submitted empty. |
| 5 | Destination market selector | Dropdown select | Target export markets & carbon regimes (e.g., `Germany (EU CBAM)`, `Japan`) | Yes (`*`) | Required (`FR-03-01`). Placeholder: `Select destination market`. Highlighted in red with inline error *"Please select a destination market."* if submitted empty. |
| 6 | Scoring basis field | Read-only display field | Static formula summary: `Weighted score (carbon · CBAM cost · compliance)` | Yes | Non-editable informational field showing the `FR-03-03` scoring model applied downstream. |
| 7 | Criteria status helper text | Contextual status text | Dynamic guidance text below form inputs | Yes | Default: *"Select all three criteria, then compare. Scores use product carbon profiles and the destination CBAM cost estimate."* Criteria Selected: *"All 3 criteria selected. Ready to compare against product carbon profiles and the CBAM cost estimate for [Destination]."* Validation Error: *"All three criteria are required. Correct the highlighted fields to continue."* |
| 8 | Compare green steel options | Primary button | Action label: `Compare green steel options` (changes to `◌ Comparing…` while loading) | Yes | Triggers `UC-09` (`AD-UC-09` `a1` → `s1` → `s3`). Disabled during active API request to prevent duplicate submissions. |
| 9 | Reset criteria | Secondary button | Action label: `Reset criteria` | Yes | Clears all selected dropdown values, clears validation/no-match/error banners, and restores the **Default** state. |
| 10 | Validation error banner | Inline alert banner (red) | Dynamic count of missing fields (e.g., *"2 required criteria are missing"* + explicit field names) | Conditional | Shown when user submits without selecting all 3 required fields (`AD-UC-09` branch `d1 [Invalid]` → `s2`). |
| 11 | Comparing progress panel | Inline progress card | Step-by-step backend status feedback (`✓ Criteria validated`, `◌ Retrieving product carbon profiles…`, `○ Calculating weighted scores`) | Conditional | Shown while `SD-UC-09` steps 3–14 are executing. Transient UI feedback only. |
| 12 | No matching products notice | Inline warning card (amber) | Header: `NO MATCHING PRODUCTS` — *"No green steel products match these criteria"*; echoes selected criteria without clearing them; lists 3 actionable suggestions (different steel grade, another destination market, or reset criteria) | Conditional | Shown when Product & Carbon DB returns 0 exact matches (`AD-UC-09` branch `d2 [No]` → `s4`). |
| 13 | System / Data error banner | Inline error banner (red) | Header: *"Unable to retrieve product carbon profiles"*; message preserving user criteria; `Retry` button; error reference footer (`Reference: ERR-PCDB-503 · Product & Carbon DB unreachable`) | Conditional | Shown when Product & Carbon DB (`SD-UC-09` step 4) fails or times out. |
| 14 | How the comparison works card | Informational panel | Explains steps `01 Product category`, `02 Steel grade`, `03 Destination market` (`FR-03-01` & `FR-03-02`) | Yes | Static reference panel on the right column. |
| 15 | Comparison flow stepper | Progress tracker | 3-step visual flow: `1 Validate selected criteria`, `2 Retrieve product carbon profiles`, `3 Calculate weighted score and recommend` | Yes | Updates step icons dynamically across screen states (`1/2/3` numbers in Default; `✓` on step 1 in Criteria Selected/Loading; `!` on step 1 in Validation Error; `!` on step 2 in No Matching Products & System Error). |
| 16 | Design preview notice | Footer info banner | *"Design preview — No live product or carbon values are connected. Illustrative data only."* + traceability tags (`FR-03-01 · FR-03-02 · UC-09`) | Yes | Present in prototype/design preview mode; hide or replace in production builds. |

## 4. States

| State | What the user sees | Trigger | Notes / source |
|---|---|---|---|
| Default | All 3 dropdowns show placeholders (`Select product category`, `Select steel grade`, `Select destination market`); helper text prompts selecting all 3 criteria; Comparison flow shows steps `1`, `2`, `3` in neutral state. | User opens `S-F03-01` or clicks `Reset criteria`. | `S-F03-01 — Default.pdf` |
| Dropdown Open | Active dropdown expands below the field showing available options with a checkmark (`✓`) next to the currently highlighted/selected item (e.g., `Hot-rolled coil (HRC)`). | User clicks/taps any of the 3 criteria dropdowns. | `S-F03-01 — Dropdown Open.pdf` |
| Criteria Selected | All 3 dropdowns display valid values; helper text updates to *"All 3 criteria selected. Ready to compare..."*; Comparison flow step 1 displays `✓ Validate selected criteria`. | User selects valid values for Product category, Steel grade, and Destination market. | `S-F03-01 — Criteria Selected.pdf` |
| Validation Error | Red summary banner at top of form (*"2 required criteria are missing"*); missing dropdowns outlined in red with inline error text (*"Please select a steel grade."*, *"Please select a destination market."*); Comparison flow step 1 shows `!`. | User clicks `Compare green steel options` while one or more required criteria are unselected (`AD-UC-09` `d1 [Invalid]`). | `S-F03-01 — Validation Error.pdf` (`s2`) |
| Loading / Comparing | Primary button label changes to `◌ Comparing…` and is disabled; inline progress card **Preparing your comparison** appears below buttons showing `✓ Criteria validated`, `◌ Retrieving product carbon profiles…`, `○ Calculating weighted scores`. | User submits all 3 valid criteria (`AD-UC-09` `d1 [Valid]` → `s3`). | `S-F03-01 — Loading · Comparing.pdf` |
| Empty (No Matching Products) | Selected criteria remain preserved in dropdowns; amber card **NO MATCHING PRODUCTS** explains no verified low-carbon profiles were found for the exact combination and lists 3 adjustment tips; Comparison flow step 2 shows `!`. | Product & Carbon DB returns zero matching records (`AD-UC-09` `d2 [No]` → `s4`). | `S-F03-01 — No Matching Products.pdf` (correction loop back to `m1`) |
| System / Data Error | Selected criteria remain preserved; red error banner *"Unable to retrieve product carbon profiles"* appears at top of form with an inline `Retry` button and technical reference `ERR-PCDB-503 · Product & Carbon DB unreachable`; Comparison flow step 2 shows `!`. | Product & Carbon DB query (`SD-UC-09` step 4) fails or times out. | `S-F03-01 — System/Data Error.pdf` |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Product category / Steel grade / Destination market dropdowns | Clicks and selects an option | Opens option list, updates selected criterion, clears field-level validation error if previously invalid, and updates helper/stepper status when all 3 are filled. | Stays on `S-F03-01` |
| 2 | Compare green steel options | Clicks | Runs client/server parameter validation (`AD-UC-09` `s1`). If invalid, displays Validation Error state (`s2`). If valid, enters Loading state, queries Product & Carbon DB (`s3`), and stores retrieved data (`SD-UC-09` steps 4–7). | `S-F03-02` (if matches found & scores calculated); stays on `S-F03-01` (if validation, no-match, or DB error) |
| 3 | Reset criteria | Clicks | Resets all 3 dropdowns to default placeholders and dismisses any validation, no-match, or system error banners. | Stays on `S-F03-01` |
| 4 | Retry (in System/Data Error banner) | Clicks | Re-submits the preserved 3 criteria to Product & Carbon DB without requiring re-entry. | `S-F03-02` (on recovery) or stays on `S-F03-01` |
| 5 | Sidebar navigation items | Clicks permitted module | Navigates away from `F-03` via the global EcoTrace workspace shell. | Target screen outside `UC-09` |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | All three comparison criteria (**Product category**, **Steel grade**, and **Destination market**) are mandatory before querying product profiles. | `FR-03-01`; `AD-UC-09` `s1`, `d1` |
| SR-002 | On validation failure (`d1 [Invalid]`), the system must identify every missing field both in a top summary banner and inline beneath each missing dropdown, initiating a user correction loop without losing already-selected fields. | `AD-UC-09` `s2`; `S-F03-01 — Validation Error` |
| SR-003 | On zero matching products (`d2 [No]`), the system must preserve the user's current filter values, display the exact queried combination, and provide actionable filter adjustment suggestions rather than showing an empty results board. | `FR-03-02`; `AD-UC-09` `s4`; `S-F03-01 — No Matching Products` |
| SR-004 | On Product & Carbon DB failure (`ERR-PCDB-503`), user-selected criteria must remain preserved in state and an immediate **Retry** action must be provided. | `SD-UC-09` step 4; `S-F03-01 — System/Data Error` |
| SR-005 | Duplicate submissions must be blocked while `Comparing…` is in progress. | `UI Conventions`; `S-F03-01 — Loading · Comparing` |
| SR-006 | Step status and errors must always be communicated via explicit text and icons (`✓`, `!`, `◌`), never by border/banner color alone. | `UI Conventions` |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| `FR-03-01` (Must) | Captures and validates the user's comparison criteria (product category, steel grade, destination market) before initiating comparison (`FR-03-01-BE`). |
| `FR-03-02` (Must) | Submits validated filters to query matching green steel product carbon profiles and destination CBAM cost parameters (`FR-03-02-BE`, `FR-03-02-INT`), and handles zero-match filter adjustment guidance. |

## 8. Responsive and accessibility notes

- **Smallest supported width:** `360px` (mobile) and `1280px+` (standard desktop workspace).
- **What collapses or stacks on a narrow screen:** Below the desktop breakpoint (`< 1024px`), the left workspace sidebar collapses into a slide-over drawer; the two-column main layout stacks vertically with the **Comparison criteria** card on top and **How the comparison works / Comparison flow** below; the 2-column form grid (`Product category` / `Steel grade` and `Destination market` / `Scoring basis`) stacks into a single column.
- **Information never carried by colour alone:** Validation errors pair red borders with `!` icons and explicit field-level text; stepper progress uses numeric badges (`1`, `2`, `3`), checkmarks (`✓`), spinners (`◌`), and warning icons (`!`).
- **Text contrast and minimum size:** Minimum body text size `14px` (`12px` for helper/reference tags); all text and interactive controls meet WCAG AA (`>= 4.5:1` contrast ratio). Every dropdown has a visible `<label>` with a required indicator (`*`) and `aria-invalid` / `aria-describedby` bindings on error.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | Should **Steel grade** dropdown options be dynamically filtered based on the selected **Product category** (cascading dropdown), or list all grades independently? | No | Open — confirm with `FR-03-01` data dictionary before frontend integration. |
| 2 | When `d2 [No]` occurs (`S-F03-01 — No Matching Products`), `AD-UC-09` mentions *"Check partial matches or suggest filter adjustments"*. Should v1 display clickable partial-match product cards directly on `S-F03-01`, or keep the current text-based filter adjustment suggestions? | No | Resolved for v1 UI (`S-F03-01 — No Matching Products` displays actionable text suggestions while preserving inputs). |
| 3 | Are destination markets limited to CBAM jurisdictions (e.g., Germany / EU) or do non-CBAM markets (e.g., Japan in the No Matching Products frame) use local carbon pricing rules under `FR-03-02`? | Yes, for CBAM cost calculation rules | Open — confirm market-to-carbon-rule mapping in `FR-03-02`. |

---

## Completion checklist

- [x] All 7 numbered state mockups are embedded in Section 2 and stored in the repository's `img/` folder.
- [x] Every visible element in the mockup appears in the element inventory.
- [x] Every input element has a validation rule or an explicit read-only rule.
- [x] All states (Default, Dropdown Open, Criteria Selected, Validation Error, Loading, Empty/No Match, System Error) are filled in and traced to `AD-UC-09` / `SD-UC-09`.
- [x] Every navigation target (`S-F03-02`) exists in the Screen List.
- [x] Every linked FR (`FR-03-01`, `FR-03-02`) exists in the Function List and `AD-UC-09`.
