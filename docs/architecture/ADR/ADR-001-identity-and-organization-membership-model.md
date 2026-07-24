# ADR-001 — Identity and Organization Membership Model

- **Status:** Accepted
- **Date:** 2026-07-24
- **Related decisions:** M18-01A, M18-03, M18-10, M18-11, M18-12, M18-13

## Context

A person may work for more than one organization in the portal.  
User identity, organization access, and role assignments must therefore be managed separately.

## Decision

- Each person has one global `User Account`.
- A `User Account` may have multiple `Organization Memberships`.
- Roles are assigned through an `Organization Membership`, not directly to the `User Account`.
- Each membership has its own lifecycle and status.
- Ending one membership does not disable unrelated memberships.
- Email is a changeable login identifier; the internal user ID is permanent.
- Shared human accounts are not allowed.
- External systems use separate `Integration Identities`.

## Consequences

- One person may have different roles in different organizations.
- Authorization is evaluated within one active membership context.
- Organization offboarding affects only the related membership.
- Global account security actions may affect access across all memberships.
- Account, membership, and role assignment remain separate concepts.
