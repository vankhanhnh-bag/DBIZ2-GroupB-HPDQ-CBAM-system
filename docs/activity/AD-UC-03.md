# Activity Diagram: AD-UC-03 Access batch carbon intensity

| Field | Value |
|---|---|
| AD ID | AD-UC-03 |
| Use case | UC-03 Access batch carbon intensity |
| Feature | F-01 Carbon Traceability & Product Carbon Footprint Management |
| Primary actor | Production Department (data provider for the Must calculation path) |
| Lanes | EcoTrace system (single black-box lane) |
| Inputs (L3–L5) | Context Diagram; Use Case Diagram; W0 Table 1 and Table 4; W1 FR-01-03 and FR-01-04; W4 Prioritization; Architecture v1.1; SD-UC-03 |
| Trigger | Production, energy and applicable emission-factor data become available for the same batch. |
| Preconditions | The batch is identifiable and the required production, energy and applicable emission-factor inputs are available. |
| Success end state | EcoTrace calculates the carbon intensity for the production batch. |
| Failure end state | Not specified in FR-01-03. Missing-input and calculation-failure behavior is left open rather than invented. |
| FRs covered | FR-01-03 (Must only) |
| Excluded related FR | FR-01-04 (Should) — user selection and display of batch carbon intensity and contributing sources. |
| Business rules applied | The calculation uses production data, energy data and applicable emission-factor data. The exact formula, unit and factor-selection rule are not specified in W1. |
| Version / status | v1.0 · Draft for review |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph SYS["EcoTrace system"]
    start@{ shape: sm-circ }
    s1("Identify the batch for which production, energy and applicable emission-factor data are available (FR-01-03)")
    s2("Obtain production and energy inputs for the identified batch (FR-01-03)")
    s3("Obtain the applicable GHG methodology and emission factors (FR-01-03; library maintained under FR-01-02)")
    s4("Calculate batch carbon intensity from production, energy and emission factors (FR-01-03)")
    o1["Batch carbon intensity [Calculated]"]
    stop@{ shape: fr-circ }
    start --> s1 --> s2 --> s3 --> s4 --> o1 --> stop
  end
```

Reading notes:

- This diagram models the **Must-only calculation path**. FR-01-03 is triggered when its required inputs are available; no user selection or display step is added.
- The Production Department is the primary actor of UC-03 in W0 and the Use Case Diagram, but the Must calculation is data-triggered. The actor is therefore recorded as the input provider rather than shown as a separate user-action lane.
- FR-01-04 (Should) is intentionally excluded. Its selection/view interaction belongs to a later Should-scope diagram or an explicitly expanded UC-03 flow.
- GHG methodology and emission factors are maintained through UC-02 / FR-01-02; UC-03 only consumes the applicable data.
- The object node represents the calculated output, not proof of persistent storage. No save/persist action is shown because FR-01-03 does not explicitly require it.
- No decision or failure branch is modeled. The FR's WHEN clause defines the input-availability trigger, but W1 does not specify what to do when required data are missing or inconsistent.
- The single EcoTrace lane follows the black-box activity-diagram convention: internal services, library and integration adapter are not separate activity lanes.

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| s1 | Action / trigger qualification | Identify the batch with required inputs available | FR-01-03 trigger condition |
| s2 | Action | Obtain production and energy inputs for the batch | FR-01-03; Context Diagram production/energy data flow |
| s3 | Action | Obtain applicable methodology and emission factors | FR-01-03 input condition; supporting library maintained under FR-01-02 / UC-02 |
| s4 | Action | Calculate batch carbon intensity | FR-01-03 (Must) |
| o1 | Object node | Batch carbon intensity [Calculated] | Output of FR-01-03; not a persistence claim |

## 3. Branches and acceptance tests

No decision nodes are included. FR-01-03 defines a data-availability trigger rather than an explicit yes/no validation rule.

| Scenario | Given | When | Then |
|---|---|---|---|
| Required-input trigger | Production data, energy data and applicable emission-factor data are available for batch B-001 | EcoTrace processes the available inputs for B-001 | EcoTrace calculates batch carbon intensity from the three required input categories (FR-01-03). |

## 4. Object states used

| Object | State shown | Notes |
|---|---|---|
| Batch carbon intensity result | Calculated | Logical output state only. Whether the value is persisted for later retrieval is an open question; FR-01-04 (Should) expects retrieval. |

## 5. Screens touched

FR-01-03 (Must) is a background calculation triggered when production, energy and applicable emission-factor data are available. It does not require a user-facing screen in this activity.

| Activity / actor action | Screen ID |
|---|---|
| Calculate batch carbon intensity from production, energy and applicable emission-factor data | N/A — background processing under FR-01-03; no screen action is defined |
| Select a production batch and view its carbon intensity and contributing data sources | S-F01-04 — Batch Carbon Intensity; related user-facing screen for FR-01-04 (Should), excluded from this Must-only activity |

The Screen List currently labels S-F01-04 as Must while W1/W4 classify the linked viewing function FR-01-04 as Should. The ID is recorded for traceability, but the screen is not added to the FR-01-03 Must activity.

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | Which production and energy source systems are authoritative, and how do they connect to the Integration Adapter? The Context Diagram, W0 Table 4 and Architecture do not fully agree on the energy-source mapping. | Yes — for final integration detail | Use generic production and energy inputs; do not assume an unconfirmed source endpoint. |
| 2 | Must the batch carbon intensity result be stored, and which data entity/store supports FR-01-04 retrieval? | Yes — before claiming persistence or completing the Should view flow | Show only the calculated output object; no persistence action is assumed. |
| 3 | What calculation formula, unit, process boundary and emission-factor effective-date rule apply? | Yes — for detailed calculation acceptance criteria | Use only the input categories stated in FR-01-03; do not invent a formula. |
| 4 | What happens when production, energy or emission-factor inputs are missing or inconsistent? | No — for the stated trigger path; yes for failure-path completeness | No failure branch is modeled until a rule or FR is confirmed. |
| 5 | The Screen List labels S-F01-04 (Batch Carbon Intensity) as Must, but W1/W4 classify FR-01-04 (View batch carbon intensity) as Should and FR-01-03 calculation is a background function. Should the screen priority/mapping be corrected, or is a separate Must-level display requirement missing? | Yes — for Screen List / W1 traceability sign-off | Keep the current diagram limited to FR-01-03 Must; record S-F01-04 as the related FR-01-04 viewing screen, not an action in this activity. |
| 6 | The Screen List references `screen-spec-S-F01-04.md`, but that detailed file is not present in the current `docs/external-design/screens/` directory. Should the detailed screen spec be added? | No — for this background Must activity; yes for full UI handover | Use only the S-F01-04 name, actor, purpose and mapping from the Screen List. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Mermaid syntax / render | Static syntax reviewed; not independently rendered on mermaid.live | Draft | 2026-11-16 |
| Every decision has guarded outgoing flows | N/A — no decision nodes | Draft | 2026-11-16 |
| No action has two incoming flows | Pass (linear flow) | Draft | 2026-11-16 |
| Every fork has a matching join | N/A — no fork/join | Draft | 2026-11-16 |
| Every system action cites an FR | Pass — s1 to s4 cite FR-01-03; s3 also notes FR-01-02 as the supporting library-maintenance requirement | Draft | 2026-11-16 |
| Every Must FR of UC-03 appears in the diagram | Pass — FR-01-03 included; FR-01-04 is Should and deliberately excluded | Draft | 2026-11-16 |
| Lane names follow the black-box activity convention | Pass — one EcoTrace system lane; no UI/API/database lanes | Draft | 2026-11-16 |
| Related sequence diagram uses the same trigger, inputs, output and FR ID | Pass against SD-UC-03 | Draft | 2026-11-16 |
| Open data-source and persistence assumptions are recorded | Pass — Sections 6 and SD-UC-03 open questions | Draft | 2026-11-16 |
| Screen mapping cross-check | Pass — S-F01-04 is identified as the related FR-01-04 Should viewer; not part of the FR-01-03 Must calculation | Assistant (source cross-check) | 2026-11-19 |
| Peer review / AI critic | Not yet performed | — | — |

*Template and notation follow the team's existing AD-UC-02 / AD-UC-04 structure and Mermaid `swimlane-beta` convention.*