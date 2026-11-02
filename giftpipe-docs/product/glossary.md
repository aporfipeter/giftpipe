# GiftPipe Domain Glossary

## Introduction

This glossary defines the shared domain language currently established during GiftPipe product discovery. It is intended to be used consistently across product documentation, architecture discussions, implementation work, and AI coding-agent instructions.

The glossary describes **domain meaning**, not database schemas or UI implementation. Where discovery has not yet produced a firm conclusion, the relevant concept is marked **Tentative** or **Unresolved** rather than silently completing the design.

Later explicit discovery decisions supersede earlier proposals.

---

# Domain terminology

## People and recipient context

### Person

**Definition:**  
A person for whom the GiftPipe user captures gift ideas, performs gift research, tracks occasions, and records gifting history. A Person does not need to be a GiftPipe user.

**Relationships:**  
A Person owns or is associated with a [PersonProfile](#personprofile), [GiftIdeas](#giftidea), and [Occasions](#occasion).

**Important distinctions:**  
A Person is the recipient/giftee being planned for, not the GiftPipe account holder performing the planning.

**Status:** Confirmed

---

### PersonProfile

**Definition:**  
The user-maintained description of a Person that provides durable contextual information useful for gifting. It may contain both structured information, such as interests or dislikes, and free-form notes.

**Relationships:**  
Belongs to a [Person](#person) and may be used as context by GiftPipe's AI capabilities.

**Important distinctions:**  
The PersonProfile represents information explicitly maintained by the user. Gift outcomes, AI inferences, and observed gifting patterns must not silently modify it.

**Status:** Confirmed

---

### Recipient

**Definition:**  
A role played by a [Person](#person) when a gift is being considered for them. Recipient was frequently used during discovery to describe the required Person association when capturing a GiftIdea.

**Status:** Confirmed

---

### Gift History

**Definition:**  
The accumulated record of gifts considered and actually given to a Person across occasions, including known concrete gifts and optional feedback about how successful they were.

**Relationships:**  
Derived primarily from resolved [OccasionGiftIdeas](#occasiongiftidea), their [OccasionGiftCandidates](#occasiongiftcandidate), and recorded outcomes.

**Important distinctions:**  
Gift History is observational history. It is not the same thing as the explicitly maintained [PersonProfile](#personprofile).

**Status:** Confirmed

---

## Gift ideas and durable research

### GiftIdea

**Definition:**  
A durable, open-ended gifting idea associated with a Person. It may represent a broad topic or interest such as "Whisky", a more focused intent such as "Japanese whisky", or an already concrete idea such as "Metallica tickets".

A GiftIdea can exist independently of an Occasion and can accumulate research over time.

It may also be captured with only a Person assigned and no idea content. A system-supplied placeholder title identifies that record without implying user-entered gifting intent. Title assignment behavior belongs to [the capture requirements](requirements.md#fr-004--capture-a-giftidea-with-minimal-required-information).

**Relationships:**  
Belongs to a [Person](#person). It may have reusable [GiftCandidates](#giftcandidate), research material, and one or more [OccasionGiftIdeas](#occasiongiftidea) connecting it to specific occasions.

**Important distinctions:**  
A GiftIdea is not required to be vague. Its degree of specificity does **not** determine whether it is a GiftIdea or GiftCandidate.

A GiftIdea itself has no intrinsic "given" outcome. What happened when it was considered for a particular Occasion belongs to the corresponding [OccasionGiftIdea](#occasiongiftidea).

**Status:** Confirmed

---

### GiftIdea Specificity

**Definition:**  
The degree to which a GiftIdea describes a broad concept versus a concrete gift. Specificity is informational rather than structural.

Examples may include:

- "Whisky"
- "Japanese whisky"
- "Nikka From the Barrel"
- "Metallica tickets"

All can validly be GiftIdeas.

**Important distinctions:**  
GiftPipe must not infer that sufficiently specific text automatically becomes a [GiftCandidate](#giftcandidate). Candidate status is determined by research context and user/system actions, not by linguistic specificity.

**Status:** Confirmed

---

### GiftCandidate

**Definition:**  
A concrete possible realization of an existing [GiftIdea](#giftidea) that has been explicitly represented during research.

For example, under the GiftIdea "AI book", "Co-Intelligence" may be represented as a GiftCandidate.

**Relationships:**  
Exists within the research context of a GiftIdea. It may be nominated for a specific Occasion through an [OccasionGiftCandidate](#occasiongiftcandidate).

**Important distinctions:**  
A GiftCandidate is **not simply a more specific GiftIdea**. The same phrase could be a GiftIdea when independently captured or a GiftCandidate when created while researching another GiftIdea.

A GiftIdea does not need any GiftCandidates to be useful, selected for an Occasion, or eventually recorded as given.

**Status:** Confirmed

---

### Candidate Nomination

**Definition:**  
The act of making an existing GiftCandidate relevant to a particular [OccasionGiftIdea](#occasiongiftidea).

**Relationships:**  
Nomination creates or establishes an [OccasionGiftCandidate](#occasiongiftcandidate) connecting the Candidate to that occasion-specific gifting context.

**Important distinctions:**  
A GiftCandidate may exist as reusable GiftIdea research without being nominated for every Occasion in which the GiftIdea is considered.

**Status:** Confirmed

---

### Research Node

**Definition:**  
A piece of structured research attached within GiftIdea or occasion-specific research. Research Nodes may represent concepts such as notes, links, product links, AI-generated research, or GiftCandidates and may support parent-child relationships.

**Relationships:**  
May belong to general GiftIdea research or occasion-specific research associated with an [OccasionGiftIdea](#occasiongiftidea).

**Important distinctions:**  
The exact Research Node taxonomy and lifecycle model have deliberately been deferred. The domain requires hierarchical research, but the final set of node types is not yet established.

**Status:** Tentative

---

### Research Tree

**Definition:**  
The hierarchical structure formed by research artifacts and their parent-child relationships. It preserves how research, alternatives, notes, links, and AI outputs relate to one another.

**Relationships:**  
Research may exist under a [GiftIdea](#giftidea) or an [OccasionGiftIdea](#occasiongiftidea).

**Important distinctions:**  
The tree is currently a domain model for relationships between research artifacts. GiftPipe is not intended to expose a generic free-form tree editor to users.

**Status:** Confirmed

---

### General GiftIdea Research

**Definition:**  
Research belonging to a GiftIdea independently of any specific Occasion. It represents reusable knowledge about the gifting idea.

For example, research into good Japanese whisky options for Dad can remain useful across multiple birthdays or holidays.

**Relationships:**  
Belongs to a [GiftIdea](#giftidea) and may supply GiftCandidates that can later be nominated for different OccasionGiftIdeas.

**Important distinctions:**  
General research is intentionally separate from research that only makes sense for a particular Occasion.

**Status:** Confirmed

---

### Occasion-Specific Research

**Definition:**  
Research performed specifically in the context of one [OccasionGiftIdea](#occasiongiftidea), such as an occasion-specific budget, theme, candidate, or constraint.

**Relationships:**  
Belongs to an OccasionGiftIdea rather than placing an `occasionId` directly on arbitrary general research nodes.

**Important distinctions:**  
General GiftIdea research is reusable across occasions; occasion-specific research describes the application of that GiftIdea to one particular Occasion.

The GiftIdea page exposes the OccasionGiftIdea contexts, including withdrawn ones, so their research can be reviewed. General research has a separate section or link. Research stays under its original context when an idea is removed from an Occasion's plan; browsing it does not promote it to general research. How information moves between the two scopes remains deferred under `OQ-008`.

**Status:** Confirmed

---

### Research Thread

**Definition:**  
A proposed conceptual container for a coherent body of hierarchical research.

**Relationships:**  
A Research Thread could potentially be owned by either a GiftIdea or OccasionGiftIdea and contain Research Nodes.

**Important distinctions:**  
The term is currently useful for reasoning about research scope and possible future operations such as promoting useful occasion research into general knowledge. It has **not** been confirmed as a required domain entity.

**Status:** Tentative

---

### LinkNode

**Definition:**  
A research artifact representing a supporting URL that is not being asserted as a specific product to give. Examples include articles, reviews, inspiration pages, or other reference material.

**Relationships:**  
May appear within GiftIdea or occasion-specific research and may have related notes or AI research.

**Important distinctions:**  
A LinkNode represents supporting material rather than a concrete product candidate.

**Status:** Confirmed

---

### ProductLinkNode

**Definition:**  
A research artifact representing a link that the user identifies as a specific product or giftable item.

**Relationships:**  
May be associated with a GiftCandidate and can support multiple product/retailer links for the same Candidate.

**Important distinctions:**  
A ProductLinkNode is not itself necessarily a GiftCandidate. The user provides the semantic distinction between "product" and "reference link" rather than GiftPipe having to reliably infer the page type.

**Status:** Confirmed

---

### Archived GiftIdea

**Definition:**  
A GiftIdea that should no longer be proactively considered or surfaced for future gifting while retaining its historical research and occasion history.

**Important distinctions:**  
Archiving is different from an Occasion outcome such as "given" or "not given". Giving a gift once does not automatically archive its GiftIdea.

Routine product usage should favor archival over destructive deletion. Permanent deletion may still be required for explicit data-management purposes.

**Status:** Confirmed

---

## Occasions and occasion planning

### Occasion

**Definition:**  
An event for which gifts may be considered. Occasions may be recurring, such as birthdays or anniversaries, or one-off events such as graduations or weddings, and may use custom user-defined labels.

**Relationships:**  
Belongs to or concerns a [Person](#person) and can have multiple [OccasionGiftIdeas](#occasiongiftidea).

**Important distinctions:**  
Named occasion types such as "birthday" are useful presets rather than assumptions that should rigidly define the domain model.

**Status:** Confirmed

---

### OccasionGiftIdea

**Definition:**  
The occasion-specific application of a [GiftIdea](#giftidea). It records that a particular GiftIdea was considered for a particular Occasion and preserves what ultimately happened.

For example:

- GiftIdea: "Whisky"
- Occasion: "Dad's Birthday 2026"
- OccasionGiftIdea: "Whisky considered for Dad's Birthday 2026"

**Relationships:**  
Connects one GiftIdea to one Occasion. It may contain occasion-specific research, [OccasionGiftCandidates](#occasiongiftcandidate), and an eventual [Occasion Outcome](#occasion-outcome).

**Important distinctions:**  
An OccasionGiftIdea is more than a purely technical join: it represents a meaningful domain event/context with its own research and outcome.

The historical relationship to the Occasion remains even when the idea was not given. An unused idea is not automatically moved to another Occasion.

The lifecycle state indicates whether the GiftIdea is currently included in the Occasion's plan. Remove changes that participation without deleting the context or its research, references, candidates, or outcomes. Add again restores the same context. Withdrawal does not establish a not-given outcome. The context remains visible and reachable from the GiftIdea page. Exact state names and representation remain unresolved under `OQ-003`.

**Status:** Confirmed

---

### OccasionGiftCandidate

**Definition:**  
A concrete candidate that is relevant within a particular [OccasionGiftIdea](#occasiongiftidea).

An OccasionGiftCandidate may arise by nominating an existing reusable [GiftCandidate](#giftcandidate) or by introducing an occasion-specific concrete option while researching that OccasionGiftIdea.

**Relationships:**  
Belongs to an OccasionGiftIdea. It may reference a reusable GiftCandidate.

**Important distinctions:**  
GiftCandidate describes a reusable concrete option within general GiftIdea research. OccasionGiftCandidate describes the role of a concrete option within one particular Occasion.

The exact identity model for occasion-only candidates remains unresolved: discovery has not yet decided whether an occasion-only option is a standalone OccasionGiftCandidate or whether every OccasionGiftCandidate must ultimately reference a common GiftCandidate entity.

**Status:** Confirmed, with an unresolved relationship detail

---

### Candidate Selection

**Definition:**  
The act of indicating that one or more OccasionGiftCandidates are currently intended to be given for a particular OccasionGiftIdea.

**Relationships:**  
Applies within the OccasionGiftIdea context rather than being a global property of the reusable GiftCandidate.

**Important distinctions:**  
Selection represents pre-occasion intention, not the historical fact that the gift was actually given. Multiple candidates may be selected because giving multiple gifts for the same Occasion is valid.

The final lifecycle/state representation has not yet been finalized.

**Status:** Tentative

---

### Multiple-Gift Occasion

**Definition:**  
An Occasion for which the user may give more than one gift, including multiple gifts originating from the same or different OccasionGiftIdeas.

**Relationships:**  
Reconciliation must therefore support multiple resolved gifts rather than assuming exactly one winner for an Occasion.

**Status:** Confirmed

---

### Occasion Outcome

**Definition:**  
The resolved result of an OccasionGiftIdea after the Occasion has occurred, indicating what happened when that GiftIdea was considered.

Known required meanings include at least the distinction between an idea being used/given and not being used/given.

**Relationships:**  
Belongs to OccasionGiftIdea and is established or confirmed during [Reconciliation](#reconciliation).

**Important distinctions:**  
The exact canonical state names and complete lifecycle have not yet been finalized.

An OccasionGiftIdea can validly resolve as given even when no GiftCandidate or OccasionGiftCandidate was ever recorded.

**Status:** Tentative

---

## Reconciliation and gifting outcomes

### Reconciliation

**Definition:**  
The post-occasion process through which GiftPipe invites the user to record what actually happened, including gifts given or an explicit report that no gift was given, and resolves relevant planning outcomes where they exist.

Reconciliation is intended to reduce manual bookkeeping and preserve accurate gifting history.

**Relationships:**  
Operates at Occasion level, across its OccasionGiftIdeas and OccasionGiftCandidates where they exist, and contributes to Gift History. It may begin when the Occasion has no associated ideas.

**Important distinctions:**  
Reconciliation is deterministic from known GiftPipe structure rather than dependent on unrestricted AI interpretation.

It must support multiple gifts per Occasion.

An explicit report that no gift was given is a known outcome. Declining to answer or leaving a follow-up unanswered does not establish a no-gift outcome; any unreported outcome remains unknown. Exact lifecycle representation remains open; see [the reconciliation rules](../domain/domain-model.md#dm-r022--reconciliation-supports-unplanned-occasions-and-explicit-no-gift-outcomes).

**Status:** Confirmed

---

### Occasion-Level Reconciliation

**Definition:**  
A focused reconciliation interaction covering the Occasion as a whole rather than asking separate unrelated questions about every gift idea.

GiftPipe can present the relevant GiftIdeas first, with nominated candidates beneath them when candidates exist.

**Relationships:**  
Uses all relevant open [OccasionGiftIdeas](#occasiongiftidea) for an Occasion where they exist. Zero associated ideas is a valid starting point; the user can add the first previously uncaptured gift or explicitly report that no gift was given.

**Status:** Confirmed

---

### Unspecified Gift Outcome

**Definition:**  
A valid resolved case in which the user confirms that a GiftIdea was used for an Occasion but does not record a concrete Candidate.

For example, the user may have captured "Whisky", bought a bottle offline, and later confirm only that "Whisky" was given.

**Relationships:**  
Produces a resolved OccasionGiftIdea without requiring a fulfilled GiftCandidate.

**Important distinctions:**  
GiftPipe must not force the user to retroactively create a concrete Candidate merely to complete reconciliation.

**Status:** Confirmed

---

### Previously Uncaptured Gift

**Definition:**  
A gift reported during reconciliation that was not previously represented by an OccasionGiftIdea.

**Relationships:**  
Recording it creates a new GiftIdea and corresponding OccasionGiftIdea so that the gift becomes part of the Person's history.

**Important distinctions:**  
GiftPipe must not infer a separate broad GiftIdea and concrete Candidate merely from the wording's apparent specificity. For example, "Metallica tickets" can simply become the new GiftIdea.

A Candidate is created only when the user/system is explicitly operating within a candidate-research context.

**Status:** Confirmed

---

### Gift Success Feedback

**Definition:**  
Optional post-gift feedback describing how well a given gift was received, using a deliberately lightweight assessment such as "Great", "Okay", or "Poor".

**Relationships:**  
Forms part of Gift History and may later inform AI recommendations.

**Important distinctions:**  
Success feedback must not automatically modify the PersonProfile.

**Status:** Confirmed

---

## Research state and derived knowledge

### Raw Research History

**Definition:**  
The complete preserved research structure for a GiftIdea or OccasionGiftIdea, including active, rejected, superseded, or otherwise historically relevant research.

**Relationships:**  
Forms the authoritative history from which summaries or current-state representations may be derived.

**Important distinctions:**  
Rejected candidates remain part of Raw Research History because rejection reasons are valuable context for future research and AI recommendations.

**Status:** Confirmed

---

### Current Research State

**Definition:**  
A compact representation of the presently relevant conclusions from a larger research history, such as current constraints, viable candidates, rejected options, and important recent notes.

**Relationships:**  
Derived from Raw Research History and useful as AI context.

**Important distinctions:**  
Current Research State is derived information rather than the source of truth. GiftPipe should not need to send an indefinitely growing raw research tree to AI for every operation.

The exact persistence and refresh mechanism remains deferred.

**Status:** Confirmed

---

### Research Summary

**Definition:**  
An AI- or system-generated compact summary of a GiftIdea or research context intended to make large research histories easier to consume or provide to later AI operations.

**Relationships:**  
Derived from Raw Research History and/or Current Research State.

**Important distinctions:**  
A Research Summary is disposable derived information. It does not replace the underlying research as authoritative history.

The exact rules governing when summaries are generated or refreshed remain undecided.

**Status:** Tentative

---

### Rejected Candidate

**Definition:**  
A GiftCandidate or occasion-specific candidate that has been ruled out while retaining the reason or context for that decision.

**Relationships:**  
Remains part of the relevant research history and can inform later summaries and AI recommendations.

**Important distinctions:**  
Rejection should not delete the Candidate or erase its historical value.

The exact lifecycle-state vocabulary remains deferred.

**Status:** Confirmed

---

## AI-assisted gift research

### Bounded AI

**Definition:**  
GiftPipe's approach to AI in which the system exposes explicit gift-related capabilities rather than providing an unrestricted general-purpose assistant.

Examples discussed include:

- enhance an idea,
- generate candidates,
- find alternatives,
- evaluate fit,
- compare candidates,
- summarize research,
- suggest gifts for an Occasion.

**Relationships:**  
Bounded AI operates on Person, GiftIdea, OccasionGiftIdea, Candidate, and research context as appropriate.

**Important distinctions:**  
GiftPipe is not intended to provide an unrestricted chatbot for unrelated requests.

The application determines which capabilities exist; AI must not invent arbitrary new capabilities at runtime.

**Status:** Confirmed

---

### AI Action

**Definition:**  
A specific bounded AI capability that performs one gift-related research task using an explicitly defined purpose and context.

**Relationships:**  
May operate on different research objects depending on the action and may create persistent research outputs.

**Important distinctions:**  
The exact implementation model for AI Actions has deliberately been deferred.

AI-driven ranking of which actions to surface is explicitly outside the initial MVP.

**Status:** Confirmed at the domain level; implementation unresolved

---

### AI Research Output

**Definition:**  
A persistent research result created by a bounded AI operation, such as generated candidates, an evaluation, comparison, enhancement, or summary.

**Relationships:**  
Becomes part of the relevant GiftIdea or OccasionGiftIdea research history and may itself be further researched.

**Important distinctions:**  
Useful AI research is not treated merely as transient chat text.

**Status:** Confirmed

---

### Contextual Gift Recommendation

**Definition:**  
A gift recommendation generated using available Person context, previous gifting history, and relevant existing GiftIdea or Occasion research rather than relying only on a generic prompt.

**Relationships:**  
May be requested manually or proactively generated when an Occasion approaches and the user has no useful manual ideas recorded.

**Important distinctions:**  
User-captured ideas take priority over speculative AI recommendations. If suitable manual ideas already exist, proactive AI generation should not automatically compete with them.

**Status:** Confirmed

---

### AI Enhancement

**Definition:**  
A bounded AI action that develops an existing user-captured GiftIdea or research item while remaining scoped to that original intent.

For example, enhancing "Dad wants an AI book" may produce several concrete book candidates.

**Important distinctions:**  
AI Enhancement differs from broad gift suggestion: it researches the captured idea rather than replacing it with unrelated categories.

**Status:** Confirmed

---

## Reminders and proactive behavior

### Reminder Defaults

**Definition:**
Up to three user-configured reminder templates copied into newly created Occasions. The initial configuration contains one reminder five days before at 8 AM, using the account timezone, which initially comes from the device. The user can edit or delete defaults and change the default delivery time.

**Important distinctions:**
These are templates for new Occasions, not shared reminders that continue to control existing schedules. A copied reminder belongs to its Occasion and counts toward that Occasion's five-entry limit. Exact duplicate offset/time templates merge without using another slot. There is no disabled state in the MVP.

**Status:** Confirmed behavior; technical representation unresolved

---

### Occasion Reminder Schedule

**Definition:**
An Occasion's own editable collection of up to five reminders, including copied defaults and manual entries. Each entry has a day offset from the Occasion and a local delivery time in the account timezone.

**Important distinctions:**
The saved schedule is distinct from an [Occasion Reminder](#occasion-reminder) notification. Exact date-and-time duplicates merge into one entry; different times on the same day are permitted. Editing or deleting an occasion entry does not edit its originating default. Date moves retain offsets and local times, with confirmation before deleting reminders that would become past due. Defaults that have expired at creation are skipped without fallback scheduling; having no scheduled reminder prompts an informational warning, including when no defaults were configured. See [FR-037](requirements.md#fr-037--manage-occasion-reminder-defaults-and-schedules) for the editor and warning behavior.

Recurrence, detailed timezone changes, and delivery/retry behavior remain unresolved. This does not specify persistence entities or delivery-history retention.

**Status:** Confirmed behavior; technical representation unresolved

---

### Occasion Reminder

**Definition:**  
A brief proactive notification informing the user that an Occasion is approaching and linking to its in-app [Occasion Overview](#occasion-overview).

**Relationships:**  
Uses Occasion information. The linked overview presents current planning contexts and the Person's other eligible ideas, including ideas used before or associated with other Occasions, subject to recipient and archival constraints.

**Important distinctions:**
Its pre-occasion scheduling follows the Occasion's [reminder schedule](#occasion-reminder-schedule), configured through the bell presets/calendar and date/time controls in [FR-037](requirements.md#fr-037--manage-occasion-reminder-defaults-and-schedules). The notification layer is intended to remain thin and largely platform agnostic. Email may eventually include more information than other channels, but email-only initial delivery and channel-specific content limits are not established decisions. Opening the linked overview does not change gift-planning data.

**Status:** Confirmed

---

### Occasion Overview

**Definition:**
The in-app view of an Occasion's planning context, accessible through normal app navigation and from an Occasion Reminder.

**Relationships:**
Brings together Occasion information, ideas and candidates currently in the plan, other eligible ideas for the Person, and previous gifting context. Prior use or association with another Occasion does not exclude an otherwise eligible idea. Business rules rank available ideas; a preview shows the first x, and View all opens the full eligible list in a modal. Links to detailed research rather than incorporating the full research workflow.

**Important distinctions:**
There is one Occasion Overview rather than a separate Reminder Screen. It may support preparation before an Occasion and access to reconciliation and history afterward. Rounded + / − controls on the overview and full-list modal provide quick planning changes. Opening an idea retains the Occasion context for its Add/Remove action. The ranking rules, preview count, and remaining presentation details remain open under `OQ-020`. The GiftIdea page provides separate access to general research and each occasion context's research. A [Research Summary](#research-summary) is derived research information, not the name of this screen.

Displaying an idea in the overview does not create an OccasionGiftIdea or change its outcome. Explicit removal changes planning participation while preserving the context and research; re-addition restores that same context (`DM-R023`).

**Status:** Confirmed

---

### AI-Generated Occasion Suggestion

**Definition:**  
A contextual recommendation generated proactively when an important Occasion approaches and no sufficiently useful user-captured gift idea is available.

**Important distinctions:**  
Automatic generation is intended to be constrained both to manage AI cost and to avoid excessive user messaging.

If manual GiftIdeas already provide useful options, GiftPipe should surface those first and make further AI generation user-initiated.

**Status:** Confirmed

---

### Post-Occasion Follow-Up

**Definition:**  
A proactive prompt after an Occasion that invites the user to perform Reconciliation and optionally provide Gift Success Feedback.

**Relationships:**  
Feeds Gift History and resolves OccasionGiftIdeas.

**Important distinctions:**
The invitation must not imply an obligation to use GiftPipe or call out missing app activity. It can invite reconciliation whether or not the user planned in the app. For example, "Would you like to record what you gave for Dad's Birthday?" illustrates the intended tone; it is not fixed final copy. See [FR-034](requirements.md#fr-034--follow-up-on-unresolved-post-occasion-outcomes) for notification behavior and its unresolved scheduling boundary.

**Status:** Confirmed

---

# Terminology aliases

| Alias or earlier term | Canonical term | Notes |
|---|---|---|
| Candidate | GiftCandidate | "Candidate" was widely used as shorthand during discovery. |
| Root idea | GiftIdea | Used when contrasting the durable GiftIdea with its descendant research. |
| Gift intent | GiftIdea | Describes the role of the GiftIdea; not established as a separate entity. |
| Person / giftee / recipient | Person | "Recipient" and "giftee" describe the Person's role in gifting contexts. |
| General research | General GiftIdea Research | Canonical phrasing used here for research reusable across occasions. |
| Occasion research | Occasion-Specific Research | Research scoped to one OccasionGiftIdea. |
| Product link | ProductLinkNode | User-indicated URL representing a specific product/giftable item. |
| Reference link | LinkNode | User-indicated supporting material such as an article or review. |
| OGI | OccasionGiftIdea | "OGI" was used informally in later discovery discussion to mean OccasionGiftIdea. |
| Original Gift Idea | **Not established** | The conversation does not establish "Original Gift Idea" as a canonical construct. It should not be assumed to be synonymous with OGI without a future explicit decision. |

---

# Open terminology questions

These are contextual summaries of terminology uncertainty. The linked entries in the [Open Questions Register](../discovery/open-questions.md) own material unresolved questions, alternatives, and status. Minor details below remain local until they meet the register's inclusion criteria; they are not approved decisions.

## OccasionGiftCandidate identity

The domain meaning is established, but whether an OccasionGiftCandidate must reference a reusable GiftCandidate or may exist only within its OccasionGiftIdea remains open under [OQ-001](../discovery/open-questions.md#oq-001--can-an-occasiongiftcandidate-exist-without-an-underlying-reusable-giftcandidate).

---

## Candidate lifecycle vocabulary

Newly added, shortlisted, and rejected are discussed concepts; canonical state names remain open under [OQ-002](../discovery/open-questions.md#oq-002--what-lifecycle-states-belong-to-a-reusable-giftcandidate). The earlier term `active` was considered too broad.

---

## Meaning and representation of selection

Selection means one or more concrete options intended for a specific Occasion, distinct from the historical outcome "given". Its exact lifecycle/state representation remains open under [OQ-003](../discovery/open-questions.md#oq-003--what-is-the-exact-outcomestate-model-for-occasiongiftidea-and-occasiongiftcandidate).

---

## OccasionGiftIdea outcome vocabulary

Used/given, not used/not given, and still unresolved are established meanings. Exact state names and additional states remain open under [OQ-003](../discovery/open-questions.md#oq-003--what-is-the-exact-outcomestate-model-for-occasiongiftidea-and-occasiongiftcandidate).

---

## Research Node taxonomy

The hierarchical research model and LinkNode/ProductLinkNode distinction are established. The complete taxonomy and valid node relationships remain open under [OQ-007](../discovery/open-questions.md#oq-007--what-is-the-minimum-stable-taxonomy-of-research-node-types).

---

## Research Thread as an entity

Research Thread is a useful conceptual grouping, not a confirmed domain entity. Its status remains open under [OQ-006](../discovery/open-questions.md#oq-006--is-researchthread-a-real-domain-concept-or-only-a-useful-conceptual-grouping).

---

## Promotion between research scopes

How useful occasion-specific research becomes reusable general GiftIdea research remains open under [OQ-008](../discovery/open-questions.md#oq-008--how-should-occasion-specific-research-become-reusable-general-giftidea-research); move, copy, reference, and summary semantics have not been selected.

---

## Product-link capture and automatic Candidate creation

ProductLinkNode is distinct from GiftCandidate. Whether product-link-only initial capture also creates a GiftCandidate remains open under [OQ-011](../discovery/open-questions.md#oq-011--when-a-new-giftidea-is-captured-from-only-a-productlinknode-should-giftpipe-also-create-a-giftcandidate).

---

## GiftIdea shape

GiftIdea's domain role and recipient-only capture are confirmed. A human-entered idea or title is not required; GiftPipe assigns a title according to the [capture requirements](requirements.md#fr-004--capture-a-giftidea-with-minimal-required-information). Other attributes remain unspecified. This entry retains that context; material questions about those attributes belong in the register when identified. Title-assignment edge cases remain open under [OQ-018](../discovery/open-questions.md#oq-018--how-should-giftidea-title-assignment-handle-failures-and-numbering).

---

## Research summary lifecycle

Raw Research History is authoritative; Current Research State and summaries are derived. Creation, caching, invalidation, and refresh rules remain open under [OQ-017](../discovery/open-questions.md#oq-017--how-should-current-research-state-and-research-summary-be-derived-and-refreshed).

---

## AI Action representation

Bounded AI and explicit AI Actions are accepted product concepts.

The exact representation of an AI Action—including prompts, schemas, handlers, applicability rules, or persistence configuration—is an architectural concern that has not yet been settled. These details remain local context until they materially affect product behavior, domain meaning, or an upcoming architectural choice; this entry is not a separate active question register.

---

## Reconciliation lifecycle details

The core reconciliation behavior is established:

- operate at Occasion level,
- allow entry with zero associated ideas,
- support multiple gifts,
- show GiftIdeas and their nominated candidates,
- allow previously uncaptured gifts to be added,
- permit a GiftIdea to resolve as given without a concrete Candidate,
- accept an explicit report that no gift was given, distinct from an unanswered follow-up.

Detailed state representation, correction/reopening, partial reconciliation, and follow-up timing remain open under [OQ-003](../discovery/open-questions.md#oq-003--what-is-the-exact-outcomestate-model-for-occasiongiftidea-and-occasiongiftcandidate), [OQ-010](../discovery/open-questions.md#oq-010--can-a-completed-reconciliation-be-reopened-or-corrected), [OQ-013](../discovery/open-questions.md#oq-013--how-should-partially-completed-reconciliation-behave), and [OQ-015](../discovery/open-questions.md#oq-015--what-should-control-post-occasion-reconciliation-follow-up-frequency-and-stopping-conditions) in [the open-questions register](../discovery/open-questions.md).
