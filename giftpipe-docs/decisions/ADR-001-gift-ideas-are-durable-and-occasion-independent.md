# ADR-001 — Keep GiftIdeas durable and independent of Occasions

**Status:** Accepted

**Decision area:** Domain

## Context

GiftPipe began with the need to capture gift thoughts before a specific occasion becomes urgent. During discovery, an important modeling question was whether an idea should belong directly to a birthday, Christmas, anniversary, or other occasion, or whether it should survive independently and be reusable later.

The discussions repeatedly used examples such as "Whisky" or "Japanese whisky" being useful across more than one birthday or holiday. The later domain model also established that being given or not given for one Occasion must not itself retire the underlying GiftIdea.

## Decision

A **GiftIdea is a durable, Person-scoped concept that exists independently of any Occasion**.

A GiftIdea may:

- exist without an Occasion,
- later be considered for one or more Occasions,
- accumulate research over time,
- remain useful after being given or not given for a particular Occasion,
- and be excluded from proactive future consideration through explicit archival rather than through an Occasion outcome.

Occasion-specific consideration is represented separately through OccasionGiftIdea.

## Alternatives considered

### Tie GiftIdeas directly to an Occasion

This interpretation appeared naturally in early gift-planning examples but was rejected as the model evolved. It would make reusable interests and long-lived research awkward because the same idea would need to be recreated or moved between occasions.

### Automatically retire an idea after it is given or not given

This was explicitly rejected. A gift idea may still be useful on a later Occasion, and "given" or "not given" describes an occasion outcome rather than the lifecycle of the durable GiftIdea.

## Rationale

The accepted model reflects the product's capture-first purpose: an idea should be worth saving even when the user does not yet know when they will use it.

Keeping the GiftIdea durable also preserves cumulative research and allows GiftPipe to build useful gifting context over time.

## Consequences

### Explicitly discussed

- A GiftIdea does not require an Occasion at capture time.
- The same GiftIdea can be considered across multiple Occasions.
- Giving or not giving an idea does not automatically archive it.
- Archival is distinct from an Occasion outcome.

### Direct implications

- Occasion-specific outcomes cannot live solely on GiftIdea.
- Historical occasion associations must be modeled separately.
- GiftIdea research can remain reusable across Occasions.

### Still unresolved

Detailed archive/reactivation behavior remains open.

## Related documentation

- [Product Vision](../product/vision.md)
- [Domain Model](../domain/domain-model.md) — `DM-R002`, `DM-R019`
- [Product Requirements](../product/requirements.md) — `FR-004`, `FR-006`, `FR-007`
- [Open Questions](../discovery/open-questions.md) — `OQ-012`

## Historical evidence

Discovery explicitly moved away from treating ideas as occasion-owned. The recurring examples showed the same idea being considered at different occasions, and later discussion established that unused ideas remain associated with their original occasion history while the underlying GiftIdea remains available for future use.
