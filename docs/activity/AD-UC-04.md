# Activity Diagram: AD-UC-04 Calculate shipment-level PCF

| Field | Value |
| --- | --- |
| AD ID | AD-UC-04 |
| Use case | UC-04 Calculate shipment-level PCF |
| Feature | F-01 — Carbon Traceability & Product Carbon Footprint Management |
| Primary actor | Export Sales & Marketing |
| Lanes | Export Sales & Marketing · EcoTrace · Logistics Partner (Hoa Phat Logistics) |
| Inputs (L3–L5) | Context Diagram; Use Case Diagram; SD-UC-04; W0 Table 1 and Table 2; W1 FR-01-03, FR-01-05 and FR-01-07; W4 Prioritization; Architecture; Screen List; Usage Flow; Screen Transition; Screen Spec S-F01-01 |
| Trigger | An Export Sales & Marketing user selects a shipment and requests its PCF. |
| Preconditions | The Export Sales & Marketing user is signed in and authorized to use shipment PCF functions (shared UC-10 sign-in and FR-04-03 authorization gate). Shipment/order data, constituent-batch linkage/intensity and applicable logistics emissions are available for the nominal calculation path. |
| Success end state | The shipment-level PCF has been calculated from constituent-batch carbon intensities and applicable logistics emissions, stored, and shown to the requester. |
| Failure end state | No PCF is calculated; the system reports that required shipment, batch-intensity, or linkage data is missing. |
| FRs covered | FR-01-05 (Must, direct UC-04 calculation); FR-01-03 and FR-01-07 are supporting batch-intensity/linkage inputs. The missing-data branch uses proposed FR-01-09, which is not present in W1 and is not an approved FR. |
| Business rules applied | FR-01-05 defines shipment PCF as an aggregation of constituent-batch carbon intensity and applicable logistics emissions. No separately numbered BR ID is supplied in the current repository. |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph ACT["Export Sales & Marketing"]
    start@{ shape: sm-circ }
    a1("Select shipment for PCF calculation")
    a2("See missing-data notice")
    a3("Review shipment-level PCF")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end
  subgraph SYS["EcoTrace"]
    s1("Retrieve shipment and batch references from SAP (FR-01-05)")
    s2("Retrieve constituent-batch carbon intensities (FR-01-05)")
    d1{"Shipment, batch intensities and linkage all available?"}
    s3("Flag missing shipment or batch data (FR-01-09)")
    s4("Request shipment logistics emissions (FR-01-05)")
    s5("Calculate shipment-level PCF from batch intensity and logistics emissions (FR-01-05)")
    o1["Shipment PCF [Calculated]"]
    s6("Store shipment-level PCF (FR-01-05)")
  end
  subgraph LOG["Logistics Partner (Hoa Phat Logistics)"]
    e1("Provide shipment logistics emissions")
  end
  start --> a1 --> s1 --> s2 --> d1
  d1 -->|"[no]"| s3 --> a2 --> stop2
  d1 -->|"[yes]"| s4 --> e1 --> s5 --> o1 --> s6 --> a3 --> stop1
```

Reading notes:

- The system is one black-box lane: SAP, the carbon-data/traceability service, and the PCF database
  from the sequence diagram are not separate lanes.
- `FR-01-09` does not exist in W1 yet. It is an open item to report to the integrator (see section 6),
  not an invented requirement — do not treat it as confirmed until it is added.
- No fork/join: unlike AD-UC-01, this use case has no two steps that genuinely happen in any order,
  so a fork would add nothing but noise.

## 2. Node trace

| Node | Kind | Text | Traces to |
| --- | --- | --- | --- |
| s1 | Action | Retrieve shipment and batch references from SAP | FR-01-05; SD-UC-04 steps 3-4 |
| s2 | Action | Retrieve constituent-batch carbon intensities | FR-01-05; SD-UC-04 steps 5-6 |
| d1 | Decision | Shipment, batch intensities and linkage all available? | FR-01-09 (new) |
| s3 | Action | Flag missing shipment or batch data | FR-01-09 (new) |
| s4 | Action | Request shipment logistics emissions | FR-01-05; SD-UC-04 step 7 |
| e1 | Action (external) | Provide shipment logistics emissions | SD-UC-04 step 8 |
| s5 | Action | Calculate shipment-level PCF from batch intensity and logistics emissions | FR-01-05; SD-UC-04 step 9 |
| o1 | Object node | Shipment PCF [Calculated] | FR-01-05 (state value) |
| s6 | Action | Store shipment-level PCF | FR-01-05; SD-UC-04 step 10 |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
| --- | --- | --- | --- |
| d1 [yes] | Shipment SH-001 has batches B-01, B-02 with known carbon intensity and a known batch-to-shipment linkage | Export Sales selects SH-001 for PCF calculation | The system calculates, stores, and displays the shipment-level PCF |
| d1 [no] | Shipment SH-002 has a batch with no recorded carbon intensity | Export Sales selects SH-002 for PCF calculation | No PCF is calculated; the user sees a missing-data notice naming the gap |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| Shipment PCF | Calculated | Calculated is the state shown in this activity and screen spec; no further lifecycle-status enumeration is specified in W1. Persistence is described in SD-UC-04 / Screen Spec but remains a requirement-traceability question. |

## 5. Screens touched

The user-facing selection, request, missing-data feedback and calculated-result review occur on S-F01-01. The Logistics Partner's emissions exchange is an external system interaction, not a separate EcoTrace screen.

| Actor action / outcome | Screen ID |
|---|---|
| Select a shipment and request shipment-level PCF calculation | S-F01-01 — Shipment-level PCF Calculation |
| Review missing shipment/batch/linkage data notice on the no-data path | S-F01-01 — Missing-data notice state (the associated FR-01-09 is proposed and not present in W1) |
| Review the calculated shipment-level PCF, batch inputs and applicable logistics emissions | S-F01-01 — Success / calculated state |
| Provide shipment logistics emissions to EcoTrace | N/A — Hoa Phat Logistics external integration; no EcoTrace user screen is identified |
| Sign in before opening shipment PCF (precondition only; not part of UC-04 activity) | S-F00-01 — Sign in (UC-10) |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | What should EcoTrace do if shipment-to-batch linkage, batch intensity, or logistics emissions are missing or invalid? | Yes — for complete failure-path and FR traceability | The current AD shows a missing-data notice and blocks calculation, matching the Screen Spec and proposed FR-01-09. However, FR-01-09 is absent from W1; do not treat this branch as an approved Must requirement until W1 is updated. |
| 2 | Does FR-01-05 formally require permanent storage of the shipment-level PCF? | Yes — for persistence traceability | SD-UC-04 and Screen Spec S-F01-01 say the calculated PCF is stored in PCF Records; the current W1 FR-01-05 SHALL says calculate. Confirm whether W1 should explicitly include persistence or whether it is an approved design detail. |
| 3 | The Context Diagram labels the incoming information as SAP / ERP data from Technology & Digital Transformation, while Architecture identifies SAP S/4HANA as an external system connected through the Integration Adapter. Should the Context Diagram be updated to show the same system boundary and integration? | No — for the activity's screen mapping; yes for cross-document consistency | Keep SAP S/4HANA as a named data source in the system action; do not add an extra AD lane. |

## 7. Verification log

| Check | Result | By | Date |
| --- | --- | --- | --- |
| Renders on mermaid.live | Not yet independently tested | Team reviewer | |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass (d1: [yes]/[no]) | | |
| No action has two incoming flows (merge used instead) | Pass (no shared entry points) | | |
| Every fork has a matching join | N/A (no forks) | | |
| Every system action cites an FR | Core PCF calculation actions trace to FR-01-05 and supporting input FRs; missing-data action s3 is explicitly linked to proposed FR-01-09, not present in W1 (Q1) | Assistant (source cross-check) | 2026-11-19 |
| Every Must FR of UC-04 appears in the diagram | Pass — FR-01-05 is the direct Must requirement; FR-01-03/FR-01-07 are supporting inputs. FR-01-09 remains proposed and unapproved (Q1). | Assistant (source cross-check) | 2026-11-19 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass — Export Sales & Marketing is the initiating actor; Hoa Phat Logistics is the external logistics party in the Context Diagram | Assistant (source cross-check) | 2026-11-19 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass against SD-UC-04 | | |
| AI critic prompt run, findings resolved or logged as open questions | Done; 2 prior open questions resolved, 1 remains informational | | |

---

*Template source: DBIZ2 Lesson 7 (VJCBI College, FTU); notation: `mermaid-activity-patterns.md`. This activity diagram models one use case as a black box. Internal databases, UI, services and APIs from the sequence diagram are intentionally not independent lanes.*
