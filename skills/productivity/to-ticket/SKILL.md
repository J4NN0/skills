---
name: to-ticket
description: Draft a concise, standalone engineering or product ticket from the useful context, findings, decisions, and proposed fix in the current conversation. Use when the user wants to turn a discussion into a ticket or issue for later tracking; do not use for status summaries or to create an issue in an external tracker unless explicitly requested.
---

# To Ticket

Turn the current conversation into a ticket that someone can understand without reading the chat.

## Draft the ticket

- Extract only ticket-relevant facts: the situation, actual and desired behavior, impact, confirmed cause, constraints, decisions, and agreed fix.
- Prefer the latest conclusion when the conversation contains exploration or superseded ideas.
- State confirmed findings as facts. Mark unresolved or inferred details clearly, and do not invent reproduction steps, scope, priority, estimates, owners, identifiers, or technical conclusions.
- Describe the problem and outcome before implementation detail. Include code locations or technical notes only when they help the future implementer.
- Do not narrate the conversation or use phrases such as "we discussed."
- Ask a question only when a missing detail prevents a coherent or materially accurate ticket. Otherwise draft the best ticket possible and omit unsupported fields.

## Output format

Return only a ready-to-copy Markdown ticket with this shape, adapting or omitting optional sections to keep it concise:

```markdown
# <short, action-oriented title>

## Context
<Why this matters and the minimum background needed.>

## Problem
<What happens now, when it happens, and the relevant impact.>

## Expected outcome
<The desired behavior or agreed result.>

## Implementation notes
<Optional: confirmed cause, agreed approach, constraints, or relevant code locations.>

## Acceptance criteria
- <Optional: 2-4 observable, testable conditions>

## Open questions
- <Optional: only unresolved decisions that matter>
```

Use compact prose and short bullets. Aim for roughly 100-250 words; use fewer when the issue is simple. Avoid repeating the same point across sections. Omit `Implementation notes`, `Acceptance criteria`, or `Open questions` when the conversation provides no useful content for them.

Draft only. Creating or updating a ticket in an external system requires a separate explicit request.
