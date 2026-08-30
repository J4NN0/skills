# Skills

Personal collection of [Claude Code agent skills](https://docs.claude.com/en/docs/claude-code/skills).

| Skill | Description |
|---|---|
| `find-docs` | Fetch up-to-date library/framework docs via Context7[^1] |
| `grilling` | Stress-test a plan or design through relentless questioning[^2] |
| `grill-me` | `/grill-me` — a relentless interview to sharpen a plan or design[^2] |
| `pr-description` | Write concise PR descriptions from the repo diff and context |
| `systematic-debugging` | Four-phase debugging with root cause analysis before fixes[^3] |

## Installation

<details>
<summary><strong>Claude Code</strong></summary>

```bash
claude plugins install j4nn0-skills
```

Or, from inside a session:

```bash
/plugin install j4nn0-skills
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
