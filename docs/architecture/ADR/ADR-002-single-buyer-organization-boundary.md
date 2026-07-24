# ADR-002 — Single-Buyer Organization Boundary

- **Status:** Accepted
- **Date:** 2026-07-24
- **Related decisions:** M18-02, M18-04

## Context

The first version of the portal will be operated by one buyer organization and used by multiple supplier companies.

## Decision

- The portal follows a `single-buyer, multi-supplier` model.
- A separate `Tenant` concept is not used in the current system.
- Buyer-side and supplier-side companies are represented as `Organizations`.
- A legal entity is represented as an `Organization`.
- A branch is represented as a child `Organization` under a legal entity.
- Supplier organizations are not independent tenants.
- This ADR must be reopened if the product later becomes a multi-buyer SaaS platform.

## Consequences

- Data isolation is based on organizations, memberships, permissions, and scopes.
- The standalone portal remains simpler than a multi-tenant SaaS model.
- Multiple unrelated buyer customers are not supported.
- A future multi-buyer model would affect ownership, authorization, audit, integrations, and business records.
