# Agent Skills

Personal collection of [Claude Code agent skills](https://docs.claude.com/en/docs/claude-code/skills).

## Install

Skills live in `~/.claude/skills`, one folder per skill, each with a `SKILL.md` inside:

```
~/.claude/skills/
├── grilling/
│   └── SKILL.md
├── pr-description/
│   └── SKILL.md
└── ...
```

**Fresh setup** — clone this repo straight into place (requires `~/.claude/skills` to not exist yet):

```sh
git clone https://github.com/J4NN0/agent-skills.git ~/.claude/skills
```

**Already have skills there** — clone to a temp dir and copy the folders in:

```sh
git clone https://github.com/J4NN0/agent-skills.git /tmp/agent-skills
mkdir -p ~/.claude/skills
for d in /tmp/agent-skills/*/; do cp -R "${d%/}" ~/.claude/skills/; done
rm -rf /tmp/agent-skills
```

This copies only the skill folders — `.git` and this README stay behind. Re-run it any time to update; existing skills of the same name are overwritten.

Start a new Claude Code session and the skills show up. Verify by running `/` and looking for them in the list.

## Skills

| Skill | Description |
|---|---|
| `domain-modeling` | Build and sharpen a project's domain model and ubiquitous language |
| `find-docs` | Fetch up-to-date library/framework docs via Context7 |
| `grilling` | Stress-test a plan or design through relentless questioning |
| `grill-me` | `/grill-me` — a relentless interview to sharpen a plan or design |
| `grill-with-docs` | `/grill-with-docs` — same, but writes ADRs and a glossary as it goes |
| `pr-description` | Write concise PR descriptions from the repo diff and context |
| `systematic-debugging` | Four-phase debugging with root cause analysis before fixes |
| `webapp-testing` | Drive and test local web apps with Playwright |

## Sources

- [Systematic Debugging](https://github.com/obra/superpowers/blob/main/skills/systematic-debugging/SKILL.md)
- [Webapp Testing](https://github.com/anthropics/skills/blob/main/skills/webapp-testing/SKILL.md)
- [Context7](https://context7.com/install)
- [Grill Me](https://github.com/mattpocock/skills/blob/main/skills/engineering/grill-with-docs/SKILL.md)
