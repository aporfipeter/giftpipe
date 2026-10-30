# ADR-002 — Determine GiftCandidate status by context, not textual specificity

**Status:** Accepted

**Decision area:** Domain

## Context

Discovery repeatedly encountered the question of when a captured phrase should be considered a GiftIdea and when it should be a GiftCandidate.

A tempting rule was to treat vague concepts such as "Whisky" as GiftIdeas and concrete concepts such as "Nikka From the Barrel" or "Metallica tickets" as Candidates. That approach broke down because the same phrase can legitimately appear at different structural levels depending on how the user got there.

## Decision

**GiftIdea specificity does not determine whether something is a GiftIdea or GiftCandidate.**

A GiftIdea may be arbitrarily vague or concrete.

A GiftCandidate is a concrete option created **within the research context of an existing GiftIdea**. Candidate-ness is therefore structural and contextual, not inferred from linguistic specificity.

Examples:

- A user directly captures "Metallica tickets" for Dad → this can be a GiftIdea.
- A user researches "Concert tickets" and adds "Metallica tickets" as one option → this is a GiftCandidate.

When a previously uncaptured gift is entered during reconciliation, GiftPipe does not invent a broader parent GiftIdea solely because the entered text sounds concrete.

## Alternatives considered

### Infer Candidate status from specificity

This was explicitly rejected. The system cannot reliably decide that a phrase is "specific enough" to become a Candidate, and doing so would make capture more complicated and unpredictable.

### Always create both a broad GiftIdea and a concrete Candidate

This was rejected in the reconciliation example. Entering "Metallica tickets" does not justify silently inventing a parent such as "Concert tickets".

## Rationale

The accepted rule protects frictionless capture and avoids semantic guessing.

It also keeps the model consistent: GiftIdea and GiftCandidate represent different **roles in the research structure**, not different levels of textual detail.

## Consequences

### Explicitly discussed

- Broad and concrete text are both valid GiftIdeas.
- GiftCandidates require an existing GiftIdea research context.
- A GiftIdea can be given without ever having a GiftCandidate.
- Reconciliation must not manufacture Candidates solely from wording specificity.

### Direct implications

- Automatic classification based only on text is insufficient.
- UI context and user action determine whether a new concrete option is a Candidate.

### Still unresolved

The identity relationship between a reusable GiftCandidate and an OccasionGiftCandidate created directly within an Occasion remains open.

## Related documentation

- [Domain Model](../domain/domain-model.md) — `DM-R003`, `DM-R004`, `DM-R005`, `DM-R012`
- [Product Requirements](../product/requirements.md) — `FR-004`, `FR-005`, `FR-008`, `FR-031`
- [Open Questions](../discovery/open-questions.md) — `OQ-001`

## Historical evidence

The key correction occurred when a reconciliation example proposed turning "Metallica tickets" into a generic "Concert tickets" GiftIdea plus Candidate. Discovery rejected that inference and established the rule that Candidate-ness is structural rather than semantic specificity.
