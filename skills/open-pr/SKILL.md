---
name: open-pr
description: Open a GitHub PR for the current change — writes the title and description from the diff and embeds screenshots/videos inline via pr-media. Use when the user asks to open, create, submit, or raise a pull request, especially one that should include images or a video demo.
---

# Open PR

Open a high-quality GitHub pull request for the current change, with images and
videos embedded **inline** in the description.

> **Tool-agnostic.** This file is written as a plain procedure so any coding
> agent or CLI can follow it (Claude Code, and others). The frontmatter above is
> for tools that read the "skills" format; tools that don't can ignore it and
> just follow the steps. See `PORTABILITY` in the repo README for adapters.

**Run this with full context of the change.** It needs the diff and the reasoning
behind it. Do not hand it to a fresh/isolated agent that only sees the file — it
would lose the "why" that makes a good PR description.

## Prerequisites

- `gh` installed and authenticated (`gh auth status`). **Required.**
- [`pr-media`](https://github.com/MatteoSchifano/gh-pr-media) for inline media —
  **optional**. Install via `gh extension install MatteoSchifano/gh-pr-media` or
  `npm i -g pr-media`. Inline **playing video** needs its `browser` strategy,
  which drives an already-logged-in local browser, so it assumes a developer
  machine, not headless CI. If `pr-media` is absent, still open the PR (see step 5).

## Process

### 1. Establish the change context

- Confirm the current branch and the base branch (usually the repo default).
- Read the diff against the base (`git diff <base>...HEAD`, `git log`, changed
  files). Summarize what changed and, critically, **why**.
- If the branch is the default branch, stop and offer to create a feature branch
  first — never open a PR from an unrelated default branch.
- If there are uncommitted changes, ask whether to commit them first.

### 2. Gather media

- Ask for image/video paths, or use paths already produced in the session
  (e.g. Playwright output).
- If the change is visual and no media exists, offer to capture it — but do not
  block PR creation on it.

### 3. Draft the PR

Write a title and description from the change context. Match the repo's
conventions if a `PULL_REQUEST_TEMPLATE` exists.

- **Title:** concise, imperative, conventional-commit style if the repo uses it.
- **Body:** what changed, why, how to test, linked issues (`Closes #N`) when
  applicable. Leave a clear spot where media will go.

Show the draft to the user before creating anything.

### 4. Create the PR

```bash
gh pr create --base <base> --head <branch> --title "<title>" --body "<body>"
```

Capture the PR URL from the output.

### 5. Embed media inline (graceful if unavailable)

If `pr-media` is installed, delegate the media step to it — do not reimplement
the upload:

```bash
gh pr-media add <files...> --pr-url <pr-url> --to description
```

- On a dev machine, prefer `--strategy browser` so video plays inline; `auto`
  picks it outside CI.

If `pr-media` is **not** installed: the PR is already open — do not fail. Tell the
user: "PR created. Install `pr-media` to embed images/video inline," and stop.
The core value (a well-authored PR) must never be hostage to the media tool.

### 6. Report

Print the final PR URL and a one-line summary (title, files embedded, strategy).

## Notes

- `pr-media` never reads or stores your session cookie; the `browser` strategy
  reuses your logged-in browser session. Never add a cookie-extraction step.
- Authorship and media hosting are decoupled — it does not matter which account
  uploads the media; the PR author is whoever `gh` is authenticated as.
