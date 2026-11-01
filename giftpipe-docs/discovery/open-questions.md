# GiftPipe Open Questions Register

## 1. Purpose

This register is the authoritative location for active unresolved questions that materially affect GiftPipe's product behavior, domain model, requirements, or significant architectural choices.

It exists to preserve uncertainty explicitly and to prevent contributors or AI coding agents from treating an unanswered question as permission to choose a convenient interpretation.

The glossary and domain model may retain short contextual summaries that link to the authoritative entries here. Those summaries are not separate question registers; maintain unresolved alternatives and status in this register, and propagate any approved resolution to the owning documentation. Preserve existing local identifiers for traceability.

Questions belong here when a real decision is still required. Minor implementation details that do not affect domain meaning, user-observable behavior, or an upcoming architectural choice should remain outside this register until they become relevant.

This document should be read together with:

- [Documentation Guide](../README.md), which defines documentation ownership and maintenance principles.
- [Domain Model](../domain/domain-model.md), which records established domain structure, invariants, and unresolved modeling context.
- [Glossary](../product/glossary.md), which owns canonical vocabulary.

**Generation provenance:** At the time this register was generated, the conversation-generated [giftpipe-docs/README.md](../README.md) and [giftpipe-docs/domain/domain-model.md](../domain/domain-model.md) were available for inspection. A repository file for [giftpipe-docs/product/glossary.md](../product/glossary.md) was not available, so glossary uncertainties were cross-checked against the glossary content previously generated in the discovery conversation rather than a repository file.

Unresolved questions are **not approved requirements or implementation instructions**.

---

## 2. Overview

| Category                          |  High | Medium |   Low |  Total |
| --------------------------------- | ----: | -----: | ----: | -----: |
| Domain model                      |     3 |      2 |     3 |      8 |
| Domain operations and lifecycle   |     0 |      5 |     1 |      6 |
| Requirements and user experience  |     0 |      5 |     0 |      5 |
| Architecture and technical design |     0 |      0 |     2 |      2 |
| **Total**                         | **3** | **12** | **6** | **21** |

---

## 3. Open questions

### Domain model

#### OQ-001 — Can an OccasionGiftCandidate exist without an underlying reusable GiftCandidate?

- **Priority:** High
- **Status:** Open
- **Context:** OccasionGiftCandidate has established domain meaning, but its identity model is not settled. A candidate nominated from general GiftIdea research clearly references a reusable GiftCandidate. The unresolved case is a new candidate created directly while working inside an OccasionGiftIdea.
- **Options considered:**
  - Every OccasionGiftCandidate ultimately references a reusable GiftCandidate.
  - An OccasionGiftCandidate may exist only within its OccasionGiftIdea and may later be promoted or otherwise made reusable.
- **Impact:** GiftCandidate identity, OccasionGiftCandidate ownership, nomination semantics, reconciliation history, cross-occasion reuse, and aggregate design.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q001`, `DM-Q007`, `DM-Q008`; `DM-R009`, `DM-R010`.

#### OQ-002 — What lifecycle states belong to a reusable GiftCandidate?

- **Priority:** Medium
- **Status:** Open
- **Context:** Candidate concepts such as added, shortlisted, and rejected have been discussed. The earlier term `active` was considered too broad, and `selected` was explicitly moved out of global GiftCandidate state into the occasion context.
- **Impact:** Research behavior, candidate filtering, rejection history, and future requirements for managing candidate state.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q002`; `DM-R013`.

#### OQ-003 — What is the exact outcome/state model for OccasionGiftIdea and OccasionGiftCandidate?

- **Priority:** High
- **Status:** Open
- **Context:** The domain already supports several established facts: an OccasionGiftIdea can be used without a concrete candidate; multiple OccasionGiftCandidates can be intended or given; multiple gifts may be given for one Occasion; and reconciliation records actual outcomes. Reconciliation may also begin with zero associated ideas, and the user can explicitly report that no gift was given for the Occasion. An unanswered follow-up does not establish a no-gift outcome. The exact state vocabulary and division of responsibility between OccasionGiftIdea and OccasionGiftCandidate, including how an occasion-level no-gift report is represented, are still unsettled.
- **Options considered:** Meanings equivalent to open/unresolved, used/given, not used/not given, candidate selected/intended, candidate given, and candidate rejected-for-this-occasion have appeared in discussion, but no final lifecycle has been approved. Additional OccasionGiftIdea state names such as abandoned or deferred have also been considered without being selected.
- **Impact:** Reconciliation, history, reminders, selection semantics, acceptance criteria, and future persistence design.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q003`, `DM-Q004`, `DM-Q018`; `DM-R008`, `DM-R010`, `DM-R011`, `DM-R022`.

#### OQ-004 — How should recurring Occasion definitions relate to specific occurrences?

- **Priority:** High
- **Status:** Open
- **Context:** GiftPipe supports recurring occasions such as birthdays and anniversaries as well as one-off occasions. Reconciliation and history, however, operate on a specific occurrence such as "Dad's Birthday 2026". The identity boundary between the recurring definition and an individual occurrence has not been established.
- **Impact:** Occasion identity, reminder scheduling, OccasionGiftIdea association, history, reconciliation, and date behavior.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q013`.

#### OQ-005 — May the same GiftIdea have more than one OccasionGiftIdea for the same Occasion occurrence?

- **Priority:** Medium
- **Status:** Open
- **Context:** OccasionGiftIdea is conceptually distinguished by GiftIdea plus Occasion, but the discovery conversation has not established whether duplicate associations are valid or should collapse into one ongoing occasion context.
- **Impact:** OccasionGiftIdea identity, nomination, reconciliation, deduplication, and user-visible history.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q014`.

#### OQ-006 — Is ResearchThread a real domain concept or only a useful conceptual grouping?

- **Priority:** Low
- **Status:** Open
- **Context:** A ResearchThread was proposed as a possible container for research beneath either GiftIdea or OccasionGiftIdea and may be useful for later operations such as promoting a body of occasion research into durable knowledge. It has not been accepted as a required domain entity.
- **Impact:** Research ownership, terminology, and future domain/technical modeling.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q009`.

#### OQ-007 — What is the minimum stable taxonomy of Research Node types?

- **Priority:** Low
- **Status:** Open
- **Context:** Hierarchical research is established, and LinkNode plus ProductLinkNode are accepted distinctions. Candidates, notes, and AI outputs have also been discussed, but the complete taxonomy was intentionally deferred so it can emerge through iteration.
- **Impact:** Research semantics, bounded AI actions, future requirements, and implementation modeling.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q010`, `DM-Q019`; `DM-R017`, `DM-R018`.

#### OQ-008 — How should occasion-specific research become reusable general GiftIdea research?

- **Priority:** Low
- **Status:** Open
- **Context:** General GiftIdea research and Occasion-specific research are deliberately separate. Discovery has discussed promoting useful occasion discoveries into durable general knowledge but has not decided whether the operation should move, copy, reference, summarize, or otherwise reconcile the information.
- **Impact:** Research history integrity, cross-occasion reuse, Candidate reuse, and possible ResearchThread semantics.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q011`; `DM-R014`.

---

### Domain operations and lifecycle

#### OQ-009 — Is candidate nomination reversible, and what history should remain if a nomination is withdrawn?

- **Priority:** Medium
- **Status:** Open
- **Context:** Nomination makes a reusable GiftCandidate relevant to a particular OccasionGiftIdea. The conversation established nomination but did not establish what happens if the user later removes that candidate from consideration before reconciliation.
- **Impact:** OccasionGiftCandidate lifecycle, research history, reconciliation, and auditability of prior consideration.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q006`; `DM-R009`.

#### OQ-010 — Can a completed reconciliation be reopened or corrected?

- **Priority:** Medium
- **Status:** Open
- **Context:** Reconciliation records what was actually given and feeds Gift History. Users may make mistakes or learn additional detail later, but correction/reopening behavior has not been discussed.
- **Impact:** Historical accuracy, lifecycle reversibility, Gift Success Feedback, and acceptance criteria.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q005`.

#### OQ-011 — When a new GiftIdea is captured from only a ProductLinkNode, should GiftPipe also create a GiftCandidate?

- **Priority:** Medium
- **Status:** Open
- **Context:** The user explicitly chooses whether a URL is a product link or a supporting reference link. ProductLinkNode is not itself a GiftCandidate. What remains unsettled is the first-capture case: a user creates a new gift for Dad by pasting only a product link. Deriving a GiftIdea title from a single link is established under `FR-004`; that title assignment does not settle whether a GiftCandidate is also created.
- **Options considered:**
  - Create the GiftIdea plus ProductLinkNode only.
  - Create the GiftIdea, a GiftCandidate, and attach the ProductLinkNode beneath that candidate.
- **Impact:** Frictionless capture semantics, initial research structure, reconciliation, and candidate identity.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q012`; `DM-R003`, `DM-R018`.

#### OQ-012 — What are the archive and reactivation semantics for GiftIdea?

- **Priority:** Low
- **Status:** Open
- **Context:** Archival is the established way to exclude an idea from proactive future consideration without destroying history. Whether an archived idea can be restored, and what operations remain available while archived, have not been established. Permanent deletion must remain possible for explicit data-management purposes, as recorded in `DM-Q015`; its exact semantics, including effects on associated research and historical records, remain unresolved. This question owns that existing deletion uncertainty alongside archive and reactivation behavior.
- **Impact:** GiftIdea lifecycle, search/history behavior, future reuse, privacy/data management, and preservation of related history.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q015`, `DM-Q020`; `DM-R019`.

#### OQ-013 — How should partially completed reconciliation behave?

- **Priority:** Medium
- **Status:** Open
- **Context:** Occasion-level reconciliation may involve several OccasionGiftIdeas and multiple gifts. The user may resolve some items, add an uncaptured gift, and leave other outcomes uncertain. Reconciliation may begin with zero associated ideas, and an explicit report that no gift was given is valid. These accepted behaviors do not settle partial-completion semantics, the treatment of remaining planning items, or their state transitions. Reconciliation can be deferred, but deferral or silence is not itself a no-gift report.
- **Impact:** Occasion lifecycle, reminder/follow-up behavior, open versus resolved states, and acceptance criteria.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q004`, `DM-Q018`; Reconciliation section; `DM-R008`, `DM-R011`.

#### OQ-018 — How should GiftIdea title assignment handle failures and numbering?

- **Priority:** Medium
- **Status:** Open
- **Context:** Recipient-only capture is valid. An explicit title wins; otherwise one link supplies a derived title, while no content or multiple links use a generic placeholder such as "Gift for Dad", numbered as needed to distinguish existing placeholders. The exact numbering and collision rules, and the behavior when a single link cannot supply a usable title, have not been decided.
- **Unresolved details:** Title retrieval failure or delay; placeholder numbering sequence and collision scope. A generic fallback on link-title failure was suggested but not accepted as part of this batch.
- **Impact:** Capture acceptance criteria and generated title behavior. These details do not reopen the accepted recipient-only capture rule or title precedence.
- **References:** [Requirements](../product/requirements.md#fr-004--capture-a-giftidea-with-minimal-required-information) — `FR-004`; [Domain Model](../domain/domain-model.md#81-capture-giftidea); `OQ-011` separately owns product-link-only Candidate creation.

---

### Requirements and user experience

#### OQ-014 — What qualifies as having a sufficiently useful manual idea so proactive AI suggestions should not run?

- **Priority:** Medium
- **Status:** Open
- **Context:** The agreed product principle is to surface user-captured ideas first and generate proactive AI suggestions when an approaching occasion has no useful manual ideas. "Useful" has not yet been defined. A vague GiftIdea such as "Whisky" may be useful enough for one user but may still benefit from proactive research for another. Recipient-only GiftIdeas with generated placeholder titles are now valid; whether such a record qualifies as useful manual context is also unresolved. The Occasion Overview includes the Person's ideas with no Occasion associations; `OQ-020` tracks the remaining pool boundaries. Display eligibility alone does not settle the usefulness threshold.
- **Impact:** Reminder behavior, AI cost, user annoyance, proactive suggestion requirements, and settings.
- **References:** Product discovery; [Domain Model](../domain/domain-model.md) — bounded AI and OccasionGiftIdea concepts.

#### OQ-015 — What should control post-occasion reconciliation follow-up frequency and stopping conditions?

- **Priority:** Medium
- **Status:** Open
- **Context:** GiftPipe should proactively invite the user after an Occasion to record what happened so outcomes do not depend on manual maintenance. The invitation is neutral, does not imply an obligation to use GiftPipe, and does not call out missing ideas or planning activity. It can apply when no ideas were associated with the Occasion. An explicit no-gift outcome is valid, while an unanswered follow-up does not establish that outcome. Follow-up must avoid excessive prompting. Exact timing, retry behavior, and stopping conditions, including how recorded outcomes affect them, remain undefined.
- **Impact:** Reconciliation completion rate, notification requirements, user experience, and lifecycle of unresolved occasions.
- **References:** [Requirements](../product/requirements.md#fr-034--follow-up-on-unresolved-post-occasion-outcomes) — `FR-034`; [Domain Model](../domain/domain-model.md#87-reconcile-occasion) — Reconciliation; `DM-R022`.

#### OQ-019 — What controls pre-occasion notification timing and settings interaction?

- **Priority:** Medium
- **Status:** Open
- **Context:** Brief Occasion Reminders link to the in-app Occasion Overview. Their timing must be discussed separately from overview content. App-level and occasion-level notification settings and their interaction have been identified for exploration, but no scheduling or precedence policy is approved.
- **Unresolved details:** When an Occasion becomes eligible for notification; whether and when notifications repeat; app-level defaults and occasion-level settings; their precedence and any disabling or stopping behavior.
- **Impact:** Notification scheduling, user control, acceptance criteria, and avoiding excessive interruption. This is distinct from the post-occasion reconciliation follow-up policy in `OQ-015`.
- **References:** [Requirements](../product/requirements.md#fr-021--remind-users-about-approaching-occasions) — `FR-021`; `OQ-004` owns recurring occurrence identity.

#### OQ-020 — What are the remaining Occasion Overview idea-pool and presentation rules?

- **Priority:** Medium
- **Status:** Open
- **Context:** The Occasion Overview is shared by ordinary navigation and notification entry. It presents existing occasion plans, includes the Person's ideas with no Occasion associations, provides previous gifting context, and links to detailed research. Merely opening it does not change gift-planning data. The complete hierarchy and eligibility boundaries are not established.
- **Unresolved details:** Whether and how ideas associated with other Occasions are offered for the approaching Occasion; ordering and overlap among unassociated ideas, current research, and previously given gifts; detailed research navigation and UI. Existing archival constraints remain in force; this question does not authorize resurfacing archived ideas proactively.
- **Impact:** Resurfacing usefulness, navigation, presentation, and the idea pool evaluated by `OQ-014`. Previous gifts appearing as history does not itself decide their automatic eligibility for a new Occasion.
- **References:** [Requirements](../product/requirements.md#fr-021--remind-users-about-approaching-occasions) — `FR-021`; [Occasion Overview](../product/glossary.md#occasion-overview); [Domain Model](../domain/domain-model.md#dm-r019--giving-a-gift-does-not-retire-the-giftidea) — `DM-R019`, `DM-R021`.

#### OQ-021 — Is initial notification delivery email-only, and what content belongs in each channel?

- **Priority:** Medium
- **Status:** Open
- **Context:** The notification layer should remain thin and largely platform agnostic, with brief notifications directing users to the Occasion Overview. Email-only initial delivery was recalled during discussion but could not be verified in the repository; it remains proposed scope. Email may permit more context than a push notification, but the content distinction is deferred.
- **Impact:** Initial-release scope, supported delivery channels, notification content, and channel-specific acceptance criteria. Mentioning a channel does not commit the product to supporting it.
- **References:** [Requirements](../product/requirements.md#fr-021--remind-users-about-approaching-occasions) — `FR-021`; [Release boundaries](../product/requirements.md#release-boundaries).

---

### Architecture and technical design

#### OQ-016 — What are the actual DDD aggregate and consistency boundaries?

- **Priority:** Low
- **Status:** Blocked
- **Depends on:** OQ-001, OQ-003, and OQ-004
- **Context:** Person, GiftIdea, OccasionGiftIdea, and Occasion have all been identified as plausible aggregate or coordination boundaries, but no aggregate roots or transactional consistency rules have been established.
- **Impact:** Application-service boundaries, persistence design, transactional behavior, and implementation architecture.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q016`; Aggregate boundaries and ownership section.

#### OQ-017 — How should Current Research State and Research Summary be derived and refreshed?

- **Priority:** Low
- **Status:** Open
- **Context:** Raw Research History is authoritative, while Current Research State and summaries are derived to avoid feeding an ever-growing tree into AI. The project intentionally deferred whether summaries are generated lazily, cached, invalidated after changes, or refreshed by another policy.
- **Impact:** AI context construction, model cost, stale-summary risk, and future technical design.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q017`; research-state concepts.

---

## 4. Decision dependencies

| Question                                  | Depends on                     | Reason                                                                                                                           |
| ----------------------------------------- | ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------- |
| OQ-003 — Occasion/candidate outcome model | OQ-001 informs part of it      | Outcome representation depends partly on whether an occasion-only candidate has independent identity                             |
| OQ-008 — Promotion from occasion research | OQ-001, and potentially OQ-006 | Promotion semantics depend on what an occasion-only candidate/research container actually is                                     |
| OQ-013 — Partial reconciliation           | OQ-003                         | Partial completion needs a stable definition of resolved and unresolved OGI/OGC states                                           |
| OQ-016 — Aggregate boundaries             | OQ-001, OQ-003, OQ-004         | Ownership and consistency boundaries should follow settled identity and lifecycle semantics rather than decide them accidentally |

Other questions can be explored independently unless later discovery reveals stronger coupling.

---

## 5. Resolution process

When resolving an open question:

1. Discuss the question and evaluate the relevant alternatives.
2. Record the resulting decision in the appropriate authoritative document.
3. Create or update an ADR when the choice is significant enough that its rationale and consequences should be preserved.
4. Update affected documentation and cross-references.
5. Remove the question from this active register once the resolution has been incorporated.

Resolved questions should not remain indefinitely in the active backlog. Historical rationale belongs in decision records or the appropriate authoritative documentation.

When another document refers to the question or its resolution, preserve the stable `OQ-xxx` identifier.

---

## Guidance for AI coding agents

- Do not independently resolve High-priority domain questions while implementing features.
- An unresolved question is not authorization to choose the easiest interpretation.
- Surface a relevant open question when it materially affects the requested task.
- Low-priority questions do not automatically block unrelated work.
- Clearly distinguish a proposed resolution from a decision explicitly approved through discovery.
