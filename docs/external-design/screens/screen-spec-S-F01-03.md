# Screen Spec: S-F01-03 GHG Methodology & Emission Factor Library

> **Product:** EcoTrace — HPDQ Carbon Management & Reporting System  
> **Status:** Draft for team review  
> **Source of truth:** UC-02, SD-UC-02, AD-UC-02, FR-01-02 in W1, Screen List and UI Conventions.

| Field | Value |
|---|---|
| Screen ID | S-F01-03 |
| Screen name | GHG Methodology & Emission Factor Library |
| Actor | Sustainability & ESG Department |
| Priority | Must |
| Feature | F-01 Carbon Traceability & Product Carbon Footprint Management |
| Use case | UC-02 Manage GHG methodology & emission factors |
| Activity diagram | AD-UC-02 — enter parameters, validate, check for an existing factor/version, require justification for changes, version the record, write the audit event, and confirm success |
| Sequence diagram | SD-UC-02 — submit methodology version, factor value, unit and effective date; validate; check version existence; request change justification when the factor/version exists; store/version and write audit log |
| Linked requirement | FR-01-02 (Must) |
| Inputs | Methodology version; factor value; unit; effective date; conditional change justification |
| Important limitation | Exact validation ranges, allowed units, methodology/factor naming scheme, effective-date conflict rules and justification policy wording are not specified in FR-01-02. |

## 1. Purpose

**Shown when:** an authorized Sustainability & ESG user opens the GHG Methodology & Emission Factor Library to configure a GHG calculation methodology version or emission factor for use in carbon calculations.

**The user provides:** methodology version, factor value, unit and effective date. If the submitted factor/version already exists, the system asks for a change justification before allowing the update/versioning operation to proceed.

**The system does:** validate the four parameters; check whether the factor/version already exists; require a justification for an existing factor/version; set creation or update metadata; create a new version while keeping the previous version; write an audit-log entry containing who, when, old-to-new values and reason; then return a saved confirmation.

**The user leaves this screen when:** they review the saved confirmation or navigate to another permitted EcoTrace workspace feature. UC-02 does not specify a separate post-save destination.

## 2. Mockup
<img width="1440" height="1024" alt="image" src="https://github.com/user-attachments/assets/44977b37-ba30-4d17-bda2-0b7810138f19" />

**Default, numbered**

<img width="1440" height="1024" alt="S-F01-03 — Default - numbered" src="https://github.com/user-attachments/assets/bad5ba2b-bb5a-467d-afc8-d7234b058e25" />

**Loading, numbered**

<img width="1440" height="1024" alt="S-F01-03 — Loading · Validating   Saving - numbered" src="https://github.com/user-attachments/assets/29be2692-8d0c-4653-9d96-03051a53ffd4" />
 
**Success, numbered**

<img width="1440" height="1024" alt="S-F01-03 — Success · Version Saved - numbered" src="https://github.com/user-attachments/assets/cd035ed8-b358-4e16-8a3e-ecaaaa024491" />

**Error, numbered**

<img width="1440" height="1024" alt="S-F01-03 — Validation Error - numbered" src="https://github.com/user-attachments/assets/bdd16167-09e4-42c0-8e68-90fdffb256f2" />

**Change justification, numbered**

<img width="1440" height="1024" alt="S-F01-03 — Change Justification Required - numbered" src="https://github.com/user-attachments/assets/2aab388b-1a78-492e-9d91-472ba481fac9" />


## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation / behaviour |
|---|---|---|---|---|---|
| 1 | Application shell | Sidebar + top bar | EcoTrace navigation; signed-in Sustainability & ESG role | Yes | Shows Overview, applicable F-01/F-02/F-03 destinations and Help & Support only; hides F-04 administration navigation from standard users. Authorization remains enforced by the platform (FR-04-03). |
| 2 | Breadcrumb and page title | Header | Carbon Traceability & PCF / GHG Methodology & Emission Factors | Yes | Identifies the current screen. |
| 3 | Methodology version | Text input / version identifier | Methodology version supplied by Sustainability & ESG user (FR-01-02; SD-UC-02) | Yes | Must capture the configured methodology version. Exact version naming pattern is not specified. |
| 4 | Factor value | Numeric input | GHG emission factor value supplied by Sustainability & ESG user (FR-01-02) | Yes | Must capture the factor value. Allowed range, precision and numeric representation are not specified. |
| 5 | Unit | Select / text input (control type TBD) | Unit associated with the factor value (FR-01-02) | Yes | Must capture the unit. The list of permitted units is not specified. |
| 6 | Effective date | Date picker | Effective date supplied by Sustainability & ESG user (FR-01-02) | Yes | Must capture the effective date. Conflict rules and minimum/maximum dates are not specified. |
| 7 | Change justification | Text area | User-provided reason for changing an existing factor/version (SD-UC-02; AD-UC-02 d3) | Conditional | Required only when an existing factor/version is detected. The exact minimum length/content policy is not specified. |
| 8 | Save configuration | Primary button | Static action label | Yes | Submits the four parameters for validation. For an existing factor/version, the user must also provide the required justification. |
| 9 | Validation feedback | Error banner / field feedback | Validation errors returned by EcoTrace (SD-UC-02 validation alternative; AD-UC-02 d1) | Conditional | Explains that one or more parameters are invalid. Exact field-level messages depend on the approved validation rules. |
| 10 | Existing-version notice | Informational/warning banner | Existing factor/version detected by the GHG library lookup (SD-UC-02; AD-UC-02 d2) | Conditional | Explains that a change justification is required before the system versions the existing record. |
| 11 | Version history note | Read-only information panel | Versioning behaviour: create a new version and keep the previous version (SD-UC-02; AD-UC-02 s6) | Yes | Do not silently overwrite the previous version. |
| 12 | Audit-trail note | Read-only information panel / success detail | Audit entry fields: who, when, old-to-new values, reason (SD-UC-02; AD-UC-02 s7) | Yes, in success state | Show a concise confirmation that the audit event was recorded. Do not invent actor names, timestamps or event IDs. |
| 13 | Saved confirmation | Success banner / confirmation panel | Saved confirmation returned by EcoTrace (SD-UC-02; AD-UC-02) | Conditional | Display only after the new version is stored and the audit event is recorded. |
| 14 | Configuration overview |  |  |  | |

## 4. States

The five states below follow the validation, existing-version and justification branches in SD-UC-02 / AD-UC-02. **There is no separate save-failure or network-error state in the current UC-02 sequence/activity diagrams**, so this specification does not invent one.

| State | What the user sees | Trigger | Source / notes |
|---|---|---|---|
| **1. Default — Ready to configure** | Form with four labelled fields: Methodology version, Factor value, Unit and Effective date. Primary action: **Save configuration**. A side panel explains that valid parameters are checked for an existing factor/version and that changes are version-controlled. | Sustainability & ESG user opens S-F01-03. | FR-01-02; SD-UC-02 steps 1–2; AD-UC-02 actor action a1. |
| **2. Loading — Validating & saving** | Entered values remain visible; inputs and primary action are temporarily disabled. A progress panel communicates the processing sequence: validating parameters → checking whether the factor/version exists → storing the new version → recording the audit event. | User submits valid-looking configuration data. | Transient presentation state inferred from the multi-step system processing in SD-UC-02. It is not a persisted methodology/factor status. |
| **3. Validation Error — Correction required** | Error banner: **Some configuration parameters are invalid.** Supporting text: **Review the validation feedback, correct the affected values, and save again.** The form remains editable. Field-level messages appear only where the validator identifies a problem. | The system determines that methodology version, factor value, unit or effective date is missing/invalid. | SD-UC-02 validation alternative; AD-UC-02 d1 [Invalid] → s2 → correction loop. Exact validation rules are not defined in FR-01-02. |
| **4. Change Justification Required — Existing factor/version** | Informational/warning banner: **An existing factor or methodology version was found.** The four configuration fields remain visible. A required **Change justification** text area appears with helper text asking why the existing factor/version is being changed. Primary action: **Save new version**. If the user submits without a reason, show **Change justification is required** and remain in this state. | The submitted parameters are valid and the library reports an existing factor/version; the user has not yet provided a reason. | SD-UC-02 steps 4b–9; AD-UC-02 d2 [Existing] → d3 [No] → s3. With a reason provided, the flow proceeds to update metadata, create a new version and write the audit event. |
| **5. Success — Version saved** | Success banner: **Methodology / emission factor version saved successfully.** Show a summary of Methodology version, Factor value, Unit and Effective date using the saved values. Show **Previous version retained** and **Audit event recorded**. Where the record is new, show creation metadata; where it is an update, show update metadata and the supplied justification. | Parameters are valid; if the factor/version existed, a change justification is provided; the new version is stored and the audit event is written. | SD-UC-02 steps 10–16; AD-UC-02 s4/s5 → s6 → s7 → a2. AD-UC-02 specifies Created By/At or Updated By/At and Change Justification metadata. |

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Methodology version | Enters the methodology version | Retains the submitted version identifier for validation. | Stays on S-F01-03 |
| 2 | Factor value | Enters the factor value | Retains the submitted numeric value for validation. | Stays on S-F01-03 |
| 3 | Unit | Selects or enters the factor unit | Retains the submitted unit for validation. | Stays on S-F01-03 |
| 4 | Effective date | Selects the date | Retains the submitted effective date for validation. | Stays on S-F01-03 |
| 5 | Save configuration | Clicks | Submits the four parameters; EcoTrace validates them and checks whether the factor/version exists. | Stays on S-F01-03 |
| 6 | Validation feedback | Reviews the returned errors, corrects the affected field(s), and submits again | EcoTrace revalidates the corrected parameters. | Stays on S-F01-03 |
| 7 | Change justification | Enters a reason after an existing factor/version is detected | EcoTrace accepts the reason as part of the update request and proceeds with update metadata/versioning. | Stays on S-F01-03 |
| 8 | Save new version | Clicks after providing justification | EcoTrace records update metadata and justification, creates a new version while keeping the previous version, writes the audit entry, and returns saved confirmation. | Stays on S-F01-03 |
| 9 | Global navigation | Selects another permitted feature | Opens the selected EcoTrace destination subject to authorization. | Target screen outside UC-02 |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | The configuration captures methodology version, factor value, unit and effective date. | FR-01-02 |
| SR-002 | The system validates the submitted parameters before checking and storing the factor/version. | SD-UC-02 steps 2–3; AD-UC-02 s1/d1 |
| SR-003 | Invalid parameters return validation errors and route the user back to correct the input. | SD-UC-02 validation alternative; AD-UC-02 d1 [Invalid] |
| SR-004 | The system checks whether the submitted factor/version already exists in the GHG methodology & emission factor library. | SD-UC-02 steps 4b–5b; AD-UC-02 d2 |
| SR-005 | A change justification is required when an existing factor/version is modified. | SD-UC-02 steps 6–9; AD-UC-02 d3 |
| SR-006 | For a new record, the system sets Created By and Created At metadata. | AD-UC-02 s4 |
| SR-007 | For an existing record change, the system records Updated By, Updated At and Change Justification metadata. | SD-UC-02 step 10; AD-UC-02 s5 |
| SR-008 | The system creates a new version and keeps the previous version. | SD-UC-02 step 11; AD-UC-02 s6 and reading note |
| SR-009 | The system writes an audit-log entry containing who, when, old-to-new values and reason. | SD-UC-02 steps 13–14; AD-UC-02 s7 |
| SR-010 | Saved confirmation is shown only after version storage and audit logging are confirmed. | SD-UC-02 steps 12–16; AD-UC-02 success end state |
| SR-011 | The standard operational-user sidebar does not display F-04 administration links. Sign-in is not a sidebar item. | EcoTrace UI Conventions; shared authorization policy FR-04-03 |
| SR-012 | Status and validation outcomes must be communicated with text/icon as well as colour. | UI Conventions §5 |

## 7. Linked requirements

| ID | What this screen does for it |
|---|---|
| FR-01-02 (Must) | Provides the user interface to configure and submit methodology version, factor value, unit and effective date; supports validation, version creation/preservation and storage of the configuration for use in carbon calculations. |

The justification, metadata and audit-log details are modelled in SD-UC-02 and AD-UC-02 as part of the UC-02 workflow and are not assigned separate FR IDs in the current W1 baseline.

## 8. Responsive and accessibility notes

- Follow the shared EcoTrace application shell from `ui-conventions.md`: standard-user sidebar, top bar, breadcrumbs and consistent page-title placement. Do not show F-04 administration links.
- On smaller widths, collapse the sidebar into a drawer and stack the form and versioning guidance panel in one column.
- Use explicit labels above all four parameters and the conditional change-justification field; do not rely on placeholders as the only labels.
- Keep the effective-date field keyboard accessible and readable at narrow widths.
- During validation and saving, provide visible progress feedback and disable duplicate submission.
- Associate validation errors with the affected inputs. Do not mark all fields invalid if the validator has not identified them.
- Make the required nature of change justification explicit when an existing factor/version is detected.
- Success and validation states must be distinguishable by text and icon as well as colour.
- Ensure body text contrast at least 4.5:1 and interactive targets at least 24 × 24 CSS px, preferably 40–44 px for common controls, per UI Conventions §5.

## 9. Open questions

| # | Question | Blocking? | Status / default |
|---|---|---|---|
| 1 | What is the canonical identifier/name of a methodology or emission factor, in addition to methodology version, factor value, unit and effective date? | Yes — for final record lookup and form design | Open. The sequence diagram only explicitly lists the four inputs; no separate name/code field is invented. |
| 2 | What validation ranges, precision rules, allowed units and date/effective-period rules apply? | Yes — for final validation and acceptance criteria | Open. UI shows neutral inputs; it does not invent constraints. |
| 3 | What is the required content/length policy for change justification? | Yes — for final validation of existing-factor changes | Open. A required text area is shown, but no minimum length or accepted reason taxonomy is assumed. |
| 4 | AD-UC-02 states that the previous version is kept active alongside the new version. Is this intended for every factor and methodology type? | No — but confirm before implementation | AD-UC-02 reading note is followed as currently written; previous version is retained. |
| 5 | If the library write or audit-log write fails, what should the user see and does the system roll back the new version? | Yes — before designing a system/network error state | Open. The current sequence/activity diagrams do not define a storage/audit failure branch; no rollback or failure state is invented. |

---

## Completion checklist

- [x] Screen ID, actor, feature, priority and UC-02 match the Screen List / W0-W1 baseline.
- [x] All four FR-01-02 input groups are represented.
- [x] Five states are specified: Default, Loading, Validation Error, Change Justification Required and Success.
- [x] Existing-factor/version branch and mandatory change justification match SD-UC-02 / AD-UC-02.
- [x] New/update metadata, previous-version retention and audit event are reflected.
- [x] Standard operational sidebar excludes F-04.
- [x] Unspecified identifiers, units, validation rules and failure handling are listed as open questions.
- [ ] Figma plugin executed and five frames visually reviewed by the team.
- [ ] Open questions 1–3 confirmed against the methodology/emission-factor data contract.
- [ ] Library-write and audit-write failure handling confirmed before adding a system-error state.
