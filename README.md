# Skills

Personal collection of [Claude Code agent skills](https://docs.claude.com/en/docs/claude-code/skills).

| Skill | Description |
|---|---|
| `find-docs` | Fetch up-to-date library/framework docs via Context7[^1] |
| `grill-me` | A relentless interview to sharpen a plan or design[^2] |
| `pr-description` | Write concise PR descriptions from the repo diff and context |
| `systematic-debugging` | Investigates failures with reproducible evidence before applying a verified fix[^3] |
| `to-ticket` | Draft a standalone ticket from the current conversation |

## Installation

<details>
<summary><strong>Claude Code</strong></summary>

Add the marketplace once, then install:

```bash
claude plugin marketplace add J4NN0/skills
claude plugin install j4nn0-skills@J4NN0
```

Or, from inside a session:

```
/plugin marketplace add J4NN0/skills
/plugin install j4nn0-skills@J4NN0
```

To pick up new or updated skills later:

```bash
claude plugin marketplace update J4NN0
claude plugin update j4nn0-skills
```

</details>

<details>
<summary><strong>Codex, and other agents</strong></summary>

```bash
npx skills@latest add j4nn0/skills
```

Choose the skills you need, and the coding agents to install them in.

</details>

[^1]: [Context7](https://context7.com/install)
[^2]: Adapted from [mattpocock/skills — grill-me](https://github.com/mattpocock/skills/blob/main/skills/productivity/grill-me/SKILL.md)
[^3]: Adapted from [obra/superpowers — systematic-debugging](https://github.com/obra/superpowers/blob/main/skills/systematic-debugging/SKILL.md)
