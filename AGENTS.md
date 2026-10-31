# Gift|Pipe|🎁

This project is in product development phase. We are only doing exploration and documentation at this point.

### Documentation change control

Project documentation is the authoritative record of established product and domain decisions. Exploratory discussion does not automatically change that authority.

- Editorial corrections may be made directly when documentation editing is authorized.
- Propagation of an already approved decision may be delegated to Luna, within the established scope.
- Changes to product semantics, domain rules, requirements, or accepted decisions require an explicit documentation change proposal and user approval before editing.
- Sol owns impact analysis, proposal preparation, and final review.
- Luna may apply approved changes but must not introduce new product or architectural decisions.
- Proposals must distinguish approved changes, affected documents, and unresolved implications.
- If implementation reveals an unapproved semantic choice, escalate it rather than deciding independently.
- Preserve existing identifiers, established decisions, and unresolved questions unless their modification is explicitly approved.
- Do not treat a discussion conclusion as permission to modify documentation.
- Do not commit changes without explicit user authorization.

#### Documentation change batching

- During Explore mode, Sol may accumulate documentation-relevant conclusions throughout a discussion without immediately proposing edits.
- Track established decisions, tentative conclusions, and unresolved questions separately.
- When the amount of accumulated documentation-relevant conclusions reaches a logical checkpoint (eg. an area is sufficiently discussed and its points are addressed) or a size that would sit outside the amount of text a human user can digest in one sitting, suggest to perform a documentation update batch.
- Do not treat an idea as approved merely because it was discussed or received positive feedback.
- Maintain the pending change batch within the conversation; do not create files or modify documentation automatically.
- Avoid interrupting exploration with repeated approval requests.
- When the user requests a documentation update or concludes the discussion, consolidate relevant changes into a single proposal.
- Group changes by underlying decision, identifying affected documents and dependencies.
- Distinguish semantic decisions from mechanical documentation propagation.
- Allow the user to approve, reject, defer, or refine individual changes within the batch.
- Apply only approved changes, delegating bounded edits to Luna when useful.
- If approved changes conflict or depend on deferred decisions, identify the conflict before editing.
- Sol reviews the resulting diff for completeness, consistency, and unintended semantic changes.
- Do not commit without explicit user authorization.

A documentation change batch is a proposed set of changes, not an authoritative decision record. Only approved changes propagated into the appropriate documentation become part of the project's documented source of truth.

#### Conversation traceability

- When applying an approved documentation change batch, record the source conversation's session ID in the batch's durable provenance (for example, an audit follow-up section or a relevant decision record), so the user can revisit the actual discussion.
- Obtain the ID from the session environment when available; never invent one. If it is unavailable, state that explicitly.
- Keep approved changes and unresolved proposals distinguishable in the provenance. A session reference does not replace the authoritative product and domain documentation.

Audit-remediation discussion begun on 2026-10-31: session `01a190be-86f0-72b2-9a8a-58b09c8cdac3`. Follow-up scope and progress belong in [the documentation audit](giftpipe-docs/audits/documentation-audit.md).

Follow `giftpipe-docs/README.md` for documentation ownership and cross-reference conventions.
