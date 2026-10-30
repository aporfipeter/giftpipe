# ADR-005 — Bound AI to explicit gifting actions and persist useful research

**Status:** Accepted

**Decision area:** Product

## Context

AI is an important part of GiftPipe, but discovery explicitly questioned whether the product should expose a general chat experience.

The concern was that an unrestricted assistant would expand GiftPipe beyond its purpose and allow unrelated requests. At the same time, AI is valuable for developing a captured idea, finding alternatives, comparing candidates, evaluating fit, summarizing research, or filling a genuine gap near an Occasion.

## Decision

GiftPipe AI is **bounded to explicit gifting-related capabilities** rather than offered as an unrestricted general-purpose chatbot.

Developers define the capabilities GiftPipe supports. The product determines which actions are valid in a given context. The user invokes an action, and useful output is persisted as part of the relevant research where appropriate.

Free-text instructions may exist inside a bounded action, but that does not make the product a general chat surface.

User-captured ideas remain primary context; AI assists, enriches, or fills gaps rather than replacing them by default.

## Alternatives considered

### Unrestricted ChatGPT-style conversation

This was explicitly rejected for the current product direction. Discovery noted that GiftPipe should not become a place for unrelated coding requests or general life conversation.

### AI invents actions dynamically

This was rejected in favor of developer-defined capabilities. AI may eventually rank valid actions, but it does not create arbitrary capabilities at runtime.

### Treat AI output as transient chat

This was rejected. Useful AI research should remain available for later research and future context.

## Rationale

The accepted direction keeps AI aligned with GiftPipe's product purpose while preserving user trust, controlling scope, and allowing research to accumulate over time.

It also supports a cleaner distinction between the domain model and AI: AI operates on established GiftPipe concepts rather than defining the model itself.

## Consequences

### Explicitly discussed

- AI actions are gifting-specific.
- Useful AI output persists in research.
- AI can enhance an existing GiftIdea without replacing its intent.
- AI can use recipient, history, and relevant research context.
- AI-driven ranking of actions is not required initially.

### Direct implications

- An AI adapter or implementation may change without changing the product decision.
- Core GiftPipe data remains meaningful without AI output.

### Still unresolved

- The exact action taxonomy.
- How current research summaries are refreshed.
- The threshold for when proactive AI should run near an Occasion.

## Related documentation

- [Product Vision](../product/vision.md)
- [Domain Model](../domain/domain-model.md) — `DM-R015`, `DM-R016`
- [Product Requirements](../product/requirements.md) — `FR-024`, `FR-025`, `FR-026`, `FR-027`, `NFR-006`, `NFR-007`
- [Open Questions](../discovery/open-questions.md) — `OQ-014`, `OQ-017`

## Historical evidence

Discovery explicitly described the desired AI model as action-oriented and bounded, including actions such as research, alternatives, comparison, summarization, expansion, and fit evaluation. The user explicitly rejected the idea of GiftPipe becoming an unrestricted assistant and accepted the bounded-action direction.
