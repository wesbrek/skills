# skills

A [Claude Code](https://claude.com/claude-code) plugin marketplace by **wesbrek**.
Skills are written to be **tool-agnostic** so they also work in other agents/CLIs.

## Plugins

| Plugin | Description |
|--------|-------------|
| [`open-pr`](./plugins/open-pr) | Open a GitHub PR for the current change — writes the title/description from the diff and embeds screenshots/videos inline via [`pr-media`](https://github.com/MatteoSchifano/gh-pr-media). |

## Install (Claude Code)

```shell
/plugin marketplace add wesbrek/skills
/plugin install open-pr@wesbrek
/reload-plugins
```

Then invoke with `/open-pr` (or just ask to open a pull request).

## Install (other tools)

There is no single skill format every CLI reads, so each skill's `SKILL.md`
body is plain, tool-neutral markdown — a procedure any agent can follow.

- **Copy into any project** (Claude Code standalone):
  ```bash
  npx degit wesbrek/skills/plugins/open-pr .claude/skills/open-pr
  ```
- **Cursor / Windsurf (rules)** — paste the `SKILL.md` body into a rule file
  (e.g. `.cursor/rules/open-pr.mdc`); ignore the frontmatter.
- **Copilot CLI / Codex / others** — reference the `SKILL.md` body from your
  tool's instruction/prompt file, or paste it into an `AGENTS.md`.

The frontmatter is a convenience for tools that support it; the **body is the
portable contract**.

## License

[MIT](./LICENSE) © wesbrek
