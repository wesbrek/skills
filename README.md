# skills

Agent skills by **wesbrek**.

## Skills

| Skill | Description |
|-------|-------------|
| [`open‑pr`](./skills/open-pr) | Open a GitHub pull request for the current change, with screenshots or a video demo embedded inline via [`pr-media`](https://github.com/MatteoSchifano/gh-pr-media). |

## Install

One command, works across 20+ agents (Claude Code, Cursor, Copilot, Windsurf, and more):

```bash
npx skills add wesbrek/skills
```

Then invoke with `/open-pr`, or just ask your agent to open a pull request.

## Portability

Each skill is written as a plain procedure, so any coding agent or CLI can follow
it by reading the file directly. The YAML frontmatter is there for tools that
read the "skills" format — tools that don't can ignore it and just follow the
steps.

## License

[MIT](./LICENSE) © wesbrek
