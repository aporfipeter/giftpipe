# GiftPipe Product Vision

## 1. Product overview

GiftPipe is a personal gift-planning product for people who want to remember good gift ideas before an important occasion becomes urgent.

The product is built around a simple behavior: when a gift thought occurs, capture it quickly for the relevant person, then let GiftPipe preserve and resurface that context when it becomes useful. Ideas may begin as vague thoughts, concrete products, links, or research and can develop over time.

GiftPipe also uses contextual AI to help when the user wants to develop an idea further or when an approaching occasion has no sufficiently useful captured ideas. AI is intended to build on the user's own knowledge, recipient context, previous gifts, and accumulated research rather than replace them.

The product is intended to be developed as a real consumer application rather than only as a technical learning exercise.

---

## 2. Problem statement

Gift ideas often occur at inconvenient times and are easy to lose. By the time a birthday, anniversary, nameday, holiday, or other occasion approaches, the user may remember that they once had a good idea without remembering what it was.

Common lightweight approaches such as notes, browser bookmarks, messages to oneself, or memory can preserve fragments, but they do not naturally connect an idea to the person it is for, relevant occasions, previous research, other candidate gifts, or the eventual outcome.

The problem is therefore not only discovering gifts at the last moment. It is preserving useful gifting context over time and making it available again when the user needs to make a decision.

A second problem appears when the user has no useful idea. Generic AI recommendations can generate options, but they are more valuable when grounded in what the user already knows about the recipient, what has been given previously, and what has already been considered or rejected.

GiftPipe aims to reduce both forms of friction: losing good ideas before they become actionable, and starting from zero when an occasion approaches.

The discussion so far represents product discovery rather than validated market-wide user research. These problems are established motivations for GiftPipe, not yet proven claims about all gift-givers.

---

## 3. Target users and stakeholders

### Individual gift-giver

The primary user is an individual who buys gifts for friends, family members, or other people in their life.

Their main needs are to capture ideas quickly when they occur, retain context about the intended recipient, revisit and research ideas later, prepare for upcoming occasions without starting from scratch, and preserve useful gifting history over time.

The expected value is less last-minute uncertainty and a growing body of useful gifting knowledge that becomes more valuable through continued use.

### Gift recipient / Person

The people being planned for are important stakeholders in the domain but are not required to be GiftPipe users.

A user privately maintains information relevant to gifting for these people. The initial product model does not require recipients to create accounts or participate.

### Early external users

GiftPipe is intended to be tried by real users early rather than remain a single-user personal prototype. Friends and family can use their own independent accounts and provide feedback on whether the capture, reminder, research, and reconciliation loops are genuinely useful.

Each account is private. No data sharing of any kind between accounts is allowed in the current product scope.

---

## 4. Product vision statement

**GiftPipe helps people capture gift ideas when inspiration strikes, develop them when useful, and bring the right context back at the right occasion. Over time, it becomes a personal memory for gifting, strengthened by recipient context, gifting history, and focused AI assistance.**

---

## 5. Value proposition

### Primary value

GiftPipe preserves gift ideas and gifting context over time so that users do not have to recreate their thinking when an occasion approaches.

The product is valuable even if the user never performs detailed research: a simple idea such as "Whisky" associated with Dad can be captured in seconds and resurfaced later.

### Secondary value

When the user wants more help, GiftPipe can turn an initial thought into an evolving research process. A vague idea may lead to concrete candidates, links, notes, comparisons, or AI-assisted exploration without losing the original intent.

When the user has no useful captured idea for an approaching occasion, contextual AI can help generate relevant suggestions using available recipient context and gifting history.

After the occasion, lightweight reconciliation preserves what actually happened. This gradually improves the usefulness of future history and recommendations without requiring the user to maintain detailed records manually.

---

## 6. Product goals

### Goal: Make spontaneous gift capture worth doing

**Rationale:** The product's central loop depends on users recording ideas before they disappear. Capture therefore needs to require very little effort and tolerate incomplete information.

**Status:** Confirmed

### Goal: Resurface the user's own ideas before replacing them with generated ones

**Rationale:** A thought the user captured months earlier may contain more personal relevance than a newly generated generic suggestion. GiftPipe should preserve and prioritize that human context.

**Status:** Confirmed

### Goal: Let gift ideas develop over time

**Rationale:** An idea may start vague and later accumulate candidates, notes, links, comparisons, and AI research. The product should preserve that evolution instead of forcing users to decide everything at initial capture.

**Status:** Confirmed

### Goal: Provide contextual AI assistance without becoming a general-purpose chatbot

**Rationale:** AI is useful when it performs focused gifting tasks using relevant context. Unrestricted general conversation would dilute the product purpose and create behavior outside GiftPipe's intended domain.

**Status:** Confirmed

### Goal: Reduce manual maintenance of gifting history

**Rationale:** Users should not have to remember to return after an occasion and update every idea manually. Post-occasion reconciliation should make preserving outcomes lightweight.

**Status:** Confirmed

### Goal: Build cumulative value through continued use

**Rationale:** Gift ideas, research, previous outcomes, and recipient context become more useful when retained across occasions instead of discarded after each gift.

**Status:** Confirmed

### Goal: Validate GiftPipe with real users early

**Rationale:** The project is intended to become a real product, and early independent users such as friends and family should help reveal whether the core behavior is genuinely useful.

**Status:** Confirmed

---

## 7. Product principles

### Frictionless capture comes first

The user should be able to save a GiftIdea with only a recipient assigned, even before entering any idea content. GiftPipe supplies a placeholder title when needed; further detail can be added later. Generated placeholders identify records without implying that the user has supplied a gifting thought.

The [capture requirements](requirements.md#fr-004--capture-a-giftidea-with-minimal-required-information) define title precedence for explicit titles, single-link capture, and generic placeholders.

The product should not require the user to fully classify or research an idea before saving it.

### Preserve intent before enriching it

GiftPipe should retain what the user originally meant rather than overwrite it with later research or AI output.

A captured idea may remain valuable even when no concrete product or candidate is ever recorded.

### Human ideas outrank speculative AI suggestions

Existing user-captured or explicitly adopted ideas should be surfaced before GiftPipe automatically generates alternatives. User-supplied idea content counts as useful; recipient-only empty placeholders do not suppress generation. A due Occasion Reminder initiates the check. When no useful ideas exist, the Occasion Overview can show transient options in a separate Consider these section. When recipient context is also absent, an inline prompt lets the user add profile details or explicitly request labelled general ideas; it does not require a completed profile. Add to occasion explicitly saves a chosen idea and its occasion context; unadded options last for the current planning visit, not permanent research history. See [ADR-008](../decisions/ADR-008-keep-proactive-suggestions-transient-until-adopted.md) for this boundary.

AI fills gaps, develops an idea, evaluates options, or assists research. It should not make the user's own saved context secondary by default.

Occasion notifications should be brief and direct the user to an in-app Occasion Overview. The overview brings together current occasion plans and other eligible ideas for the Person, including previously used ideas and those associated with other Occasions, subject to recipient and archival constraints. It presents a ranked preview with access to the full list and links to separately scoped general and occasion-specific research. Opening it through a notification does not change gift-planning data. Users control pre-occasion timing through editable defaults copied into individual occasion schedules. Defaults initially supply a reminder five days before at 8 AM, and users may change or delete them; detailed controls are specified in [the reminder settings](requirements.md#fr-037--manage-occasion-reminder-defaults-and-schedules). Recurrence, detailed timezone/delivery policies, delivery channels, exact ranking rules, the preview count, and remaining overview presentation details remain separate discovery topics; see [the reminder requirements](requirements.md#fr-021--remind-users-about-approaching-occasions).

### AI should be bounded to gifting work

GiftPipe should expose purposeful AI actions such as enhancing an idea, generating candidates, comparing options, evaluating fit, or summarizing research.

The product should not become a general-purpose assistant for unrelated requests.

### Specificity should not create unnecessary friction

GiftPipe should not force users to decide whether text is sufficiently broad to be an idea or sufficiently concrete to be a candidate.

A GiftIdea can be vague or highly specific. Structure should follow the user's context and actions rather than arbitrary linguistic classification.

### Research should preserve useful history

Rejected options, prior research, and previous gifting outcomes can remain useful later. The product should favor preserving meaningful context rather than silently discarding it.

### Occasion outcomes should be easy to reconcile

The system should help the user close the loop after an occasion through a focused, lightweight interaction rather than expecting ongoing manual record keeping.

### Proactive behavior should be restrained

Reminders and proactive AI can be valuable, but they should be contained. GiftPipe should avoid excessive messages and unnecessary AI generation.

---

## 8. High-level product scope

### Established scope

GiftPipe's established product scope includes:

- private individual user accounts,
- Person profiles for friends, family members, and other gift recipients,
- structured and free-form recipient context,
- quick capture of GiftIdeas,
- capture of vague or concrete ideas,
- optional association of GiftIdeas with Occasions,
- recurring and one-off gift occasions at a conceptual level,
- general gift research that can persist across occasions,
- occasion-specific research,
- concrete gift candidates,
- supporting links and explicitly identified product links,
- multiple ideas for the same occasion,
- multiple gifts for the same occasion,
- reminders before relevant occasions,
- contextual AI assistance for gifting tasks,
- proactive AI suggestions when an approaching occasion lacks useful manual ideas,
- persistent AI-generated research,
- post-occasion reconciliation,
- optional feedback about how a gift was received,
- accumulated gifting history.

The detailed semantics of several of these capabilities remain in discovery and are tracked separately rather than resolved in this vision.

### Potential or evolving scope

The following directions have been discussed but are not yet fully committed or defined:

- richer mechanisms for moving useful occasion-specific research into reusable general research,
- automatically ranking which bounded AI actions are most relevant in a given context,
- free-text AI interaction within tightly constrained gifting safeguards,
- richer research summarization as histories become large,
- deeper handling of candidate reuse across occasions.

These are not established requirements.

### Out of scope / non-goals

The following have been explicitly excluded or deferred from the currently intended product direction:

- native real-time commerce search,
- browsing retailer catalogs from within GiftPipe,
- live price comparison or stock checking as a core GiftPipe capability,
- unrestricted general-purpose AI chat,
- requiring recipients to participate in GiftPipe,
- collaborative household or shared-profile behavior in the initial product model.

Users may still save links to actual products from external sites.

Native mobile applications are not required for the initial product direction; the established usage principle is phone-first rather than native-first.

---

## 9. Constraints and assumptions

### Confirmed constraints

**Phone-first usage context.** Gift ideas often occur away from a desk, so the product should support fast mobile capture as a primary usage pattern.

**Private individual account model initially.** All account data is private to its user, including recipient context, ideas, candidates, research, occasions, and gifting history. No data sharing of any kind between accounts is allowed in the current product scope. Recipients do not need accounts.

**Recipient is central to capture.** A captured GiftIdea belongs to a Person. The product is not intended as a generic bookmark or research manager.

**No native commerce layer.** GiftPipe may store external product links but does not initially act as a shopping search or product-comparison service.

**AI cost and attention matter.** Automatic AI generation should be constrained. Proactive recommendations are intended to fill genuine gaps, not run continuously.

**User-entered recipient context remains user-controlled.** AI and gift outcomes may inform recommendations, but they should not silently rewrite the explicit PersonProfile.

### Assumptions requiring validation

The current vision assumes that users will find enough value in capturing ideas ahead of time to build an ongoing habit.

It also assumes that preserving gifting context and resurfacing it later is more useful than relying only on last-minute recommendation generation.

These assumptions motivate the product but have not yet been validated through formal user research.

---

## 10. Success criteria

### Explicitly established success direction

GiftPipe would be meaningfully successful if real users use it as an ongoing place to capture gift thoughts and later benefit from those ideas being resurfaced around relevant occasions.

The product should demonstrate that quick capture is easy enough to use when an idea occurs, previously captured information is useful when an occasion approaches, users can develop an idea without losing the original thought, proactive reminders help rather than annoy, reconciliation is lightweight enough to preserve useful history, and contextual AI adds value without becoming the primary reason the product exists.

No quantitative adoption, retention, revenue, or usage targets have been agreed.

### Potential indicators requiring validation

Future product validation could examine whether users:

- return to previously captured GiftIdeas,
- use resurfaced ideas when choosing gifts,
- voluntarily enrich ideas through research,
- complete post-occasion reconciliation,
- find contextual AI suggestions more useful than starting from a blank prompt,
- and continue using GiftPipe across multiple occasions.

These are possible indicators, not approved KPIs.

---

## 11. Open strategic questions

Most currently tracked open questions concern detailed domain semantics rather than the overall product vision.

Two active questions have direct product-behavior implications:

- **OQ-022 — How should reminder-triggered suggestions be prepared and handed to the Occasion Overview?** The usefulness boundary is now resolved under OQ-014; exact generation execution and temporary handoff behavior remain open.
- **OQ-015 — What should control post-occasion reconciliation follow-up frequency and stopping conditions?** This affects how proactive GiftPipe can be without becoming intrusive.

The current domain register also contains unresolved questions about candidate identity, occasion occurrence identity, and reconciliation state. These should remain in [the open questions register](../discovery/open-questions.md) rather than being independently resolved here.

No unresolved question currently changes the central product vision that GiftPipe is a capture-first, occasion-aware gifting memory with contextual, bounded AI assistance.

---

## 12. Relationship to other documents

The [Documentation Guide](../README.md) defines the overall documentation structure and ownership principles.

The [Glossary](glossary.md) owns canonical GiftPipe vocabulary and terminology distinctions.

**Generation provenance:** At the time this vision was generated, a repository copy of the glossary was not available for direct inspection; the glossary produced earlier in discovery was used as contextual reference.

The [Domain Model](../domain/domain-model.md) owns the conceptual relationships, invariants, lifecycle behavior, and domain processes underlying the product.

The [Requirements](requirements.md) own detailed functional and non-functional expectations and acceptance criteria. This vision should not substitute for those requirements.

The [Open Questions Register](../discovery/open-questions.md) is the authoritative location for unresolved decisions. This document should reference those questions rather than maintain an independent backlog.

Decision records should preserve the rationale and consequences of significant product, domain, or architectural choices when such records become warranted.

Changes to the established product purpose, scope, or principles should trigger review of this vision and any affected glossary, domain, requirements, question, or decision documentation.
