# ADR-003 — Membership-Scoped Authorization

- **Status:** Accepted
- **Date:** 2026-07-24
- **Related decisions:** M18-20, M18-23, M18-24, M18-26, M18-27, M18-28, M18-29

## Context

A role name alone does not define enough information for authorization.  
The system must determine both which action is allowed and which records that action may affect.

## Decision

Human-user authorization follows this path:

```text
User Account
→ Active Organization Membership
→ Role Assignment
→ Fixed Role
→ Role Permission Grant
```

A `Role Permission Grant` contains one inseparable pair:

```text
Permission + Scope
```

- `Permission` defines the allowed action on a resource type.
- `Scope` defines the records on which that action may operate.
- Authorization searches for a single grant whose Permission matches the requested action **and** whose Scope contains the target record.
- Permission and Scope are not evaluated as alternative conditions.
- Roles are evaluated only within the selected active membership context.
- Grants from different memberships are never combined.
- Multiple roles may contribute explicit grants, but the system must not invent a broader scope.
- Record state and segregation-of-duties checks remain additional authorization conditions.
- `GLOBAL` is a data scope, not an authorization bypass.

## Example

```text
Permission: view_order
Scope: ASSIGNED_RECORDS
```

This grant allows the user to view an order only when:

```text
requested action = view_order
AND
target order is assigned to the user
```

## Consequences

- The same Permission may be used with different Scopes in different roles.
- Authorization checks require both the requested action and the target record.
- Role Assignment may narrow context but must not broaden the Scope defined by a grant.
- The model prevents a Permission from one role from being combined with the broader Scope of another role.
- Technical design must define concrete Scope filters for each resource type.
