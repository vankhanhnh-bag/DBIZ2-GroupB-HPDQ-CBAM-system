# Project brief: EcoTrace — HPDQ Carbon Management & Reporting System

> DBIZ2 Digital Business Project II — project brief for Lessons 1–2 (kick-off, project planning and client handover). This document summarizes the **group's modeled HPDQ business case** using the current EcoTrace repository. It is not evidence of a signed specification or independently confirmed HPDQ approval.

| Field | Value |
|---|---|
| Product | **EcoTrace** — HPDQ Carbon Management & Reporting System |
| Business client / case | Hòa Phát Dung Quất (HPDQ), as modeled in the coursework |
| Client contact | **To confirm** — no named contact is recorded in the current repository |
| Student team | DBIZ2 **Group B** — individual project and feature owners to confirm |
| Course | DBIZ2 Digital Business Project II; intended handover to DBIZ3 (build phase) |
| Delivery | Analysis and design package in DBIZ2; proposed implementation scope and estimates are documented separately |

## Problem

The [CVCA](../context/cvca.md) identifies opportunities caused by carbon information being spread across supplier documents, production and energy records, GHG methods, and shipment/logistics data. Without a unified and traceable view, it is difficult to prepare consistent batch- and shipment-level product carbon footprints (PCFs), identify Scope 3 supplier data gaps, and support export decisions involving CBAM exposure. Customers and auditors also need reliable sources and supporting evidence for reported carbon information.

These are **business problems and opportunities inferred in the group's CVCA**, not measured operational deficiencies or findings from verified client interviews. External assurance, financing, and partner services mentioned in the CVCA do not automatically become functions that EcoTrace itself implements.

## Goal of the project

Design EcoTrace so authorized users can consolidate carbon inputs, trace a steel product from supplier/raw material through production batches to shipment, calculate and review shipment-level PCF, estimate CBAM exposure using versioned regulatory rules, and compare eligible green-steel options using carbon and cost information. Platform authentication, role-based authorization, and audit logging support controlled use of these functions.

The intended business outcomes are more traceable carbon records, more consistent shipment-based calculations, and better-supported compliance and product decisions. The related stakeholder and system requirements are in the [CVCA](../context/cvca.md) and [architecture](../architecture/architecture.md). The [W6 cash-flow model](../function-list/W6_CashFlow.md) presents **team-assumption-based** investment scenarios; its benefit estimates are not verified client performance targets.

## Project scope

The four feature IDs and associated use cases are established in [W0 Scope](../function-list/W0_Scope.md) and specified in [W1 Function List](../function-list/W1_FunctionList.md).

| Feature | Business capability | Use cases | Main user groups |
|---|---|---|---|
| **F-01** Carbon Traceability & Product Carbon Footprint Management | Supplier/raw-material carbon submissions, GHG methods and emission factors, batch carbon intensity, shipment PCF, Scope 3 assessment, and carbon traceability | UC-01 to UC-06 | Procurement, Production, Sustainability & ESG, Export Sales & Marketing |
| **F-02** CBAM Compliance & Carbon Cost Estimation | Shipment CBAM estimation and review; versioned CBAM regulatory rules, validation, and activation | UC-07, UC-08 | Export Sales & Marketing, Procurement & Raw Materials, Compliance / Product Owner |
| **F-03** Green Steel Comparison & Recommendation | Filter product profiles, score and compare eligible green-steel options, and recommend alternatives | UC-09 | Customers / Procurement |
| **F-04** Platform, Access & Operations | Sign-in, role-based access control and authorization, and system audit records | UC-10 to UC-12 | All authorized users, System Admin, Auditor |

Feature inclusion does **not** imply that every FR is a Must-have. Release priorities are defined individually in [W4 Prioritization](../function-list/W4_Prioritization.md), including Should/Could items such as Scope 3 assessment and some CBAM report/sharing capabilities.

### Boundaries and dependencies

- **External inputs:** SAP S/4HANA order/shipment information, production and energy-related records, Hoa Phat Logistics shipment/batch links, and an external EU ETS/carbon-price source. The proposed [architecture](../architecture/architecture.md) uses an integration adapter and an EcoTrace database; actual integration access is not yet demonstrated by these documents.
- **Explicitly out of Release 1:** live IoT streaming of tier-2/tier-3 supplier emissions; direct online settlement or purchase of CBAM certificates; and automated issuance of independent BSI certification or EPDs. These exclusions are recorded in W0 Table 3.
- **Scope/acceptance decisions still requiring confirmation:** exact client contact and approvals; access to external data; finalized calculation/rule parameters; and outstanding cross-document traceability or priority mismatches. Do not treat draft screens, sample figures, or projected financial outcomes as production behavior.

## Team and roles

The repository establishes feature and deliverable responsibilities but **does not record a confirmed member-to-role assignment** in the W0 feature-owner column. The table below identifies responsibilities to allocate without inventing named owners.

| Role | Responsibility | Assigned member |
|---|---|---|
| Project integrator / coordinator | Maintain identifiers and document consistency, coordinate reviews and final package | To confirm |
| F-01 feature owner | Carbon traceability and PCF requirements, diagrams, and screens | To confirm |
| F-02 feature owner | CBAM cost and regulation requirements, diagrams, and screens | To confirm |
| F-03 feature owner | Green-steel comparison requirements, diagrams, and screens | To confirm |
| F-04 / platform owner | Authentication, permissions, audit requirements, and shared access | To confirm |
| Client liaison / QA and documentation | Track open questions, acceptance criteria, review and handover completeness | To confirm |

## Milestones in DBIZ2

| When | Milestone | Review package |
|---|---|---|
| After Lesson 4 | Senior Meeting — scope and system-design confirmation | [CVCA](../context/cvca.md), [Context Diagram](../context/context_diagram.md), [Use Case Diagram](../use-case/use-case-diagram.md), sequence diagrams, [architecture](../architecture/architecture.md) |
| After Lesson 6 | Midterm — functional scope and investment analysis | W0–W5 [function list and prioritization](../function-list/W0_Scope.md), plus [W6 cash-flow scenarios](../function-list/W6_CashFlow.md) |
| After Lesson 8 | Senior Meeting — final package review | [Activity diagrams](../activity/activity-index.md), [Usage Flow](../external-design/usage-flow.md), [Screen List](../external-design/screen-list.md), [Screen Transition](../external-design/screen-transition.md), and [Screen Specs](../external-design/screens/screen-spec-S-F02-01.md) |

The assembled DBIZ2 package is intended as the baseline for the later DBIZ3 implementation phase. Final acceptance, confirmed stakeholders, and implementation readiness remain subject to group and instructor review.
