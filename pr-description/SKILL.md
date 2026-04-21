---
name: pr-description
description: write concise github pull request descriptions for code changes using the current repository context and recent conversation context. use when the user asks for a pr description, pr summary, or rationale for a branch or diff, especially from inside a repo with the branch already pushed. inspect git and gh context first when available, then explain the context behind the change and what changed in short, natural prose. prefer prose-first output with minimal bullets and avoid over-structured templates unless a small cleanup section clearly benefits readability.
---

# PR description

Write short GitHub-ready PR descriptions that explain **why the change exists** and **what changed**.

Favor natural prose over rigid templates. The best output usually reads like a short engineering note, not release notes.

## Working style

Before drafting, ground the description in available context.

1. Use recent conversation context first if it already explains the motivation.
2. Inspect the repository state when available.
3. Infer the implementation shape from the diff and touched files.
4. Prefer a confident draft over asking the user to restate the rationale.
5. Only mention details that are visible in the diff, commit history, or conversation context.

## Repository inspection

When tools are available, inspect in roughly this order:

- `git status --short`
- `git branch --show-current`
- `git diff --stat origin/main...HEAD`
- `git diff origin/main...HEAD`
- `git log --oneline origin/main..HEAD`
- `gh pr diff` if a PR already exists
- `gh pr view --json title,body,files` if useful

If `origin/main` is not the right base, use the repo's actual default branch.

Also pay attention to:

- renamed or moved files
- deleted code paths
- config/env var cleanup
- extracted shared utilities
- package or dependency changes
- whether the change is behavioral or mostly structural

## Output shape

Default to this structure:

```markdown
[1 short paragraph of context]

[1 short paragraph starting with "This PR ..." describing the change made]
```

Keep it brief. Usually 2 paragraphs is enough.

### Preferred style

- First paragraph: explain the previous state, inconsistency, ambiguity, duplication, or limitation.
- Second paragraph: explain what this PR does to resolve it.
- Use plain prose.
- Keep the tone crisp and engineering-focused.
- Avoid headings when the two-paragraph form reads cleanly.
- Avoid filler like "## Summary", "## Testing", or "## Checklist" unless the user asked for them.
- Avoid long bullet lists by default.

### When bullets are acceptable

Use bullets only when the PR is a small structural cleanup and a short list makes the exact changes clearer.

In that case use this shape:

```markdown
[1–2 sentences of context and framing]

This PR [one-sentence summary].

- [change 1]
- [change 2]
- [change 3]
```

Keep bullets short and few. Usually 3 bullets is enough.

## What to emphasize

Prioritize:

1. production or codebase ambiguity removed
2. consistency with existing patterns
3. extracted duplication or shared setup
4. clearer ownership of configuration and wiring
5. package/module boundary cleanup
6. removal of dead or misleading provider paths

Do not just narrate the diff. Explain the reason the diff matters.

## Writing rules

- Start from the motivation, not the file list.
- Use "This PR ..." in the second paragraph unless a bullet variant is clearly better.
- Name old and new paths only when they help the reader understand the refactor.
- Mention internal symbols, env vars, classes, or packages only when they are load-bearing.
- Prefer "aligns", "extracts", "removes", "replaces", "consolidates", "makes X the only path", "separates concerns".
- Avoid exaggerated claims like "improves performance" or "simplifies" unless the diff clearly supports them.
- Avoid saying "refactor" alone; explain what became more consistent or less ambiguous.
- Keep output compact enough to paste directly into GitHub without editing.

## Examples

### Example: provider cleanup

```markdown
The embedding client was recently switched to Vertex AI, but the old config and code still supported an OpenAI path selectable via an `EMBEDDING_PROVIDER` env var. This left the codebase in an ambiguous state where it was not clear from reading the code which provider was actually active in production.

This PR makes the Vertex path the only path by removing the OpenAI embedding dependencies and related provider wiring.
```

### Example: implementation consistency

```markdown
The previous `GeminiEmbeddingClient` used the Google AI Studio REST API, authenticated via a `GEMINI_API_KEY` query parameter. This was inconsistent with the rest of the Gemini and Claude integration, which already authenticates through Vertex via the `google-genai` SDK.

This PR replaces that implementation with the same SDK pattern and introduces a dedicated `embeddings/` package to separate embedding concerns from `llm/`.
```

### Example: shared utility extraction

```markdown
The same `_setup_gcp_credentials()` logic existed in both `llm/gemini.py` and `llm/claude.py`, and would have been duplicated again for Vertex-based embeddings.

This PR extracts that logic into `gcp_credentials.py` as a standalone `setup_gcp_credentials()` helper and moves the call to application startup so credentials are initialized once per process rather than on every provider instantiation.
```

### Example: small cleanup with bullets

```markdown
This PR aligns embedding client initialization and settings loading. Most of the embedding logic is unchanged; this is a small structural cleanup to make `OpenAIEmbeddingClient` consistent with how `GeminiEmbeddingClient` was already set up.

- `OpenAISettings` moves into the central config module, following the existing pattern for environment-backed settings
- `OpenAIEmbeddingClient` now receives `api_key` and `model` through its constructor instead of loading settings internally
- `UnifiedEmbeddingProvider` becomes the single place that reads settings and wires both embedding clients
```

## Fallback behavior

If the rationale is weak or only partly visible from the diff:

- make the motivation as specific as the evidence allows
- avoid inventing business context
- phrase uncertain intent conservatively

Good fallback:

```markdown
The current implementation spreads provider setup across multiple modules, which makes initialization behavior harder to follow and easy to duplicate when adding new providers.

This PR consolidates that setup into a shared helper and updates provider wiring so initialization happens in one place.
```
