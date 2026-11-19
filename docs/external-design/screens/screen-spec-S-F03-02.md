# Screen Spec: S-F03-02 Green Steel Comparison & Recommendation Board

> **Product:** EcoTrace — HPDQ Carbon Management & Reporting System  
> **Status:** Draft — all 5 UI state frames (Default, Loading · Calculating Scores, Success · Recommendation Saved, Partial Carbon Data, System/Data Error) are defined in the supplied design exports.  
> **Source of truth:** UC-09 sequence diagram (`SD-UC-09`), `AD-UC-09`, `FR-03-03` & `FR-03-04`, Screen List, and UI Conventions.

| Field | Value |
|---|---|
| Screen ID | `S-F03-02` |
| Screen name | Green Steel Comparison & Recommendation Board |
| Actor | Customers / Procurement |
| Priority | Must |
| Feature | `F-03` Green Steel Comparison & Recommendation |
| Use case | `UC-09` Compare and recommend green steel options |
| Activity diagram | `AD-UC-09` — fetch PCF & data quality (`s5`), calculate carbon intensity & CBAM cost (`s6`), calculate overall score (`s7`), rank and assign Primary / Alternative flags (`s8`), store results (`s9`), and display results (`a2`) |
| Linked sequence | `SD-UC-09` — steps 8–15: EcoTrace BE calculates weighted score (`8`), stores comparison results in EcoTrace Database (`9–10`), sorts and assigns Primary/Alternative flags (`11`), stores recommendation (`12–13`), returns options to web UI (`14`), and displays options to Customer (`15`) |
| Mockup image | `img/S-F03-02.png` (covers 5 exported state frames: Default, Loading · Calculating Scores, Success · Recommendation Saved, Partial Carbon Data, System/Data Error) |
| Status values | `Calculated & Ranked` (object state of Comparison & Recommendation Results in `AD-UC-09` `o1`); recommendation flags displayed per option: `PRIMARY`, `ALTERNATIVE`, `SELECTED`, `REVIEW`, `INCOMPLETE DATA`. |

## 1. Purpose

**Shown when:** the system finishes retrieving matching green steel profiles from `S-F03-01` and initiates weighted scoring and recommendation ranking (`UC-09` `FR-03-03`, `FR-03-04`).

**The user does:** reviews the weighted comparison scores, carbon intensity, estimated CBAM export cost, PCF data compliance, and ranked Primary / Alternative flags, and confirms/saves a selected green steel option (or returns to edit filter criteria).

**The system does:** executes weighted scoring (`50%` Carbon intensity, `30%` CBAM cost, `20%` Compliance), ranks matching products from highest to lowest score, assigns `PRIMARY` / `ALTERNATIVE` flags (downgrading options with incomplete carbon data), persists comparison and recommendation records to the EcoTrace Database, and displays the board.

**The user leaves this screen when:** they click **Edit criteria** to return to `S-F03-01`, click **View carbon profile** to inspect underlying product PCF traceability (`F-01`), or navigate to another module via the EcoTrace workspace sidebar.

## 2. Mockup

![S-F03-02](img/S-F03-02.png)

The supplied design exports follow the agreed EcoTrace visual direction: dark navy left sidebar (`EcoTrace · HPDQ`), white main workspace, breadcrumb header (`Green Steel / Comparison Board`), active criteria summary strip with **Edit criteria** action, two-column upper section (left: **Recommended / Selected Option** hero card; right: **Score breakdown** weight card), and full-width **Product comparison** ranked table below.

- **Supplied UI state frames:**
  1. `S-F03-02 — Loading · Calculating Scores.pdf`
  2. `S-F03-02 — Partial Carbon Data.pdf`
  3. `S-F03-02 — Success · Recommendation Saved.pdf`
  4. `S-F03-02 — System/Data Error.pdf`

Illustrative product names, scores (`92`, `84`, `82`, `61*`), and emission/cost figures in the mockups are design placeholders and must be populated dynamically from backend calculation outputs.

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation / behaviour |
|---|---|---|---|---|---|
| 1 | App shell | Navigation shell | EcoTrace workspace navigation (`F-03 Green Steel → Product Comparison` active); role badge `Customers / Procurement` | Yes | Consistent global shell; hide `F-04` admin links for operational users. |
| 2 | Breadcrumb & page header | Header | Breadcrumb: `Green Steel / Comparison Board`; Title: `Green Steel Comparison & Recommendation Board`; Subtitle: *"Review weighted scores and the recommended green steel option for your selected criteria."* | Yes | Static page identification. |
| 3 | Active criteria summary bar | Context bar & button | Displays `Comparing for`: **Product category** (e.g., `Hot-rolled coil (HRC)`), **Steel grade** (e.g., `S355 · Low-carbon EAF`), **Destination market** (e.g., `Germany (EU CBAM)`), **Scoring model** (`Weighted score`), plus `Edit criteria` button | Yes | Preserves context from `S-F03-01`. Clicking `Edit criteria` navigates back to `S-F03-01` with criteria pre-filled. |
| 4 | Top recommendation hero card | Summary card | Displays top-ranked product title, rationale summary, overall score badge (`[score] / 100`), 3 KPI metrics (**Carbon intensity** in `tCO2e per tonne`, **CBAM cost estimate** in `€/tonne exported`, **Compliance** `% verified PCF data`), and status/action footer | Yes (in loaded states) | Header badge reflects state: `RECOMMENDED · PRIMARY OPTION` (default/partial data), `SELECTED · RECOMMENDATION SAVED` (saved state), or `— Recommendation unavailable` (error state). |
| 5 | Select this option / Saved confirmation | Primary button / status box | Before selection: `Select this option` primary button + `View carbon profile` secondary button. After selection: green confirmation box `✓ Recommendation saved` naming the selected product and criteria. | Yes | Clicking `Select this option` persists the customer's chosen recommendation (`SD-UC-09` steps 12–13) and transitions the card to `SELECTED · RECOMMENDATION SAVED`. |
| 6 | View carbon profile | Secondary button | Action label: `View carbon profile` | Yes | Opens detailed PCF breakdown for the recommended product (`F-01` Carbon Traceability). Disabled in System/Data Error state. |
| 7 | Score breakdown card | Read-only criteria card | Displays weighting applied to each criterion (`FR-03-03`): **Carbon intensity (`50%`)**, **CBAM cost (`30%`)**, **Compliance (`20%`)**, plus `Design preview` notice | Yes | Static explanation of the weighted scoring algorithm (`AD-UC-09` `s7`). |
| 8 | Calculating progress card | Inline progress panel | Header: `Calculating your recommendation` — *"Scoring [N] products against your criteria and saving the result."* with step checklist (`✓ Criteria validated`, `✓ Product carbon profiles loaded`, `◌ Calculating weighted scores…`, `○ Saving recommendation`) | Conditional | Shown during `S-F03-02 — Loading · Calculating Scores` while `SD-UC-09` steps 8–13 run. |
| 9 | Partial carbon data alert banner | Inline warning banner (amber) | Header: *"Carbon data is incomplete for [X] of [N] options."* naming the specific product missing carbon intensity and explaining its provisional score; includes `Review missing data` button | Conditional | Shown when at least one matched product lacks complete verified PCF metrics (`AD-UC-09` `s5` data quality check). |
| 10 | System / Data error banner | Inline error banner (red) | Header: *"Unable to calculate the recommendation"* — *"The scoring service did not respond. Your criteria are saved — please retry."* + `Retry` button + footer `Reference: ERR-SCORE-503 · Scoring service unreachable` | Conditional | Shown when the scoring calculation service fails (`SD-UC-09` step 8). |
| 11 | Product comparison table | Ranked data table | Columns: `Rank`, `Product`, `Carbon intensity (tCO2e/t)`, `CBAM cost (€/t)`, `Compliance`, `Weighted score`, `Recommendation flag`. Sorted by weighted score descending. | Yes | Shows skeleton rows while loading; empty placeholder with `ERR-SCORE-503` on error; ranked rows with flag badges (`PRIMARY` / `SELECTED`, `ALTERNATIVE`, `REVIEW`, `INCOMPLETE DATA`) when loaded. |
| 12 | Table footnote & traceability footer | Footnote & tags | Explains provisional score rules (`*`) when incomplete data exists; bottom tags: `FR-03-03 · FR-03-04 · UC-09 · Weighted product comparison & recommendation` | Yes | Always visible below comparison table. |

## 4. States

| State | What the user sees | Trigger | Notes / source |
|---|---|---|---|
| Loading / Calculating Scores | Active criteria bar is visible; inline progress card **Calculating your recommendation** shows step-by-step scoring & saving progress; **Score breakdown** (`50% / 30% / 20%`) is visible; **Product comparison** table shows 3 skeleton placeholder rows with *"Calculating scores…"* | Transition from `S-F03-01` after profiles are retrieved and `SD-UC-09` steps 8–13 are running. | `S-F03-02 — Loading · Calculating Scores.pdf` |
| Partial Carbon Data (Loaded / Unsaved) | Top hero card shows `RECOMMENDED · PRIMARY OPTION` (`92 / 100`) with active `Select this option` button; amber banner warns *"Carbon data is incomplete for 1 of 3 options"* with `Review missing data` button; table ranks verified options first (`1 PRIMARY`, `2 ALTERNATIVE`) and places incomplete option last (`3 INCOMPLETE DATA`, `Not reported`, `€22.6 (est.)`, `58%`, provisional score `61*`) with explanatory footnote. | Scoring completes (`AD-UC-09` `s5–s8`), and one or more matched products have missing/unverified PCF intensity data. | `S-F03-02 — Partial Carbon Data.pdf` |
| Success / Recommendation Saved | Top hero card updates badge to `SELECTED · RECOMMENDATION SAVED`; `Select this option` button is replaced by green confirmation banner `✓ Recommendation saved` (*"[Product] is now the selected option for [Criteria]"*); table row 1 flag updates from `PRIMARY` to `SELECTED` alongside `ALTERNATIVE` and `REVIEW` rows. | Weighted scoring & recommendation storage complete (`SD-UC-09` steps 11–15) or user clicks `Select this option`. | `S-F03-02 — Success · Recommendation Saved.pdf` (`AD-UC-09` `o1 [Calculated & Ranked]`, `s9`) |
| System / Data Error | Red error banner *"Unable to calculate the recommendation"* with `Retry` button; hero card shows `— Recommendation unavailable` and `—` for all 3 KPIs with disabled action buttons; comparison table shows *"Comparison results are unavailable"* and `Reference: ERR-SCORE-503 · Scoring service unreachable`. | Scoring service fails or times out during `SD-UC-09` step 8. | `S-F03-02 — System/Data Error.pdf` |
| Empty (no data) | **Not applicable on `S-F03-02`.** Zero-match queries are intercepted on `S-F03-01` (`AD-UC-09` branch `d2 [No]` → `s4`) before navigating to this board. | Handled upstream on `S-F03-01`. | `AD-UC-09` section 3 |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Edit criteria | Clicks | Returns to the filter screen with the current Product category, Steel grade, and Destination market pre-populated for modification. | `S-F03-01` |
| 2 | Select this option | Clicks | Persists the selected product as the customer's chosen recommendation in EcoTrace Database (`SD-UC-09` steps 12–13) and updates the UI to the **Success · Recommendation Saved** state (`SELECTED` flag). | Stays on `S-F03-02` |
| 3 | View carbon profile | Clicks | Opens the detailed Product Carbon Footprint & traceability breakdown for the recommended steel product (`F-01`). | Target `F-01` PCF detail screen (outside `UC-09`) |
| 4 | Review missing data (in Partial Carbon Data banner) | Clicks | Scrolls to / highlights the incomplete product row in the comparison table or opens its data quality details (`AD-UC-09` `s5`). | Stays on `S-F03-02` |
| 5 | Retry (in System/Data Error banner) | Clicks | Re-triggers weighted score calculation and recommendation persistence for the preserved criteria (`SD-UC-09` steps 8–14). | Stays on `S-F03-02` |
| 6 | Sidebar navigation items | Clicks permitted module | Navigates to another module via the global EcoTrace workspace shell. | Target screen outside `UC-09` |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | Weighted scores must be calculated using the approved criterion weights: **50% Carbon intensity** (lower embedded emissions score higher), **30% CBAM cost** (lower estimated export cost scores higher), and **20% Compliance** (verified PCF coverage and data completeness). | `FR-03-03`; `AD-UC-09` `s5–s7`; `S-F03-02` Score breakdown |
| SR-002 | Matching options must be sorted in descending order of weighted score and assigned explicit recommendation flags (`PRIMARY` / `SELECTED`, `ALTERNATIVE`, `REVIEW`, or `INCOMPLETE DATA`). | `FR-03-04`; `AD-UC-09` `s8`; `SD-UC-09` step 11 |
| SR-003 | Options with missing or unverified carbon intensity must receive a provisional score marked with `*` (`INCOMPLETE DATA`), use estimated CBAM cost marked `(est.)`, and always rank below fully verified options regardless of raw partial score. | `FR-03-03`, `FR-03-04`; `S-F03-02 — Partial Carbon Data` |
| SR-004 | Calculated comparison results and saved recommendations must be persisted in the EcoTrace Database before confirming `Recommendation saved` to the user. | `FR-03-04`; `AD-UC-09` `s9`; `SD-UC-09` steps 9–13 |
| SR-005 | All numeric metrics must be displayed with explicit units (`tCO2e/t` or `tCO2e per tonne`, `€/t` or `per tonne exported`, `%`, `/ 100`). Missing metrics must display `Not reported` or `—` rather than `0`. | `UI Conventions` |
| SR-006 | Recommendation flags and alert states must be conveyed using text badges and icons (`✓`, `!`, `*`), never by badge color alone. | `UI Conventions` |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| `FR-03-03` (Must) | Computes and displays weighted scores (`50%` carbon intensity, `30%` CBAM cost, `20%` compliance) and data completeness metrics for all matching green steel options (`FR-03-03-BE`, `FR-03-03-FE`). |
| `FR-03-04` (Must) | Ranks scored products, assigns `PRIMARY` / `ALTERNATIVE` / `INCOMPLETE DATA` flags, persists comparison and recommendation records, and lets the user confirm/save their chosen option (`FR-03-04-BE`, `FR-03-04-FE`). |

## 8. Responsive and accessibility notes

- **Smallest supported width:** `360px` (mobile) and `1280px+` (standard desktop workspace).
- **What collapses or stacks on a narrow screen:** Below `1024px`, the workspace sidebar collapses into a drawer; the upper two-column grid (**Recommended option** card and **Score breakdown** card) stacks vertically; the 3 KPI tiles inside the hero card stack on `< 480px`; the **Product comparison** table enables horizontal scrolling (`overflow-x: auto`) with sticky `Rank` and `Product` columns so metrics remain readable.
- **Information never carried by colour alone:** Flags (`PRIMARY`, `SELECTED`, `ALTERNATIVE`, `REVIEW`, `INCOMPLETE DATA`) use distinct uppercase text labels inside pill badges; provisional values pair amber styling with `*` and `(est.)` text markers.
- **Text contrast and minimum size:** Minimum `14px` body text (`12px` for table units and footnotes); all status pills, warning banners, and buttons maintain `>= 4.5:1` WCAG AA contrast. Skeleton loading rows expose `aria-busy="true"` and status announcements use `aria-live="polite"`.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | Are the scoring weights (`50%` Carbon intensity, `30%` CBAM cost, `20%` Compliance) fixed system constants in v1 (`FR-03-03`), or will Procurement users be allowed to customize weights in a future release? | No | Resolved for v1 — read-only display in the **Score breakdown** panel. |
| 2 | Does clicking `Select this option` auto-save immediately on load of the top `PRIMARY` option (as implied by `SD-UC-09` steps 12–13), or require an explicit user click as shown in `S-F03-02 — Partial Carbon Data` → `Success · Recommendation Saved`? | No | Clarified in UI flow — system stores the auto-ranked recommendation (`SD-UC-09` step 12), while explicit user confirmation via `Select this option` locks the `SELECTED` state. |
| 3 | Which specific screen ID in `F-01` does the **View carbon profile** button navigate to (e.g., product-level PCF detail vs. `S-F01-01`)? | No | Open — confirm target screen ID in `F-01` Screen List. |

---

## Completion checklist

- [x] The mockup is referenced as `img/S-F03-02.png` covering all 4 exported UI states.
- [x] Every visible element in the mockup appears in the element inventory.
- [x] All applicable states (Loading, Partial Carbon Data, Success/Saved, System Error, plus N/A justification for Empty) are filled in and traced to `AD-UC-09` / `SD-UC-09`.
- [x] Every navigation target (`S-F03-01`) exists in the Screen List.
- [x] Every element that displays data names its metric, unit, and source (`FR-03-03`, `FR-03-04`).
- [x] Every linked FR (`FR-03-03`, `FR-03-04`) exists in the Function List and `AD-UC-09`.
