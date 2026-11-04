# GiftPipe Documentation Guide

## Purpose

This directory contains GiftPipe's durable project documentation.

GiftPipe preserves conversation-driven discovery in Git-versioned Markdown documentation. The goal is to preserve established product and domain knowledge without prematurely freezing unresolved design work.

The documentation should help both human contributors and AI coding agents understand:

- what GiftPipe is trying to achieve,
- which domain concepts and rules are established,
- which requirements are expected,
- why important decisions were made,
- and which questions are still open.

This structure is intentionally lightweight and may evolve as the project matures.

---

## Documentation structure

The current documentation structure is:

```text
giftpipe-docs/
├── README.md
├── product/
│   ├── vision.md
│   ├── glossary.md
│   └── requirements.md
├── domain/
│   └── domain-model.md
├── decisions/
│   ├── README.md
│   └── ADR-*.md
├── discovery/
│   └── open-questions.md
└── audits/
    └── documentation-audit.md
```

The documents below exist in the repository. The decision index links to the eight Accepted ADRs; the audit records the original assessment and subsequent approved remediation batches.

**Generation provenance:** When this guide was first established, only the guide was being created here. The glossary had been produced during discovery but its repository presence was unverified; other paths described the planned organization. The inventory below now describes existing files.

---

## Documentation inventory

| Path                          | Responsibility                     | Contains                                                                                                |
| ----------------------------- | ---------------------------------- | ------------------------------------------------------------------------------------------------------- |
| [giftpipe-docs/README.md](README.md)     | Documentation map                  | Structure, information ownership, maintenance principles, and guidance for contributors and agents      |
| [product/vision.md](product/vision.md)           | Product direction                  | Purpose, target problem, goals, scope, exclusions, product principles                                   |
| [product/glossary.md](product/glossary.md)         | Shared language                    | Canonical domain terms, aliases, distinctions, terminology status                                       |
| [product/requirements.md](product/requirements.md)     | Expected behavior                  | Functional and non-functional requirements, capabilities, user-observable behavior, acceptance criteria |
| [domain/domain-model.md](domain/domain-model.md)      | Domain structure and rules         | Entities, value objects, relationships, invariants, lifecycles, domain processes                        |
| [decisions/README.md](decisions/README.md)         | Decision-record index and guidance | Significant decisions and links to the existing individual decision records                   |
| [discovery/open-questions.md](discovery/open-questions.md) | Active uncertainty                 | Unresolved questions, competing proposals, deferred design topics, areas requiring investigation        |
| [audits/documentation-audit.md](audits/documentation-audit.md) | Audit and remediation history | Baseline findings, approved follow-up scope, progress, and source-conversation provenance |

The inventory describes the current repository. New documents or directories should be introduced when they provide a clear ownership boundary, not merely because more files seem desirable.

---

## Information ownership

Each important piece of project knowledge should have one primary authoritative location.

Other documents may summarize or reference that information, but should avoid maintaining independently evolving copies.

### Product vision

[product/vision.md](product/vision.md) owns the explanation of:

- why GiftPipe exists,
- which problems it addresses,
- product goals,
- intended scope,
- explicit exclusions,
- high-level product principles.

It should explain product direction without becoming a detailed requirements or domain-model document.

### Glossary

[product/glossary.md](product/glossary.md) owns:

- canonical GiftPipe terminology,
- concise domain definitions,
- synonyms and aliases,
- distinctions between similar terms,
- terminology whose meaning remains tentative or unresolved.

Other documentation should use glossary terminology consistently rather than redefining established terms.

### Domain model

[domain/domain-model.md](domain/domain-model.md) owns:

- domain entities and value objects,
- relationships between concepts,
- invariants,
- lifecycle behavior,
- domain processes,
- rules governing how concepts interact.

The glossary explains what terms mean. The domain model explains how those concepts behave together.

### Requirements

[product/requirements.md](product/requirements.md) owns:

- functional expectations,
- non-functional expectations,
- user-observable behavior,
- business capabilities,
- acceptance criteria.

Requirements should describe what the product must do without unnecessarily prescribing implementation details.

### Decision records

`decisions/` owns significant decisions whose rationale is worth preserving.

A decision record should capture enough context to explain:

- the problem being decided,
- alternatives considered where relevant,
- the selected direction,
- rationale,
- important consequences.

Not every minor implementation choice needs a formal decision record.

### Discovery

[discovery/open-questions.md](discovery/open-questions.md) owns unresolved material.

This includes:

- unanswered product questions,
- competing domain interpretations,
- deferred design decisions,
- uncertain terminology,
- areas requiring experimentation or validation.

Open questions should not be silently promoted into requirements or domain rules until they are actually resolved.

---

## How the documents relate

The documents complement one another but do not form a mandatory waterfall process.

The glossary provides the shared vocabulary used throughout the project.

The domain model uses that vocabulary to describe concepts, relationships, rules, and lifecycle behavior.

Requirements describe the system behavior users and external actors should be able to observe.

The product vision explains why those capabilities matter and what GiftPipe is intentionally trying to become or avoid becoming.

Decision records preserve the reasoning behind important choices that shape any of those areas.

Open questions collect uncertainty that has not yet earned a place as established knowledge.

Documentation can be created and refined iteratively as discovery continues.

Link to existing documents using relative Markdown links. For a future file that does not exist, use an inline path until it is created.

---

## Maintenance principles

### 1. Single source of truth

Each definition, rule, requirement, or decision should have one authoritative location.

Prefer references over duplicated descriptions that may drift apart.

### 2. Explicit certainty

Confirmed, tentative, and unresolved knowledge must remain distinguishable.

Do not present exploratory proposals as established behavior.

### 3. Evidence over invention

Document what has actually been established through discovery, implementation, or explicit decision.

Do not silently fill gaps merely to make a document appear complete.

### 4. Consistent terminology

Use canonical terms from the glossary throughout the documentation.

When terminology changes, update affected documents rather than allowing old and new names to coexist ambiguously.

### 5. Change propagation

When established knowledge changes, inspect the documentation that depends on it.

Update definitions, relationships, requirements, decision references, or open questions where necessary.

### 6. Relative links

Use relative Markdown links when referring to documentation files that already exist in the repository.

Keep documentation portable within the repository.

### 7. Lightweight maintenance

Prefer useful documentation over documentation process.

Avoid unnecessary duplication, elaborate templates, excessive metadata, or governance overhead that does not improve understanding.

---

## Modularization

Start with cohesive documents rather than aggressively splitting documentation into many small files.

Documents such as [product/glossary.md](product/glossary.md) and [domain/domain-model.md](domain/domain-model.md) should remain unified while they are easy to understand and maintain.

Consider splitting a document when:

- it contains multiple independently evolving responsibilities,
- a domain area develops substantial self-contained rules or behavior,
- navigation or maintenance becomes difficult,
- duplication suggests that information ownership is unclear,
- AI coding agents repeatedly struggle to retrieve the relevant context.

Do not split documentation solely because a file exceeds a particular line count, size, or token count.

Before substantial reorganization:

- identify meaningful conceptual boundaries,
- preserve existing decisions and uncertainty markers,
- retain a concise parent overview or index,
- update affected cross-references,
- avoid creating unnecessary fragmentation.

Documentation boundaries do not need to mirror domain entities, aggregates, modules, services, or database tables.

---

## Guidance for AI coding agents

Before making domain-sensitive changes:

1. Read this file to understand documentation ownership.
2. Consult the relevant authoritative documents.
3. Verify that a referenced planned document actually exists before relying on it.
4. Do not treat tentative or unresolved material as approved requirements.
5. Use canonical glossary terminology.
6. Update the owning document when established terminology or behavior changes.
7. Surface contradictions or missing information instead of silently choosing an interpretation.
8. Propose substantial documentation restructuring before performing it.

This guidance complements, but does not replace, the repository-level instructions in [AGENTS.md](../AGENTS.md).

---

## Evolving this structure

This documentation map should change when the project develops a clear need for different boundaries.

The purpose of the structure is to make GiftPipe knowledge easier to preserve, retrieve, and evolve—not to constrain future discovery.
