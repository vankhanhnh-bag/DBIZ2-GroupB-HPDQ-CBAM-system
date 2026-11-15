# UI Conventions: EcoTrace

> **Version:** v1.0 · **Status:** Draft for team confirmation  
> **Product:** EcoTrace — HPDQ Carbon Management & Reporting System  
> **Scope:** Responsive web application covering F-01 Carbon Traceability & PCF, F-02 CBAM Compliance & Carbon Cost, F-03 Green Steel Comparison, and F-04 Platform, Access & Operations.

## Visual direction

The initial mockup is the **visual reference**, not a source of functional requirements. Keep its Hòa Phát navy / EcoTrace green identity, clean white workspace, compact left navigation and data-dashboard feel. Use the approved Hòa Phát and EcoTrace logo assets; do not redraw or approximate the logos.

| Token | Proposed value | Usage |
|---|---|---|
| Brand navy | `#003068` | Brand, sidebar and primary headings |
| Action blue | `#155EEF` | Primary actions, active navigation and links |
| EcoTrace mint | `#4FCEAA` | Restrained accent, supporting highlights and positive indicators |
| Page background | `#F5F7FA` | Main application canvas |
| Surface | `#FFFFFF` | Cards, tables, forms and dialogs |
| Main text | `#102A43` | Headings and body copy |
| Muted text | `#61758A` | Hints, metadata and secondary labels |
| Border | `#D9E2EC` | Card, input and table boundaries |
| Success / warning / error | `#15803D` / `#B45309` / `#B42318` | Semantic feedback, always paired with text or an icon |

*Color values are proposed approximations from the supplied concept mockup, not confirmed corporate brand specifications. Confirm them against official brand assets before final implementation.*

- **Application shell:** On desktop, use a persistent left sidebar (about 240 px), a top app bar (about 64 px), breadcrumbs, page title and a main content area with 24 px page padding. Do not reproduce the full Hòa Phát public-website navigation inside the authenticated application; use a compact EcoTrace app header with Hòa Phát attribution where appropriate.
- **Navigation:** Group destinations by the four W0 features. Show the current location clearly. Display only destinations available to the signed-in role; still enforce authorization on every protected request (FR-04-03).
- **Responsive layout:** At widths below 1200 px, collapse the sidebar into a drawer. At mobile widths, use a single-column layout, stack KPI cards and allow wide data tables to scroll or switch to a readable card/list presentation. Minimum supported width: 360 px.
- **Layout and typography:** Use a consistent 8 px spacing rhythm, 8–12 px card radius, subtle borders and restrained shadows. Use Inter or a comparable sans-serif with a system-font fallback. Keep dense operational data readable; avoid decorative gradients, oversized hero text and unnecessary animation.
- **Data visualization:** Every chart must identify its period, units, axis labels and legend. Keep numeric precision consistent. Never rely on color alone to distinguish lifecycle stages, data-quality levels or statuses.

## 1. Language and tone

- **Interface language:** English by default for the current design baseline, matching the supplied mockup and repository screen names. Vietnamese localization may be added as a complete, consistent alternative; do not mix languages within one screen.
- **Address the user as:** “you” in English; “bạn” in Vietnamese. Use neutral, professional wording suitable for HPDQ departments, auditors and customers.
- **Terminology:** Write the full term on first use, followed by its abbreviation: Product Carbon Footprint (PCF), Carbon Border Adjustment Mechanism (CBAM), Role-Based Access Control (RBAC), and tonnes of CO₂ equivalent (tCO₂e). Keep feature, use-case and screen names aligned with W0 and the Screen List.
- **Button labels:** Use an action verb plus an object, such as “Calculate PCF”, “Estimate CBAM cost”, “Save role configuration”, “Apply filters” and “View audit logs”. Avoid vague labels such as “OK”, “Yes” or “Submit”.

## 2. Formats

| Item | Format | Example |
|---|---|---|
| Date | `dd/MM/yyyy` | `15/11/2026` |
| Date and time | `dd/MM/yyyy HH:mm`, 24-hour clock; show ICT (UTC+07:00) where audit timestamps need a timezone | `15/11/2026 14:30 ICT` |
| Decimal / thousands | Decimal point; comma thousands separator | `1,234.56` |
| Percentage | One or two decimal places as required; always show `%` | `68.5%` |
| Currency | Amount followed by ISO currency code; do not infer currency | `1,250,000 VND`; `85.50 EUR` |
| Carbon / PCF | Always show unit and denominator | `1.28 tCO₂e/t product`; `420 kgCO₂e/t steel` |
| EU ETS / carbon price | Currency per emissions unit | `85.50 EUR/tCO₂e` |
| Identifiers | Preserve source-system value and capitalization; do not reformat | Order No., Shipment No., Production Lot, Batch ID, Supplier ID |
| Names | Show the person's or organization's supplied display name; retain official department names | `Export Sales & Marketing`; `Sustainability & ESG` |

## 3. Components (use these names in every Screen Spec)

| Component | When to use | Notes |
|---|---|---|
| App shell (sidebar + top bar) | Global navigation and signed-in context | Role-aware navigation; keep page title and breadcrumbs consistent |
| Primary button | The one main action of a screen | One visually dominant primary action per screen |
| Secondary button | Alternative or back actions | Outline or neutral treatment; never compete with the primary action |
| Text input / Select / Date picker | Search, filters and data entry | Label above control; helper or validation text below; never rely on placeholder alone |
| Filter bar | Narrowing shipments, products, users or audit events | Keep applied filters visible and provide a clear way to remove them |
| Data table | Shipment, product, user, rule-set or audit records | Clear column labels; right-align numeric values; preserve units; support sorting only where defined |
| KPI card / Chart | PCF, emissions, CBAM exposure and comparison summaries | State value, unit, period and source context; include a text/table alternative for charts |
| Status badge | Data quality, record or process status | Use an approved status label plus icon/text; do not communicate status by color alone |
| Banner | Important, non-blocking information or warnings | Place near the affected content; explain the impact and next action |
| Modal | Confirm or cancel one consequential action | Never use a modal for navigation; show the action and consequence clearly |
| Toast | Short confirmation after success | Concise, non-blocking, disappears after about 4 seconds; important errors remain visible inline |

## 4. Messages

| Situation | Pattern | Example |
|---|---|---|
| Validation error | What is wrong + how to fix it | “The selected shipment has no linked production batch. Check the shipment-to-batch mapping, then try again.” |
| System / network error | What happened + safe next action | “EcoTrace could not retrieve the shipment data. Retry. If the problem continues, contact support.” |
| Empty state | Why there is no result + next action | “No audit events match these filters. Change the date range or clear the filters.” |
| Permission denied | State that access is unavailable + who can help | “You do not have permission to change access settings. Contact a System Admin.” |
| Success | State what was completed | “Shipment PCF calculated successfully.” / “RBAC configuration updated.” |

Do not expose passwords, OTPs, access tokens or other authentication secrets in messages, tables or audit-log details. Do not invent business statuses: display only states defined by the relevant use case or data contract.

## 5. Accessibility minimums

- Body text contrast at least **4.5:1** (WCAG 2.2, success criterion 1.4.3).
- Never communicate status, data quality, chart series or errors by color alone; pair color with text, icon, pattern or label (WCAG 2.2, 1.4.1).
- Interactive targets at least **24 × 24 CSS px** (WCAG 2.2, 2.5.8); prefer about 40–44 px for frequently used touch controls.
- All controls must have visible keyboard focus, accessible names and a logical tab order. Associate field errors with their inputs.
- Use semantic headings and table headers. Provide text summaries or tabular alternatives for charts.
- Smallest supported screen width: **360 px**. Content must remain usable at browser zoom and on narrow screens.
