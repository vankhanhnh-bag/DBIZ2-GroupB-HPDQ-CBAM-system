# Screen Spec: S-F00-01 Sign in

> EcoTrace — HPDQ Carbon Management & Reporting System. Draft for team review.

| Field | Value |
|---|---|
| Screen ID | S-F00-01 |
| Screen name | Sign in |
| Actor | All users |
| Priority | Must |
| Feature | F-00 Shared screens (supports F-04 Platform, Access & Operations) |
| Activity diagrams | AD-UC-10 (actions "Provide credentials or OTP", "Review rejected sign-in") |
| Mockup images | `img/S-F00-01 — Default : Sign In.png` (default), `img/S-F00-01 — Default : Sign In - numbered.png`, `img/S-F00-01 — Invalid Credentials - numbered.png` (section 2) |
| Frame width | 1440px (exported at 1×, 1440 pixels) |
| Figma frame | [01 — Done screens](https://www.figma.com/design/FTJibQtZrHIMHfwsJx7lT9/EcoTrace?node-id=8-73) |
| Status | Draft for team review |

## 1. Purpose

**Shown when:** a user who is not authenticated opens EcoTrace, or requests a protected destination while no valid session exists.

**The user leaves this screen when:** their credentials are accepted and a session is established — they continue to the destination they originally requested, which may belong to another feature and is outside this screen list. If the credentials are rejected, the user stays on this screen and can try again.

## 2. Mockup

![S-F00-01 — Default](img/S-F00-01%20%E2%80%94%20Default%20%3A%20Sign%20In.png)

**Numbered:** each number refers to the matching # in the element inventory in section 3. Both designed UI states are shown directly below. Element 9 (authentication failed notice) appears only in the Invalid Credentials state.

| Default — numbered | Invalid Credentials — numbered |
|---|---|
| ![S-F00-01 — Default numbered](img/S-F00-01%20%E2%80%94%20Default%20%3A%20Sign%20In%20-%20numbered.png) | ![S-F00-01 — Invalid Credentials numbered](img/S-F00-01%20%E2%80%94%20Invalid%20Credentials%20-%20numbered.png) |

All colours, fonts and corners follow the shared design conventions in
[ui-conventions.md](../ui-conventions.md).

## 3. Element inventory

| # | Element | Type | Content / data source | Required | Validation |
|---|---|---|---|---|---|
| 1 | Header band with Hòa Phát logo | Global layout | Approved Hòa Phát logo on the brand-navy band | Yes | Use the approved logo assets. No sidebar or role-specific navigation is shown before authentication. |
| 2 | Brand panel | Static content | "EcoTrace — Carbon Management & Reporting System"; "Secure platform access for authorized HPDQ users." | Yes | Static text only; shows no account or system data. |
| 3 | Heading and instruction | Header | "Sign in to EcoTrace"; "Use your configured account credentials or OTP." | Yes | English, consistent with UI Conventions §1. |
| 4 | User ID | Text input | User identifier typed by the user (FR-04-01) | Yes | Label above the field; placeholder alone is not the label. Format rules for the ID are not defined in W1 (open question 5). |
| 5 | Password or configured OTP | Masked input | Password or one-time password, depending on the method configured for the account (FR-04-01) | Yes | Characters are masked. The value is never shown, echoed in a message or written to a log. OTP format and lifetime are not defined. |
| 6 | Authentication-method note | Helper text | "Your account uses the authentication method configured by EcoTrace." | Yes | Informational only; does not list or imply which methods exist. |
| 7 | Sign in button | Primary button | Submits the User ID and credential for verification (FR-04-01) | Yes | The one primary action. A required-field check before submitting is a proposal, pending open question 2. |
| 8 | Access-control note | Helper text | "Authentication and protected access are controlled by RBAC." (FR-04-03 context) | Yes | Static. Spell out Role-Based Access Control (RBAC) where the screen first introduces it, per UI Conventions §1. |
| 9 | Authentication failed notice | Error banner | Result of a rejected sign-in: "Authentication failed — Check your ID or credentials and try again." (FR-04-01 failure branch, SD-UC-10) | Conditional | Pair the colour with an icon and text. Do not say which of the two values was wrong. Do not show passwords, OTPs or tokens. Retry and lockout rules are not defined (open question 2). |

The small trace line below the card in the mockup (screen ID, UC and FR references) is a design annotation, not a user-facing element.

## 4. States

| State | What the user sees | Trigger |
|---|---|---|
| Default — Sign-in form | Brand panel, User ID and credential fields, method note, Sign in button and access-control note. | An unauthenticated user opens EcoTrace or a protected page. |
| Invalid Credentials — Rejected sign-in | The same form with the Authentication failed notice above the User ID field. The fields stay available so the user can retry. | The authentication check rejects the submitted credentials (FR-04-01 failure branch). |

Loading, system-error and lockout states are not designed yet (open questions 2 and 6).

## 5. Interactions and navigation

| # | Element | User action | System response | Goes to screen |
|---|---|---|---|---|
| 1 | Entry | Opens EcoTrace or requests a protected page while signed out | Detects that there is no valid session and shows the sign-in form. | S-F00-01 |
| 2 | User ID | Types the user ID | Holds the value in the form; nothing is sent yet. | Stays on S-F00-01 |
| 3 | Password or configured OTP | Types the credential | Holds the value masked in the form; nothing is sent yet. | Stays on S-F00-01 |
| 4 | Sign in button | Selects Sign in with valid credentials | Verifies the identity, establishes a secure session (FR-04-01), and reads the user's roles and permissions (FR-04-03). | The destination the user requested (outside this screen list), for example S-F04-01 for a System Admin |
| 5 | Sign in button | Selects Sign in with invalid credentials | Rejects the attempt and creates no session (FR-04-01 failure branch); shows the Authentication failed notice. | Stays on S-F00-01 |
| 6 | Form | Corrects the values and selects Sign in again | Repeats the verification. | S-F00-01 or the requested destination |

## 6. Screen-level rules

| Rule ID | Rule | Source |
|---|---|---|
| SR-001 | Show S-F00-01 whenever an unauthenticated user opens EcoTrace or requests a protected page, and return the user to the destination originally requested after success. | Screen List; Screen Transition §1 |
| SR-002 | Do not create a session unless the credentials are valid. | FR-04-01; AD-UC-10 d1 |
| SR-003 | Read the user's roles and permissions before granting access to a protected resource. | FR-04-03; AD-UC-10 s3, d2 |
| SR-004 | Do not expose passwords, OTPs, access tokens or other authentication secrets in messages, fields after submission, tables or logs. | UI Conventions §4 |
| SR-005 | The failure notice says what happened and what to do next, and never relies on colour alone. | UI Conventions §§4–5 |
| SR-006 | Do not show the authenticated application shell (sidebar, role navigation) before sign-in. | UI Conventions — Visual direction |
| SR-007 | Access-denied outcomes for a destination are shown on that destination screen, not on S-F00-01. | AD-UC-10 §5; Screen List ("not a screen" list) |
| SR-008 | Use English consistently; the button label is an action verb ("Sign in"). | UI Conventions §1 |
| SR-009 | Brute-force protection is a back-end work item of FR-04-01. No lockout message or counter is shown until it is approved. | W1 FR-04-01-BE; AD-UC-10 open question 2 |

## 7. Linked requirements

| FR ID | What this screen does for it |
|---|---|
| FR-04-01 | Collects the credentials, submits them for verification, and shows the rejection result. A successful sign-in grants the secure session. |
| FR-04-03 | After authentication, roles and permissions are read before a protected destination is opened (shared access gate). |

FR-04-04 does not cover sign-in events (it only covers system-altering actions). Whether sign-ins must be audited is an open question (AD-UC-10 open question 1).

## 8. Responsive and accessibility notes

- Smallest supported width: 360px; below that the layout is single-column, with the brand panel above the form.
- Every field has a visible label, an accessible name and a logical tab order.
- The error notice is tied to the form and is announced to assistive technology.
- Body text contrast at least 4.5:1; the error is never shown by colour alone.

## 9. Open questions

| # | Question | Blocking? | Status |
|---|---|---|---|
| 1 | Must successful and failed sign-in attempts create audit events? W1 FR-04-04 only covers system-altering actions. | Yes — audit scope | Open (AD-UC-10 Q1) |
| 2 | What are the exact rejection message, retry limit and lockout rules? W1 FR-04-01 only defines the valid-credentials case. | Yes — failure-path contract | Open (AD-UC-10 Q2) |
| 3 | Is a session issued only if the user has access to at least one resource? | Yes — session policy | Open (AD-UC-10 Q3) |
| 4 | Is OTP verified inside EcoTrace or by an external identity service that the Context Diagram does not show? | No | Open (AD-UC-10 Q4) |
| 5 | What format rules apply to the User ID and the credential? | No | Open |
| 6 | Which states are required beyond Default and Invalid Credentials (loading, service unavailable, locked account)? | No | Open |
| 7 | Do external customers (UC-09) sign in through this same screen? The Screen List says "All users". | No | Open |

---

## Completion checklist

- [x] The mockup is a separate cropped image file, named with the Screen ID, at the frame width.
- [x] The numbered image exists, and its numbers correspond to the # column of the element inventory.
- [x] Every primary visible element in the mockup appears in the element inventory.
- [ ] Every colour in the mockup is confirmed as a design token from ui-conventions.md.
- [ ] Every input element has an approved validation rule or an explicit "none" (field formats are pending).
- [ ] All states/surfaces are filled in (only Default and Invalid Credentials are designed).
- [x] Every navigation target is a Screen ID that exists in the Screen List or the stated workspace.
- [ ] Every data field has been confirmed against the final service data contract.
- [x] Every linked FR exists in the Function List and the relevant AD-UC-10 actions are covered by interaction rows.
