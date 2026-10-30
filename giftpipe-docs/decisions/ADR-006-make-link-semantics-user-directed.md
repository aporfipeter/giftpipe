# ADR-006 — Let users distinguish reference links from product links

**Status:** Accepted

**Decision area:** Domain

## Context

GiftPipe allows users to save URLs as part of gift research. A difficult question emerged: how should the system know whether a pasted URL represents a concrete product or merely supporting material such as an article about Japanese whiskies?

One proposed direction was to inspect metadata, structured data, page type, heuristics, and AI classification. That implied a substantial and fragile page-classification problem.

The later discussion proposed two explicit user actions instead.

## Decision

GiftPipe distinguishes two link semantics:

- **LinkNode** — supporting material such as an article, review, inspiration page, or other reference.
- **ProductLinkNode** — a URL the user identifies as representing a specific product or giftable item.

The user supplies the semantic intent through the capture action rather than GiftPipe being required to universally infer what kind of page a URL is.

A ProductLinkNode is **not automatically the GiftCandidate itself**. A Candidate may have multiple ProductLinkNodes.

## Alternatives considered

### Automatically classify every pasted page

This was discussed using Open Graph metadata, JSON-LD, page heuristics, and possibly AI classification. It was not selected as the core semantic mechanism because it would require GiftPipe to reliably determine what arbitrary pages mean.

### Treat every URL as the same generic link type

This would fail to preserve the useful distinction between product-specific research and supporting material.

### Make ProductLinkNode equivalent to GiftCandidate

This was rejected because one Candidate may have multiple retailer or product links.

## Rationale

The user explicitly accepted the two-option approach because it removes the need for a large set of page parsers while retaining clear domain intent.

The accepted model also keeps URL metadata enrichment optional: metadata may improve the experience without being responsible for the semantic classification.

## Consequences

### Explicitly discussed

- The product needs distinct "product" and "reference" link actions in user-facing language.
- Multiple product links may support one Candidate.
- Supporting articles remain research rather than silently creating Candidates.
- Product links do not by themselves determine Candidate identity.

### Direct implications

- Universal automatic page classification is not required for semantic correctness.
- Metadata extraction can remain enrichment rather than the source of domain truth.

### Still unresolved

When a new GiftIdea is created from only a ProductLinkNode, it remains unresolved whether GiftPipe should also create a GiftCandidate automatically.

## Related documentation

- [Domain Model](../domain/domain-model.md) — `DM-R017`, `DM-R018`
- [Product Requirements](../product/requirements.md) — `FR-011`, `FR-012`, `FR-013`
- [Open Questions](../discovery/open-questions.md) — `OQ-011`

## Historical evidence

The user explicitly proposed two options, LinkNode and ProductLinkNode, to avoid "a buttload of page parsers" while recognizing possible user confusion. The proposal was accepted, with the additional clarification that ProductLinkNode should remain separate from GiftCandidate because one Candidate can have multiple product URLs.
