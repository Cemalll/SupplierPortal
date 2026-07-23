# Architecture Decisions — User, Role, Security, and Audit

> **Module:** 18 — User, Role, Security, and Audit
> **Phase:** Phase 1 — Decision Discovery
> **File:** `decisions.md`
> **Status:** In progress — 42 of 44 active decisions accepted, 2 open

This file records the business and architectural decisions for the User, Role, Security, and Audit module of the Supplier Portal.

Record only decisions that are expensive to change, affect multiple modules, or are likely to be discussed again later. Technical implementation details such as library names, exact endpoint paths, table names, and middleware file names belong to later design phases.

> **Conventions:** Status values, priority values, and the decision entry template used in this file are defined once, centrally, in [`architecture/adr-conventions.md`](../architecture/adr-conventions.md). Do not redefine them here or in any other module's decision log — update the shared file instead.

> **Merge note:** 4 decisions (M18-05, M18-22, M18-34, and previously M18-25) asked a question that duplicated or was fully subsumed by another decision. Three remain merged as traceable stubs. M18-25 was **un-merged** after review — field-level visibility is a distinct authorization dimension from action-level permission and needed its own decision (see M18-23 / M18-25 below).

---

## Canonical Terminology

Use these terms consistently in code, schema, and future decisions — do not invent synonyms.

- **Principal:** Any actor that can act in the system — human or non-human.
- **User Account:** A human principal's global portal identity (one per person, independent of any organization).
- **Integration Identity:** A non-human principal used by one external system or integration.
- **Organization Membership:** The relationship between a User Account and an Organization — carries its own lifecycle, independent of the account's lifecycle.
- **Role:** A named bundle of human-user authorization grants, assigned through an Organization Membership.
- **Role Assignment:** The assignment of a Role to a specific Organization Membership.
- **Permission:** An action allowed on a resource type, such as view, create, edit, delete, approve, or export.
- **Scope:** The subset of records to which a permission applies, such as own records, assigned records, or a whole organization.
- **Role Permission Grant:** A Permission + Scope pair contained by a Role.
- **Integration Grant:** A Permission + Scope pair assigned directly to an Integration Identity; it is not a human-facing Role.
- **Tenant:** Not used in this system — see M18-02 (single-buyer portal, suppliers are Organizations).

---

## AI Consumption Rules

If this file is handed to an AI (or a human) to implement code, the following rules are normative:

1. Only decisions with `Status: ACCEPTED` are normative and implementable.
2. Do not implement or guess the resolution of a decision with `Status: OPEN`.
3. `MERGED` and `SPLIT` entries are non-normative references — follow the canonical decision or decisions they point to.
4. `POST_MVP` and `OUT_OF_SCOPE` capabilities must not be built as part of the current vertical slice.
5. Do not introduce database tables, API behavior, or permission checks that contradict an accepted decision.
6. If two accepted decisions appear inconsistent, stop the affected implementation and report the decision IDs — do not silently pick one.
7. The absence of a decision does not grant permission to invent business behavior. Ask instead of assuming.
8. Do not treat Principal, Role, User Account, Organization Membership, Role Permission Grant, and Integration Grant as the same concept — see Canonical Terminology above.
9. Implement only the decision IDs explicitly listed in the current vertical-slice specification (see M18-45).

---

## Decision Index

| ID | Decision | Priority | Status |
|---|---|---:|---|
| M18-01A | Principal Types | P0 | ACCEPTED |
| M18-01B | Initial Role Catalogue | P0 | ACCEPTED |
| M18-02 | Tenant Definition | P0 | ACCEPTED |
| M18-03 | Multi-Organization Membership | P0 | ACCEPTED |
| M18-04 | Legal Entity and Branch Model | P0 | ACCEPTED |
| M18-05 | Internal-User Data Scope | P0 | MERGED → M18-24 |
| M18-06 | User and Organization Source of Truth | P0 | ACCEPTED |
| M18-07 | Account Creation Methods | P0 | ACCEPTED |
| M18-08 | Supplier-Side User Administration | P0 | ACCEPTED |
| M18-09 | Organization Membership Approval | P0 | ACCEPTED |
| M18-10 | Account, Verification, Security & Membership States | P0 | ACCEPTED |
| M18-11 | User Offboarding | P0 | ACCEPTED |
| M18-12 | Email Changes and Duplicate Accounts | P1 | ACCEPTED |
| M18-13 | Shared Accounts | P0 | ACCEPTED |
| M18-14 | Authentication Methods | P0 | ACCEPTED |
| M18-15 | Multi-Factor Authentication | P1 | ACCEPTED |
| M18-16 | Password Reset and Account Recovery | P1 | ACCEPTED |
| M18-17 | Session Policy | P1 | ACCEPTED |
| M18-18 | Failed Sign-Ins and Account Lockout | P1 | ACCEPTED |
| M18-19 | User Impersonation | P2 | ACCEPTED |
| M18-20 | Authorization Model | P0 | ACCEPTED |
| M18-21 | Fixed and Custom Roles | P0 | ACCEPTED |
| M18-22 | Initial Role Catalogue (duplicate) | P0 | MERGED → M18-01B |
| M18-23 | Permission Granularity (action-level) | P0 | ACCEPTED |
| M18-24 | Authorization Scope Model | P0 | ACCEPTED |
| M18-25 | Sensitive-Field Visibility | P0 | ACCEPTED |
| M18-26 | State-Dependent Permissions | P0 | ACCEPTED |
| M18-27 | Role Assignment Authority | P0 | ACCEPTED |
| M18-28 | Multiple-Role Evaluation & Conflict Resolution | P0 | ACCEPTED |
| M18-29 | Segregation of Duties | P1 | ACCEPTED |
| M18-30 | Temporary Delegation | P2 | ACCEPTED |
| M18-31 | Super Admin and Emergency Access | P1 | ACCEPTED |
| M18-32 | Audited Event Categories | P0 | ACCEPTED |
| M18-33 | Audit Event Contents | P0 | ACCEPTED |
| M18-34 | Old and New Value Storage (duplicate) | P0 | MERGED → M18-33 |
| M18-35 | Audit Immutability | P0 | ACCEPTED |
| M18-36 | Audit Retention | P1 | **OPEN** |
| M18-37 | Audit Visibility and Export | P0 | ACCEPTED |
| M18-38 | Security Alerts | P1 | ACCEPTED |
| M18-39 | Audit, History, Log, and Trace Separation | P0 | ACCEPTED |
| M18-40 | ERP Data Ownership & Sync Direction | P0 | ACCEPTED |
| M18-41 | Integration Identities (split) | P0 | SPLIT → M18-41A / M18-41B |
| M18-41A | Integration Identity Model | P0 | ACCEPTED |
| M18-41B | Integration Authentication Mechanism | P0 | **OPEN** |
| M18-42 | Production Administrative Access | P1 | ACCEPTED |
| M18-43 | Compromised-Account Response | P1 | ACCEPTED |
| M18-44 | Module 18 MVP Scope | P0 | ACCEPTED |
| M18-45 | First Vertical Slice | P1 | ACCEPTED |

---

## M18-01A — Principal Types

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** All modules

### Decision

The system has exactly two principal types:

1. **Human User** — a person with a User Account.
2. **Integration Identity** — a non-human principal used by one external system or integration.

Human Users receive human-facing Roles only through Organization Memberships and Role Assignments.

Integration Identities do not hold Organization Memberships and do not receive human-facing Roles such as Buyer, Auditor, or Supplier Admin. They receive Integration Grants containing only the permissions and scopes required for that integration — see M18-41A.

### Rationale

The original list mixed a principal type (Integration Identity) with human roles (Buyer, Auditor, etc.). Separating principal types from authorization assignments preserves multi-role membership, keeps service-account security rules isolated, and prevents an external system from being modeled as a human user.

### Consequences

- **Positive:** Human and non-human actors have explicit, independent authorization paths.
- **Positive:** CaniasERP can later authenticate through OAuth, an API credential, or a Canias-specific mechanism without being given a human Role.
- **Negative:** The authorization model contains two grant paths: Role Assignments for Human Users and Integration Grants for Integration Identities.

### MVP Classification
MVP foundation. Integration Identity use is later phase.


---

## M18-01B — Initial Role Catalogue

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** All modules

### Decision

The system will support the following roles (all assignable only to Human Users via an Organization Membership):

1. Portal Admin
2. Auditor
3. Buyer
4. Approver
5. Finance User
6. Quality User
7. Supplier Admin
8. Supplier User

*(M18-22 asked the same question and is merged here.)*

### MVP Classification
MVP

---

## M18-02 — Tenant Definition

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** Module 00, Module 19, all business modules

### Decision

Single-buyer portal; suppliers are Organizations, not tenants.

### Rationale

Derived from M18-04 (legal entities are separate organizations, branches are child units) — this structure implies one buyer company operating the portal.

### Open Issues
- This assumes a single buyer company. If a multi-buyer SaaS model is ever planned, reopen this decision.

### MVP Classification
MVP

---

## M18-03 — Multi-Organization Membership

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** User Management, Supplier Profile

### Decision

One User Account may hold multiple Organization Memberships. Each membership has its own lifecycle — see M18-10 and M18-11.

### MVP Classification
MVP

---

## M18-04 — Legal Entity and Branch Model

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** Supplier Registration, Orders, Contracts, Invoices

### Decision

Legal entities are separate Organizations; branches are child units of a legal entity's Organization.

### MVP Classification
MVP

---

## M18-05 — Internal-User Data Scope

- **Status:** MERGED → M18-24
- **Priority:** P0

### Note
Uses the same scope catalog as M18-24 (Authorization Scope Model). Folded in there rather than decided separately.

---

## M18-06 — User and Organization Source of Truth

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** Module 19, User Management, Supplier Profile

### Decision

Portal is the source of truth for the following data. The standalone MVP performs no ERP synchronization — see M18-40.

| Data | Source of Truth |
|---|---|
| Portal user profile | Portal |
| Organization membership | Portal |
| Portal role assignments | Portal |
| Password credentials | Portal (MVP) |
| Authentication identity | Portal (MVP), external IdP may be added later (M18-14) |
| Supplier master data | Decided per relevant business module |
| ERP mapping identifiers | Future integration layer |
| ERP employee termination status | ERP in the future integration; not consumed by the standalone MVP |

### Rationale

The earlier statement "Portal owns all" was too broad. This ownership matrix distinguishes portal-owned identity/access data from future ERP-owned or module-owned data. Owning user and organization data does not force portal-only authentication forever.

### MVP Classification
MVP


---

## M18-07 — Account Creation Methods

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** Supplier Invitation, Supplier Registration

### Decision

Invitation-based creation only for MVP. Open self-registration is supported by the underlying model but its rollout is deferred — see M18-44.

### MVP Classification
MVP (invitation-based only). Open self-registration: Later phase.

---

## M18-08 — Supplier-Side User Administration

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** User Management, Supplier Profile

### Decision

A Supplier Admin can, within their own organization:

1. Invite new Supplier Users.
2. Assign supplier-side roles.
3. Deactivate users by ending the relevant Organization Membership, not the global User Account — see M18-11.
4. Request the appointment of another Supplier Admin.

Supplier User invitations do not require Buyer approval once the organization already has an approved Supplier Admin. A new Supplier Admin appointment does require Buyer approval under M18-09.

### MVP Classification
MVP


---

## M18-09 — Organization Membership Approval

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** Supplier Invitation, Supplier Registration

### Decision

- The first Supplier Admin of a supplier organization is invited by a Buyer or Portal Admin and requires Buyer-side approval before the membership becomes `ACTIVE`.
- Once the organization has an approved Supplier Admin, that Supplier Admin may create and manage Supplier User memberships without further Buyer approval.
- Appointing an additional or replacement Supplier Admin requires Buyer approval.
- Approval applies to the Organization Membership and proposed Supplier Admin authority; it does not replace email verification.

A user may authenticate only after the User Account is email-verified. Authentication alone does not grant access to organization data: an `ACTIVE` Organization Membership is also required under M18-10.

### Rationale

This rule limits Buyer approval to the two points that create or expand supplier-side administrative authority: the first Supplier Admin and later Supplier Admin appointments. Routine Supplier User onboarding remains under the approved Supplier Admin's control.

### MVP Classification
MVP


---

## M18-10 — Account, Verification, Security & Membership States

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** Authentication, User Management

### Decision

State is modeled as **four independent dimensions**, not one combined status field. A user can, for example, be `account_status = ACTIVE`, `verification_status = VERIFIED`, `security_status = LOCKED`, and have one `ACTIVE` membership and another `ENDED` membership at the same time.

```
account_status (per User Account, global):
- ACTIVE
- SUSPENDED
- DISABLED

verification_status (per User Account):
- PENDING
- VERIFIED

security_status (per User Account):
- NORMAL
- LOCKED
- COMPROMISED

membership_status (per Organization Membership):
- INVITED
- PENDING_APPROVAL
- ACTIVE
- SUSPENDED
- ENDED
```

### Effective Access Invariants

A User Account may authenticate only when all of the following are true:

- `account_status = ACTIVE`
- `verification_status = VERIFIED`
- `security_status = NORMAL`

A Human User may act within an Organization only when all of the following are true:

- The User Account is eligible to authenticate.
- The relevant Organization Membership is `ACTIVE`.
- The request is evaluated under that exact Membership context.
- At least one valid Role Permission Grant authorizes the requested action and record scope.

A verified user with no `ACTIVE` membership may access only a restricted account/membership-status experience and may not access organization business data.

### Rationale

The earlier single linear status list mixed onboarding, account lifecycle, security state, and organization membership state. Splitting these dimensions removes the conflict with multi-organization membership and gives authorization a precise access predicate.

### Consequences

- **Positive:** Multi-organization offboarding and security lockout behave independently.
- **Positive:** AI and implementation code receive an explicit authentication and organization-access rule.
- **Negative:** More state combinations must be tested than with a single status field.

### MVP Classification
MVP


---

## M18-11 — User Offboarding

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** Tasks, Approvals, Audit

### Decision

When a user leaves an organization, **only that Organization Membership** is set to `ENDED`. The global User Account stays `ACTIVE` if the user has any other active membership. The global User Account is only set to `SUSPENDED` or `DISABLED` by a security decision (M18-43), a user's own request, or a Portal Admin decision — never automatically as a side effect of losing one membership.

### Rationale

Directly derived from M18-03 (multi-org membership) — the review correctly caught that the earlier "deactivate the account" wording would have wrongly cut off a user's access to *other* organizations they still belong to.

### Open Issues
- Because of M18-40 (ERP does not write to the portal automatically), an employee who leaves in the ERP does not auto-end their portal membership — a manual or semi-automated trigger is still needed here.

### MVP Classification
MVP

---

## M18-12 — Email Changes and Duplicate Accounts

- **Status:** ACCEPTED
- **Priority:** P1
- **Related modules:** Authentication, User Management

### Decision

Email is a mutable login identifier. Every User Account has a separate, immutable internal user ID that email changes never affect.

### MVP Classification
MVP

---

## M18-13 — Shared Accounts

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** Authentication, Audit

### Decision

Shared human accounts are fully prohibited. (Integration Identity is not a human account and is governed separately — see M18-01A, M18-41A, M18-41B.)

### MVP Classification
MVP

---

## M18-14 — Authentication Methods

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** Authentication, Module 19

### Decision

Portal credentials for all users in MVP; SSO may be added later for internal users. Note: this is an authentication-mechanism choice, not a data-ownership one — see M18-06, which clarifies that portal ownership of user/org data does not by itself require portal-only authentication.

### MVP Classification
MVP

---

## M18-15 — Multi-Factor Authentication
**Decision:** Deferred beyond MVP.


---

## M18-16 — Password Reset and Account Recovery
**Decision:** Email reset link.


---

## M18-17 — Session Policy
**Decision:** MVP does not enforce a single active session — multiple concurrent sessions are allowed, with a standard inactivity/absolute timeout (values set in technical design). Single-active-session enforcement is deferred — see M18-44 (Later phase).


---

## M18-18 — Failed Sign-Ins and Account Lockout
**Decision:** Account-based throttling/lockout.


---

## M18-19 — User Impersonation
**Decision:** Read-only impersonation only — support staff can view as a user but cannot perform actions. **Delivery:** Later phase — not built as part of MVP (see M18-44).

---

## M18-20 — Authorization Model

- **Status:** ACCEPTED
- **Priority:** P0

### Decision
RBAC plus data scope (role determines *what* actions are possible; scope determines *which records* they apply to — see M18-24).

### MVP Classification
MVP

---

## M18-21 — Fixed and Custom Roles
**Decision:** Fixed roles in MVP; custom roles may be added later.


---

## M18-22 — Initial Role Catalogue
**Status:** MERGED → M18-01B.

---

## M18-23 — Permission Granularity (action-level)

- **Status:** ACCEPTED
- **Priority:** P0

### Decision
Permissions are defined as explicit actions on a resource type. The MVP baseline includes `view`, `create`, `edit`, and `delete`; domain-specific actions such as `approve`, `reject`, `export`, `invite`, or `deactivate` are modeled as separate permissions when required.

A broad permission such as `manage_all` must not be introduced as a shortcut unless accepted by a later decision.

### Rationale
Field-level visibility (which specific fields a permitted user can see) is a **separate authorization dimension** from action-level permission — a Finance User and a Supplier User can both have `view` on an invoice while seeing different fields. This is why M18-25 was un-merged into its own decision rather than folded in here (see below).

### MVP Classification
MVP

---

## M18-24 — Authorization Scope Model

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** All modules

### Decision

Authorization is a composition of **Permission** and **Scope**.

A Human User receives authorization through this path:

```
Organization Membership
→ Role Assignment
→ Role
→ Role Permission Grant (Permission + Scope)
```

The scope catalog is:

```
GLOBAL
BUYER_ORGANIZATION
LEGAL_ENTITY
BUSINESS_UNIT
SUPPLIER_ORGANIZATION
ASSIGNED_RECORDS
OWN_RECORDS
```

`READ_ONLY` is not a Scope. Read-only behavior is expressed by granting only read-oriented Permissions such as `view` and `export`.

The following is the normative MVP baseline for data scope. Exact resource actions within each Role are defined during technical design, but technical design must not change these data-scope boundaries without updating this decision:

| Role | MVP Data Scope | Permission Character |
|---|---|---|
| Portal Admin | `GLOBAL` | Only explicitly granted administration actions |
| Auditor | `GLOBAL` | Read/export only |
| Buyer | `ASSIGNED_RECORDS` | Buyer actions on assigned records |
| Approver | `ASSIGNED_RECORDS` | View/approve on assigned records |
| Finance User | `LEGAL_ENTITY` | Finance actions within assigned legal entity context |
| Quality User | `LEGAL_ENTITY` | Quality actions within assigned legal entity context |
| Supplier Admin | `SUPPLIER_ORGANIZATION` | Supplier administration within own organization |
| Supplier User | `SUPPLIER_ORGANIZATION` | Limited supplier actions within own organization |

A Role Assignment may narrow the applicable organization or legal-entity context but may not broaden a Role Permission Grant beyond its defined Scope.

`GLOBAL` is a data scope, not an authorization bypass. Portal Admin still needs an explicit Permission for every action and does not receive break-glass-only capabilities.

### Rationale

The earlier "assigned records for all internal users" rule did not work for Auditor, Portal Admin, Finance User, or Quality User. Separating Permission from Scope also prevents read-only behavior from being encoded as a misleading scope value.

### MVP Classification
MVP


---

## M18-25 — Sensitive-Field Visibility

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** Supplier Profile, Finance, Risk, Sourcing

### Decision

No general-purpose field-level permission engine in MVP. However, fields explicitly marked sensitive are redacted in API responses based on role and organization context. The sensitive-field catalog (e.g. bank details, tax ID, discount rates) is defined during the technical design of each relevant module.

### Rationale

Un-merged from M18-23 after review — action-level permission (`view`) and field-level visibility are different authorization dimensions and conflating them risks exposing sensitive fields to anyone with `view` access.

### MVP Classification
MVP (lean version — full field-level engine is post-MVP if ever needed)

---

## M18-26 — State-Dependent Permissions
**Decision:** Yes — authorization takes record state into account (e.g. an approved order can no longer be edited).


---

## M18-27 — Role Assignment Authority
**Decision:** Portal Admin assigns internal roles; Supplier Admin assigns their own organization's supplier-side roles (subject to M18-09's approval rule for Supplier Admin appointments).

---

## M18-28 — Multiple-Role Evaluation & Conflict Resolution

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** Authorization

### Decision

Roles are evaluated only within the user's active Organization Membership context. Permissions from a membership in one organization never apply while operating in another organization.

Within one active Membership:

- All explicit Role Permission Grants from the assigned Roles are unioned.
- There is no explicit deny in MVP.
- Each grant keeps its own Permission and Scope.
- The system may combine explicitly granted access, but it must not invent a broader Scope or treat one Role's Scope as applying to another Role's Permission.

Example: if one Role explicitly grants `view invoice` within `LEGAL_ENTITY` and another explicitly grants `approve invoice` within `ASSIGNED_RECORDS`, the system applies those two grants separately. It must not create `approve invoice` within `LEGAL_ENTITY` unless that grant is explicitly defined.

### Rationale

This rule supports multiple Roles without silent privilege escalation and makes the Permission + Scope pairing mechanically enforceable.

### MVP Classification
MVP


---

## M18-29 — Segregation of Duties
**Decision:** For orders, supplier registration, and critical changes: the person who created/opened the request cannot approve it.


---

## M18-30 — Temporary Delegation
**Decision:** Not supported.

---

## M18-31 — Super Admin and Emergency Access

- **Status:** ACCEPTED
- **Priority:** P1
- **Related modules:** Security, Audit, Operations

### Decision

There is **no permanent, application-level Super Admin role**. Any need for broad emergency access is handled exclusively through the break-glass model defined in M18-42 — not through a standing role.

### Rationale

The review pointed out that declaring "no Super Admin" here while separately allowing "limited support access with approval" in M18-42 creates a risk: if the support-access grant isn't tightly bounded, it becomes a de facto Super Admin in practice. M18-42 now defines the concrete constraints that keep this true.

### MVP Classification
MVP

---

## M18-32 — Audited Event Categories
**Decision:** The business audit infrastructure records the following event categories:

- `IDENTITY_AND_ACCESS` — invitations, account activation, membership and role changes, session revocation
- `SECURITY` — failed sign-ins, lockout, compromise response
- `BUSINESS_DATA_CHANGE` — creation and modification of business records
- `APPROVAL` — approval, rejection, and approval-related state changes
- `ADMINISTRATION` — privileged configuration and administrative actions

Technical error logs and distributed traces are not business audit events — see M18-39.

---

## M18-33 — Audit Event Contents

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** All modules

### Decision

For a changed record, the MVP audit event stores:

- actor
- action
- target resource type and identifier
- organization context
- timestamp
- result
- event category
- `changed_fields`
- `new_values`

Previous or old field values are **not** stored in the MVP audit trail.

For create operations, the created values may be stored as `new_values`. For update operations, only the fields changed by the operation and their resulting values are stored. For delete operations, the event records the deletion action, actor, target identifier, organization context, timestamp, and result; a full pre-deletion snapshot is not stored. *(M18-34 is merged here.)*

Sensitive business values may be redacted or hashed where appropriate. Passwords, password hashes, reset tokens, session tokens, API keys, client secrets, private keys, and other authentication secrets must never be stored in audit values or metadata.

### Rationale

The balanced standalone MVP audit trail is intended to answer: **who performed an action, when it happened, which resource was affected, and what the resulting values were**.

Storing previous values would duplicate more business and personal data and increase the audit payload. Full historical reconstruction is not an MVP requirement.

### Consequences

- The audit trail cannot directly show the value that existed immediately before a change.
- The audit trail must not be treated as a complete record-version history.
- A business module that requires full historical reconstruction must introduce a separate business-history or versioning decision.
- Implementations must not introduce an `old_values` field unless this decision is reopened.

### MVP Classification
MVP

---

## M18-34 — Old and New Value Storage

- **Status:** MERGED → M18-33

### Note

M18-33 decides that the MVP audit trail stores changed fields and resulting new values only. Previous values are not stored.

---

## M18-35 — Audit Immutability
**Decision:** Audit events are append-only and immutable through normal application and administrative operations during their approved retention lifecycle. They cannot be edited or manually deleted. Archival, anonymization, or deletion may occur only through the retention process accepted under M18-36.

---

## M18-36 — Audit Retention

- **Status:** OPEN
- **Priority:** P1
- **Owner:** _unassigned — recommend Legal/Compliance_
- **Target date:** _unassigned_
- **Related modules:** Audit, Legal, Privacy

### Decision Question
How long is each audit event category retained?

### Options Considered
1. One retention period for all categories
2. Different periods per category (e.g. security events retained longer than routine business events)
3. Archive after a period
4. Anonymize after a period

### Decision
_To be completed._

### Temporary Default
No automatic deletion of any audit record until this decision is made.

### Open Issues
- This is typically driven by legal/compliance requirements (data protection law, sector rules) — start there rather than picking a number arbitrarily.

---

## M18-37 — Audit Visibility and Export

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** Audit UI, Reporting

### Decision

Audit events carry a visibility classification:

```
INTERNAL_ONLY
SUPPLIER_VISIBLE
AUDITOR_ONLY
SECURITY_ONLY
```

Auditor and Portal Admin can see all events. Supplier Admin can see only events belonging to their own organization **and** classified `SUPPLIER_VISIBLE`. Security, internal-administration, support-access, and technical events are never shown to Supplier Admin.

### Rationale

The review flagged that letting Supplier Admin see *all* of their organization's audit events risks leaking internal Buyer-side information (IP addresses, support access, internal notes, security events tied to the same org). The visibility classes above scope this down to business events that are genuinely appropriate to share externally.

### Open Issues
- The exact classification of each event category (from M18-32) into one of these four classes needs to be finalized during technical design.

### MVP Classification
MVP

---

## M18-38 — Security Alerts
**Decision:** No alerts in MVP.

---

## M18-39 — Audit, History, Log, and Trace Separation

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** Workflow, Integration, Operations

### Decision

Business audit events and business lifecycle/state-change events share one business-audit event infrastructure in MVP.

This infrastructure stores the event and its resulting new values under M18-33; it is **not** a complete record-version history and cannot be used to reconstruct every previous state.

Application error logs, operational logs, and technical/distributed traces are **not** stored in the business audit infrastructure. They belong to a separate operational observability channel from the beginning.

A separate business-history or record-versioning capability is deferred to a later phase and must be introduced only when a business module requires full historical reconstruction.

### Rationale

Business audit events and technical observability have different audiences, volumes, retention needs, and sensitivity. Keeping technical logs and traces outside the business audit store avoids performance, privacy, and retention conflicts while preserving a lean MVP audit model.

### MVP Classification
MVP

---

## M18-40 — ERP Data Ownership & Sync Direction

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** Module 19, User Management

### Decision

The MVP is a standalone portal and implements **no CaniasERP synchronization**.

The Portal remains the source of truth for portal User Accounts, Organization Memberships, Role Assignments, and portal login credentials. Portal login credentials must never be sent to ERP.

The future CaniasERP integration contract — synchronized data fields, direction for each field, triggers, frequency, conflict handling, and ERP mapping identifiers — will be decided during the CaniasERP integration design after the actual installation and available interfaces are inspected.

No ERP-facing table, job, event publisher, inbound write path, or synchronization behavior may be invented as part of the standalone MVP.

### Rationale

The earlier direction matrix committed to data flows that have not yet been validated against the actual CaniasERP installation. The stable architectural decision is the standalone boundary and portal ownership of portal identity/access data; vendor-specific synchronization decisions are intentionally deferred.

### MVP Classification
MVP boundary. ERP integration: Later phase.


---

## M18-41 — Integration Identities

- **Status:** SPLIT → M18-41A / M18-41B
- **Priority:** P0

### Superseded Structure

The original question mixed two different decisions: whether each external system has its own identity, and which credential mechanism that identity uses. It is split into M18-41A and M18-41B below.

---

## M18-41A — Integration Identity Model

- **Status:** ACCEPTED
- **Priority:** P0
- **Related modules:** Module 19, Audit, Security

### Decision

Each external system or integration is represented by its own separate non-human Integration Identity.

- Human User Accounts must not be used for system-to-system communication.
- One Integration Identity must not be shared by unrelated external systems.
- Test and production environments must not share an identity or credential.
- An Integration Identity receives only explicit Integration Grants and is independently auditable, disableable, and revocable.

### MVP Classification
Required architectural foundation; actual ERP integration is later phase.

---

## M18-41B — Integration Authentication Mechanism

- **Status:** OPEN
- **Priority:** P0
- **Owner:** Integration owner / CaniasERP administrator — unassigned
- **Target date:** Before CaniasERP integration technical design
- **Related modules:** Module 19, Audit, Security

### Decision Question

Which authentication mechanism will CaniasERP and other external systems use when calling the portal?

### Candidate Mechanisms

1. OAuth 2.0 Client Credentials
2. A scope-limited, expirable, rotatable API credential
3. A CaniasERP-specific service credential or supported mechanism

### Decision

_To be completed during the CaniasERP integration phase after the actual CaniasERP installation and supported interfaces are inspected._

### Constraints Already Accepted

- The mechanism must authenticate a separate Integration Identity from M18-41A.
- Authentication-specific logic must be isolated from business services and authorization rules.
- Changing the mechanism later must not require rewriting business modules.
- Shared human accounts and shared cross-system credentials are prohibited.


---

## M18-42 — Production Administrative Access

- **Status:** ACCEPTED
- **Priority:** P1
- **Related modules:** Operations, Security, Audit

### Decision

Production administrative access is **not a standing role**. It is granted only as a break-glass access request that is:

- Request-based (someone has to ask for it)
- Separately approved (not self-granted)
- Time-limited (expires automatically)
- Scope-limited (only the systems/data needed)
- Justification-required (a reason is recorded)
- Fully audited (every action while elevated is logged)
- Read-only by default, unless write access is specifically justified and approved

### Rationale

This is the concrete mechanism that keeps M18-31 ("no Super Admin") true in practice — without these constraints, a "limited support access with approval" grant can silently become a permanent all-access account.

### MVP Classification
Later phase — the break-glass workflow itself is not built in MVP. In MVP, production access is handled through standard engineering deployment access only; there is no in-app elevated-access grant mechanism until this is built.

---

## M18-43 — Compromised-Account Response
**Decision:** Terminate sessions and suspend the account.

---

## M18-44 — Module 18 MVP Scope

- **Status:** ACCEPTED
- **Priority:** P0
- **Owner:** Product Owner
- **Decision date:** 2026-07-23
- **Related modules:** Project Roadmap

### Decision Question
Which capabilities are MVP, later phase, or out of scope?

### Decision

Module 18 will use a **balanced standalone MVP** scope.

### MVP

#### Identity and Account Access

- Invitation-based account creation only; open self-registration is not included.
- Email verification.
- Sign-in with portal credentials.
- Password reset through an email link.
- Basic account-based sign-in throttling and lockout.
- Multiple concurrent sessions with inactivity and absolute timeouts.

#### Organization Membership

- User Account and Organization Membership are modeled separately.
- One User Account may have multiple Organization Memberships.
- Existing User Accounts are reused when invited to another Organization; duplicate accounts are not created.
- The first Supplier Admin requires Buyer approval.
- An approved Supplier Admin may invite and manage Supplier Users in their own organization.
- Ending one Membership does not disable the global User Account while another active Membership exists.

#### Roles and Authorization

- Fixed roles.
- Role Permission Grants composed of Permission and Scope.
- Authorization evaluated under one active Organization Membership context.
- Organization and data-scope isolation.
- State-dependent authorization for workflows included in the MVP.
- Basic segregation of duties for approval workflows included in the MVP.

The initial Role catalogue may contain all accepted fixed Roles. However, only Roles required by business modules included in the product MVP need complete UI and business workflows in the first release.

#### Audit and Sensitive Data

- Basic append-only business audit trail.
- Identity/access, security, business-change, approval, and administration events.
- Changed fields and resulting new values are stored; previous values are not stored.
- Full historical reconstruction from audit events is not an MVP requirement.
- Authentication secrets are never stored in audit events.
- Basic internal audit viewing for Portal Admin.
- Simple redaction for explicitly classified sensitive fields.
- Technical logs and traces remain separate from business audit.

### Later Phase

- Open self-registration.
- Single-active-session enforcement.
- MFA.
- SSO.
- Custom roles.
- Temporary delegation.
- User impersonation.
- Break-glass production access.
- Security alerts.
- General-purpose field-level permission engine.
- Full audit visibility UI, export, and reporting.
- Automated audit retention, archival, anonymization, or deletion.
- Separate business-history or record-versioning capability.
- CaniasERP integration.
- Integration Identity authentication mechanism.

### Out of Scope for the Standalone MVP

- ERP synchronization tables, jobs, events, and inbound write paths.
- Shared human accounts or Integration Identities shared across unrelated systems.
- A permanent Super Admin role.
- General workflow or policy engines created only for possible future use.
- Full record reconstruction from audit events.

### Rationale

This balanced standalone MVP provides a usable identity, membership, authorization, and audit foundation without introducing enterprise support tooling or vendor-specific ERP integration before those needs are validated.

### MVP Classification
MVP

---

## M18-45 — First Vertical Slice

- **Status:** ACCEPTED
- **Priority:** P1
- **Related modules:** Supplier Invitation, Authentication, Authorization, Audit

### Decision

Supplier Admin invitation and activation: invite → accept → verify → approve where required → activate membership → sign in → authorize within organization → end membership.

The first Supplier Admin is invited by a Buyer or Portal Admin and requires Buyer approval under M18-09.

### Normative Existing-Account Rule

If the invited email belongs to an existing User Account, the system must reuse that User Account and create a new Organization Membership. It must not create a duplicate User Account.

If the existing account is already `VERIFIED` and the verified email has not changed, email verification is not repeated. Invitation acceptance and any required membership approval still apply.

### Decision IDs Exercised by This Slice

M18-01A, M18-01B, M18-03, M18-06, M18-07, M18-08, M18-09, M18-10, M18-11, M18-12, M18-13, M18-14, M18-17, M18-20, M18-23, M18-24, M18-27, M18-28, M18-32, M18-33, M18-35, M18-37, and M18-45.

### Remaining Technical-Design Questions

- Invitation token lifetime and single-use behavior
- Expired, revoked, and resent invitation behavior
- Transaction and idempotency rules for invitation acceptance and membership activation
- Session representation and invalidation mechanism for an ended Membership context
- Exact audit event names and payloads for each step

These questions may define implementation mechanics but must not contradict the accepted decisions above.

### MVP Classification
MVP

---

# Recommended First Vertical Slice

1. A Buyer or Portal Admin invites the first Supplier Admin.
2. The recipient accepts the invitation.
3. A new User Account is created only if no account already exists for that person; otherwise the existing account is reused.
4. The user verifies the email address if it is not already verified.
5. Buyer approval is completed where M18-09 requires it.
6. The Organization Membership becomes `ACTIVE`.
7. The user signs in when the effective authentication conditions in M18-10 are satisfied.
8. Authorization is evaluated only within the active Membership context and its explicit Role Permission Grants.
9. An authorized administrator ends the Membership without disabling unrelated memberships.
10. Sessions operating under the ended Membership context become invalid.
11. Every important step is written to the business audit trail, with changed fields and resulting new values where applicable, and without authentication secrets.

---

# Phase 1 Completion Checklist

- [x] Tenant and data-isolation model decided.
- [x] Principal types and human Roles separated and defined.
- [x] Integration Identity model separated from its future authentication mechanism.
- [x] Multi-organization membership decided.
- [x] Account lifecycle separated from membership lifecycle.
- [x] Effective authentication and organization-access invariants defined.
- [x] Internal-user data scopes defined as Permission + Scope grants.
- [x] Account creation and lifecycle rules defined.
- [x] Authentication and session policies defined at business level.
- [x] Authorization model and multi-role conflict resolution decided.
- [x] Sensitive-field visibility defined as its own decision.
- [x] Role-assignment authority defined.
- [x] Audit categories and event contents defined, including changed fields, resulting new values, and secret exclusions.
- [x] Audit immutability defined consistently with future retention.
- [ ] Audit retention period defined. (M18-36 open)
- [x] Standalone MVP boundary and future ERP-decision boundary defined. (M18-40)
- [x] Separate Integration Identity per external system decided. (M18-41A)
- [ ] Integration Identity authentication mechanism decided. (M18-41B open)
- [x] Balanced standalone MVP and deferred capabilities accepted. (M18-44)
- [x] First vertical slice selected, with remaining technical-design questions listed.
- [ ] M18-36 has an assigned owner and target date; M18-41B has an owner assigned.

---

> **Repository location:** See `docs/00-roadmap.md` for the canonical folder structure — not repeated here to avoid drift between modules.
