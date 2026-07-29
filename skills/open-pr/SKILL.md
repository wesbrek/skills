---
name: open-pr
description: Open a GitHub PR for the current change — writes the title and description from the diff and embeds screenshots/videos inline via pr-media. Use when the user asks to open, create, submit, or raise a pull request, especially one that should include images or a video demo.
---

# Open PR

Open a high-quality GitHub pull request for the current change, with images and
videos embedded **inline** in the description.

This skill runs in the main conversation on purpose — it needs the full context
of the change (the diff, what you did and why, the media you generated). Do not
delegate it to a cold subagent that would lose that context.

## Prerequisites

- `gh` installed and authenticated (`gh auth status`).
- [`pr-media`](https://github.com/MatteoSchifano/gh-pr-media) available for inline
  media (`gh extension install MatteoSchifano/gh-pr-media`, or `npm i -g pr-media`).
  Inline **playing video** requires its `browser` strategy, which drives an
  already-logged-in local browser — so this skill assumes it runs on a developer
  machine, not a headless CI runner. (In headless CI, video can only be a link.)

## Process

### 1. Establish the change context

- Confirm the current branch and the base branch (usually the repo default).
- Read the diff against the base (`git diff <base>...HEAD`, `git log`, changed
  files). Summarize what changed and, critically, **why** — pull the reasoning
  from the conversation, not just the diff.
- If the branch is the default branch, stop and offer to create a feature branch
  first — never open a PR from an unrelated default branch.
- If there are uncommitted changes, ask whether to commit them first.

### 2. Gather media

- Ask the user for image/video paths, or use paths already produced in the
  conversation (e.g. Playwright output).
- If the change is visual and no media exists, offer to capture it (e.g. run the
  project's Playwright flow) — but do not block PR creation on it.

### 3. Draft the PR

Write a title and description from the change context. Match the repo's
conventions if a `PULL_REQUEST_TEMPLATE` exists.

- **Title:** concise, imperative, conventional-commit style if the repo uses it.
- **Body:** what changed, why, how to test, and linked issues (`Closes #N`) when
  applicable. Leave a clear spot where media will go.

Show the draft to the user before creating anything.

### 4. Create the PR

```bash
gh pr create --base <base> --head <branch> --title "<title>" --body "<body>"
```

Capture the PR URL from the output.

### 5. Embed media inline

Delegate the media step to `pr-media` — do not reimplement the upload:

```bash
gh pr-media add <files...> --pr-url <pr-url> --to description
```

- On a dev machine, prefer the `browser` strategy so video plays inline
  (`--strategy browser`); `auto` will pick it outside CI.
- Confirm the media rendered by re-fetching the PR body if needed.

### 6. Report

Print the final PR URL and a one-line summary of what was included (title, files
embedded, strategy used).

## Notes

- `pr-media` never reads or stores your session cookie; the `browser` strategy
  reuses your logged-in browser session. Do not add any cookie-extraction step.
- Authorship and media hosting are decoupled — it does not matter which account
  uploads the media; the PR author is whoever `gh` is authenticated as.
