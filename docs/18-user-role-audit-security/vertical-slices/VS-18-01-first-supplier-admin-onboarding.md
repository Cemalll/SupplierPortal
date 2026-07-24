# VS-18-01 — First Supplier Admin Onboarding

- **Status:** Ready for Technical Design
- **Module:** 18 — User, Role, Security, and Audit
- **Related decision:** M18-45

## 1. Business Goal

Enable the buyer side to onboard the first administrator of a supplier organization in a controlled and auditable way.

At the end of this slice, the approved Supplier Admin can sign in and act only within the supplier organization covered by the active membership.

## 2. Preconditions

The following bootstrap setup already exists:

- One Buyer Organization
- One verified and active Portal Admin User Account
- One active buyer-side Organization Membership
- The Portal Admin Role assigned through that Membership

The bootstrap process is required before this slice but is not implemented as part of this slice.

## 3. Main Flow

```text
Buyer or Portal Admin
→ creates Supplier Admin Invitation
→ recipient accepts Invitation
→ existing User Account is reused, or a new one is created
→ email is verified when required
→ Supplier Admin Membership waits for Buyer approval
→ Buyer approves Supplier Admin authority
→ Organization Membership becomes ACTIVE
→ Supplier Admin Role Assignment becomes effective
→ user signs in with portal credentials
→ authorization is evaluated under the active Membership context
→ important actions are written to the Audit trail
```

## 4. Included Capabilities

### Invitation

- Invitation-based onboarding only
- Invitation targets one supplier Organization
- Invitation proposes Supplier Admin authority
- Open self-registration is not included

### User Account

- Create a new User Account only when no matching account exists
- Reuse an existing User Account when one already exists
- Do not create a duplicate User Account for the same person
- Skip repeated email verification when the same email is already verified

### Organization Membership

- Create or link one supplier-side Organization Membership
- Do not grant organization business access before the Membership becomes `ACTIVE`
- Ending this Membership must not disable unrelated Memberships

### Approval

- The first Supplier Admin requires Buyer-side approval
- Email verification and Membership approval are separate gates
- Approval applies to the Membership and Supplier Admin authority
- Rejection must keep Supplier Admin authority ineffective

### Role and Authorization

- Assign the `Supplier Admin` Role through the supplier Organization Membership
- Do not assign the Role directly to the User Account
- Evaluate authorization under the exact active Membership context
- Require one matching `Permission + Scope` grant
- Use `SUPPLIER_ORGANIZATION` as the maximum supplier-side data boundary
- Do not combine grants across Memberships

### Authentication and Session

Authentication requires:

```text
account_status = ACTIVE
AND verification_status = VERIFIED
AND security_status = NORMAL
```

Organization access additionally requires:

```text
membership_status = ACTIVE
AND session = ACTIVE
AND matching Permission + Scope grant
```

### Audit

Record important events for:

- Invitation creation and acceptance
- User Account creation or reuse
- Email verification
- Membership approval or rejection
- Membership activation
- Role Assignment
- Membership ending

Audit Events must:

- Be append-only
- Store actor, action, target, organization context, timestamp, result, category, `changed_fields`, and `new_values`
- Never store `old_values`
- Never store passwords, tokens, API keys, or other secrets

## 5. Conceptual Path

```text 
Buyer or Portal Admin
→ Invitation
→ User Account
→ Organization Membership
→ Membership Approval
→ Role Assignment
→ Supplier Admin Role
→ Role Permission Grant
→ Permission + Scope
→ Session
→ Organization Access Context
→ Audit Event
```

## 6. Referenced Decisions

```text
M18-01A, M18-01B, M18-03, M18-06, M18-07, M18-08,
M18-09, M18-10, M18-11, M18-12, M18-13, M18-14,
M18-17, M18-20, M18-23, M18-24, M18-27, M18-28,
M18-32, M18-33, M18-35, M18-37, M18-45
```

## 7. Out of Scope

- Buyer Organization and initial Portal Admin bootstrap implementation
- Open self-registration
- Supplier User onboarding
- Additional or replacement Supplier Admin onboarding
- MFA and SSO
- Custom Roles and delegation
- User impersonation and break-glass access
- ERP synchronization
- Integration Identity authentication
- General workflow engine
- Full record-version history
- Full audit reporting and export

## 8. Acceptance Criteria

1. A Buyer or Portal Admin can invite the first Supplier Admin of a supplier Organization.
2. No organization access is granted before acceptance, required verification, and Buyer approval.
3. An existing User Account is reused instead of creating a duplicate.
4. The Supplier Admin Role is assigned through the Organization Membership.
5. The Membership becomes `ACTIVE` only after all required gates succeed.
6. Sign-in requires valid account, verification, and security states.
7. Supplier data access requires an active Membership and one matching `Permission + Scope` grant.
8. Ending the Membership removes access to that supplier Organization without disabling unrelated Memberships.
9. Important lifecycle events are written to the Audit trail.
10. Audit records contain no authentication secrets or previous field values.

## 9. Technical Design Questions

Resolve these in `03-technical-design.md` before implementation:

- Invitation token lifetime and single-use behavior
- Expired, revoked, and resent Invitation behavior
- Existing-account matching rule
- Transaction and idempotency boundaries
- Representation of rejected approval
- Password hashing and credential storage
- Session representation and timeout values
- Membership-context selection and invalidation
- Exact Supplier Admin Permission matrix
- Concrete `SUPPLIER_ORGANIZATION` Scope filtering
- Exact Audit Event names and visibility classes

## 10. Completion Condition

The slice is complete when the first Supplier Admin can be invited, verified, approved, activated, authenticated, authorized within the supplier Organization, safely offboarded, and audited according to the referenced accepted decisions.
