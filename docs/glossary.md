# Supplier Portal Glossary

> **Purpose:** This file defines the canonical terms used across decision logs, conceptual models, technical designs, vertical-slice specifications, APIs, schemas, and code.
>
> Use these terms consistently. Do not introduce alternative names for the same concept without updating this glossary and the related decisions.

---

## Architecture and Delivery Terms

### Architecture Decision Record (ADR)

A short record of an important architectural decision that affects multiple modules or is expensive to change later.

An ADR normally contains:

- Context
- Decision
- Consequences
- Status

### Decision Log

A module-level document containing accepted, open, merged, deferred, and out-of-scope business or architectural decisions.

Accepted decisions are binding for later design and implementation.

### Conceptual Model

A technology-independent representation of domain concepts, relationships, rules, states, and invariants.

A conceptual entity does not automatically imply a database table or software class.

### Technical Design

The implementation-oriented design that converts accepted decisions and conceptual concepts into:

- Schemas
- API contracts
- Validation rules
- Authorization checks
- Transaction boundaries
- Error behavior
- Tests

### Vertical Slice

An end-to-end unit of delivery that produces a complete business behavior across the required layers.

A vertical slice is not limited to one technical layer such as UI, API, or database.

### MVP

The minimum product scope selected for the first usable release.

A capability marked as later phase or out of scope must not be implemented as part of the MVP.

### Source of Truth

The system that owns the authoritative value of a specific data element.

Data ownership must be decided per data category or field and must not be inferred from integration direction alone.

---

## Identity Terms

### Principal

Any human or non-human actor recognized by the system.

The portal has two principal types:

- Human User
- Integration Identity

### Human User

A person who interacts with the portal through a `User Account`.

Human users receive authorization through organization memberships and role assignments.

### User Account

The global portal identity of one person.

A User Account:

- Is independent of any single organization
- May hold multiple Organization Memberships
- Uses a permanent internal identifier
- May use email as a changeable login identifier

### Integration Identity

A non-human identity representing one external system or integration.

An Integration Identity:

- Does not hold an Organization Membership
- Does not receive human-facing Roles
- Receives explicit Integration Grants
- Must not be shared by unrelated systems
- Must be separate between test and production environments

### Shared Human Account

One account used by multiple people.

Shared human accounts are prohibited because they prevent reliable authorization and audit attribution.

---

## Organization Terms

### Organization

The general domain concept representing a buyer-side or supplier-side legal entity or branch.

Every Organization is classified by:

- Organization side
- Organization unit kind

### Organization Side

Identifies which side of the portal an Organization belongs to.

Allowed conceptual values:

- `BUYER`
- `SUPPLIER`

### Organization Unit Kind

Identifies the structural type of an Organization.

Allowed conceptual values:

- `LEGAL_ENTITY`
- `BRANCH`

### Legal Entity

A legally independent company represented as an Organization.

Example:

```text
ABC Manufacturing Inc.
organization_side = SUPPLIER
unit_kind = LEGAL_ENTITY
```

### Branch

A child Organization belonging to one parent Legal Entity Organization.

Example:

```text
ABC Manufacturing — Gebze Branch
organization_side = SUPPLIER
unit_kind = BRANCH
parent = ABC Manufacturing Inc.
```

### Tenant

An isolated customer boundary in a multi-tenant product.

The current portal does not use a Tenant concept because it follows a single-buyer, multi-supplier model. Supplier Organizations are not tenants.

---

## Membership and Onboarding Terms

### Invitation

A controlled request to onboard a person into an Organization.

An Invitation targets:

- One person or email address
- One Organization
- One intended membership relationship
- One proposed role or authority where applicable

### Organization Membership

The relationship between one User Account and one Organization.

A Membership carries:

- Membership status
- Organization context
- Role Assignments
- Approval requirement where applicable

Roles are assigned through Memberships, not directly to User Accounts.

### Membership Approval

A buyer-side approval decision required when granting Supplier Admin authority.

It is required for:

- The first Supplier Admin
- An additional Supplier Admin
- A replacement Supplier Admin

It is not a general-purpose workflow engine.

### Organization Access Context

The exact active Organization Membership under which a request is evaluated.

Authorization from one Membership must never be reused in another Membership context.

---

## Role and Authorization Terms

### Role

A fixed, named bundle of human-user authorization grants.

Initial role catalog:

- Portal Admin
- Auditor
- Buyer
- Approver
- Finance User
- Quality User
- Supplier Admin
- Supplier User

### Role Assignment

The assignment of one Role to one Organization Membership.

A Role Assignment may narrow the applicable organization or legal-entity context but must not broaden the Scope defined by a grant.

### Permission

An explicit action that may be performed on a resource type.

Examples:

```text
view order
edit membership
invite supplier-user
approve supplier-admin-membership
export audit-event
```

Permission answers:

> What action may be performed?

### Scope

The set of records on which a Permission may operate.

Scope answers:

> On which records may the action be performed?

Canonical Scope values:

- `GLOBAL`
- `BUYER_ORGANIZATION`
- `LEGAL_ENTITY`
- `BUSINESS_UNIT`
- `SUPPLIER_ORGANIZATION`
- `ASSIGNED_RECORDS`
- `OWN_RECORDS`

`READ_ONLY` is not a Scope. Read-only behavior is expressed through read-oriented Permissions.

### Role Permission Grant

One inseparable `Permission + Scope` pair contained by a Role.

Example:

```text
Permission: view_order
Scope: ASSIGNED_RECORDS
```

Authorization succeeds only when:

```text
requested action matches Permission
AND
target record is inside Scope
```

### Integration Grant

One explicit `Permission + Scope` pair assigned directly to an Integration Identity.

Integration Grants are the non-human authorization path and are separate from human Roles.

### Authorization

The decision that determines whether a principal may perform a requested action on a target resource.

For a human user, authorization considers:

- Account eligibility
- Active Session
- Active Organization Membership
- Exact Membership context
- Matching Permission + Scope grant
- Record state
- Segregation of duties

### State-Dependent Authorization

An authorization rule that also considers the current state of the target record.

Example:

```text
An approved order may be viewable but no longer editable.
```

### Segregation of Duties (SoD)

A control preventing conflicting responsibilities from being performed by the same person.

Example:

```text
The person who created a critical request cannot approve the same request.
```

### Explicit Deny

A rule that directly blocks an action even when another grant allows it.

Explicit deny is not part of the current MVP authorization model.

---

## Authentication and Security Terms

### Authentication

The process of verifying who a principal is.

Authentication does not automatically grant access to organization business data.

### Portal Credential

The MVP authentication mechanism used by a human User Account.

The current MVP uses portal-managed credentials. SSO may be introduced later.

### Session

One authenticated login session belonging to a User Account.

Multiple concurrent Sessions are allowed in the MVP.

### Account Status

The global lifecycle state of a User Account.

Conceptual values:

- `ACTIVE`
- `SUSPENDED`
- `DISABLED`

### Verification Status

The email-verification state of a User Account.

Conceptual values:

- `PENDING`
- `VERIFIED`

### Security Status

The current security condition of a User Account.

Conceptual values:

- `NORMAL`
- `LOCKED`
- `COMPROMISED`

### Membership Status

The lifecycle state of one Organization Membership.

Conceptual values:

- `INVITED`
- `PENDING_APPROVAL`
- `ACTIVE`
- `SUSPENDED`
- `ENDED`

### Effective Authentication Eligibility

A User Account is eligible to authenticate only when:

```text
account_status = ACTIVE
AND
verification_status = VERIFIED
AND
security_status = NORMAL
```

### Effective Organization Access

A successfully authenticated user may access organization business data only when the selected Organization Membership is also `ACTIVE` and the requested action is authorized.

---

## Data Protection and Audit Terms

### Sensitive Data Classification

Metadata marking fields that require restricted visibility or redaction.

Examples may include:

- Bank details
- Tax identifiers
- Commercial discount information
- Security-related metadata

### Redaction

Removing, masking, partially displaying, or replacing sensitive values before returning data to a viewer.

The exact redaction method belongs to technical design.

### Audit Event

An append-only record of an important identity, security, business, approval, or administrative action.

An Audit Event may contain:

- Actor
- Action
- Target resource type and identifier
- Organization context
- Timestamp
- Result
- Event category
- Changed fields
- Resulting new values

### Business Audit

The channel used to provide accountability for important portal actions.

Business audit answers questions such as:

- Who acted?
- What action occurred?
- Which resource was affected?
- When did it occur?
- What was the result?
- What resulting values were recorded?

### Audit Immutability

The rule that Audit Events cannot be edited or manually deleted through normal application or administrative operations.

### Record History

A complete sequence of previous versions of a business record.

The MVP audit model is not a complete record-history or versioning system.

### Operational Log

A technical record used to diagnose application behavior, failures, infrastructure events, or operational conditions.

Operational logs are separate from business audit.

### Distributed Trace

A technical representation of how one request travels across services, components, or processing steps.

Distributed traces are part of observability, not business audit.

### Observability

The technical capability used to understand system behavior through logs, metrics, and traces.

Observability and business audit have different purposes, audiences, access rules, and retention needs.

---

## Integration Terms

### ERP Integration

The exchange of data between the Supplier Portal and an ERP system such as CaniasERP.

ERP synchronization is not part of the standalone MVP.

### Integration Authentication Mechanism

The credential or protocol used by an external system to authenticate as an Integration Identity.

Candidate mechanisms may include:

- OAuth 2.0 Client Credentials
- Scope-limited API credentials
- A CaniasERP-supported service credential

The mechanism remains an open integration-phase decision.

### Synchronization Direction

The direction in which a data element flows between systems.

Possible directions include:

- Portal to ERP
- ERP to Portal
- Bidirectional
- No synchronization

Synchronization direction must be decided per field or data category.

### ERP Mapping Identifier

A future identifier used to associate a portal record with its corresponding ERP record.

ERP mapping identifiers are not part of the standalone MVP design.

---

## Decision Status Terms

### `ACCEPTED`

The decision is approved and binding for conceptual modeling, technical design, and implementation within its delivery scope.

### `OPEN`

The decision is unresolved.

It must not be guessed or implemented until the required information or authority is available.

### `MERGED`

The question is covered by another canonical decision.

### `SPLIT`

The original question has been separated into multiple independent decisions.

### `POST_MVP`

The capability is accepted as a possible later-phase feature but is not part of the MVP.

### `OUT_OF_SCOPE`

The capability must not be built within the current product or delivery boundary.

---

## Usage Rule

When a term used in a decision, conceptual model, technical design, vertical-slice specification, schema, API, or code conflicts with this glossary:

1. Stop and identify the conflicting term.
2. Check the related accepted decisions.
3. Update the glossary and affected documents together.
4. Do not silently introduce a synonym or a different meaning.
