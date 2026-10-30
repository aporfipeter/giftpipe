# ADR-003 — Use OccasionGiftIdea as a first-class occasion-specific context

**Status:** Accepted

**Decision area:** Domain

## Context

GiftPipe needs to preserve a durable GiftIdea while also recording what happened when that idea was considered for a particular Occasion.

An early modeling direction considered attaching occasion information directly to research nodes or treating the GiftIdea–Occasion relationship as a simple link. As the research and reconciliation flows became clearer, that was insufficient: occasion-specific research, candidate relevance, and outcomes all belong to the relationship between a GiftIdea and one Occasion.

## Decision

Represent the application of a GiftIdea to an Occasion as a first-class domain concept: **OccasionGiftIdea**.

OccasionGiftIdea represents:

> what happened when this GiftIdea was considered for this particular Occasion.

It is the context for:

- occasion-specific research,
- occasion-specific candidate nomination,
- occasion-specific outcome/reconciliation,
- and historical preservation of the fact that the idea was considered.

General GiftIdea research remains separate from Occasion-specific research.

## Alternatives considered

### Treat GiftIdea–Occasion as a passive join

This was rejected because the relationship has its own behavior and history, including research and outcome.

### Put an optional Occasion identifier on every research node

This was discussed and rejected as unnecessarily awkward. It would mix general and occasion-specific research inside one tree and complicate context boundaries.

## Rationale

The accepted model matches the product behavior discovered through examples:

- the same GiftIdea can be reused across Occasions,
- research may be useful generally or only for one Occasion,
- and an Occasion needs to preserve whether an idea was used without changing the durable GiftIdea itself.

Separating the research scopes also provides cleaner context boundaries for later AI assistance.

## Consequences

### Explicitly discussed

- GiftIdea owns reusable general research.
- OccasionGiftIdea owns Occasion-specific research.
- Historical OccasionGiftIdea associations remain even when the idea was not used.
- Candidates become relevant to a particular Occasion through the occasion context rather than automatically.

### Direct implications

- OccasionGiftIdea has domain meaning beyond persistence linkage.
- AI context can distinguish reusable research from one-off occasion working notes.

### Still unresolved

- Whether ResearchThread becomes a literal domain concept.
- How occasion-specific research is promoted or referenced into general research.
- Whether duplicate OccasionGiftIdeas for the same GiftIdea and Occasion are valid.
- Exact OccasionGiftCandidate identity semantics.

## Related documentation

- [Domain Model](../domain/domain-model.md) — `DM-R002`, `DM-R006`, `DM-R009`, `DM-R014`
- [Product Requirements](../product/requirements.md) — `FR-015`, `FR-016`, `FR-017`, `FR-018`
- [Open Questions](../discovery/open-questions.md) — `OQ-001`, `OQ-005`, `OQ-006`, `OQ-008`

## Historical evidence

Discovery explicitly compared general research under GiftIdea with occasion-specific research under OccasionGiftIdea and rejected the idea of placing a nullable `occasionId` on every research node. The OccasionGiftIdea concept then became the basis for Occasion pages, reconciliation, and historical outcomes.
