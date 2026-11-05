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

**Generation provenance:** A repository copy of [glossary.md](glossary.md) was not available when this document was generated. Terminology was therefore checked against the glossary content previously produced during discovery, but this document does not claim to have inspected [glossary.md](glossary.md) as a repository file.

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
- All account data is private to that account; no data sharing of any kind between accounts is allowed in the current product scope.
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
- A Person alone is sufficient to save a GiftIdea; no title, idea content, or link is required.
- An explicitly supplied title takes precedence over attached links and generated titles.
- With exactly one link and no explicit title, GiftPipe derives the title from that link.
- With no content, or multiple links and no explicit title, GiftPipe assigns a generic title such as "Gift for Dad" using the Person's name. Numbered placeholders such as "Gift 1 for Dad" and "Gift 2 for Dad" distinguish existing placeholder titles.
- The user can save a vague idea such as "Whisky".
- The user can save a concrete idea such as "Metallica tickets".
- A GiftCandidate is not required at capture time.
- An Occasion is not required at capture time.
- Starting capture from a Person prepopulates that Person. Capture from the main page requires a Person to be assigned before Occasion selection is enabled; see `FR-015`.

**References:** `DM-R001`, `DM-R002`, `DM-R003`, `DM-R005`.

**Limitations:**
Title-derivation failure handling and the exact placeholder numbering/collision rules remain open under [OQ-018](../discovery/open-questions.md#oq-018--how-should-giftidea-title-assignment-handle-failures-and-numbering). Link-derived titles do not settle automatic GiftCandidate creation, which remains under `OQ-011`. A recipient-only placeholder without idea content does not suppress proactive suggestions; the settled usefulness rule is in `FR-022`.

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
- Prior use and association with another Occasion do not exclude an otherwise eligible GiftIdea from the current Occasion's available ideas. Recipient and archival constraints still apply.

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
- The GiftIdea page provides a distinct section or link for general research and visible links to its OccasionGiftIdeas, labelled by Occasion and showing whether they remain in the plan.
- The user can open each occasion context to review its specific research, including when the idea has been removed from that Occasion's plan. General and occasion-specific research remain separate; navigation does not merge or promote them.

**References:** [Domain Model](../domain/domain-model.md), Research Tree concept; `DM-R014`, `DM-R023`; `OQ-007`, `OQ-008`.

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
- Opening an idea from an Occasion retains the Occasion context. Its page offers `Add to <Occasion>` when not in the plan and `Remove from <Occasion>` when included. Opening the idea to inspect its details or research does not add it.
- The Occasion Overview and its full-list modal provide rounded + / − buttons for quick adds and removes. These invoke the same participation behavior as the contextual idea-page action.
- First-time addition creates an OccasionGiftIdea. Removal changes only its planning-participation state, retaining the context, both references, research, candidate context, and recorded outcomes.
- Re-adding a removed idea to the same Occasion restores participation through the same OccasionGiftIdea with its research intact, rather than creating a fresh context.
- Removal does not archive the durable GiftIdea or record a not-given outcome. The preserved occasion context remains reachable from the GiftIdea page.
- The GiftIdea and Occasion must belong to the same Person; a cross-recipient association is rejected.
- Occasion selection lists only the assigned Person's Occasions.
- During capture, Occasion selection is disabled until a Person is assigned. Capture started from a Person prepopulates that Person.
- If the Person changes during capture, an Occasion selected for the previous Person cannot remain a valid association. The UI's exact clearing or reselection interaction is unspecified.

**Unresolved boundaries:** Re-adding a withdrawn idea restores its existing context; this does not settle broader duplicate-context rules under [OQ-005](../discovery/open-questions.md#oq-005--may-the-same-giftidea-have-more-than-one-occasiongiftidea-for-the-same-occasion-occurrence). Exact lifecycle names and representation remain under `OQ-003`. Availability and behavior of planning controls after completed reconciliation remain under `OQ-003` and `OQ-010`; this requirement does not authorize reopening or modifying recorded outcomes.

**References:** `DM-R002`, `DM-R005`, `DM-R020`, `DM-R023`.

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
- Removing an idea from the plan under `FR-015` changes planning participation, not the actual-gift outcome; its preserved occasion context and research remain accessible. Withdrawal alone is not reconciliation.
- The underlying GiftIdea remains eligible for future occasions unless archived.

**References:** `DM-R006`, `DM-R019`, `DM-R023`.

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
Resurfacing saved context at the right time is central to the product value proposition. Brief notifications bring the user into a shared in-app planning view, keeping the notification layer thin and largely platform agnostic.

**Acceptance criteria:**

- An approaching Occasion can trigger a reminder at one of its scheduled times. Defaults, individual schedules, and date-change behavior follow `FR-037` and `DM-R024`.
- The notification briefly identifies the Occasion, Person, and time remaining, and links to the Occasion Overview in the app.
- The same Occasion Overview is accessible through normal app navigation; reminder entry does not require a separate Reminder Screen.
- The overview distinguishes ideas and candidates currently in the Occasion's plan from other available GiftIdeas for the same Person.
- The available pool includes otherwise eligible ideas regardless of prior use, association with other Occasions, or previous withdrawal from this Occasion's plan. The same-Person constraint (`DM-R020`) and established exclusion of archived ideas from proactive consideration (`DM-R019`) remain in force.
- Empty placeholders outside the current plan with no idea or recommendation content are omitted from the available-ideas picker. Explicitly included empty stubs remain visible in the current plan but do not suppress proactive suggestions (`FR-022`).
- Business rules rank available ideas for this Occasion. A preview shows the first x ideas; View all opens a modal exposing the full eligible list from that ranked pool. Ranking rules and the value of x remain TBD under `OQ-020`.
- The overview and modal offer rounded + / − controls with the behavior in `FR-015`; browsing an idea retains the Occasion context for its contextual Add/Remove action.
- Previous gifts and outcomes remain available as context; prior use does not by itself disqualify an idea.
- Existing user-captured ideas and explicitly adopted suggestions are prioritized ahead of speculative alternatives. Placeholder display, empty states, the separate Consider these section, and adoption/departure behavior follow `FR-038`.
- The overview links to detailed research. On the GiftIdea page, general research and linked OccasionGiftIdea research are separately accessible as specified in `FR-010`. The detailed research workflow and remaining layout details remain deferred.
- Opening the overview through a notification does not change gift-planning data: it does not create OccasionGiftIdeas, nominate candidates, record outcomes, or mark preparation or reconciliation complete.
- The overview supports occasion context before and after the Occasion, including access to reconciliation and history. Reconciliation entry and explicit no-gift outcomes follow `FR-028`.

**References:** [Product Vision](vision.md), Human ideas outrank speculative AI suggestions; [Occasion Overview](glossary.md#occasion-overview); [FR-037](#fr-037--manage-occasion-reminder-defaults-and-schedules); `DM-R021`, `DM-R024`.

**Limitations:**
The core pre-occasion schedule and defaults behavior is established in `FR-037`. Detailed timezone-change, daylight-saving, and delivery/retry behavior remains open under `OQ-019`; recurrence remains under `OQ-004`. The ranking rules, preview count, and remaining presentation details remain under `OQ-020`; eligibility, the preview/modal pattern, and research-scope navigation are established above. Email-only initial delivery and channel-specific content remain proposals under `OQ-021`. This requirement does not select a delivery channel or detailed research-screen design.

---

### FR-022 — Avoid unnecessary proactive AI when useful manual ideas exist

**Status:** Confirmed

**Requirement:**
GiftPipe must suppress automatic proactive generation when the Person's eligible idea pool contains a useful user-captured or explicitly adopted idea, following `DM-R025`.

**Rationale:**
Recorded intent gives the user something to elaborate on. Generation should not require the user to research an idea or add it to this Occasion first.

**Acceptance criteria:**

- A user-supplied idea title, description, link, or other captured idea material is sufficient; a vague idea needs no candidate or research to count.
- Eligibility covers the same Person's valid pool, including ideas not in the current plan and ideas previously used or associated elsewhere. Recipient and archival constraints still apply.
- A recipient-only placeholder with no idea content does not suppress generation, including when explicitly added to the Occasion. A saved link still counts if title derivation failed and the displayed title is generic.
- A suggestion explicitly adopted through Add to occasion counts as useful saved intent on subsequent reminder checks. Merely generated, unadopted suggestions do not become user-adopted content.
- A due Occasion Reminder initiates the check. Automatic generation is suppressed when a qualifying idea exists, so it does not run for every reminder. Explicit bounded AI actions remain available.

**References:** `DM-R025`; [OQ-014 resolution](../discovery/open-questions.md#oq-014--what-qualifies-as-having-a-sufficiently-useful-manual-idea-so-proactive-ai-suggestions-should-not-run); [ADR-008](../decisions/ADR-008-keep-proactive-suggestions-transient-until-adopted.md).

**Limitation:**
The usefulness boundary and reminder coupling are established; exact generation execution and temporary handoff behavior remain under `OQ-022`.

---

### FR-023 — Proactively generate contextual suggestions when needed

**Status:** Confirmed

**Requirement:**
GiftPipe must be capable of offering contextual proactive suggestions when a due Occasion Reminder's eligibility check finds no useful saved ideas under FR-022.

**Rationale:**
The reminder can lead to saved ideas or, when none are useful, options the user can consider without automatically adding data to their plan.

**Acceptance criteria:**

- Suggestions can use available PersonProfile context, Gift History, and relevant gifting research without overwriting captured ideas.
- With no useful saved ideas, use relevant recipient context from the existing profile, Gift History, and research/idea leads where available. If that context is absent, the reminder opens an empty OO with the inline cold-start prompt in FR-038; generation waits for the user to add details or choose general ideas. A name alone is not personalized interest context.
- Generated results appear as transient suggestions in a distinct "Consider these" section, following FR-038. A reminder remains brief and links to the overview.
- Generating or displaying suggestions does not create GiftIdeas, OccasionGiftIdeas, candidates, or durable research nodes. Add to occasion explicitly establishes the saved idea and plan context.
- A later reminder evaluates the current eligible pool again. An adopted suggestion suppresses automatic generation as useful saved intent. No regeneration is required merely because one suggestion was adopted during the current visit; the remaining suggestions stay available for that visit.

**References:** `DM-R025`; FR-022, FR-038; [ADR-008](../decisions/ADR-008-keep-proactive-suggestions-transient-until-adopted.md).

**Limitation:**
Exact execution timing relative to notification delivery/opening, temporary handoff/storage, late entry, and failure behavior remain under `OQ-022`. This does not establish a delivery channel or a general automatic-generation trigger on ordinary navigation.

---

### FR-037 — Manage Occasion reminder defaults and schedules

**Status:** Confirmed

**Requirement:**
GiftPipe must let users configure reminder defaults and manage independently owned reminder schedules for each Occasion, following `DM-R024`.

**Rationale:**
New occasions should receive useful defaults while users retain control over their own schedules, including choosing to have no reminders.

**Acceptance criteria — app defaults:**

- The user can configure zero to three reminder-default entries. The app initially supplies one entry for five days before the Occasion at 8 AM; the user can edit or delete it.
- A + action exposes another default row until three entries exist. Exact duplicate offset/time entries merge silently rather than taking another slot.
- The default delivery time initially is 8 AM and can be changed. Each occasion reminder's date and time can be changed independently. Changing defaults does not rewrite schedules already owned by existing Occasions.
- Times use an account timezone initially taken from the device; the user can change it explicitly. Travel alone does not change the account timezone. Effects of an explicit timezone change on existing reminders remain under `OQ-019`.

**Acceptance criteria — Occasion schedule and editor:**

- Creating an Occasion copies applicable future reminders from the user's current defaults. These count toward the total limit of five reminder entries for that Occasion; up to three copied defaults can therefore leave room for additional manual entries.
- The user can edit or delete copied and manually added entries. There is no disabled-reminder state in the MVP.
- Tapping the Occasion's bell icon exposes presets for one, seven, and thirty days before the Occasion, plus a calendar action.
- The calendar marks the Occasion date, and the user can choose the day for a reminder. Calendar selection establishes a relative day offset that is retained if the Occasion moves.
- Existing reminder dates and times are visible with controls to change them. Different times on the same day are allowed and represent separate alerts.
- Scheduling the exact same date and time as an existing reminder silently merges into that reminder. It does not create a second alert or consume another slot, even when five entries already exist.

**Acceptance criteria — expired defaults and date moves:**

- Defaults that would produce reminder times already in the past are skipped when an Occasion is created. No fallback date or immediate catch-up alert is generated.
- If applying defaults leaves a new Occasion with no scheduled reminder, display: "No reminder is set for this occasion. Consider setting one manually." This also applies when the user has deleted all global defaults. The warning is informational; the user may continue without adding a reminder. If at least one default is scheduled, this zero-reminder warning does not apply.
- Moving an Occasion retains each reminder's relative day offset and local delivery time. If the new date would put x reminders in the past, first prompt: "Moving the occasion would remove x reminders. Are you sure?"
- Confirming applies the date move and deletes those expired reminders; cancelling leaves both the original date and its reminders unchanged.

**References:** [Reminder Defaults](glossary.md#reminder-defaults), [Occasion Reminder Schedule](glossary.md#occasion-reminder-schedule); `DM-R024`; [FR-021](#fr-021--remind-users-about-approaching-occasions).

**Limitations:**
Recurring-occurrence identity and how defaults apply to future recurring occurrences remain under `OQ-004`. `OQ-019` retains detailed timezone-change, daylight-saving, manual past-time/boundary handling, notification delivery/retry, and already-delivered reminder behavior. Channel choice remains under `OQ-021`; post-occasion reconciliation follow-up remains separately under `OQ-015`. The potential future system for detailed GiftIdea plans and deadlines is outside this MVP requirement.

---

### FR-038 — Adopt transient suggestions within an occasion-planning visit

**Status:** Confirmed

**Requirement:**
GiftPipe must let users consider and explicitly adopt transient proactive suggestions within a reminder-originated Occasion Overview visit, preserving saved intent and clearly handling departure.

**Acceptance criteria:**

- The existing hierarchy remains current-plan ideas first, followed by the ranked preview of other valid ideas. Empty placeholders outside the current plan with neither idea nor recommendation content are omitted from the picker, while explicitly included empty stubs remain visible in the current plan.
- When no useful ideas exist, an empty state replaces the otherwise empty hierarchy, followed by "Consider these". If intentional empty plan stubs exist, retain them above the available-ideas empty state and suggestion section.
- In a reminder-originated visit with neither useful ideas nor usable recipient context from the existing profile, Gift History, or research/idea leads, immediately show an inline prompt within the OO empty state. Offer Add details and Create general ideas; the initial prompt is not a blocking dialog.
- Add details opens the Person's interest-profile (PersonProfile) editor in a modal. The user can add free-form text and Save; saving persists the explicitly supplied profile details, closes the modal, and starts generation in the same OO visit using the added context. A complete structured profile is not required. Cancelling returns to the inline prompt without generating.
- Create general ideas explicitly starts generation using only the Person's name as supplied recipient context and the model's general knowledge. The name identifies the recipient; do not infer interests from it or imply personalization. Label the results as general ideas. This action does not fill or change PersonProfile.
- If usable recipient context already exists, skip this cold-start prompt and follow the ordinary reminder-triggered generation flow. An empty interest field alone does not establish that all recipient context is absent.
- The profile modal is part of the same occasion-planning visit. Profile details saved there are durable even if no suggestion is adopted; the generated suggestions remain transient until Add to occasion. Saving a profile is not suggestion adoption for the exit guard.
- Each suggestion offers Add to occasion. The action creates a GiftIdea containing its content for the Occasion's Person and an OccasionGiftIdea included in that plan. The UI places the adopted idea in the current-plan section; it does not infer a GiftCandidate from concrete wording or silently fill an unrelated placeholder.
- Unadded suggestions stay available after an adoption for the rest of the visit. The adopted idea is durable; displaying the other suggestions does not save them.
- Opening idea details, research, or modals for the same Occasion and returning is part of the same visit. It does not discard suggestions or trigger the departure confirmation.
- Leaving that occasion-planning visit discards unadded suggestions. Saved ideas remain, and explicit bounded AI actions can still generate further ideas later.
- For normal app-controlled departure, show confirmation only when the visit came from a reminder, transient suggestions are available, and none were adopted during that visit. Suggested copy: "Leave this occasion? You haven't saved any suggestions. Leaving will discard them." Actions: Keep browsing / Leave and discard.
- Keep browsing preserves the visit and list. Leave and discard permits departure and discards unadded suggestions. This guard does not apply after at least one adoption or when no transient suggestions were shown.
- The MVP does not require interception or recovery for browser/app closure or process termination; those may discard unadded suggestions without confirmation.

**References:** `DM-R021`, `DM-R025`; FR-021, FR-022, FR-023; [ADR-008](../decisions/ADR-008-keep-proactive-suggestions-transient-until-adopted.md).

**Limitations:**
Technical handoff and temporary-state mechanics remain under `OQ-022`; detailed ranking and presentation remain under `OQ-020`. The Person/PersonProfile field model and how non-interest attributes can provide context remain open under `OQ-023`; no gender or relationship-type field is established here. This is not a persistence schema, full research-node taxonomy, or research-promotion policy.

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
AI output should contribute to cumulative research rather than disappear as transient chat. Unadopted proactive suggestions are a separate temporary proposal stage under `FR-038`; retaining useful research does not require automatically persisting those proposals.

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
- Reconciliation can begin even when the Occasion has zero OccasionGiftIdeas and no prior gift-planning activity in GiftPipe.
- The user can explicitly record that no gift was given for the Occasion.
- When no outcome has been reported, declining to answer or leaving a follow-up unanswered leaves it unknown; it does not imply that no gift was given.
- The user can record that an OccasionGiftIdea was used.
- The user can record that an OccasionGiftIdea was not used.
- Reconciliation preserves historical OccasionGiftIdea associations.

**References:** `DM-R006`, `DM-R011`, `DM-R022`.

**Limitations:**
The exact representation of outcomes, including an explicit no-gift report, remains under `OQ-003`. Correction/reopening remains under `OQ-010`, partial completion under `OQ-013`, and notification timing and stopping rules under `OQ-015`. These confirmed behaviors do not resolve those lifecycle choices.

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
- The user can record the first gift when the Occasion has no existing OccasionGiftIdeas, using the same gift-creation and association behavior below.
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
- A follow-up may invite the user to record what happened even when no ideas were associated with the Occasion.
- Follow-up wording is neutral and optional in tone. It does not imply an obligation to use GiftPipe or describe missing ideas or planning activity as an omission by the user.
- The system must avoid indefinite or excessive prompting.
- Exact timing, retry frequency, and stopping conditions cannot be finalized until `OQ-015` is resolved.

**References:** `OQ-015`.

**Illustrative wording:**
"Would you like to record what you gave for Dad's Birthday?" illustrates the intended invitation. "You haven't added any gift ideas for Dad's Birthday" frames missing app activity as the reason for prompting and should not be used. These examples establish a wording principle, not fixed final copy or a notification schedule.

---

### FR-035 — Preserve cumulative Gift History

**Status:** Confirmed

**Requirement:**  
GiftPipe must preserve historical gifting outcomes across occasions so they can be reviewed and used as future gifting context.

**Acceptance criteria:**

- Given gifts remain represented after reconciliation.
- Unused OccasionGiftIdeas remain historically represented.
- Gift History can distinguish what was considered from what was actually given where that information was captured.
- An explicit report that no gift was given remains distinguishable from an unknown outcome due to no response; its exact representation remains under `OQ-003`.
- Future AI assistance can use relevant Gift History.

**References:** `DM-R006`, `DM-R013`, `DM-R019`, `DM-R022`.

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

### NFR-003 — Keep all account data private

**Status:** Confirmed

**Requirement:**  
The current product model must keep all account data private to that user. No data sharing of any kind between accounts is allowed, including recipient context, gift ideas and candidates, research, occasions, and gifting history.

**Rationale:**  
The established participation model is independent private accounts.

**Verification:**

- Recipients are not required to access or contribute to the user's planning data.
- Through supported interfaces, one independent account cannot read or change another account's private data. Verification covers recipient context, gift ideas and candidates, research, occasions, and gifting history.
- No supported interface permits data sharing between accounts in the current product scope.

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
- Pre-occasion schedules follow `FR-037`: at most three default entries and five entries per Occasion, with exact duplicates merged.

**References:** `FR-022`, `OQ-015`, `OQ-022`.

**Limitation:**  
The pre-occasion schedule controls in `FR-037` are established. The usefulness boundary is established in `FR-022`; generation execution/handoff under `OQ-022` and post-occasion follow-up timing/stopping under `OQ-015` remain unresolved, so this broader quality objective is not yet fully measurable.

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
Recipient-only capture is established. Title-derivation failure handling and exact placeholder numbering remain under `OQ-018`; product-link-only Candidate creation remains under `OQ-011`.

---

## 5.2 Capture an idea for a known Occasion

**Purpose:**  
Preserve an idea and immediately make it relevant to an upcoming Occasion.

**Initial conditions:**  
A Person and an Occasion belonging to that same Person are available. Occasion selection follows `FR-015`.

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
An Occasion and its Person exist. Viewing the Occasion Overview does not require an existing OccasionGiftIdea; nominating or creating occasion-specific candidates requires the corresponding occasion-specific context.

**Relevant requirements:**  
`FR-010`, `FR-015`, `FR-018`, `FR-019`, `FR-020`, `FR-021` through `FR-023`.

**Expected outcome:**  
The user can open the Occasion Overview directly or through a brief notification. It presents the current plan and a ranked preview of other eligible ideas for the Person, including ideas used before or associated with other Occasions. View all exposes the full eligible list in a modal. Rounded + / − controls add or remove ideas from the plan; inspecting an idea opens it with the Occasion context and its explicit Add/Remove action. Removal retains the OccasionGiftIdea and its research; re-addition restores the same context. General research and individual occasion-specific research remain separately accessible from the GiftIdea page. Candidate nomination or creation remains explicit where applicable; merely viewing ideas or research does not change planning data. A reminder-originated visit with no useful ideas can show Consider these; explicit adoption, retention through related detail views, and departure follow `FR-038`. Ranking rules, the preview count, and remaining presentation details are unresolved under `OQ-020`.

**Unresolved questions:**  
Occasion-only candidate identity depends on `OQ-001`; exact candidate state semantics depend on `OQ-003`; proactive eligibility follows `FR-022` and `FR-023`, with generation execution/handoff details under `OQ-022`. Pre-occasion schedules follow `FR-037`; detailed timezone and delivery behavior remains under `OQ-019`; ranking rules, preview count, and remaining presentation details depend on `OQ-020`; initial delivery channel and channel-specific content depend on `OQ-021`.

---

## 5.5 Reconcile an Occasion

**Purpose:**  
Record what actually happened after the Occasion.

**Initial conditions:**  
The Occasion has occurred or is ready to be reconciled. Existing OccasionGiftIdeas are optional; reconciliation may begin with zero associated ideas.

**Relevant requirements:**  
`FR-028` through `FR-036`.

**Expected outcome:**  
GiftPipe records which ideas or candidates were actually given, allows previously uncaptured gifts to be added even when nothing was previously planned, preserves unused ideas historically, and optionally records success feedback. The user may instead explicitly report that no gift was given. An unanswered follow-up does not establish a no-gift outcome; any unreported outcome remains unknown. Follow-up wording follows the neutral invitation principle in `FR-034`.

**Unresolved questions:**  
Outcome-state semantics, including explicit no-gift representation, depend on `OQ-003`; correction/reopening depends on `OQ-010`; partial reconciliation depends on `OQ-013`; follow-up timing and stopping policy depend on `OQ-015`.

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
| `FR-015` | `OQ-005` | Broader duplicate-context rules for the same GiftIdea and Occasion occurrence | Re-addition reuses its existing context; other duplicate-context handling remains open |
| `FR-015` | `OQ-003`, `OQ-010` | Exact participation-state representation and control behavior after completed reconciliation | Withdrawal and restoration preserve context; they do not settle completed-outcome correction or reopening |
| `FR-010` | `OQ-008` | Promotion of research between scopes | Separate browsing is established; movement, copying, or promotion remains undecided |
| `FR-019` | `OQ-001` | Whether an occasion-only candidate must reference a reusable GiftCandidate | Candidate creation semantics remain blocked |
| `FR-020`, `FR-030` | `OQ-003` | Exact OccasionGiftCandidate selection/outcome state model | State transitions and some acceptance criteria remain incomplete |
| `FR-028`, `FR-035` | `OQ-003` | Exact representation of reconciliation outcomes, including explicit no-gift reports | Confirmed observable outcomes do not establish the full state model |
| `FR-028` | `OQ-010` | Whether completed reconciliation can be corrected or reopened | Forward outcome recording is established; correction behavior remains open |
| `FR-003`, `FR-027`, `FR-038` | `OQ-023` | Person/PersonProfile fields and context beyond interests | Existing free-form profile editing and the cold-start flow are established; additional structured fields are undecided |
| `FR-023`, `FR-038` | `OQ-022` | Exact reminder-to-overview generation execution, temporary handoff, and failure behavior | The usefulness boundary, reminder coupling, adoption, and visit lifetime are established; technical execution details remain open |
| `FR-034` | `OQ-015` | Reconciliation follow-up frequency and stopping rules | Notification behavior remains incomplete |
| `FR-036` | `OQ-013` | Partial reconciliation semantics | Incomplete Occasion resolution behavior remains blocked |
| `FR-004` | `OQ-018` | Link-title derivation failure handling and exact placeholder numbering | Basic title precedence is established; edge-case behavior is not finalized |
| `FR-021`, `FR-037` | `OQ-019` | Detailed timezone-change, daylight-saving, scheduling-boundary, and delivery/retry behavior | Core defaults and schedules are testable; operational timing details remain open |
| `FR-037` | `OQ-004` | Recurring occurrence identity and application of defaults to future occurrences | The one-Occasion scheduling contract does not establish recurring-instance behavior |
| `FR-021` | `OQ-020` | Ranking rules, preview count x, and remaining presentation details | Eligible pool, ranked preview, full-list modal, and scope navigation are established; ranking acceptance criteria remain incomplete |
| `FR-021` | `OQ-021` | Initial delivery channel and channel-specific content | Email-only initial delivery is not an established release commitment |

Other open questions in the register can remain deferred without preventing implementation of unrelated confirmed behavior.

---

# 8. Traceability and related documentation

The [Product Vision](vision.md) defines why GiftPipe exists, its value proposition, high-level product principles, and scope boundaries.

The [Glossary](glossary.md) owns canonical vocabulary. The original generation circumstances are recorded in the provenance note in section 1.

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
