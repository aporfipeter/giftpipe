# ADR-004 — Reconcile actual gifting without forcing a single concrete Candidate

**Status:** Accepted

**Decision area:** Domain

## Context

GiftPipe needs to close the loop after an Occasion without requiring users to maintain perfect planning data in advance.

Discovery explored several cases:

- a GiftIdea with no Candidates,
- a GiftIdea with one Candidate,
- a GiftIdea with several Candidates,
- multiple GiftIdeas for one Occasion,
- more than one gift actually being given,
- and a gift that was never captured before the Occasion.

A model that required exactly one winning Candidate could not represent these cases faithfully.

## Decision

Reconciliation is an **Occasion-level process** that records what actually happened and preserves the historical planning context.

The process must allow:

- an OccasionGiftIdea to resolve as used/given even with no concrete Candidate,
- one or more known candidates to be recorded as actually given,
- multiple gifts to be recorded for the same Occasion,
- previously uncaptured gifts to be added during reconciliation,
- unused OccasionGiftIdeas to remain in history rather than being deleted.

A previously uncaptured gift becomes a new GiftIdea associated with that Occasion; GiftPipe does not invent an additional Candidate solely from textual specificity.

## Alternatives considered

### Require one selected Candidate as the fulfilled gift

This was rejected because a user may give something represented only by a root GiftIdea and because multiple gifts are valid.

### Automatically close all other options after one gift is chosen

This was rejected in the multi-gift discussion. Selecting one gift does not prove that no other gift was given.

### Remove unused Occasion associations

This was rejected because the fact that an idea was considered but not used is useful history.

## Rationale

The accepted approach minimizes post-Occasion maintenance while preserving truthful history at whatever level of detail the user actually captured.

It also distinguishes planning intent from reality: what was intended before the Occasion may differ from what was ultimately given.

## Consequences

### Explicitly discussed

- Reconciliation can resolve at GiftIdea level without a Candidate.
- Multiple gifts may be recorded.
- Known candidates can be identified as fulfilled when available.
- "Add another gift" remains meaningful even after other gifts are recorded.
- Previously uncaptured gifts become GiftIdeas.
- Unused OccasionGiftIdeas remain historical.

### Direct implications

- Candidate selection cannot be the sole source of fulfillment truth.
- Gift history must tolerate different levels of specificity.

### Still unresolved

- Exact OccasionGiftIdea and OccasionGiftCandidate state vocabulary.
- Partial reconciliation semantics.
- Whether completed reconciliation can be reopened or corrected.
- Follow-up timing and stopping rules.

## Related documentation

- [Domain Model](../domain/domain-model.md) — `DM-R006`, `DM-R008`, `DM-R011`, `DM-R012`
- [Product Requirements](../product/requirements.md) — `FR-028` through `FR-036`
- [Open Questions](../discovery/open-questions.md) — `OQ-003`, `OQ-010`, `OQ-013`, `OQ-015`

## Historical evidence

The decisive examples involved Dad's Birthday with separate "Guitar" and "Piano" OccasionGiftIdeas. "Piano" could be recorded as given without a Candidate, while "Fender Stratocaster" could be recorded through a known Candidate. The discussion then explicitly kept an "Add another gift" path because multiple gifts may have been given.
