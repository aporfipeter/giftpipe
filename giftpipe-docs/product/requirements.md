# GiftPipe Product Requirements

## 1. Purpose and scope

This document specifies GiftPipe's established functional and non-functional requirements.

It is the authoritative reference for **what GiftPipe is expected to do**. It is intended to support implementation planning, acceptance criteria, testing, and AI-assisted development without prescribing database structures, APIs, frameworks, UI component architecture, or other technical implementation choices.

The requirements are grounded in:

- the GiftPipe discovery conversation,
- the [Documentation Guide](../README.md),
- the [Product Vision](vision.md),
- the [Domain Model](../domain/domain-model.md),
- and the [Open Questions Register](../discovery/open-questions.md).

A repository copy of `glossary.md` was not available when this document was generated. Terminology was therefore checked against the glossary content previously produced during discovery, but this document does not claim to have inspected `glossary.md` as a repository file.

Requirements use established GiftPipe terminology and defer unresolved behavior to the open questions register rather than silently choosing an interpretation.

---

## 2. Requirements conventions

### Requirement status

Each requirement is classified as:

- **Confirmed** — explicitly accepted or unambiguously established.
- **Tentative** — discussed as intended behavior but not conclusively settled.
- **Blocked** — final behavior cannot be specified because a relevant open question remains unresolved.

A requirement's certainty status is separate from implementation status or delivery priority.

### Requirement identifiers

Functional requirements use stable identifiers `FR-001`, `FR-002`, and so on.

Non-functional requirements use stable identifiers `NFR-001`, `NFR-002`, and so on.

Identifiers should not be reused for unrelated requirements if a requirement is later removed or superseded.

### Domain references

Where relevant, requirements reference authoritative domain rules from the [Domain Model](../domain/domain-model.md) using identifiers such as `DM-R001`.

Requirements describe observable behavior or system-enforced consequences. The domain model remains authoritative for the underlying invariant.

---

# 3. Functional requirements

## 3.1 User and recipient context

### FR-001 — Support independent user accounts

**Status:** Confirmed

**Requirement:**  
GiftPipe must support independent user accounts in which each user manages their own recipient, gift, occasion, research, and gifting-history data.

**Rationale:**  
The established initial participation model is private individual use rather than shared household or collaborative planning.

**Acceptance criteria:**

- A user can maintain their own GiftPipe data independently of another user.
- A Person being planned for does not need to be a GiftPipe user.
- GiftPipe does not require recipient participation in order to plan gifts for that Person.

**References:** [Product Vision](vision.md), Established scope and constraints.

---

### FR-002 — Manage recipient Persons

**Status:** Confirmed

**Requirement:**  
GiftPipe must allow a user to represent people for whom they plan gifts.

**Rationale:**  
All GiftIdeas are Person-scoped.

**Acceptance criteria:**

- The user can create a Person.
- GiftIdeas can be associated with that Person.
- Occasions can be associated with that Person.

**References:** `DM-R001`.

---

### FR-003 — Maintain recipient profile context

**Status:** Confirmed

**Requirement:**  
GiftPipe must allow the user to maintain gifting-relevant PersonProfile information using both structured context and free-form notes.

**Rationale:**  
Recipient context is intended to support manual planning and contextual AI recommendations.

**Acceptance criteria:**

- The user can maintain explicit recipient information beyond the Person's name.
- The profile can include structured information and free-form contextual notes.
- AI-generated inferences and gift outcomes do not silently alter the user-maintained PersonProfile.

**References:** `DM-R015`.

---

## 3.2 GiftIdea capture and management

### FR-004 — Capture a GiftIdea with minimal required information

**Status:** Confirmed

**Requirement:**  
GiftPipe must allow the user to capture a GiftIdea for a Person without requiring the user to fully research, categorize, or concretize the idea first.

**Rationale:**  
Frictionless capture is the primary product loop.

**Acceptance criteria:**

- A Person is required for a GiftIdea.
- The user can save a vague idea such as "Whisky".
- The user can save a concrete idea such as "Metallica tickets".
- A GiftCandidate is not required at capture time.
- An Occasion is not required at capture time.

**References:** `DM-R001`, `DM-R002`, `DM-R003`, `DM-R005`.

---

### FR-005 — Preserve GiftIdea specificity without reclassification

**Status:** Confirmed

**Requirement:**  
GiftPipe must not reclassify a GiftIdea as a GiftCandidate merely because the captured wording is highly specific.

**Rationale:**  
GiftIdea specificity is informational rather than structural.

**Acceptance criteria:**

- Broad and concrete user-entered GiftIdeas remain valid GiftIdeas.
- Candidate creation depends on research context or explicit candidate-related behavior, not wording specificity alone.

**References:** `DM-R003`.

---

### FR-006 — Keep GiftIdeas reusable across occasions

**Status:** Confirmed

**Requirement:**  
GiftPipe must allow a GiftIdea to remain available across multiple occasions unless the user archives it.

**Rationale:**  
A GiftIdea represents durable gifting intent or interest rather than a one-time occasion record.

**Acceptance criteria:**

- A GiftIdea can exist before any Occasion association.
- A GiftIdea can be considered for more than one Occasion over time.
- Giving or not giving the GiftIdea for one Occasion does not automatically archive it.

**References:** `DM-R002`, `DM-R019`.

---

### FR-007 — Archive GiftIdeas without erasing history

**Status:** Confirmed

**Requirement:**  
GiftPipe must support archiving a GiftIdea so it is no longer proactively considered while preserving its historical research and occasion associations.

**Rationale:**  
Archived ideas should stop resurfacing without destroying useful history.

**Acceptance criteria:**

- An archived GiftIdea remains part of historical records.
- Archival does not erase previous OccasionGiftIdeas or research.
- Archival is distinct from an Occasion outcome.

**References:** `DM-R019`, `OQ-012`.

**Limitation:**  
Reactivation and detailed archive-state behavior remain unresolved under `OQ-012`.

---

## 3.3 Gift research and candidates

### FR-008 — Add reusable GiftCandidates within GiftIdea research

**Status:** Confirmed

**Requirement:**  
GiftPipe must allow the user to represent concrete gift options as GiftCandidates within the research context of an existing GiftIdea.

**Rationale:**  
A GiftCandidate represents one concrete realization of a broader or ongoing GiftIdea.

**Acceptance criteria:**

- A GiftIdea can have zero GiftCandidates.
- A GiftIdea can have multiple GiftCandidates.
- A GiftCandidate is created in the context of an existing GiftIdea.
- Adding a GiftCandidate does not automatically make it relevant to every Occasion.

**References:** `DM-R004`, `DM-R005`, `DM-R009`.

---

### FR-009 — Preserve rejected candidates as research history

**Status:** Confirmed

**Requirement:**  
GiftPipe must preserve rejected GiftCandidates and their available context rather than deleting them from research history.

**Rationale:**  
Rejected options and rejection reasons are useful for future research and AI assistance.

**Acceptance criteria:**

- Rejecting a candidate does not erase its historical existence.
- Rejected candidate information remains available as research context.

**References:** `DM-R013`.

---

### FR-010 — Support hierarchical research

**Status:** Confirmed

**Requirement:**  
GiftPipe must support research artifacts that can relate hierarchically to one another within a GiftIdea or OccasionGiftIdea research context.

**Rationale:**  
Gift research can evolve from a root idea into candidates, links, notes, and AI-generated research.

**Acceptance criteria:**

- Research can contain parent-child relationships.
- GiftCandidate research can contain supporting research beneath a candidate.
- GiftPipe is not required to expose arbitrary free-form tree editing to satisfy this requirement.

**References:** [Domain Model](../domain/domain-model.md), Research Tree concept; `OQ-007`.

---

### FR-011 — Distinguish reference links from product links

**Status:** Confirmed

**Requirement:**  
GiftPipe must allow the user to explicitly distinguish a supporting reference link from a product link.

**Rationale:**  
The product should not require universal page classification to decide whether a URL represents supporting research or a giftable product.

**Acceptance criteria:**

- The user can add a URL as a supporting reference.
- The user can add a URL as a product link.
- A supporting link is not treated as a giftable product merely because it mentions products.
- A product link is not automatically equivalent to a GiftCandidate.

**References:** `DM-R017`, `DM-R018`.

---

### FR-012 — Allow multiple product links for one candidate

**Status:** Confirmed

**Requirement:**  
GiftPipe must allow multiple ProductLinkNodes to support the same GiftCandidate.

**Rationale:**  
The same concrete gift may be represented by multiple retailer or product URLs.

**Acceptance criteria:**

- A GiftCandidate can retain more than one product link.
- Adding an additional product link does not require creating a new GiftCandidate solely because the URL differs.

**References:** [Domain Model](../domain/domain-model.md), ProductLinkNode relationships.

---

### FR-013 — Handle product-link-only initial capture

**Status:** Blocked

**Requirement:**  
GiftPipe must support creating a new GiftIdea when the user's initial capture consists of a product link, but the exact resulting candidate structure is not yet finalized.

**Acceptance criteria:**

- A product-link-only capture can result in a valid GiftIdea.
- The product link is preserved.
- Whether GiftPipe must also create a GiftCandidate cannot be finalized until `OQ-011` is resolved.

**References:** `DM-R003`, `DM-R018`, `OQ-011`.

---

## 3.4 Occasion management and association

### FR-014 — Support recurring and one-off occasions

**Status:** Confirmed

**Requirement:**  
GiftPipe must support gift-relevant Occasions that may be recurring or one-off.

**Rationale:**  
Examples discussed include birthdays, anniversaries, namedays, holidays, graduations, weddings, and custom occasions.

**Acceptance criteria:**

- The product can represent recurring Occasions.
- The product can represent one-off Occasions.
- The product is not limited to a fixed exhaustive list of occasion names.

**References:** [Product Vision](vision.md), Established scope; `OQ-004`.

**Limitation:**  
The identity relationship between a recurring Occasion definition and a concrete occurrence remains unresolved under `OQ-004`.

---

### FR-015 — Associate a GiftIdea with an Occasion

**Status:** Confirmed

**Requirement:**  
GiftPipe must allow a GiftIdea to be associated with an Occasion, creating an occasion-specific gifting context.

**Rationale:**  
A durable GiftIdea and a particular Occasion have distinct meanings.

**Acceptance criteria:**

- A GiftIdea can be associated with an Occasion after the GiftIdea already exists.
- A GiftIdea can be associated with an Occasion during initial capture.
- The association preserves the GiftIdea independently of the Occasion.
- The resulting OccasionGiftIdea can exist without any candidate.

**References:** `DM-R002`, `DM-R005`.

---

### FR-016 — Support multiple GiftIdeas for one Occasion

**Status:** Confirmed

**Requirement:**  
GiftPipe must allow multiple GiftIdeas to be considered for the same Occasion.

**Acceptance criteria:**

- One Occasion can have multiple OccasionGiftIdeas.
- Each OccasionGiftIdea retains its own outcome history after reconciliation.

**References:** `DM-R007`.

---

### FR-017 — Preserve unused OccasionGiftIdeas

**Status:** Confirmed

**Requirement:**  
When a GiftIdea is not used for an Occasion, GiftPipe must retain the OccasionGiftIdea association and record the outcome rather than removing the association.

**Rationale:**  
The historical fact that an idea was considered but not used remains useful.

**Acceptance criteria:**

- Reconciliation can record that an OccasionGiftIdea was not used.
- The historical association to that Occasion remains available afterward.
- The underlying GiftIdea remains eligible for future occasions unless archived.

**References:** `DM-R006`, `DM-R019`.

---

## 3.5 Candidate nomination and occasion-specific candidates

### FR-018 — Nominate reusable GiftCandidates for an OccasionGiftIdea

**Status:** Confirmed

**Requirement:**  
GiftPipe must allow an existing GiftCandidate from a GiftIdea to be nominated as relevant to a particular OccasionGiftIdea.

**Rationale:**  
General GiftIdea research and occasion-specific candidate relevance are distinct.

**Acceptance criteria:**

- The user can make an existing GiftCandidate relevant to one OccasionGiftIdea.
- That nomination does not automatically make the same GiftCandidate relevant to other OccasionGiftIdeas.
- Nomination preserves the relationship to the underlying GiftIdea.

**References:** `DM-R009`.

---

### FR-019 — Create a new candidate while working in an OccasionGiftIdea

**Status:** Blocked

**Requirement:**  
GiftPipe must allow the user to introduce a new concrete candidate while researching a specific OccasionGiftIdea.

**Acceptance criteria:**

- A new concrete option can be added in the occasion context.
- It becomes relevant to that OccasionGiftIdea.
- Whether it must also become a reusable GiftCandidate cannot be finalized until `OQ-001` is resolved.

**References:** `OQ-001`.

---

### FR-020 — Support multiple intended or given candidates for one OccasionGiftIdea

**Status:** Confirmed

**Requirement:**  
GiftPipe must support more than one concrete gift candidate being relevant, intended, or ultimately given for a single OccasionGiftIdea.

**Rationale:**  
Giving multiple gifts for one Occasion is explicitly supported.

**Acceptance criteria:**

- One OccasionGiftIdea can have multiple OccasionGiftCandidates.
- The system does not enforce a single-winner constraint for an OccasionGiftIdea.
- Multiple concrete candidates can ultimately be recorded as given.

**References:** `DM-R008`, `DM-R010`.

**Limitation:**  
Exact candidate state names and transition rules are unresolved under `OQ-003`.

---

## 3.6 Reminders and proactive assistance

### FR-021 — Remind users about approaching Occasions

**Status:** Confirmed

**Requirement:**  
GiftPipe must proactively remind the user about relevant approaching Occasions.

**Rationale:**  
Resurfacing saved context at the right time is central to the product value proposition.

**Acceptance criteria:**

- An approaching Occasion can trigger a reminder.
- The reminder can surface relevant user-captured GiftIdeas.
- Existing user-captured ideas are prioritized ahead of speculative AI alternatives.

**References:** [Product Vision](vision.md), Human ideas outrank speculative AI suggestions.

---

### FR-022 — Avoid unnecessary proactive AI when useful manual ideas exist

**Status:** Blocked

**Requirement:**  
GiftPipe must suppress automatic proactive AI gift generation when the Occasion already has sufficiently useful user-captured ideas.

**Rationale:**  
The product should respect human-captured context, control AI cost, and avoid unnecessary noise.

**Acceptance criteria:**

- Automatic AI generation does not always run for every reminder.
- The exact rule for determining when manual ideas are "sufficiently useful" cannot be finalized until `OQ-014` is resolved.

**References:** `OQ-014`.

---

### FR-023 — Proactively generate contextual suggestions when needed

**Status:** Confirmed

**Requirement:**  
GiftPipe must be capable of proactively generating contextual gift suggestions for an approaching Occasion when the configured conditions for lacking useful manual ideas are satisfied.

**Rationale:**  
The product should help users avoid starting from zero when an Occasion is near.

**Acceptance criteria:**

- Proactive suggestions can use available PersonProfile context.
- Proactive suggestions can use relevant Gift History.
- Proactive suggestions can use relevant existing gifting context.
- Generated suggestions do not overwrite user-captured ideas.

**References:** [Product Vision](vision.md), Contextual AI scope; `OQ-014`.

**Limitation:**  
The trigger threshold remains dependent on `OQ-014`.

---

## 3.7 Bounded AI research

### FR-024 — Provide bounded gifting-specific AI actions

**Status:** Confirmed

**Requirement:**  
GiftPipe must expose AI capabilities as explicit gifting-related actions rather than as an unrestricted general-purpose assistant.

**Rationale:**  
AI should support the GiftPipe domain instead of becoming a general chatbot.

**Acceptance criteria:**

- AI actions operate within a gifting-related purpose.
- The product does not intentionally support unrelated general-purpose requests as part of the established AI model.
- GiftPipe controls which AI capabilities exist.

**References:** [Product Vision](vision.md), AI should be bounded to gifting work.

---

### FR-025 — Enhance an existing GiftIdea without replacing its intent

**Status:** Confirmed

**Requirement:**  
GiftPipe must support AI-assisted enhancement of an existing GiftIdea while preserving the original captured intent.

**Rationale:**  
A vague thought such as "Dad wants an AI book" may become a research project without losing the original idea.

**Acceptance criteria:**

- AI enhancement can produce further research or concrete candidates.
- The original GiftIdea remains preserved.
- Enhancement remains scoped to the GiftIdea rather than silently replacing it with unrelated gift categories.

**References:** [Product Vision](vision.md), Preserve intent before enriching it.

---

### FR-026 — Persist useful AI research outputs

**Status:** Confirmed

**Requirement:**  
Useful AI-generated research must be preservable as part of the relevant GiftIdea or OccasionGiftIdea research history.

**Rationale:**  
AI output should contribute to cumulative research rather than disappear as transient chat.

**Acceptance criteria:**

- AI-generated candidates can remain available for further research.
- AI-generated evaluations, comparisons, enhancements, or summaries can be retained where relevant.
- Persisted AI output can later be used as research context.

**References:** `DM-R016`.

---

### FR-027 — Use relevant gifting context for AI assistance

**Status:** Confirmed

**Requirement:**  
GiftPipe's contextual AI actions must be able to use relevant recipient and gifting context where that context exists.

**Acceptance criteria:**

- AI can use PersonProfile information.
- AI can use previous Gift History where relevant.
- AI can use relevant GiftIdea or OccasionGiftIdea research.
- AI does not silently modify PersonProfile based on inferred patterns.

**References:** `DM-R015`, [Product Vision](vision.md).

---

## 3.8 Reconciliation and gifting history

### FR-028 — Reconcile an Occasion after it occurs

**Status:** Confirmed

**Requirement:**  
GiftPipe must support post-Occasion reconciliation that records what the user actually gave.

**Rationale:**  
Gift history should not depend on the user manually maintaining every planning state before or after the Occasion.

**Acceptance criteria:**

- Reconciliation can operate across the Occasion's relevant OccasionGiftIdeas.
- The user can record that an OccasionGiftIdea was used.
- The user can record that an OccasionGiftIdea was not used.
- Reconciliation preserves historical OccasionGiftIdea associations.

**References:** `DM-R006`, `DM-R011`.

---

### FR-029 — Reconcile an OccasionGiftIdea without a concrete candidate

**Status:** Confirmed

**Requirement:**  
GiftPipe must allow an OccasionGiftIdea to be recorded as used/given even when no GiftCandidate or OccasionGiftCandidate was ever created.

**Rationale:**  
A user may save "Whisky", buy a bottle offline, and never record the exact bottle.

**Acceptance criteria:**

- Reconciliation does not force creation of a concrete candidate.
- Gift History can preserve the GiftIdea-level outcome.

**References:** `DM-R005`, `DM-R011`.

---

### FR-030 — Reconcile using known candidates when they exist

**Status:** Confirmed

**Requirement:**  
When an OccasionGiftIdea has nominated candidates, GiftPipe must allow reconciliation to identify which of those concrete candidates were actually given.

**Acceptance criteria:**

- Known candidates can be selected as actual gifts during reconciliation.
- More than one known candidate can be recorded as given for the same Occasion.
- Reconciliation does not assume that only one candidate can win.

**References:** `DM-R008`, `DM-R010`.

**Limitation:**  
Exact state-transition terminology remains blocked by `OQ-003`.

---

### FR-031 — Add previously uncaptured gifts during reconciliation

**Status:** Confirmed

**Requirement:**  
GiftPipe must allow the user to record gifts during reconciliation that were not previously captured.

**Rationale:**  
Actual gifting behavior may diverge from prior planning.

**Acceptance criteria:**

- The user can add another gift during reconciliation even after recording one or more existing gifts.
- The newly entered gift becomes a new GiftIdea for the Person.
- The new GiftIdea is associated with the Occasion through an OccasionGiftIdea.
- The new OccasionGiftIdea is recorded as used/given.
- GiftPipe does not infer a separate GiftCandidate solely because the entered description is concrete.

**References:** `DM-R003`, `DM-R012`.

---

### FR-032 — Support multiple gifts for one Occasion

**Status:** Confirmed

**Requirement:**  
GiftPipe must allow reconciliation to record multiple gifts for the same Occasion.

**Acceptance criteria:**

- The user can record more than one GiftIdea as used/given.
- The user can record more than one candidate as given where applicable.
- Adding a previously uncaptured gift remains available even after another gift has already been recorded.

**References:** `DM-R008`.

---

### FR-033 — Collect optional gift-success feedback

**Status:** Confirmed

**Requirement:**  
GiftPipe must allow the user to optionally record lightweight feedback about how a given gift was received.

**Rationale:**  
Outcome feedback can improve historical context and future recommendations.

**Acceptance criteria:**

- Providing feedback is optional.
- Gift success feedback is retained as gifting history.
- Feedback does not automatically modify PersonProfile.

**References:** `DM-R015`.

---

### FR-034 — Follow up on unresolved post-Occasion outcomes

**Status:** Blocked

**Requirement:**  
GiftPipe must be able to follow up after an Occasion when reconciliation remains incomplete, but the frequency and stopping conditions are not yet established.

**Acceptance criteria:**

- GiftPipe can prompt the user to reconcile an Occasion after it occurs.
- The system must avoid indefinite or excessive prompting.
- Exact timing, retry frequency, and stopping conditions cannot be finalized until `OQ-015` is resolved.

**References:** `OQ-015`.

---

### FR-035 — Preserve cumulative Gift History

**Status:** Confirmed

**Requirement:**  
GiftPipe must preserve historical gifting outcomes across occasions so they can be reviewed and used as future gifting context.

**Acceptance criteria:**

- Given gifts remain represented after reconciliation.
- Unused OccasionGiftIdeas remain historically represented.
- Gift History can distinguish what was considered from what was actually given where that information was captured.
- Future AI assistance can use relevant Gift History.

**References:** `DM-R006`, `DM-R013`, `DM-R019`.

---

## 3.9 Partially defined behavior

### FR-036 — Support partially completed reconciliation

**Status:** Blocked

**Requirement:**  
GiftPipe must allow reconciliation behavior to accommodate an Occasion with multiple GiftIdeas and gifts, but the exact semantics of leaving only part of that Occasion reconciled are not yet finalized.

**Acceptance criteria:**

- The product must not assume that resolving one gift necessarily resolves every other OccasionGiftIdea.
- Final rules for partially resolved OccasionGiftIdeas depend on `OQ-013`.

**References:** `OQ-013`.

---

# 4. Non-functional requirements

## 4.1 Usability

### NFR-001 — Phone-first usability

**Status:** Confirmed

**Requirement:**  
GiftPipe must be usable effectively on a phone for its core capture, occasion, research, reminder follow-up, and reconciliation behaviors.

**Rationale:**  
Gift ideas often occur away from a desk, and quick mobile capture is an established product constraint.

**Verification:**

- Core established workflows can be completed from a phone-sized interface.
- No core capture workflow requires desktop-only interaction.

**References:** [Product Vision](vision.md), Constraints and assumptions.

---

### NFR-002 — Minimize capture effort

**Status:** Confirmed

**Requirement:**  
GiftIdea capture must tolerate incomplete information and must not require research or candidate creation before saving.

**Rationale:**  
Frictionless capture is the primary behavior the product is intended to encourage.

**Verification:**

- A user can save a GiftIdea for a Person without an Occasion or GiftCandidate.
- A user is not required to resolve specificity or candidate semantics before saving.

**References:** `DM-R001`, `DM-R002`, `DM-R005`.

---

## 4.2 Privacy and data control

### NFR-003 — Keep recipient planning private by default

**Status:** Confirmed

**Requirement:**  
The initial product model must treat each user's recipient, gift-planning, and gifting-history data as private to that user rather than shared by default with other GiftPipe users or recipients.

**Rationale:**  
The established participation model is independent private accounts.

**Verification:**

- Recipients are not required to access or contribute to the user's planning data.
- Another independent GiftPipe user does not implicitly share the same Person or gift-planning data.

**References:** [Product Vision](vision.md), Private individual account model.

---

### NFR-004 — Preserve explicit user control over PersonProfile

**Status:** Confirmed

**Requirement:**  
GiftPipe must preserve a distinction between user-entered PersonProfile information and AI- or outcome-derived inference.

**Rationale:**  
The profile is explicit user-maintained truth.

**Verification:**

- AI activity does not silently change PersonProfile.
- Gift-success feedback does not silently change PersonProfile.

**References:** `DM-R015`.

---

## 4.3 Reliability and data integrity

### NFR-005 — Preserve historical research and outcomes

**Status:** Confirmed

**Requirement:**  
GiftPipe must preserve established research and historical occasion outcomes unless the user performs an explicit data-removal action.

**Rationale:**  
The product derives cumulative value from historical context.

**Verification:**

- Rejecting a candidate does not erase it from research history.
- Marking an OccasionGiftIdea unused does not remove the Occasion association.
- Giving a gift does not automatically remove its GiftIdea from future history.

**References:** `DM-R006`, `DM-R013`, `DM-R019`.

---

### NFR-006 — AI failure must not invalidate preserved user data

**Status:** Confirmed

**Requirement:**  
GiftPipe's core saved GiftIdeas, research, Occasion associations, and Gift History must remain meaningful independently of whether an AI action succeeds.

**Rationale:**  
AI is an enhancement layer rather than the primary source of truth.

**Verification:**

- Existing user-captured GiftIdeas remain available without AI output.
- Reminder and historical context can still surface saved user information even when no new AI suggestion is produced.
- AI-derived summaries do not replace raw research as authoritative history.

**References:** [Product Vision](vision.md), bounded AI principles; [Domain Model](../domain/domain-model.md), Raw Research History.

---

## 4.4 Product focus

### NFR-007 — AI behavior must remain domain-bounded

**Status:** Confirmed

**Requirement:**  
GiftPipe's intended AI capability must remain bounded to gifting-related work rather than expose an unrestricted general-purpose assistant.

**Rationale:**  
The product is intended to assist gift planning, not act as a general AI application.

**Verification:**

- Supported AI actions are defined around gifting tasks.
- Unrelated general-purpose AI behavior is not part of the established product capability.

**References:** [Product Vision](vision.md), AI should be bounded to gifting work.

---

### NFR-008 — Proactive behavior must be restrained

**Status:** Tentative

**Requirement:**  
GiftPipe should constrain proactive notifications and AI generation to avoid unnecessary cost and excessive user interruption.

**Rationale:**  
Cost management and avoiding harassment were explicit motivations for bounded proactive behavior.

**Verification:**

- The product supports conditions that limit automatic AI generation.
- The product supports stopping or limiting post-Occasion follow-up.

**References:** `OQ-014`, `OQ-015`.

**Limitation:**  
Exact thresholds and schedules remain unresolved, so this quality objective is established but not yet fully measurable.

---

# 5. Principal user workflows

## 5.1 Capture an idea for later

**Purpose:**  
Preserve a gift thought with minimal effort.

**Initial conditions:**  
A Person exists or is being established as the recipient.

**Relevant requirements:**  
`FR-002`, `FR-004`, `FR-005`, `FR-006`.

**Expected outcome:**  
A GiftIdea exists for the Person and can remain independent of any Occasion or GiftCandidate.

**Unresolved questions:**  
None required for the core workflow.

---

## 5.2 Capture an idea for a known Occasion

**Purpose:**  
Preserve an idea and immediately make it relevant to an upcoming Occasion.

**Initial conditions:**  
A Person and relevant Occasion are available.

**Relevant requirements:**  
`FR-004`, `FR-015`, `FR-016`.

**Expected outcome:**  
The GiftIdea exists independently and an OccasionGiftIdea records that it is being considered for that Occasion.

**Unresolved questions:**  
Recurring Occasion occurrence identity depends on `OQ-004`.

---

## 5.3 Research a GiftIdea

**Purpose:**  
Develop a vague or concrete GiftIdea into more useful gifting knowledge.

**Initial conditions:**  
A GiftIdea exists.

**Relevant requirements:**  
`FR-008` through `FR-013`, `FR-024` through `FR-027`.

**Expected outcome:**  
The user may accumulate candidates, links, notes, or AI-assisted research without losing the original GiftIdea.

**Unresolved questions:**  
Product-link-only capture depends on `OQ-011`. Full research taxonomy remains deliberately deferred.

---

## 5.4 Prepare for an Occasion

**Purpose:**  
Bring durable GiftIdeas and concrete options into a specific Occasion context.

**Initial conditions:**  
An OccasionGiftIdea exists.

**Relevant requirements:**  
`FR-018`, `FR-019`, `FR-020`, `FR-021` through `FR-023`.

**Expected outcome:**  
Relevant candidates can be nominated or created for the Occasion, and the user is reminded as the Occasion approaches.

**Unresolved questions:**  
Occasion-only candidate identity depends on `OQ-001`; exact candidate state semantics depend on `OQ-003`; proactive AI threshold depends on `OQ-014`.

---

## 5.5 Reconcile an Occasion

**Purpose:**  
Record what actually happened after the Occasion.

**Initial conditions:**  
The Occasion has relevant OccasionGiftIdeas and has occurred or is ready to be reconciled.

**Relevant requirements:**  
`FR-028` through `FR-036`.

**Expected outcome:**  
GiftPipe records which ideas or candidates were actually given, allows previously uncaptured gifts to be added, preserves unused ideas historically, and optionally records success feedback.

**Unresolved questions:**  
Outcome-state semantics depend on `OQ-003`; partial reconciliation depends on `OQ-013`; follow-up policy depends on `OQ-015`.

---

# 6. Scope and release boundaries

## Established product scope

The following capabilities are established as part of GiftPipe's intended product direction:

- independent user accounts,
- Person and PersonProfile management,
- GiftIdea capture,
- vague and concrete GiftIdeas,
- optional Occasion association,
- recurring and one-off Occasions conceptually,
- general and occasion-specific research,
- GiftCandidates,
- candidate nomination,
- supporting and product links,
- contextual bounded AI,
- reminders,
- proactive suggestions under appropriate conditions,
- post-Occasion reconciliation,
- multiple gifts per Occasion,
- Gift Success Feedback,
- cumulative Gift History.

A confirmed requirement is not automatically an initial-release commitment.

## Tentative or evolving scope

The following are discussed but not sufficiently settled to treat as confirmed delivery scope:

- AI-driven ranking of available bounded AI actions,
- free-form AI conversation under future safeguards,
- explicit promotion/copy/reference behavior between occasion-specific and general research,
- richer automated research summarization policies.

## Explicit exclusions or deferrals

The currently established direction excludes or defers:

- native real-time commerce search,
- retailer catalog browsing as a core capability,
- live price comparison or stock checking as core product behavior,
- unrestricted general-purpose AI chat,
- mandatory recipient participation,
- collaborative household/shared-recipient planning in the initial model,
- native mobile applications as a requirement for the initial direction.

The product is phone-first, not native-first.

## Release boundaries

The complete initial-release boundary has not yet been finalized.

Discovery has established a preferred core loop:

1. capture a GiftIdea,
2. optionally associate it with an Occasion,
3. optionally research it,
4. receive an Occasion reminder,
5. reconcile the Occasion afterward.

This loop describes product direction but should not be treated as a complete release plan or prioritized backlog.

---

# 7. Open dependencies

| Requirement | Open question | Missing decision | Consequence |
|---|---|---|---|
| `FR-013` | `OQ-011` | Whether product-link-only capture also creates a GiftCandidate | Initial research structure remains blocked |
| `FR-014` | `OQ-004` | Recurring Occasion versus occurrence identity | Detailed occurrence behavior and history cannot be finalized |
| `FR-019` | `OQ-001` | Whether an occasion-only candidate must reference a reusable GiftCandidate | Candidate creation semantics remain blocked |
| `FR-020`, `FR-030` | `OQ-003` | Exact OccasionGiftCandidate selection/outcome state model | State transitions and some acceptance criteria remain incomplete |
| `FR-022`, `FR-023` | `OQ-014` | Definition of a sufficiently useful manual idea | Automatic AI trigger behavior cannot be finalized |
| `FR-034` | `OQ-015` | Reconciliation follow-up frequency and stopping rules | Notification behavior remains incomplete |
| `FR-036` | `OQ-013` | Partial reconciliation semantics | Incomplete Occasion resolution behavior remains blocked |

Other open questions in the register can remain deferred without preventing implementation of unrelated confirmed behavior.

---

# 8. Traceability and related documentation

The [Product Vision](vision.md) defines why GiftPipe exists, its value proposition, high-level product principles, and scope boundaries.

`glossary.md` is intended to own canonical vocabulary. A repository copy was unavailable during generation of this requirements document, so terminology should be revalidated against that file once it is present.

The [Domain Model](../domain/domain-model.md) owns entities, relationships, invariants, lifecycle semantics, and domain operations. Requirements reference its `DM-Rxxx` rules rather than restating them as competing domain definitions.

The [Open Questions Register](../discovery/open-questions.md) is the authoritative location for unresolved decisions. Blocked requirements in this document must remain blocked until those questions are explicitly resolved.

Decision records should preserve rationale for significant product, domain, or architectural choices when needed.

Future implementation specifications may define technical representations of these requirements, but those representations must preserve the behavior described here.

---

# 9. Guidance for implementation agents

- Confirmed requirements may guide implementation, subject to their stated limitations and dependencies.
- Tentative requirements are not authorization to implement speculative behavior.
- Blocked requirements require clarification before implementing the affected unresolved behavior.
- Consult referenced domain rules and open questions before making domain-sensitive changes.
- Do not silently expand acceptance criteria or invent missing business rules.
- Surface contradictions between requirements and authoritative product/domain documentation.
- Requirements specify expected behavior; they do not authorize arbitrary technical design choices.
