# GiftPipe Decision Records

## Purpose

This directory preserves significant product, domain, and architectural decisions made during GiftPipe discovery.

Decision records complement the domain model and requirements:

- the **domain model** describes the current concepts, invariants, and behavior,
- the **requirements** describe what the product is expected to do,
- an **ADR** preserves why a consequential choice was made, what alternatives were considered, and what constraints follow from it,
- the **open questions register** contains decisions that have not yet been made.

ADRs should not be used to turn unresolved proposals into accepted design.

## Conventions

### Filenames and identifiers

Decision records use stable sequential identifiers and descriptive filenames:

`ADR-001-short-descriptive-title.md`

Once assigned, an ADR identifier must not be reused for another decision.

### Statuses

Supported statuses are:

- **Accepted** — the decision is established and currently applies.
- **Superseded** — the decision has been explicitly replaced by a later ADR.

A superseded ADR remains in the repository as historical context and must reference its replacement.

### When to create a new ADR

Create an ADR when a decision has lasting consequences for product behavior, domain semantics, architecture, or future development and its rationale would be difficult to recover from requirements alone.

Do not create ADRs for routine implementation details, minor naming choices, or unresolved questions.

### Changing an accepted decision

Do not silently rewrite the historical rationale of an Accepted ADR to make it describe a later design.

When a significant accepted decision is replaced, create a new ADR and mark the previous record Superseded with an explicit reference to the replacement.

### Open questions are not decisions

An unresolved question belongs in [the Open Questions Register](../discovery/open-questions.md), not in an ADR.

AI coding agents and contributors must not treat the absence of a decision record as permission to resolve an open question during implementation.

## Decision index

| ID | Decision | Area | Status |
|---|---|---|---|
| [ADR-001](ADR-001-gift-ideas-are-durable-and-occasion-independent.md) | Keep GiftIdeas durable and independent of Occasions | Domain | Accepted |
| [ADR-002](ADR-002-candidate-status-is-structural-not-specificity-based.md) | Determine GiftCandidate status by context, not textual specificity | Domain | Accepted |
| [ADR-003](ADR-003-use-occasiongiftidea-as-first-class-occasion-context.md) | Use OccasionGiftIdea as a first-class occasion-specific context | Domain | Accepted |
| [ADR-004](ADR-004-reconcile-outcomes-without-forcing-single-candidate.md) | Reconcile actual gifting without forcing a single concrete Candidate | Domain | Accepted |
| [ADR-005](ADR-005-bound-ai-to-explicit-gifting-actions.md) | Bound AI to explicit gifting actions and persist useful research | Product | Accepted |
| [ADR-006](ADR-006-make-link-semantics-user-directed.md) | Let users distinguish reference links from product links | Domain | Accepted |
| [ADR-007](ADR-007-keep-personprofile-explicitly-user-controlled.md) | Keep PersonProfile explicitly user-controlled | Domain | Accepted |
| [ADR-008](ADR-008-keep-proactive-suggestions-transient-until-adopted.md) | Keep proactive suggestions transient until explicitly adopted | Product | Accepted |

## Guidance for AI coding agents

Accepted ADRs constrain subsequent design and implementation.

Superseded ADRs are historical context, not current instructions. Open questions are not settled decisions. Agents must not silently replace an Accepted decision with a conflicting design and should consult the related domain rules and requirements before applying an ADR.

A significant change to an Accepted decision should be represented through a new superseding ADR rather than through undocumented implementation drift.
