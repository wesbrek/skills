# skills

A collection of [Claude Code](https://claude.com/claude-code) skills.

## Skills

| Skill | Description |
|-------|-------------|
| [`open-pr`](./skills/open-pr) | Open a GitHub PR for the current change — writes the title/description from the diff and embeds screenshots/videos inline via [`pr-media`](https://github.com/MatteoSchifano/gh-pr-media). |

## Install

Copy a skill into your project's `.claude/skills/` (or your global `~/.claude/skills/`):

```bash
# one skill, into the current project
npx degit wesbrek/skills/skills/open-pr .claude/skills/open-pr
```

Then invoke it in Claude Code with `/open-pr`, or let it trigger automatically
when you ask to open a pull request.
