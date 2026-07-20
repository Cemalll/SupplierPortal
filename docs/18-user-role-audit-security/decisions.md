# Architecture Decisions — User, Role, Security, and Audit

> **Module:** 18 — User, Role, Security, and Audit  
> **Phase:** Phase 1 — Decision Discovery  
> **File:** `decisions.md`  
> **Status:** In progress

This file records the business and architectural decisions for the User, Role, Security, and Audit module of the Supplier Portal.

Record only decisions that are expensive to change, affect multiple modules, or are likely to be discussed again later. Technical implementation details such as library names, exact endpoint paths, table names, and middleware file names belong to later design phases.

> **Conventions:** Status values, priority values, and the decision entry template used in this file are defined once, centrally, in [`architecture/adr-conventions.md`](../architecture/adr-conventions.md). Do not redefine them here or in any other module's decision log — update the shared file instead.

---

## Decision Index

| ID | Decision | Priority | Status |
|---|---|---:|---|
| M18-01 | Initial User Types | P0 | OPEN |
| M18-02 | Tenant Definition | P0 | OPEN |
| M18-03 | Multi-Organization Membership | P0 | OPEN |
| M18-04 | Legal Entity and Branch Model | P0 | OPEN |
| M18-05 | Internal-User Data Scope | P0 | OPEN |
| M18-06 | User and Organization Source of Truth | P0 | OPEN |
| M18-07 | Account Creation Methods | P0 | OPEN |
| M18-08 | Supplier-Side User Administration | P0 | OPEN |
| M18-09 | Organization Membership Approval | P0 | OPEN |
| M18-10 | Account Lifecycle States | P0 | OPEN |
| M18-11 | User Offboarding | P0 | OPEN |
| M18-12 | Email Changes and Duplicate Accounts | P1 | OPEN |
| M18-13 | Shared Accounts | P0 | OPEN |
| M18-14 | Authentication Methods | P0 | OPEN |
| M18-15 | Multi-Factor Authentication | P1 | OPEN |
| M18-16 | Password Reset and Account Recovery | P1 | OPEN |
| M18-17 | Session Policy | P1 | OPEN |
| M18-18 | Failed Sign-Ins and Account Lockout | P1 | OPEN |
| M18-19 | User Impersonation | P2 | OPEN |
| M18-20 | Authorization Model | P0 | OPEN |
| M18-21 | Fixed and Custom Roles | P0 | OPEN |
| M18-22 | Initial Role Catalogue | P0 | OPEN |
| M18-23 | Permission Granularity | P0 | OPEN |
| M18-24 | Authorization Scope Model | P0 | OPEN |
| M18-25 | Sensitive-Field Visibility | P0 | OPEN |
| M18-26 | State-Dependent Permissions | P0 | OPEN |
| M18-27 | Role Assignment Authority | P0 | OPEN |
| M18-28 | Multiple-Role Evaluation | P0 | OPEN |
| M18-29 | Segregation of Duties | P1 | OPEN |
| M18-30 | Temporary Delegation | P2 | OPEN |
| M18-31 | Super Admin and Emergency Access | P1 | OPEN |
| M18-32 | Audited Event Categories | P0 | OPEN |
| M18-33 | Audit Event Contents | P0 | OPEN |
| M18-34 | Old and New Value Storage | P0 | OPEN |
| M18-35 | Audit Immutability | P0 | OPEN |
| M18-36 | Audit Retention | P1 | OPEN |
| M18-37 | Audit Visibility and Export | P0 | OPEN |
| M18-38 | Security Alerts | P1 | OPEN |
| M18-39 | Audit, History, Log, and Trace Separation | P0 | OPEN |
| M18-40 | ERP-Driven User Changes | P0 | OPEN |
| M18-41 | Integration Identities | P0 | OPEN |
| M18-42 | Production Administrative Access | P1 | OPEN |
| M18-43 | Compromised-Account Response | P1 | OPEN |
| M18-44 | Module 18 MVP Scope | P0 | OPEN |
| M18-45 | First Vertical Slice | P0 | OPEN |

---

## M18-01 — Initial User Types

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** All modules
- **Source of truth:**

### Context

Define which actor groups can access the first release.

### Decision Question

Which user types will exist in the MVP?

### Options Considered

1. System Admin
2. Buyer
3. Buyer Manager
4. Quality User
5. Finance User
6. Auditor
7. Supplier Admin
8. Supplier User
9. Integration identity

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-02 — Tenant Definition

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Module 00, Module 19, all business modules
- **Source of truth:**

### Context

Tenant boundaries determine data isolation, authorization, database design, and future scalability.

### Decision Question

What does a tenant represent in this system?

### Options Considered

1. Each supplier is a tenant
2. Buyer and suppliers are separate tenants
3. Single-buyer portal; suppliers are organizations, not tenants
4. Full multi-tenant SaaS for multiple buyer companies

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-03 — Multi-Organization Membership

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** User Management, Supplier Profile
- **Source of truth:**

### Context

One person may work for multiple suppliers, legal entities, or business units.

### Decision Question

Can one identity belong to more than one organization?

### Options Considered

1. One account belongs to one organization
2. One identity may have multiple memberships
3. Separate account required for every organization

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-04 — Legal Entity and Branch Model

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Supplier Registration, Orders, Contracts, Invoices
- **Source of truth:**

### Context

Supplier groups may contain several legal entities, countries, tax registrations, and branches.

### Decision Question

How will supplier groups, legal entities, and branches be represented?

### Options Considered

1. One supplier group equals one organization
2. Every legal entity is a separate organization
3. Legal entities are separate and branches are child units
4. Use a general organization hierarchy

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-05 — Internal-User Data Scope

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** All business modules
- **Source of truth:**

### Context

Users with the same role may be responsible for different suppliers or categories.

### Decision Question

How will records visible to internal users be restricted?

### Options Considered

1. All records
2. Assigned records only
3. Own business unit
4. Selected categories or commodities
5. Selected countries or regions
6. Combination of role and scope

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-06 — User and Organization Source of Truth

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Module 19, User Management, Supplier Profile
- **Source of truth:**

### Context

User and organization data may exist in the portal, ERP, HR system, or corporate directory.

### Decision Question

Which system owns each user, organization, role, department, and business-unit field?

### Options Considered

1. Portal owns all
2. ERP owns organizations; portal owns users and roles
3. Identity provider owns internal users; portal owns supplier users
4. Hybrid ownership through a source-of-truth matrix

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-07 — Account Creation Methods

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Supplier Invitation, Supplier Registration
- **Source of truth:**

### Context

Different user types may require different provisioning flows.

### Decision Question

How will internal and supplier accounts be created?

### Options Considered

1. Administrator creation
2. Invitation-based registration
3. Self-registration
4. Automatic provisioning from ERP or directory
5. Supplier Admin invitation

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-08 — Supplier-Side User Administration

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** User Management, Supplier Profile
- **Source of truth:**

### Context

Supplier organizations may need to manage their own users.

### Decision Question

Which user-management actions can a Supplier Admin perform?

### Options Considered

1. Invite users
2. Assign supplier-side roles
3. Deactivate users
4. Appoint another Supplier Admin
5. Require buyer approval for sensitive actions

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-09 — Organization Membership Approval

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Supplier Invitation, Supplier Registration
- **Source of truth:**

### Context

A verified email address does not prove authority to represent an organization.

### Decision Question

Does organization membership require approval?

### Options Considered

1. Auto-approve Supplier Admin invitations
2. Require buyer-side approval
3. Auto-approve matching email domains
4. Manual approval for every membership

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-10 — Account Lifecycle States

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Authentication, User Management
- **Source of truth:**

### Context

Accounts move through invitation, verification, activation, lockout, suspension, and deactivation.

### Decision Question

Which account states and transitions will exist?

### Options Considered

1. INVITED
2. PENDING_VERIFICATION
3. ACTIVE
4. LOCKED
5. SUSPENDED
6. DISABLED
7. INVITATION_EXPIRED

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-11 — User Offboarding

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Tasks, Approvals, Audit
- **Source of truth:**

### Context

Departing users may own tasks, approvals, and historical records.

### Decision Question

What happens to the account, sessions, tasks, and records when a user leaves?

### Options Considered

1. Delete account
2. Deactivate and preserve history
3. Reassign pending tasks
4. Transfer Supplier Admin ownership

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-12 — Email Changes and Duplicate Accounts

- **Status:** OPEN
- **Priority:** P1
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Authentication, User Management
- **Source of truth:**

### Context

Email addresses may change, and one person may accidentally create multiple accounts.

### Decision Question

How will email changes, immutable identity, and duplicate accounts be handled?

### Options Considered

1. Email is permanent identity
2. Email is a mutable login identifier
3. Use a separate immutable user ID
4. Allow account merging
5. Do not support merging

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-13 — Shared Accounts

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Authentication, Audit
- **Source of truth:**

### Context

Shared mailboxes reduce accountability and audit reliability.

### Decision Question

Will shared human accounts be allowed?

### Options Considered

1. Prohibit shared accounts
2. Allow only non-human service accounts
3. Allow shared accounts with extra controls

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-14 — Authentication Methods

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Authentication, Module 19
- **Source of truth:**

### Context

Internal and supplier users may require different sign-in methods.

### Decision Question

How will each user type authenticate?

### Options Considered

1. Internal users use SSO; suppliers use portal credentials
2. All users use portal credentials
3. Portal credentials in MVP; SSO later
4. All users use an external identity provider

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-15 — Multi-Factor Authentication

- **Status:** OPEN
- **Priority:** P1
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Authentication, Security
- **Source of truth:**

### Context

Administrative and sensitive accounts require stronger protection.

### Decision Question

For whom will MFA be mandatory?

### Options Considered

1. System Admin only
2. All internal users
3. Users with sensitive permissions
4. All users
5. Deferred beyond MVP

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-16 — Password Reset and Account Recovery

- **Status:** OPEN
- **Priority:** P1
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Authentication, Notifications, Audit
- **Source of truth:**

### Context

Account recovery is a high-risk process.

### Decision Question

How will password reset and recovery work?

### Options Considered

1. Email reset link
2. Administrator-assisted recovery
3. Identity-provider recovery
4. Recovery with additional identity verification

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-17 — Session Policy

- **Status:** OPEN
- **Priority:** P1
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Authentication, Security
- **Source of truth:**

### Context

Session lifetime affects both security and user experience.

### Decision Question

What are the inactivity timeout, absolute lifetime, concurrency, and revocation rules?

### Options Considered

1. Same policy for all users
2. Stricter policy for privileged roles
3. Multiple concurrent sessions
4. Single active session
5. Remember-me enabled or disabled

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-18 — Failed Sign-Ins and Account Lockout

- **Status:** OPEN
- **Priority:** P1
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Authentication, Audit, Notifications
- **Source of truth:**

### Context

Repeated failed sign-ins may indicate credential attacks.

### Decision Question

How will throttling, lockout, and notifications be handled?

### Options Considered

1. Account-based throttling
2. IP-based throttling
3. Combined controls
4. Temporary lockout
5. Progressive delay without hard lockout

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-19 — User Impersonation

- **Status:** OPEN
- **Priority:** P2
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Support, Authentication, Audit
- **Source of truth:**

### Context

Support teams may request the ability to view the portal as another user.

### Decision Question

Will administrators be allowed to impersonate users?

### Options Considered

1. No impersonation
2. Read-only impersonation
3. Full impersonation with strict audit
4. Impersonation requiring user approval

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-20 — Authorization Model

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** All modules
- **Source of truth:**

### Context

Role checks alone may not handle organization, assignment, category, and record-state restrictions.

### Decision Question

Which authorization model will be used?

### Options Considered

1. RBAC only
2. RBAC plus data scope
3. RBAC plus scope and resource state
4. Full ABAC or policy engine

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-21 — Fixed and Custom Roles

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** User Management
- **Source of truth:**

### Context

The portal may use fixed roles or allow configurable role definitions.

### Decision Question

Will roles be fixed, configurable, or both?

### Options Considered

1. Fixed system roles only
2. Fixed roles in MVP; custom roles later
3. Buyer admins can create custom roles
4. Supplier admins can create custom roles

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-22 — Initial Role Catalogue

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** All modules
- **Source of truth:**

### Context

The MVP requires a small set of roles with separated responsibilities.

### Decision Question

Which roles will exist initially?

### Options Considered

1. System Admin
2. Buyer
3. Buyer Manager
4. Supplier Admin
5. Supplier User
6. Auditor
7. Quality User
8. Finance User

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-23 — Permission Granularity

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** All modules
- **Source of truth:**

### Context

Permissions can be defined at module, action, field, and scope level.

### Decision Question

How granular will permissions be?

### Options Considered

1. Module-level
2. Action-level
3. Field-level for sensitive data
4. Scope managed separately from permission

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-24 — Authorization Scope Model

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** All modules
- **Source of truth:**

### Context

A permission must define which records it applies to.

### Decision Question

Which scope types will be supported?

### Options Considered

1. OWN_ORGANIZATION
2. ASSIGNED_RECORDS
3. OWN_BUSINESS_UNIT
4. SELECTED_CATEGORIES
5. SELECTED_REGIONS
6. ALL

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-25 — Sensitive-Field Visibility

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Supplier Profile, Finance, Risk, Sourcing
- **Source of truth:**

### Context

Some fields must not be visible to everyone who can access the parent record.

### Decision Question

Which fields require separate visibility, masking, or update controls?

### Options Considered

1. Hide completely
2. Display masked values
3. Separate view and update permissions
4. Restrict by role and organization

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-26 — State-Dependent Permissions

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Supplier Registration, Documents, RFx, Contracts
- **Source of truth:**

### Context

Actions may be allowed only while a business record is in a specific state.

### Decision Question

Will authorization depend on the current resource state?

### Options Considered

1. State validation only in business services
2. State rules included in authorization
3. Permission check plus workflow validation

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-27 — Role Assignment Authority

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** User Management, Audit
- **Source of truth:**

### Context

Role assignment is privileged and may create escalation risks.

### Decision Question

Who can assign, remove, and approve roles?

### Options Considered

1. System Admin assigns all roles
2. Supplier Admin assigns supplier-side roles only
3. Sensitive roles require approval
4. Self-assignment is prohibited

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-28 — Multiple-Role Evaluation

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Authorization
- **Source of truth:**

### Context

A user may hold several roles across organizations.

### Decision Question

How will permissions from multiple roles be combined?

### Options Considered

1. Union of allows
2. Explicit deny overrides allow
3. Conflicting roles prohibited
4. Evaluate roles within active membership

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-29 — Segregation of Duties

- **Status:** OPEN
- **Priority:** P1
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Supplier Approval, Finance, Contracts, Quality
- **Source of truth:**

### Context

Some operations should not be initiated and approved by the same person.

### Decision Question

Which operations require segregation of duties?

### Options Considered

1. No SoD in MVP
2. Creator cannot approve own record
3. Sensitive changes require a second person
4. Configurable SoD rules

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-30 — Temporary Delegation

- **Status:** OPEN
- **Priority:** P2
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Approvals, Tasks, Notifications
- **Source of truth:**

### Context

Users may need temporary delegation during leave or absence.

### Decision Question

Will temporary task, role, or permission delegation be supported?

### Options Considered

1. No delegation
2. Task reassignment only
3. Temporary role delegation
4. Permission delegation with approval

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-31 — Super Admin and Emergency Access

- **Status:** OPEN
- **Priority:** P1
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Security, Audit, Operations
- **Source of truth:**

### Context

Emergency support may require broad access, but permanent unrestricted access is risky.

### Decision Question

How will Super Admin and emergency access be controlled?

### Options Considered

1. Permanent Super Admin
2. Time-limited emergency elevation
3. Approval-based emergency access
4. No application-level Super Admin

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-32 — Audited Event Categories

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** All modules
- **Source of truth:**

### Context

The system must define which business and security actions require audit records.

### Decision Question

Which event categories will be written to the audit trail?

### Options Considered

1. Authentication events
2. User lifecycle events
3. Membership changes
4. Role and permission changes
5. Business-data changes
6. Approval events
7. Document actions
8. Exports
9. Admin actions
10. Integration changes
11. Unauthorized attempts

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-33 — Audit Event Contents

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** All modules
- **Source of truth:**

### Context

Audit records must explain what happened without exposing secrets.

### Decision Question

Which fields will every audit event contain?

### Options Considered

1. Actor and organization
2. Role snapshot
3. Action
4. Target entity and record
5. Timestamp and result
6. IP and user agent
7. Correlation ID
8. Source system
9. Reason
10. Impersonation actor

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-34 — Old and New Value Storage

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Supplier Profile, Finance, Documents
- **Source of truth:**

### Context

Field-level history may require before-and-after values.

### Decision Question

Will audit events contain old and new values?

### Options Considered

1. Full objects
2. Changed fields only
3. Masked sensitive values
4. Metadata only for files and large objects

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-35 — Audit Immutability

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Audit, Operations
- **Source of truth:**

### Context

Audit records are trustworthy only if they cannot be silently modified.

### Decision Question

Can audit records be updated or deleted?

### Options Considered

1. Append-only
2. Corrections as new events
3. Controlled retention deletion
4. Administrator-editable records

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-36 — Audit Retention

- **Status:** OPEN
- **Priority:** P1
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Audit, Legal, Privacy
- **Source of truth:**

### Context

Audit records require defined legal, business, privacy, and storage periods.

### Decision Question

How long will each event category be retained?

### Options Considered

1. One common period
2. Different periods by category
3. Archive older records
4. Anonymize after a defined period

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-37 — Audit Visibility and Export

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Audit UI, Reporting
- **Source of truth:**

### Context

Audit records may contain sensitive information.

### Decision Question

Who can view, search, and export audit records?

### Options Considered

1. System Admin only
2. Auditor and System Admin
3. Supplier Admin sees own-organization events
4. Export requires a separate permission

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-38 — Security Alerts

- **Status:** OPEN
- **Priority:** P1
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Audit, Notifications, Security
- **Source of truth:**

### Context

High-risk events may require immediate notification.

### Decision Question

Which events generate alerts, and who receives them?

### Options Considered

1. Admin-role assignment
2. Supplier Admin replacement
3. Repeated failed sign-ins
4. Bank-detail changes
5. Bulk exports
6. Audit access
7. Account recovery
8. Suspicious device or location

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-39 — Audit, History, Log, and Trace Separation

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Workflow, Integration, Operations
- **Source of truth:**

### Context

Audit trail, business history, technical logs, security events, and integration traces serve different purposes.

### Decision Question

How will these record types be separated?

### Options Considered

1. Audit trail
2. Business history
3. Technical log
4. Security event
5. Integration trace

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-40 — ERP-Driven User Changes

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Module 19, User Management
- **Source of truth:**

### Context

User or employment changes may originate outside the portal.

### Decision Question

How will ERP or corporate-system changes affect portal accounts and memberships?

### Options Considered

1. ERP updates portal
2. Portal updates ERP
3. One-way ownership per data type
4. Manual conflict resolution

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-41 — Integration Identities

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Module 19, Audit, Security
- **Source of truth:**

### Context

External systems must not operate through human accounts.

### Decision Question

How will integrations authenticate and how will actions be represented?

### Options Considered

1. Separate service account per integration
2. Shared integration account
3. OAuth client credentials
4. Restricted signed API keys

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-42 — Production Administrative Access

- **Status:** OPEN
- **Priority:** P1
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Operations, Security, Audit
- **Source of truth:**

### Context

Production data and administration require stricter controls.

### Decision Question

Who can access production administration, databases, and manual correction functions?

### Options Considered

1. No direct developer access
2. Limited support access with approval
3. Temporary elevated access
4. Permanent operations-team access

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-43 — Compromised-Account Response

- **Status:** OPEN
- **Priority:** P1
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Authentication, Audit, Notifications
- **Source of truth:**

### Context

The portal needs a repeatable response to suspected account compromise.

### Decision Question

What actions are mandatory when an account is suspected to be compromised?

### Options Considered

1. Terminate sessions
2. Suspend account
3. Reset credentials
4. Review activity
5. Reapprove critical changes
6. Notify user and security owner

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-44 — Module 18 MVP Scope

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Project Roadmap
- **Source of truth:**

### Context

The module contains more capabilities than should be implemented initially.

### Decision Question

Which capabilities are MVP, later phase, or out of scope?

### Options Considered

1. Invitation
2. Email verification
3. Sign-in
4. Password reset
5. Fixed roles
6. Custom roles
7. Role assignment
8. Membership
9. Deactivation
10. Session revocation
11. Basic audit
12. Audit viewer
13. MFA
14. Delegation
15. SSO
16. Service accounts
17. Impersonation

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

## M18-45 — First Vertical Slice

- **Status:** OPEN
- **Priority:** P0
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:** Supplier Invitation, Authentication, Audit
- **Source of truth:**

### Context

The module should be built as one small end-to-end business capability.

### Decision Question

What will be the first complete vertical slice?

### Options Considered

1. Supplier Admin invitation and activation
2. Internal Buyer provisioning
3. Role assignment and permission validation
4. Audit capture only

### Decision

_To be completed._

### Rationale

_To be completed._

### Consequences

#### Positive

- To be completed.

#### Negative / Trade-offs

- To be completed.

### Exceptions

- None identified.

### Integration Impact

- To be completed.

### MVP Classification

- MVP / Later phase / Out of scope

### Open Issues

- None.

---

# Module 18 MVP Classification Matrix

| Capability | MVP | Later Phase | Out of Scope | Notes |
|---|:---:|:---:|:---:|---|
| User invitation |  |  |  |  |
| Email verification |  |  |  |  |
| Sign-in |  |  |  |  |
| Password reset |  |  |  |  |
| Fixed roles |  |  |  |  |
| Custom role creation |  |  |  |  |
| Role assignment |  |  |  |  |
| Organization membership |  |  |  |  |
| User deactivation |  |  |  |  |
| Session revocation |  |  |  |  |
| Basic audit trail |  |  |  |  |
| Audit viewer |  |  |  |  |
| MFA |  |  |  |  |
| Temporary delegation |  |  |  |  |
| SSO |  |  |  |  |
| Service-account management |  |  |  |  |
| Impersonation |  |  |  |  |

---

# Recommended First Vertical Slice

1. An administrator invites a Supplier Admin.
2. The user accepts the invitation.
3. The user verifies the email address.
4. The account becomes `ACTIVE`.
5. The user signs in.
6. The user can access only their own organization’s data.
7. The administrator deactivates the account.
8. Existing sessions become invalid.
9. Every important action is recorded in the audit trail.

This slice validates identity, account, organization, membership, role, permission, scope, account state, session revocation, and audit behavior together.

---

# Phase 1 Completion Checklist

- [ ] Tenant and data-isolation model decided.
- [ ] Identity and organization membership separated.
- [ ] Initial user types and roles defined.
- [ ] Multi-organization membership decided.
- [ ] Internal-user data scopes defined.
- [ ] Account creation and lifecycle rules defined.
- [ ] Authentication and session policies defined at business level.
- [ ] Authorization model decided.
- [ ] Permission and scope granularity defined.
- [ ] Role-assignment authority defined.
- [ ] Sensitive-field visibility defined.
- [ ] Audit categories and event contents defined.
- [ ] Audit immutability, retention, and visibility defined.
- [ ] ERP and identity-system ownership rules defined.
- [ ] MVP and deferred capabilities separated.
- [ ] First vertical slice selected.
- [ ] Every open decision has an owner and target date.

---

> **Repository location:** See `docs/00-roadmap.md` for the canonical folder structure — not repeated here to avoid drift between modules.
