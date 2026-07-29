# skills

A collection of coding-agent skills, written to be **tool-agnostic**.

## Skills

| Skill | Description |
|-------|-------------|
| [`open-pr`](./skills/open-pr) | Open a GitHub PR for the current change — writes the title/description from the diff and embeds screenshots/videos inline via [`pr-media`](https://github.com/MatteoSchifano/gh-pr-media). |

## Portability

There is no single skill format every CLI reads, so each skill's body is plain,
tool-neutral markdown — a procedure any agent can follow. Use it in your tool of
choice:

- **Claude Code / skills-format tools** — copy the folder into `.claude/skills/`
  (project) or `~/.claude/skills/` (global). The YAML frontmatter drives
  auto-invocation. Invoke with `/open-pr` or by asking to open a PR.
  ```bash
  npx degit wesbrek/skills/skills/open-pr .claude/skills/open-pr
  ```
- **Cursor / Windsurf (rules)** — paste the skill body into a rule file
  (e.g. `.cursor/rules/open-pr.mdc`); ignore the frontmatter.
- **GitHub Copilot CLI / Codex / others** — reference the skill body from your
  tool's instruction/prompt file, or paste it into an `AGENTS.md`.
- **Any agent** — the steps are just prose + shell commands; point the model at
  the `SKILL.md` body and it will follow them.

The rule of thumb: the frontmatter is a convenience for tools that support it;
the **body is the contract** and is portable everywhere.
