### Executive summary

GiftPipe’s documentation is a coherent foundation for continued discovery and for beginning bounded implementation work. Its strongest areas are the durable GiftIdea model, contextual candidate semantics, separation of research scopes, and reconciliation that supports multiple gifts without requiring concrete candidates.

It is **not yet a complete implementation specification**. The most consequential gaps concern minimum capture content, recipient consistency when associating ideas with occasions, and reminder eligibility and timing.

The seven Accepted ADRs agree with the current domain model and requirements. Their rationale is internally coherent. I did not independently verify their historical claims against the original discovery conversation, which was not available in the inspected documentation.

**Scope and missing paths:** `docs/` does not exist. Consequently, all requested paths under it—including `docs/README.md`, the five named product/domain/discovery files, and `docs/decisions/`—are missing at those locations. Their counterparts exist under `giftpipe-docs/`, the authoritative location identified by repository `AGENTS.md`. I audited all 14 Markdown documents there, plus the relevant repository guidance.

**Verification performed:**

- Unique definitions: 36 FRs, 8 NFRs, 19 domain rules, 20 domain questions, 17 open questions, and 7 ADRs.
- All referenced identifiers exist.
- All 107 Markdown links resolve, including heading targets.
- ADR statuses match the index; there are no supersession relationships to reconcile.
- No files were changed. The existing Obsidian `workspace.json` modification remains untouched.

### Findings summary

No Critical findings.

| ID | Severity | Category | Finding |
|---|---|---|---|
| AUD-001 | High | Completeness | Minimum valid GiftIdea content is ambiguous |
| AUD-002 | High | Completeness | Recipient consistency for occasion association is unspecified |
| AUD-003 | High | Completeness | Reminder eligibility for unassociated durable ideas is unclear |
| AUD-004 | High | Completeness | Pre-occasion reminder timing lacks a defined policy or tracked question |
| AUD-005 | Medium | Completeness | Reconciliation entry with no existing OccasionGiftIdeas is unclear |
| AUD-006 | Medium | Completeness | Privacy verification does not directly test account isolation |
| AUD-007 | Medium | Ownership | Unresolved material is maintained across overlapping registers |
| AUD-008 | Medium | Traceability | Material requirement boundaries lack nearby open-question references |
| AUD-009 | Low | Structure | The documentation map retains obsolete provisional guidance |
| AUD-010 | Low | Consistency | Open-question category priority counts are incorrect |

### Detailed findings

**AUD-001 — Minimum valid GiftIdea content is ambiguous**

**Severity:** High  
**Category:** Completeness

**Location:** [giftpipe-docs/product/vision.md:129](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/product/vision.md:129), [giftpipe-docs/domain/domain-model.md:680](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/domain/domain-model.md:680), [giftpipe-docs/product/glossary.md:753](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/product/glossary.md:753).

**Evidence:** The vision identifies the recipient as the only mandatory information. The capture operation lists a title or equivalent captured content, but its preconditions say other fields may be omitted. The glossary describes the unresolved GiftIdea shape as attributes beyond Person association **and a human-understandable idea/title**.

**Explanation:** These statements leave two materially different interpretations: saving only a Person creates a valid GiftIdea, or some captured content is also required. This affects validation and the primary capture workflow, rather than merely terminology.

**Recommended action:** Clarify the minimum valid captured content, including whether it may be derived from a link. Record the answer consistently in the domain capture preconditions and FR-004 acceptance criteria.

**Decision required:** **Yes.** The available record does not unambiguously establish whether a content-free placeholder is valid.

---

**AUD-002 — Recipient consistency for occasion association is unspecified**

**Severity:** High  
**Category:** Completeness

**Location:** [giftpipe-docs/domain/domain-model.md:154](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/domain/domain-model.md:154), [giftpipe-docs/domain/domain-model.md:186](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/domain/domain-model.md:186), [giftpipe-docs/domain/domain-model.md:287](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/domain/domain-model.md:287), [giftpipe-docs/product/requirements.md:345](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/product/requirements.md:345).

**Evidence:** GiftIdea belongs to one Person; Occasion is associated with a Person. OccasionGiftIdea references one of each. Neither its invariants nor FR-015 specifies whether their Persons must match.

**Explanation:** The relationship can be implemented structurally while connecting an idea for one recipient to another recipient’s occasion. The documentation provides no explicit eligibility rule for accepting or rejecting that association. This is a missing constraint, not a documented permission for cross-recipient planning.

**Recommended action:** Clarify the recipient compatibility rule and record it in the authoritative association semantics, with a corresponding FR-015 acceptance criterion.

**Decision required:** **Yes.** Confirm whether matching Persons is required or whether any broader association is intended.

---

**AUD-003 — Reminder eligibility for unassociated durable ideas is unclear**

**Severity:** High  
**Category:** Completeness

**Location:** [giftpipe-docs/domain/domain-model.md:999](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/domain/domain-model.md:999), [giftpipe-docs/product/glossary.md:621](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/product/glossary.md:621), [giftpipe-docs/product/requirements.md:476](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/product/requirements.md:476), [giftpipe-docs/discovery/open-questions.md:158](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/discovery/open-questions.md:158).

**Evidence:** GiftIdeas may remain independent of occasions. Occasion Reminder uses Occasion information and associated OccasionGiftIdeas. FR-021 instead refers broadly to “relevant user-captured GiftIdeas.” OQ-014 asks whether manual ideas are sufficiently useful, without explicitly defining the eligible idea pool.

**Explanation:** An idea captured only for Dad could either resurface for his approaching birthday or remain absent until manually associated. Both readings fit parts of the documentation, but produce substantially different behavior in the central capture-and-resurface loop.

**Recommended action:** Clarify whether reminders consider only associated ideas or also other eligible ideas for the Person. Distinguish displaying an idea from creating an OccasionGiftIdea. Extend OQ-014 where appropriate.

**Decision required:** **Yes.** Idea eligibility and any association side effects are not established.

---

**AUD-004 — Pre-occasion reminder timing is not sufficiently defined or tracked**

**Severity:** High  
**Category:** Completeness

**Location:** [giftpipe-docs/product/requirements.md:466](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/product/requirements.md:466), [giftpipe-docs/discovery/open-questions.md:166](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/discovery/open-questions.md:166).

**Evidence:** FR-021 requires reminders for “relevant approaching Occasions”; its acceptance criteria say an approaching Occasion can trigger a reminder. OQ-015 explicitly covers **post-occasion** follow-up timing, frequency, and stopping conditions. No equivalent pre-occasion policy is recorded.

**Explanation:** “Approaching” does not establish when a reminder becomes due or whether it should occur again. This prevents reliable acceptance testing of an established core behavior. It is separate from recurrence identity and from the usefulness threshold for AI generation.

**Recommended action:** Track the unresolved pre-occasion trigger policy explicitly, then define observable acceptance criteria once resolved. Do not infer a schedule from OQ-015.

**Decision required:** **Yes.** Reminder timing and any repetition policy remain undefined.

---

**AUD-005 — Reconciliation entry with no existing OccasionGiftIdeas is unclear**

**Severity:** Medium  
**Category:** Completeness

**Location:** [giftpipe-docs/domain/domain-model.md:155](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/domain/domain-model.md:155), [giftpipe-docs/product/requirements.md:671](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/product/requirements.md:671), [giftpipe-docs/product/requirements.md:1033](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/product/requirements.md:1033).

**Evidence:** An Occasion may have zero OccasionGiftIdeas. FR-031 supports recording previously uncaptured gifts during reconciliation. However, the reconciliation workflow’s initial conditions say the Occasion “has relevant OccasionGiftIdeas.”

**Explanation:** The documented entry condition appears to exclude an Occasion for which nothing was planned, even though adding previously uncaptured gifts is established behavior. The empty starting case is neither expressly supported nor excluded.

**Recommended action:** Clarify whether reconciliation may start with zero OccasionGiftIdeas and align its preconditions and acceptance criteria accordingly.

**Decision required:** **Yes.** The existing record does not explicitly settle this boundary.

---

**AUD-006 — Privacy verification does not directly test account isolation**

**Severity:** Medium  
**Category:** Completeness

**Location:** [giftpipe-docs/product/requirements.md:827](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/product/requirements.md:827), [giftpipe-docs/product/requirements.md:65](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/product/requirements.md:65).

**Evidence:** NFR-003 requires planning data to be private to its user. Its verification checks that recipients need not participate and that another account does not implicitly share the same data. FR-001 similarly checks independent maintenance.

**Explanation:** Independent records and optional recipient participation do not verify that another account cannot access or change private records. The privacy requirement itself is clear; its verification omits the central promised boundary.

**Recommended action:** Add explicit verification that one independent account cannot read or change another account’s private planning data through supported interfaces.

**Decision required:** **No.** This operationalizes the established private-account model without selecting an authorization architecture.

---

**AUD-007 — Unresolved material has overlapping ownership**

**Severity:** Medium  
**Category:** Ownership

**Location:** [giftpipe-docs/product/glossary.md:676](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/product/glossary.md:676), [giftpipe-docs/domain/domain-model.md:1398](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/domain/domain-model.md:1398), [giftpipe-docs/discovery/open-questions.md:5](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/discovery/open-questions.md:5).

**Evidence:** The glossary maintains substantial open-question descriptions and alternatives. The domain model maintains 20 DM-Q entries. The separate register declares itself authoritative for active uncertainty. The glossary and domain question sections lack direct OQ mappings, although the register provides mappings in the opposite direction.

Coverage also differs: [DM-Q015](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/domain/domain-model.md:1416) includes permanent deletion, while [OQ-012](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/discovery/open-questions.md:138) concentrates on archival, reactivation, and permitted archived operations.

**Explanation:** Domain-local context is useful and permitted by the guide. The maintenance problem is the absence of an explicit boundary between contextual summaries and independently maintained unresolved material. A reader cannot reliably determine which local questions are represented in the authoritative register.

**Recommended action:** Preserve existing identifiers. Add OQ references to overlapping local entries, identify those entries as contextual summaries, and reconcile material coverage differences without resolving the questions. Keep minor technical uncertainties local where the register’s inclusion criteria permit that.

**Decision required:** **No.** This concerns ownership and visibility of existing uncertainty.

---

**AUD-008 — Material requirement boundaries lack nearby question references**

**Severity:** Medium  
**Category:** Traceability

**Location:** [giftpipe-docs/product/requirements.md:345](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/product/requirements.md:345), [giftpipe-docs/product/requirements.md:611](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/product/requirements.md:611), [giftpipe-docs/product/requirements.md:1112](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/product/requirements.md:1112).

**Evidence:**

- FR-015 specifies association creation without referencing [OQ-005](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/discovery/open-questions.md:75), which leaves duplicate associations unresolved.
- FR-028 and its workflow omit [OQ-010](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/discovery/open-questions.md:119), which leaves correction or reopening unresolved.
- Neither dependency appears in the requirements dependency table.

**Explanation:** These are existing questions with direct effects on the operations being specified. Their absence near Confirmed requirements makes it easier to infer uniqueness or correction behavior accidentally. This does not make forward association creation or outcome recording wholly blocked.

**Recommended action:** Add concise references and boundary notes for duplicate association handling and completed-outcome correction. Extend the existing dependency table rather than introducing another tracking mechanism.

**Decision required:** **No.** The uncertainties are already documented.

---

**AUD-009 — The documentation map retains obsolete provisional guidance**

**Severity:** Low  
**Category:** Structure

**Location:** [giftpipe-docs/README.md:40](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/README.md:40), [giftpipe-docs/product/vision.md:303](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/product/vision.md:303), [giftpipe-docs/domain/domain-model.md:9](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/domain/domain-model.md:9).

**Evidence:** The guide still describes the other documents as intended paths until created. The vision describes requirements as a future document. Several existing authoritative documents are referenced only as inline paths despite the guide’s relative-link convention.

**Explanation:** Historical generation notes can remain accurate provenance. Current navigation and future-tense guidance, however, no longer reflect repository contents and require contributors to repeat existence checks.

**Recommended action:** Refresh the current inventory and future-tense references, and link existing documents. Preserve historical availability statements as historical notes rather than treating them as false.

**Decision required:** **No.**

---

**AUD-010 — Open-question category priority counts are incorrect**

**Severity:** Low  
**Category:** Consistency

**Location:** [giftpipe-docs/discovery/open-questions.md:25](/Users/peteraporfi/Developer/giftpipe/giftpipe-docs/discovery/open-questions.md:25).

**Evidence:** The overview reports:

- Domain model: 2 High, 3 Medium, 3 Low.
- Domain operations and lifecycle: 1 High, 3 Medium, 1 Low.

The actual entries give:

- Domain model: **3 High, 2 Medium, 3 Low**.
- Domain operations and lifecycle: **0 High, 4 Medium, 1 Low**.

The overall totals remain correct.

**Explanation:** The category summary misrepresents where the highest-priority discovery work sits, although no question’s individual priority is ambiguous.

**Recommended action:** Correct the two category rows from the existing entries.

**Decision required:** **No.**

### Implementation readiness

“Ready” here means the documented behavior supports beginning a bounded implementation slice. It does not imply a complete release specification or permission to implement during the project’s current documentation-only phase.

| Established capability | Readiness | Assessment |
|---|---|---|
| Independent accounts and basic Person creation | **Ready** | Ownership and participation expectations are clear. Strengthen privacy verification under AUD-006. |
| Explicit PersonProfile maintenance | **Partially ready** | User control and separation from inferred history are clear. The complete structured profile shape remains unspecified. |
| Basic GiftIdea capture | **Partially ready** | Person scope, vague/concrete input, and optional occasion/candidate behavior are clear. Resolve minimum content under AUD-001. |
| General research, reusable candidates, explicit link semantics | **Ready for bounded operations** | Adding candidates and reference/product links is coherent. Full lifecycle and taxonomy remain open under OQ-002 and OQ-007. |
| GiftIdea archival | **Partially ready** | Historical preservation and exclusion from proactive consideration are clear. Reactivation and other archived-state operations remain under OQ-012. |
| One-off occasion association | **Partially ready** | Core association behavior is clear. Recipient compatibility and duplicate handling need boundaries: AUD-002 and OQ-005. |
| Recurring occasion occurrences | **Blocked** | OQ-004 leaves the occurrence identity needed by history, reminders, and reconciliation unresolved. |
| Occasion reminders | **Partially ready** | The purpose and priority of manual context are clear. Eligibility and scheduling remain insufficiently defined: AUD-003 and AUD-004. |
| Manual bounded AI enhancement | **Ready for a narrow slice** | Preserving intent, using relevant context, and retaining useful research are coherent. This does not require settling the complete action catalog. |
| Proactive AI suggestions | **Blocked** | OQ-014 leaves the automatic generation threshold unresolved; reminder eligibility also matters. |
| Product-link-only initial capture | **Blocked** | FR-013 explicitly depends on OQ-011. |
| Occasion-local candidate creation | **Blocked** | FR-019 explicitly depends on OQ-001. |
| Reconciliation, history, and optional feedback | **Partially ready** | The principal outcome scenarios are strong. Complete lifecycle behavior depends on OQ-003, OQ-010, OQ-013, OQ-015, and the entry boundary in AUD-005. |

The documentation correctly treats aggregate boundaries, research promotion, summary refresh policies, and the full release boundary as unresolved. Their incompleteness does not block every capability.

The structure remains practical for humans and Codex. The larger documents have coherent responsibilities; their length alone does not justify splitting them. Concept summaries, operational rules, and worked scenarios generally serve different purposes rather than constituting harmful duplication.

### Recommended remediation batches

**1. Safe editorial or mechanical corrections**

- **AUD-009:** Refresh the inventory, future-tense references, and relative links.
- **AUD-010:** Correct category priority counts.
- **AUD-007:** Add mappings between existing uncertainty entries, preserving identifiers, meaning, and status.

**2. Changes that propagate already established decisions or boundaries**

- **AUD-006:** Translate the private-account decision into direct isolation verification.
- **AUD-008:** Surface existing duplicate-association and reconciliation-correction uncertainties beside affected requirements.
- Complete AUD-007’s coverage reconciliation using existing documented uncertainty, without resolving it.

**3. Changes requiring product clarification**

- **AUD-001:** Minimum valid captured content.
- **AUD-002:** Recipient compatibility for occasion association.
- **AUD-003:** Reminder eligibility for unassociated durable ideas.
- **AUD-004:** Pre-occasion reminder trigger policy.
- **AUD-005:** Reconciliation eligibility when no ideas were previously associated.

These batches are recommendations, not approved documentation changes.

### Recommended next steps

1. Clarify minimum capture content and recipient compatibility first; both affect foundational validation and relationships.
2. Discuss reminder eligibility, reminder timing, and OQ-014 together. They jointly determine whether the central capture-and-resurface loop behaves as intended.
3. Before committing to recurring occasion or reconciliation persistence models, resolve the relevant existing identity and outcome questions—especially OQ-004 and OQ-003.
4. Apply the mechanical and established-decision batches when documentation editing is authorized. They can proceed independently of those product decisions.

### Follow-up — first approved remediation batch

**Source conversation:** Codex session `01a190be-86f0-72b2-9a8a-58b09c8cdac3`, discussion begun 2026-10-31. Use this session ID to revisit the actual conversation; this section records the approved documentation scope, not a transcript.

**Baseline:** The original audit above was committed unchanged as `42d2b2d` before remediation. Its findings and verification counts describe that baseline. The table below records subsequent progress without rewriting the original assessment.

The user approved the capture, recipient-consistency, and notification/overview documentation batch. Changes were propagated to the [vision](../product/vision.md), [glossary](../product/glossary.md), [domain model](../domain/domain-model.md), [requirements](../product/requirements.md), and [open-questions register](../discovery/open-questions.md). [AGENTS.md](../../AGENTS.md) now requires source-session traceability for approved documentation batches.

| Finding | Progress in this batch | Remaining uncertainty |
|---|---|---|
| AUD-001 | Minimum-content ambiguity resolved: a Person alone is sufficient. Explicit titles win; a single link supplies a derived title; no content or multiple links use a generic placeholder, numbered as needed. | `OQ-018` tracks link-title failure handling and exact placeholder numbering. `OQ-011` still owns automatic Candidate creation. |
| AUD-002 | Resolved: GiftIdea and Occasion must belong to the same Person (`DM-R020`). Capture prepopulates the Person when started from a Person; Occasion selection is disabled until assignment and filtered to that Person's Occasions. | Exact UI behavior when changing the Person remains unspecified, but a previous recipient's Occasion cannot remain valid. Duplicate associations remain under `OQ-005`. |
| AUD-003 | Partially resolved: the in-app Occasion Overview includes the Person's ideas with no Occasion associations. Notifications are brief and link to that same overview. Opening it does not change planning data (`DM-R021`); detailed research is linked separately. | `OQ-020` tracks other-Occasion idea eligibility and presentation; `OQ-014` retains the usefulness threshold, including recipient-only placeholders. |
| AUD-004 | Tracking gap addressed: `OQ-019` explicitly owns pre-occasion timing, repetition, and app-level/occasion-level settings interaction. `FR-021` and the dependency table reference it. | No schedule or settings precedence was selected. Post-occasion follow-up remains separately under `OQ-015`. |
| AUD-010 | Corrected while maintaining the category overview for the four new questions (`OQ-018` through `OQ-021`). | No individual question priority changed. |

**Additional boundaries:** One Occasion Overview serves ordinary navigation and notification entry, with access to planning, research, reconciliation, and history as appropriate. Its exact layout and the research UI remain deferred. Email-only initial notification delivery was not verified as an existing decision and remains a proposal under `OQ-021`; no new channel commitment was made.

**Outside this batch:** AUD-005 through AUD-009 remain for later discussion or authorized maintenance. Existing reconciliation entry conditions, accepted ADRs, identifiers, and unresolved domain decisions have not been changed by this batch.

**Verification:** Checked 17 Markdown files, all 168 local links and heading targets, 86 requirement/rule/question identifiers, and every open-question category count. Confirmed that this audit's original committed content remains an unchanged prefix with the follow-up appended. All checks passed, as did `git diff --check` for the documentation edits. No application tests were run for this documentation-only batch.

### Follow-up — second approved remediation batch

**Source conversation:** Codex session `01a190be-86f0-72b2-9a8a-58b09c8cdac3`, continuing the discussion begun 2026-10-31. The first remediation batch was committed as `c9cabea` before this batch.

**Approved scope:** The user approved reconciliation with zero existing OccasionGiftIdeas, an explicit no-gift outcome distinct from no response, and neutral post-occasion invitations that do not imply an obligation to use GiftPipe. These changes were propagated to the [domain model](../domain/domain-model.md#87-reconcile-occasion), [requirements](../product/requirements.md#fr-028--reconcile-an-occasion-after-it-occurs), [glossary](../product/glossary.md#reconciliation), and [existing open-question context](../discovery/open-questions.md). The original audit and first follow-up remain unchanged above.

| Finding | Progress in this batch | Remaining uncertainty |
|---|---|---|
| AUD-005 | Resolved: reconciliation may begin with zero associated ideas. The user can record the first previously uncaptured gift through the existing creation and association behavior, or explicitly report that no gift was given (`DM-R022`, `FR-028`, `FR-031`). | Exact outcome representation (`OQ-003`), correction/reopening (`OQ-010`), partial completion (`OQ-013`), and follow-up timing/stopping (`OQ-015`) remain open. |
| AUD-008 | Reconciliation boundary made explicit: `FR-028`, its workflow, and the requirements dependency table now reference correction/reopening under `OQ-010`. | The separate duplicate-association boundary near `FR-015` and its dependency on `OQ-005` remain outside this batch. AUD-008 is only partially addressed. |

**Follow-up wording:** `FR-034` establishes an invitation such as "Would you like to record what you gave for Dad's Birthday?" without calling out missing app activity. This is an illustrative tone principle, not fixed final copy or a notification schedule. A lack of response does not establish that no gift was given.

**Preserved uncertainty:** No new lifecycle states, persistence representation, automatic resolution of remaining planning items, correction behavior, partial-completion rules, or notification timing/stopping rules were selected. Accepted ADRs and existing identifiers remain unchanged. AUD-006, AUD-007, AUD-009, and the remaining part of AUD-008 still await a later batch.

**Verification:** Checked 17 Markdown files, all 177 local links and heading targets, 107 requirement/rule/question identifiers (including the 20 `DM-Q` entries), and every open-question category count. Confirmed that existing identifiers and all previously committed audit content were preserved. All checks passed, as did `git diff --check`. No application tests were run for this documentation-only batch. Changes are left uncommitted for review.

### Follow-up — third approved remediation batch

**Source conversation:** Codex session `01a197c6-c2a0-743a-ac29-8c6857ceb05a`, obtained from the session environment. This conversation resumes the audit-remediation discussion recorded in the earlier follow-ups. The second batch is now committed as `e2a0754`; its earlier uncommitted-status note above describes the state at that time.

**Approved scope:** The user explicitly confirmed that all account data must be private, with no sharing of any kind between accounts in the current scope, and approved the consolidated AUD-006 through AUD-009 documentation proposal. The privacy clarification was propagated to the [vision](../product/vision.md#9-constraints-and-assumptions), [FR-001](../product/requirements.md#fr-001--support-independent-user-accounts), and [NFR-003](../product/requirements.md#nfr-003--keep-all-account-data-private). The remaining changes organize existing uncertainty and refresh navigation; they do not resolve open product or architectural questions.

| Finding | Progress in this batch | Preserved uncertainty |
|---|---|---|
| AUD-006 | Resolved: NFR-003 directly verifies that one independent account cannot read or change another account's data through supported interfaces, covering recipient context, ideas and candidates, research, occasions, and history. No supported interface permits account-to-account sharing in the current scope. | No authorization architecture was selected. |
| AUD-007 | Resolved: [domain questions](../domain/domain-model.md#11-unresolved-modeling-questions) and [glossary entries](../product/glossary.md#open-terminology-questions) are explicitly contextual summaries linked to the authoritative register. All 20 DM-Q identifiers are preserved and mapped. OQ-012 now covers the permanent-deletion uncertainty already present in DM-Q015; OQ-016 references DM-Q016. Existing additional outcome vocabulary is retained in OQ-003. | Archive/reactivation, deletion effects, outcomes, candidate identity, and other existing questions remain open. Unspecified attributes and AI Action representation remain local context until they meet the register's inclusion criteria. |
| AUD-008 | Resolved together with the second batch: [FR-015](../product/requirements.md#fr-015--associate-a-giftidea-with-an-occasion) and the [dependency table](../product/requirements.md#7-open-dependencies) now reference OQ-005 and state the duplicate-association boundary. The second batch already supplied the OQ-010 reconciliation-correction references. | Duplicate associations and correction/reopening remain undecided. Forward operations remain established. |
| AUD-009 | Resolved: the [documentation guide](../README.md#documentation-inventory) describes existing files, including the audit and ADR index. Current ownership/navigation prose uses relative links and no longer treats existing documents as future files. Original generation circumstances are explicitly historical provenance. | No documentation ownership or accepted ADR decision changed. |

**Remaining audit work:** AUD-003 remains partially resolved: OQ-020 owns the remaining idea-pool and presentation rules, and OQ-014 owns the usefulness threshold. AUD-004's tracking gap was addressed in the first batch; OQ-019 still owns the actual pre-occasion timing and settings policy. Other existing open questions remain open. This batch does not establish reminder schedules, duplicate handling, deletion behavior, or an authorization implementation.

**Change status:** Applied for review and left uncommitted. The original audit and both previous follow-ups remain unchanged above.

**Verification:** Checked 17 Markdown files, 222 relative/local links and heading targets, and the targets of 32 preserved historical absolute references using their repository equivalents. All 107 requirement/rule/question definitions, question priorities and statuses, category counts, and all 20 DM-Q mappings passed verification. Accepted ADR files and the entire previously committed audit remain unchanged. Markdown table column checks and `git diff --check` passed. No application tests were run for this documentation-only batch.

### Follow-up — fourth approved remediation batch

**Source conversation:** Codex session `01a197c6-c2a0-743a-ac29-8c6857ceb05a`, obtained from the session environment. This continues the discussion recorded in the third follow-up. The third batch was reviewed and merged through [PR #1](https://github.com/aporfipeter/giftpipe/pull/1), with merge commit `b7a2639`.

**Approved scope:** After discussing eligibility, the ranked preview and full-list modal, Add/Remove controls, retained occasion contexts, and separate research navigation, the user authorized applying the consolidated next batch on a new branch, committing, and pushing it for review. This section records that approved documentation scope; it does not select the remaining ranking or lifecycle policies.

| Underlying decision | Propagation | Preserved uncertainty |
|---|---|---|
| An otherwise eligible GiftIdea remains available despite prior use or association with other Occasions. | The [vision](../product/vision.md#7-product-principles), [FR-006](../product/requirements.md#fr-006--keep-giftideas-reusable-across-occasions), [FR-021](../product/requirements.md#fr-021--remind-users-about-approaching-occasions), the planning workflow, glossary overview/reminder definitions, and OQ-014/OQ-020. Recipient and archival constraints still apply. This resolves AUD-003's remaining eligible-pool ambiguity. | OQ-014's manual-idea usefulness threshold remains open; display eligibility does not settle proactive AI triggering. |
| The Occasion Overview distinguishes the current plan from other available ideas and ranks available ideas using business rules. The first x appear in a preview; View all exposes the full eligible list in a modal. | FR-021, its workflow and dependency entry, the [Occasion Overview](../product/glossary.md#occasion-overview), and OQ-020. | Exact ranking rules, x, and remaining presentation details stay under OQ-020. No ranking formula, score, or configuration policy was chosen. |
| Contextual Add/Remove actions and rounded + / − controls change planning participation while preserving the OccasionGiftIdea. | [FR-015](../product/requirements.md#fr-015--associate-a-giftidea-with-an-occasion), FR-017, the domain concept/lifecycle, the glossary, and new [DM-R023](../domain/domain-model.md#dm-r023--planning-participation-preserves-the-occasion-context). Removal changes lifecycle participation only; both references, research, candidate context, and outcomes remain intact. Re-addition restores that same context. | Exact state names/representation and behavior after completed reconciliation remain under OQ-003/OQ-010. Re-addition does not create a fresh context, but broader duplicate-context rules remain under OQ-005. Individual candidate-withdrawal behavior under OQ-009 is unchanged. |
| The GiftIdea page exposes its OccasionGiftIdeas, including withdrawn contexts, for reviewing occasion-specific research. General research has a separate section or link. | [FR-010](../product/requirements.md#fr-010--support-hierarchical-research), [DM-R014](../domain/domain-model.md#dm-r014--general-and-occasion-specific-research-remain-distinct), the GiftIdea/OccasionGiftIdea concepts, glossary definitions, and OQ-008/OQ-020. | Browsing does not move or merge research. Promotion between scopes stays under OQ-008; detailed research-workflow UI remains deferred. |

**Preserved boundaries:** Planning withdrawal is not reconciliation and does not record that an idea was not given or archive the GiftIdea. Viewing ideas and research still has no planning side effects. No authorization architecture, aggregate/persistence design, notification schedule, channel commitment, or accepted ADR decision was changed. Existing identifiers and question priorities/statuses are retained; OQ-003, OQ-005, OQ-008, OQ-010, OQ-014, and OQ-020 are narrowed or contextualized rather than silently closed. AUD-004's tracking gap remains addressed, while its actual timing policy remains under OQ-019.

**Review scope:** This batch is prepared on `docs/occasion-planning-research` for a separate GitHub review. All previous audit content remains unchanged above.

**Verification:** Checked 17 Markdown files, 231 relative/local links and heading targets, and the targets of 32 preserved historical absolute references through their repository equivalents. All 107 existing requirement/rule/question definitions were retained, with DM-R023 bringing the total to 108. Question priorities/statuses, category counts, all 20 DM-Q mappings, Markdown table columns, accepted ADR files, and the previously committed audit prefix passed verification. `git diff --check` passed. No application tests were run for this documentation-only batch.

### Follow-up — fifth approved remediation batch

**Source conversation:** Codex session `01a197c6-c2a0-743a-ac29-8c6857ceb05a`, obtained from the session environment. This continues the discussion recorded above. The fourth batch was merged through [PR #2](https://github.com/aporfipeter/giftpipe/pull/2), with merge commit `681e340`.

**Approved scope:** The user approved the consolidated reminder-settings proposal and authorized the same new-branch, commit, push, and GitHub review workflow. The changes define the MVP pre-occasion schedule while preserving separate uncertainty about recurrence, detailed timezone changes, and notification delivery. The original audit and all prior follow-ups remain unchanged above.

| Underlying decision | Propagation | Remaining uncertainty |
|---|---|---|
| Up to three global reminder defaults; initially one for five days before at 8 AM. Defaults are copied into newly created Occasions and become independently editable entries, counted toward five per Occasion. | New [FR-037](../product/requirements.md#fr-037--manage-occasion-reminder-defaults-and-schedules), new [DM-R024](../domain/domain-model.md#dm-r024--reminder-defaults-create-independently-owned-occasion-schedules), the Occasion concept, [glossary defaults/schedule definitions](../product/glossary.md#reminder-defaults), and the vision. Global changes affect new Occasions, not existing schedules. | Recurring-instance creation and application of defaults to future occurrences remain under OQ-004. No persistence or aggregate classification was selected. |
| The global + action adds a row up to three; the occasion bell exposes one/seven/thirty-day presets and a calendar marking the Occasion date. Occasion entries expose date/time editing and deletion. | FR-037 and glossary. Reminders are active or deleted; the earlier disabling idea is outside the approved MVP. | The contemplated detailed GiftIdea planning/deadline system is future exploration, not part of this batch. |
| Exact offset/time duplicates among defaults and exact date/time duplicates within an Occasion merge silently without consuming another slot. Different times on the same day are allowed as separate alerts. | FR-037 and DM-R024. This replaces the earlier tentative once-per-day consolidation idea. | Delivery/retry behavior remains under OQ-019; no cross-Occasion deduplication policy was introduced. |
| Date moves retain relative day offsets and local delivery times, including for calendar-selected entries. A move that would expire x reminders requires confirmation before applying the move and deleting those entries. | FR-037 and DM-R024. Cancelling preserves the original date and schedules. | Detailed handling of already-delivered reminders and time boundaries remains under OQ-019. |
| Expired defaults are skipped at creation without fallback or immediate catch-up. If no reminder is scheduled, show an informational warning, even when the defaults list is empty. | FR-037 and DM-R024. Wording: "No reminder is set for this occasion. Consider setting one manually." The user may continue without a reminder. If some defaults are schedulable, those are created. | No new manual past-time validation or notification-delivery policy was selected. |
| Delivery time defaults to 8 AM and is changeable globally and on individual occasion reminders. Schedules use an account timezone, initially from the device and explicitly changeable. | FR-037, DM-R024, glossary, and narrowed OQ-019. Travel does not automatically change the account timezone. | Effects of explicit timezone changes on existing schedules, daylight-saving boundaries, and operational delivery behavior remain open. |

**Audit progress:** AUD-004's main schedule/defaults ambiguity is now addressed through FR-037 and DM-R024, and FR-021 and its dependency table reference the established policy. OQ-019 remains open for genuinely unresolved timing and delivery details. No recurring model, notification channel, post-occasion follow-up policy, proactive AI threshold, ranking formula, or research-promotion behavior was decided. Accepted ADRs, existing identifiers, question priorities/statuses, and prior audit content are preserved. The possible ADR for the earlier withdrawal/context-retention decision remains outside this reminder batch.

**Review scope:** Prepared on `docs/occasion-reminder-settings` for a separate GitHub review.

**Verification:** Checked 17 Markdown files, 246 relative/local links and heading targets, and the targets of 32 preserved historical absolute references through repository equivalents. All 108 existing requirement/rule/question definitions were preserved; FR-037 and DM-R024 bring the total to 110. Question priorities/statuses, category counts, all 20 DM-Q mappings, Markdown table columns, accepted ADR files, and the entire previously committed audit prefix passed verification. `git diff --check` passed. No application tests were run for this documentation-only batch.
