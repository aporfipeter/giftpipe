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

Counts cover active questions only. OQ-014 is resolved below; OQ-022 tracks the remaining generation execution/handoff uncertainty.

| Category                          |  High | Medium |   Low |  Total |
| --------------------------------- | ----: | -----: | ----: | -----: |
| Domain model                      |     3 |      3 |     3 |      9 |
| Domain operations and lifecycle   |     0 |      5 |     1 |      6 |
| Requirements and user experience  |     0 |      5 |     0 |      5 |
| Architecture and technical design |     0 |      0 |     2 |      2 |
| **Total**                         | **3** | **13** | **6** | **22** |

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
- **Context:** The domain already supports several established facts: an OccasionGiftIdea can be used without a concrete candidate; multiple OccasionGiftCandidates can be intended or given; multiple gifts may be given for one Occasion; and reconciliation records actual outcomes. Reconciliation may also begin with zero associated ideas, and the user can explicitly report that no gift was given for the Occasion. An unanswered follow-up does not establish a no-gift outcome. Planning participation can be withdrawn and restored on the same OccasionGiftIdea without losing research or history; it is distinct from the actual gifting outcome. Removal alone is not a not-given outcome. The exact state vocabulary and representation of participation and outcomes, their division of responsibility between OccasionGiftIdea and OccasionGiftCandidate, behavior after completed reconciliation, and how an occasion-level no-gift report is represented remain unsettled.
- **Options considered:** Meanings equivalent to open/unresolved, used/given, not used/not given, candidate selected/intended, candidate given, and candidate rejected-for-this-occasion have appeared in discussion, but no final lifecycle has been approved. Additional OccasionGiftIdea state names such as abandoned or deferred have also been considered without being selected.
- **Impact:** Reconciliation, history, reminders, selection semantics, acceptance criteria, and future persistence design.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q003`, `DM-Q004`, `DM-Q018`; `DM-R008`, `DM-R010`, `DM-R011`, `DM-R022`, `DM-R023`.

#### OQ-004 — How should recurring Occasion definitions relate to specific occurrences?

- **Priority:** High
- **Status:** Open
- **Context:** GiftPipe supports recurring occasions such as birthdays and anniversaries as well as one-off occasions. Reconciliation and history, however, operate on a specific occurrence such as "Dad's Birthday 2026". The identity boundary between the recurring definition and an individual occurrence has not been established. The reminder defaults/schedule contract in `FR-037` does not settle when future recurring occurrences are created or when reminder defaults are copied to them.
- **Impact:** Occasion identity, reminder scheduling, OccasionGiftIdea association, history, reconciliation, and date behavior.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q013`.

#### OQ-005 — May the same GiftIdea have more than one OccasionGiftIdea for the same Occasion occurrence?

- **Priority:** Medium
- **Status:** Open
- **Context:** OccasionGiftIdea is conceptually distinguished by GiftIdea plus Occasion. Removing an idea from the plan preserves its context; re-adding it to that same Occasion restores participation on the same OccasionGiftIdea rather than creating a fresh record. This settles that re-addition path, but does not establish a general uniqueness constraint or whether other duplicate occasion contexts are permitted.
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
- **Context:** General GiftIdea research and Occasion-specific research are deliberately separate. The GiftIdea page exposes general research separately from links to individual OccasionGiftIdeas and their specific research, including withdrawn contexts. Browsing those contexts is established and does not promote or merge their research. Discovery has discussed promoting useful occasion discoveries into durable general knowledge but has not decided whether the operation should move, copy, reference, summarize, or otherwise reconcile the information.
- **Impact:** Research history integrity, cross-occasion reuse, Candidate reuse, and possible ResearchThread semantics.
- **References:** [Domain Model](../domain/domain-model.md) — `DM-Q011`; `DM-R014`.

#### OQ-023 — What recipient data belongs in Person and PersonProfile for gifting context?

- **Priority:** Medium
- **Status:** Open
- **Context:** The cold-start flow can collect free-form interest-profile details when profile, history, and idea leads provide no usable recipient context. The user requested a separate discussion of the Person data model: information beyond interests could also help provide context. Gender and relationship type were mentioned as possible examples only; neither is an approved field or inference rule.
- **Unresolved details:** Which recipient attributes are useful; whether they belong on Person or PersonProfile; optionality and editing semantics; how supplied non-interest data contributes to contextual generation when interests are empty. The schema, value sets, and interpretation of any proposed field remain undecided.
- **Impact:** Recipient capture/editing, profile shape, AI context assembly, and the boundary between an empty interest profile and absent recipient context.
- **References:** [Person](../domain/domain-model.md#41-person), [PersonProfile](../domain/domain-model.md#42-personprofile); FR-003, FR-027, FR-038.

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
- **Context:** Reconciliation records what was actually given and feeds Gift History. Users may make mistakes or learn additional detail later, but correction/reopening behavior has not been established. Planning withdrawal and re-addition do not change recorded outcomes; whether and how those planning controls are available after completed reconciliation also remains open with `OQ-003`.
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

#### OQ-015 — What should control post-occasion reconciliation follow-up frequency and stopping conditions?

- **Priority:** Medium
- **Status:** Open
- **Context:** GiftPipe should proactively invite the user after an Occasion to record what happened so outcomes do not depend on manual maintenance. The invitation is neutral, does not imply an obligation to use GiftPipe, and does not call out missing ideas or planning activity. It can apply when no ideas were associated with the Occasion. An explicit no-gift outcome is valid, while an unanswered follow-up does not establish that outcome. Follow-up must avoid excessive prompting. Exact timing, retry behavior, and stopping conditions, including how recorded outcomes affect them, remain undefined.
- **Impact:** Reconciliation completion rate, notification requirements, user experience, and lifecycle of unresolved occasions.
- **References:** [Requirements](../product/requirements.md#fr-034--follow-up-on-unresolved-post-occasion-outcomes) — `FR-034`; [Domain Model](../domain/domain-model.md#87-reconcile-occasion) — Reconciliation; `DM-R022`.

#### OQ-019 — What controls pre-occasion notification timing and settings interaction?

- **Priority:** Medium
- **Status:** Open
- **Context:** Brief Occasion Reminders link to the in-app Occasion Overview. Their timing must be discussed separately from overview content. The core settings policy is now established: up to three defaults (initially five days before at 8 AM), copied into independently editable schedules capped at five per Occasion. Exact duplicates merge; different times on one day remain separate. Reminders are active or deleted. Date moves retain offsets and local times with confirmation before deleting expired entries. Expired defaults are skipped without fallback scheduling; a zero-reminder warning also appears when the defaults list is empty. Times use an account timezone initially taken from the device and explicitly changeable. These decisions narrow this question rather than settling all operational timing behavior.
- **Unresolved details:** Effects of an explicit account-timezone change on existing reminders; daylight-saving gaps/overlaps; manual selection of past times and the exact due-now boundary; handling of already-delivered reminders when an Occasion moves; delivery failures, missed delivery, retries, and stopping behavior beyond user edits/deletion. Recurring-instance/default application is separate under `OQ-004`. No delivery history retention or scheduler architecture is selected.
- **Impact:** Notification scheduling, user control, acceptance criteria, and avoiding excessive interruption. This is distinct from the post-occasion reconciliation follow-up policy in `OQ-015`.
- **References:** [Requirements](../product/requirements.md#fr-021--remind-users-about-approaching-occasions) — `FR-021`; [Reminder settings](../product/requirements.md#fr-037--manage-occasion-reminder-defaults-and-schedules) — `FR-037`; [Domain rule](../domain/domain-model.md#dm-r024--reminder-defaults-create-independently-owned-occasion-schedules) — `DM-R024`; `OQ-004` owns recurring occurrence identity.

#### OQ-020 — What are the remaining Occasion Overview idea-pool and presentation rules?

- **Priority:** Medium
- **Status:** Open
- **Context:** The Occasion Overview is shared by ordinary navigation and notification entry. It distinguishes current plans from other available ideas for the Person. Otherwise eligible ideas remain available despite previous use, association with other Occasions, or withdrawal from this Occasion's plan. Recipient and archival constraints still apply. Empty placeholders outside the current plan without idea or recommendation content are omitted from the picker, while explicitly planned empty stubs remain visible. The separate transient Consider these section and visit/adoption behavior are established in FR-038. Business rules rank available ideas; the first x appear in a preview, and View all opens a modal containing the full eligible list. Rounded + / − controls and contextual idea-page Add/Remove actions change participation explicitly. Merely viewing ideas does not change planning data. The GiftIdea page separately exposes general research and links to each occasion context's specific research, including withdrawn contexts.
- **Unresolved details:** Exact business rules and their ordering/tie behavior; the preview count x; remaining layout, contextual labelling, transient-suggestion count/presentation, and research-workflow UI details beyond the established separate entry points. No ranking algorithm, score, configuration policy for x, or detailed research editor has been selected. Existing archival constraints remain in force; this question does not authorize resurfacing archived ideas proactively.
- **Impact:** Resurfacing usefulness, ordering, presentation, and acceptance criteria for the ranked preview and full list. Prior use no longer leaves eligibility ambiguous. FR-022 now establishes usefulness independently of the exact ranking rules; OQ-022 owns remaining generation mechanics.
- **References:** [Requirements](../product/requirements.md#fr-021--remind-users-about-approaching-occasions) — `FR-021`; [Occasion Overview](../product/glossary.md#occasion-overview); [Domain Model](../domain/domain-model.md#dm-r019--giving-a-gift-does-not-retire-the-giftidea) — `DM-R019`, `DM-R021`.

#### OQ-021 — Is initial notification delivery email-only, and what content belongs in each channel?

- **Priority:** Medium
- **Status:** Open
- **Context:** The notification layer should remain thin and largely platform agnostic, with brief notifications directing users to the Occasion Overview. Email-only initial delivery was recalled during discussion but could not be verified in the repository; it remains proposed scope. Email may permit more context than a push notification, but the content distinction is deferred.
- **Impact:** Initial-release scope, supported delivery channels, notification content, and channel-specific acceptance criteria. Mentioning a channel does not commit the product to supporting it.
- **References:** [Requirements](../product/requirements.md#fr-021--remind-users-about-approaching-occasions) — `FR-021`; [Release boundaries](../product/requirements.md#release-boundaries).

---

#### OQ-022 — How should reminder-triggered suggestions be prepared and handed to the Occasion Overview?

- **Priority:** Medium
- **Status:** Open
- **Context:** FR-022 resolves the usefulness boundary; due Occasion Reminders initiate the eligibility check. FR-023/FR-038 and DM-R025 define transient proposals, explicit adoption into durable idea/planning context, same-visit retention, and departure behavior. The cold-start interaction is also established: an immediate inline prompt offers profile editing or explicit general ideas, saved profile details start generation, cancellation returns without generation, and existing usable context bypasses the prompt. These product decisions do not select execution or temporary-state machinery; the recipient field model remains separately under `OQ-023`.
- **Unresolved details:** Exact generation timing relative to notification delivery and opening its link; temporary handoff/storage before and during the visit; late entry or changed context between reminder and opening; generation failure and list availability. No scheduler, cache, transport, or permanent suggestion archive is selected. Hard-close recovery is explicitly outside the MVP rather than an open requirement.
- **Impact:** Generation execution, notification/overview coordination, acceptance criteria for failures and late entry, and temporary-state implementation.
- **References:** [FR-023](../product/requirements.md#fr-023--proactively-generate-contextual-suggestions-when-needed), [FR-038](../product/requirements.md#fr-038--adopt-transient-suggestions-within-an-occasion-planning-visit); [ADR-008](../decisions/ADR-008-keep-proactive-suggestions-transient-until-adopted.md).

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

## Resolved question references

These entries preserve stable identifiers and links to authoritative resolutions; they are excluded from the active overview counts.

#### OQ-014 — What qualifies as having a sufficiently useful manual idea so proactive AI suggestions should not run?

- **Priority at resolution:** Medium
- **Status:** Resolved
- **Resolution:** User-supplied idea content or an explicitly adopted suggestion is sufficient across the Person's eligible idea pool. Recipient-only placeholders without idea content are insufficient, even when explicitly planned. Due Occasion Reminders initiate the check. The authoritative behavior is in [FR-022](../product/requirements.md#fr-022--avoid-unnecessary-proactive-ai-when-useful-manual-ideas-exist), DM-R025, and [ADR-008](../decisions/ADR-008-keep-proactive-suggestions-transient-until-adopted.md).
- **Remaining separate uncertainty:** OQ-022 owns execution and temporary handoff mechanics. OQ-020 retains ranking and detailed presentation; this resolution does not define those rules.

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
