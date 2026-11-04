# ADR-008 — Keep proactive suggestions transient until explicitly adopted

**Status:** Accepted

**Decision area:** Product

## Context

Occasion reminders lead users to saved ideas or, when no useful captured ideas exist, suggestions they can consider. Automatically saving every generated option would accumulate ideas and research the user never chose. Conversely, discarding a list immediately after one choice or while inspecting details would interrupt comparison.

ADR-005 establishes bounded gifting AI and retained useful research. This decision distinguishes transient proactive proposals from research and ideas the user deliberately retains; ADR-005 remains Accepted and is not superseded.

## Decision

- A due Occasion Reminder initiates the usefulness check against the Person's eligible idea pool. User-supplied idea content or an explicitly adopted suggestion is enough; empty recipient-only placeholders are not, even when deliberately planned.
- The Occasion Overview preserves current-plan intent, including explicitly added empty stubs, but excludes empty placeholders outside the current plan from the available-ideas picker. When useful saved ideas are absent, show an empty state and a separate Consider these section.
- Generated proposals stay transient until an explicit Add to occasion action creates a GiftIdea containing the chosen suggestion and its OccasionGiftIdea context. Adoption supplies useful saved intent for later reminder checks. It does not automatically classify concrete wording as a GiftCandidate.
- Other suggestions remain during the current occasion-planning visit, including detail, research, and modal navigation. They are discarded on departure; adopted ideas remain durable.
- For normal navigation away from a reminder-originated visit, confirm departure when transient suggestions exist and none were adopted. Do not require that guard after adoption or when suggestions were absent. Hard-close interception and recovery are outside the MVP.
- Explicit bounded AI actions remain available when automatic generation is suppressed or the transient list has been discarded.

## Alternatives considered

### Persist every generated suggestion automatically

Rejected for this proactive proposal stage: display alone is not user intent to capture an idea or retain research. Useful saved AI research remains governed by ADR-005 and DM-R016.

### Turn a chosen suggestion directly into a candidate

Not selected for this flow. Candidates require a GiftIdea context; Add to occasion instead creates the chosen idea and its occasion context without attaching it to an unrelated stub or inferring a candidate from specificity.

### Discard the list after the first adoption or any detail navigation

Rejected because the user may still be comparing options and must be able to inspect research without losing alternatives.

### Recover transient suggestions after a hard close

Deferred outside the MVP. The normal navigation guard protects the unadopted visit without requiring a durable suggestion archive or recovery behavior.

## Consequences

Adoption is the explicit boundary between transient proposals and saved planning intent. Recipient-only capture remains valid; visibility and AI usefulness are separate from capture validity. Keeping the list for one planning visit supports selection while leaving unchosen proposals out of permanent idea/research history. The product contract does not dictate storage technology or prevent temporary state needed to render the list.

Exact generation timing, handoff, late-entry, and failure handling remain unresolved under OQ-022. Ranking, suggestion count, and other detailed presentation remain under OQ-020. This does not resolve recurrence, candidate identity, research-node taxonomy, or research promotion.

## Related documentation

- [Requirements](../product/requirements.md#fr-022--avoid-unnecessary-proactive-ai-when-useful-manual-ideas-exist): FR-022, FR-023, FR-038.
- [Domain rule DM-R025](../domain/domain-model.md#dm-r025--proactive-suggestions-become-durable-ideas-only-through-adoption).
- [ADR-005](ADR-005-bound-ai-to-explicit-gifting-actions.md): bounded actions and retained useful research.
- [Open Questions Register](../discovery/open-questions.md): resolved OQ-014; active OQ-020 and OQ-022.

## Source conversation

Codex session `01a197c6-c2a0-743a-ac29-8c6857ceb05a`, obtained from the session environment. The user approved the consolidated batch before documentation edits.
