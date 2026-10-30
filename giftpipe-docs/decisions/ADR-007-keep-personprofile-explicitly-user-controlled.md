# ADR-007 — Keep PersonProfile explicitly user-controlled

**Status:** Accepted

**Decision area:** Domain

## Context

GiftPipe accumulates useful evidence over time: gift outcomes, success feedback, rejected candidates, AI research, and repeated gifting patterns.

Discovery considered whether these signals should automatically update the PersonProfile so that the profile becomes smarter over time.

That would make inferred information indistinguishable from facts the user intentionally recorded.

## Decision

The **PersonProfile is explicitly maintained by the user**.

Gift outcomes, success feedback, research history, and AI inference may inform future recommendations, but they must not silently mutate the PersonProfile.

If GiftPipe later proposes a profile update based on inferred information, user confirmation would be required; automatic mutation was not accepted.

## Alternatives considered

### Automatically learn profile facts from gift outcomes or AI inference

This was explicitly rejected for the initial model.

### Keep outcome-derived knowledge completely unusable

This was not chosen. Outcomes and research remain useful context for recommendations and Gift History; they are simply kept distinct from explicit profile truth.

## Rationale

The accepted distinction preserves provenance and user control. The user knows which facts they deliberately recorded, while GiftPipe can still benefit from historical evidence without presenting inference as fact.

## Consequences

### Explicitly discussed

- PersonProfile can contain structured facts and free-form notes.
- Only explicit user changes modify PersonProfile initially.
- Gift Success Feedback does not silently alter the profile.
- AI inference does not silently become a profile fact.

### Direct implications

- Gift History and PersonProfile remain conceptually distinct.
- AI context may include both without merging them.

### Still unresolved

The exact identity/classification of PersonProfile as an entity or value object is not established, but that does not alter this behavioral decision.

## Related documentation

- [Domain Model](../domain/domain-model.md) — `DM-R015`
- [Product Requirements](../product/requirements.md) — `FR-003`, `FR-027`, `FR-033`, `NFR-004`
- [Product Vision](../product/vision.md)

## Historical evidence

During discovery, the question of whether GiftPipe should update recipient profiles from outcomes was answered explicitly: initially, only the user changes the profile. AI and outcome information may remain useful elsewhere but must not silently rewrite the profile.
