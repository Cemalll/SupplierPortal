# ADR Conventions — Supplier Portal

> **File:** `docs/00-architecture/adr-conventions.md`
> **Purpose:** Single source of truth for the status values, priority values, and entry template used by every module's `decisions.md`. Referenced, never duplicated.

Every module's decision log (`docs/<module>/decisions.md`) must link back to this file instead of redefining any of the sections below. If a convention needs to change, change it here once — do not patch individual module files.

---

## Decision Status Legend

| Status | Meaning |
|---|---|
| `OPEN` | No decision has been made |
| `PROPOSED` | A preferred option exists but is not approved |
| `DECIDED` | The decision is approved |
| `DEFERRED` | The decision is intentionally postponed |
| `REJECTED` | The proposed decision was rejected |
| `SUPERSEDED` | A newer decision replaced this one |

**Rule:** A decision must be `DECIDED` before it can be included in a vibe-coding prompt as something to build. `PROPOSED` is not enough — it means a preference exists but hasn't been confirmed. `DEFERRED` decisions must be explicitly listed as out-of-scope in the same prompt, so the AI knows not to guess at them.

---

## Priority Legend

| Priority | Meaning |
|---|---|
| `P0` | Must be decided before Phase 2 (conceptual model) for the slice that depends on it |
| `P1` | Must be decided before implementation (Phase 4) of the slice that depends on it |
| `P2` | May be deferred beyond the MVP entirely |

**Note:** Priority is relative to the *slice* a decision belongs to, not to the whole module. A `P2` decision can still block a later slice's Phase 4 — it just doesn't block the *current* one.

---

## Decision Entry Template

Every decision in a module's `decisions.md` must follow this exact structure. Copy this block when adding a new decision.

```markdown
## <ID> — <Decision Title>

- **Status:** OPEN
- **Priority:** P0 / P1 / P2
- **Decision owner:**
- **Decision date:**
- **Review date:**
- **Related modules:**
- **Source of truth:**

### Context

<Why this decision exists — what forces or ambiguity make it necessary.>

### Decision Question

<The single question this entry answers.>

### Options Considered

1. <Option 1>
2. <Option 2>
3. <Option 3>

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
```

**Fields that must never stay empty once a decision moves to `DECIDED`:** `Decision`, `Rationale`, `Consequences`. These three are what actually carry meaning into a vibe-coding prompt — an approved decision with an empty `Rationale` is nearly as unusable to the AI as an open one, because the AI can't tell *why* the choice was made and may reintroduce the rejected alternative elsewhere.

---

## Repository Location

```text
supplier-portal/
├── docs/
│   ├── 00-roadmap.md
│   ├── 00-architecture/
│   │   ├── adr-conventions.md      ← this file
│   │   └── adr/
│   │       ├── 0001-<topic>.md
│   │       └── ...
│   ├── 18-user-role-security-audit/
│   │   └── decisions.md
│   ├── 01-supplier-discovery-invite/
│   │   └── decisions.md
│   └── ... (one folder per module)
└── src/
```

See `docs/00-roadmap.md` for the full rationale behind this structure — not repeated here to avoid two files claiming to be canonical for the same thing.

---

## Slicing Convention (applies to every module, not just Module 18)

When a module's decision count is large enough that not all decisions can reasonably be `DECIDED` before the first line of code, split implementation into slices:

1. Identify the smallest set of `DECIDED` decisions that produce one working, testable, end-to-end capability.
2. Everything else stays `DEFERRED`, each with an owner and a target slice/date — not just "later."
3. Every vibe-coding prompt for that slice must state, explicitly, both:
   - the `DECIDED` decisions in scope (build these), and
   - the `DEFERRED` decisions the AI must **not** silently resolve (stop and ask if one becomes necessary).
4. `DEFERRED` is a queue position, not a rejection — track it the same way you'd track a backlog item.
